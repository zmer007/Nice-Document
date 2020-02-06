设备启动后会启动 init 进程（[init main](http://aospxref.com/android-8.0.0_r36/xref/system/core/init/init.cpp#948)），此进程会根据 `init.rc` 文件进行启动后的相关调用（[文件位置](http://aospxref.com/android-8.0.0_r36/xref/system/core/rootdir/init.rc)），主要是生成许多硬件相关的守护进程，比如 android debug 守护进程，USB 守护进程等，还会启动大名鼎鼎的 'zygote' 进程，它会初始化一个非常原始的 Dalvik VM，并且会提前加载一些 framework 或已安装应用的通用类，之后会进入就绪状态，等待通知随时进行自我复制。zygote 准备就绪后，init 进入 `runtime process`。

zygote 首先会 fork 出一个叫做 SystemServer 的进程，许多核心服务（如 AMS, WMS, PMS 等）由 SystemServer 进行初始化。所有核心服务加载完毕后，系统已准备好加载应用程序了。zygote 会 fork 出第一个应用进行--桌面应用（Launcher）进行展示。

下图以 Android_8.0.0 版本为基础描述从用户点击桌面图标到 `Activity.onCreate()` 回调过程中的关键步骤

![from launcher click to onCreate callback](../resource/launcher2oncreate.jpg)

## 启动 Activity
### 模拟启动 TargetApp
使用 Android Studio 创建一个空白项目（TargetApp），包名为：`com.iyh.targetapp`，包含一个组件 `MainActivity`。在某个 Activity 中启动 `TargetApp.MainActivity` 为起点，分析系统在启动应用时的处理步骤，以下为打开 `TargetActivity.ManiActivity` 的代码
```java
// SomeActivity
Intent intent = getPackageManager().getLaunchIntentForPackage("com.iyh.targetapp");
startActivity(intent);
```
`context.getPackageManager()` 和 `PackageManager.getLaunchIntentForPackage()` 方法都是抽象方法，根据 context 继承体系知道 context 的实现类为 `ContextImpl` 在 `ContextImpl.getPackageManager()` 方法可以知道 `PackageManager` 的实现类为 `ApplicationPackageManager`，现在可以知道 `intent` 对象构建过程
```java
@Override
public Intent getLaunchIntentForPackage(String packageName) {
    Intent intentToResolve = new Intent(Intent.ACTION_MAIN);
    intentToResolve.addCategory(Intent.CATEGORY_INFO);
    intentToResolve.setPackage(packageName);
    List<ResolveInfo> ris = queryIntentActivities(intentToResolve, 0);

    if (ris == null || ris.size() <= 0) {
        // reuse the intent instance
        intentToResolve.removeCategory(Intent.CATEGORY_INFO);
        intentToResolve.addCategory(Intent.CATEGORY_LAUNCHER);
        intentToResolve.setPackage(packageName);
        ris = queryIntentActivities(intentToResolve, 0);
    }
    if (ris == null || ris.size() <= 0) {
        return null;
    }
    Intent intent = new Intent(intentToResolve);
    intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    intent.setClassName(ris.get(0).activityInfo.packageName,
            ris.get(0).activityInfo.name);
    return intent;
}
```

此处 ris 相当于 `AndroidManifest.xml` 中声名的各组件根据 `<intent-filter>` 过滤后的 ActivityInfo 列表，每个元素都包含相应 Activity 的信息。由此处代码可知 `CATEGORAY_INFO` 与 `CATEGORY_LAUNCHER` 都属于 LaunchIntent 且 `CATEGORAY_INFO` 的优先级更高。最终获取到的 Intent 对象相当于以下代码
```java
Intent intent = new Intent(Intent.ACTION_MAIN);
intent.addCategory(Intent.CATEGORY_LAUNCHER);
intent.setPackage("com.iyh.targetapp");
intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
intent.setClassName("com.iyh.targetapp", "com.iyh.targetapp.MainActivity");
```
这就是 AMS 启动一个 Activity 所需的最原始信息，接着进入 `activity.startActivity(intent)` 方法，跳过重载方法调用链，进入执行体
```java
// Activity.startActivityForResult
public void startActivityForResult(@RequiresPermission Intent intent, int requestCode, @Nullable Bundle options) {
    if (mParent == null) {
        options = transferSpringboardActivityOptions(options);
        Instrumentation.ActivityResult ar =
            mInstrumentation.execStartActivity(
                this, mMainThread.getApplicationThread(), mToken, this,
                intent, requestCode, options);
        ...
    } else {
        ...
    }
}
```
此处传入参数 requestCode 为 -1；options 为 null。一般情况 mParent 为空，在 Android 3.2 之前有个组件叫 ActivityGroup，类似 ViewGroup 它将可以包含子 Activity，在这个子 Activity 中 mParent 才不为空，不过随着 Fragment 的验证流行，这个组件已经过时退出历史了。options 类型为 Bundle 但实际确是由 `ActivityOptions.toBundle()` 方法转换而来，作用是传递 Activity 非功能性参数，比如 Activity 间的转场动画等。`transferSpringboardActivityOptions(options)` 方法判断 options 如果为 null 则会使用当前 Activity 中的 options 替换原 options。以下是 `Instrumentation.execStartActivity(...)` 方法
```java
// Instrumentation.execStartActivity
public ActivityResult execStartActivity(
        Context who, IBinder contextThread, IBinder token, Activity target,
        Intent intent, int requestCode, Bundle options) {
    IApplicationThread whoThread = (IApplicationThread) contextThread;
    ...
    try {
        // 如果当前剪切板中没有信息，就将 intent 对象中 extra 等信息转移到 ClipData（剪切板数据） 对象中
        intent.migrateExtraStreamToClipData();
        // 根据 who 可以判断该 intent 对象是否需要跨进程
        // 如果跨进程，需要检查 intent 携带数据（Uri）的访问权限，不跨进程则不需要检查
        intent.prepareToLeaveProcess(who);
        int result = ActivityManager.getService()
            .startActivity(whoThread, who.getBasePackageName(), intent,
                    intent.resolveTypeIfNeeded(who.getContentResolver()),
                    token, target != null ? target.mEmbeddedID : null,
                    requestCode, 0, null, options);
        checkStartActivityResult(result, intent);
    } catch (RemoteException e) {
        throw new RuntimeException("Failure from system", e);
    }
    return null;
}
```
参数 `contextThread` 代表当前应用的 `ApplicationThread`，它的父类为 `IApplicationThread.Stub`，aidl 定义在[这里](http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/core/java/android/app/IApplicationThread.aidl)，很显示 `contextThread` 是个跨进程组件，`ApplicationThread` 方法多是 `scheduleXxx(...)` 如
```java
// ActivityThread.ApplicationThread.scheduleDestroyActivity
public final void scheduleDestroyActivity(IBinder token, boolean finishing, int configChanges) {
    // 该方法执行者为：ActivityThread.mH
    sendMessage(H.DESTROY_ACTIVITY, token, finishing ? 1 : 0, configChanges);
}
```
可以看出 ApplicationThread 的作用是控制应用内 Activity 状态，它又是个 Stub，显然是想给其它进程提供控制当前应用的能力。参数 token 是个 IBinder，在 `Activity.attach(...)` 方法中被初始化，它其实是一个 Activity，只不会它是 native 层中的一种表达方式，整个启动流程最后阶段会看到它是如何生成的。在执行 `startActivity(...)` 之前需要找到方法的正直执行者，`ActivityManager.getService()` 最终返回对象过程如下
```java
// ActivityManager.getService
public static IActivityManager getService() {
    return IActivityManagerSingleton.get();
}   

private static final Singleton<IActivityManager> IActivityManagerSingleton =
    new Singleton<IActivityManager>() {
        @Override
        protected IActivityManager create() {
            // 使用 ServiceManager 获取 ActivityManager
            final IBinder b = ServiceManager.getService(Context.ACTIVITY_SERVICE);
            final IActivityManager am = IActivityManager.Stub.asInterface(b);
            return am;
        }
    };
```
此处 am 是个抽象类，需要进入 `ServiceManager.getService(...)` 方法查看 b 对象是哪个对象实例，以下是获取 ServiceManager 及相应服务（Context.ACTIVITY_SERVICE）的关键代码
```java
// ServiceManager.getIServiceManager()
ServiceManagerNative.asInterface(Binder.allowBlocking(BinderInternal.getContextObject()));

// ServiceManager.getService(String name)
Binder.allowBlocking(getIServiceManager().getService(name));
```
这里需要一点基础知识才能理解如何将 `BinderInternal.getContextObject()` 转换为 IServiceManager 对象。系统启动时，init 进程会解析执行[servicemanager.rc](http://aospxref.com/android-8.0.0_r36/xref/frameworks/native/cmds/servicemanager/servicemanager.rc) 文件初始化 service manager 进程，system server 会将各系统核心服务注册进 service manager，这些服务中就包括 `startActivity()` 方法真正的执行体 `ActivityManagerService`。在启动应用的过程中在 framework 层的代码都是 Binder 的客户端代理，是通过 IBinder 去获取系统服务然后赋值（转换）为 java 类。就像 `ServiceManagerNative.asInterface(..BinderInternal.getContextObject())` 方法获取 native 对象一样，过程如下
```java
BinderInternal.getContextObject
public static final native IBinder getContextObject();
```
它是个 jni 方法，位置及代码如下
```cpp
// http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/core/jni/android_util_Binder.cpp#921
921  static jobject android_os_BinderInternal_getContextObject(JNIEnv* env, jobject clazz)
922  {
923      sp<IBinder> b = ProcessState::self()->getContextObject(NULL);
924      return javaObjectForIBinder(env, b);
925  }

// http://aospxref.com/android-8.0.0_r36/xref/frameworks/native/libs/binder/ProcessState.cpp#98
98  sp<IBinder> ProcessState::getContextObject(const sp<IBinder>& /*caller*/)
99  {
100     return getStrongProxyForHandle(0);
101 }
```
在 `getStrongProxyForHandle(0)` 方法中会创建对象 `BpBinder(0)`，当使用 `ServiceManagerNative.asInterface(...)` 方法获取到的 IServiceManager 对象为 `ServiceManagerNative.ServiceManagerProxy(obj)`，其中 obj 对象就是 `BpBinder(0)`，BpBinder 中的 Bp 代表 Binder proxy 是个 Binder 的代理类（客户端类），BpBinder 构建参数 0（handle = 0）代表它所代理的真正实现为 service manager。因此 IServiceManager.getService() 执行体为
```java
// ServiceManagerNative.ServiceManagerProxy
public IBinder getService(String name) throws RemoteException {
    ...
    data.writeString(name);
    mRemote.transact(GET_SERVICE_TRANSACTION, data, reply, 0);
    ...
    return binder;
}
```
这里的 mRemote 即上面的 `BpBinder(0)`，参数 `name` 为 `ACTIVITY_SERVICE`（值为 `"activity"`），`mRemote.transact(...)` 在 native 层对应方法如下
```java
http://aospxref.com/android-8.0.0_r36/xref/frameworks/native/libs/binder/BpBinder.cpp#160
160  status_t BpBinder::transact(
161      uint32_t code, const Parcel& data, Parcel* reply, uint32_t flags)
162  {
164      if (mAlive) {
165          status_t status = IPCThreadState::self()->transact(
166              mHandle, code, data, reply, flags);
167          if (status == DEAD_OBJECT) mAlive = 0;
168          return status;
169      }
171      return DEAD_OBJECT;
172  }
```
此处 mHandle 为 0， code 为 GET_SERVICE_TRANSACTION 进入 `IPCThreadState::self()->transact(...)` 方法最终会在 `service_manager.cpp` 类的 `svclist` 列表中查找名为 `activity` 的服务
```java
// http://aospxref.com/android-8.0.0_r36/xref/frameworks/native/cmds/servicemanager/service_manager.c#148
148  struct svcinfo *find_svc(const uint16_t *s16, size_t len)
149  {
150      struct svcinfo *si;
151  
152      for (si = svclist; si; si = si->next) {
153          if ((len == si->len) &&
154              !memcmp(s16, si->name, len * sizeof(uint16_t))) {
155              return si;
156          }
157      }
158      return NULL;
159  }
```
现在需要确定谁向 svclist 注册了一个叫 `activity` 的服务。我们没有写任何自定义服务，所以这个叫 `activity` 的服务是系统注册的。简要提一下从设备开启到注册各服务过程，设备通电后内核启动完成后，首先执行 init 进程，init 进程会启动 [AppRuntime](http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/cmds/app_process/app_main.cpp#344)(派生自 AndroidRuntime)，AndroidRuntime 会创建虚拟机，创建完成虚拟机后会[注册系统 jni 方法](http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/core/jni/AndroidRuntime.cpp#1038)，AppRuntime 会根据 init 传入的通过参数[反射](http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/core/jni/AndroidRuntime.cpp#1076)进入 java 层 `ZygoteInit.main()` 方法，ZygoteInit 会 [fork](http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/core/java/com/android/internal/os/ZygoteInit.java#633) 出 SystemServer 进程，fork 真正执行体在 [native 层](http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/core/jni/com_android_internal_os_Zygote.cpp#732)，`ZygoteInit.main()` 方法会捕获 [MethodAndArgsCaller](http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/core/java/com/android/internal/os/RuntimeInit.java#266) 异常，此异常会进入 `SystemServer.main()` 方法，之后会执行 [`SystemServer.startBootstrapServices()`](http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/services/java/com/android/server/SystemServer.java#491)，此处重点看 ActivityManagerService 初始化过程，其中 [`mActivityManagerService.setSystemProcess()`](http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java#2554) 方法如下
```java
// http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java#2554
2554      public void setSystemProcess() {
2555          try {
                  // Context.ACTIVITY_SERVICE 为 "activity"
                  // this 为 ActivityManagerService
2556              ServiceManager.addService(Context.ACTIVITY_SERVICE, this, true);
                  ...
2583          } catch ...
2587      }
```
可以看到 SerivceManager 将 ActivityManagerService 添加进 ServiceManager，如上面获取服务过程一样，添加服务最终也会经由 `BpBinder(0)` 添加到 svclist 中。

至此，在 native 层转了一圈知道了 `ActivityManager.getService()` 获取的实体类为 `ActivityManagerService`，可以接着看 `ActivityManager.getService().startAdtivity(...)` 执行体了，去除调用链后，最终进入 `ActivityStarter.startActivityMayWait(...)` 方法
```java
// ActivityStarter.startActivityMayWait
final int startActivityMayWait(IApplicationThread caller, int callingUid,
            String callingPackage, Intent intent, String resolvedType,
            IVoiceInteractionSession voiceSession, IVoiceInteractor voiceInteractor,
            IBinder resultTo, String resultWho, int requestCode, int startFlags,
            ProfilerInfo profilerInfo, WaitResult outResult,
            Configuration globalConfig, Bundle bOptions, boolean ignoreTargetSecurity, int userId,
            IActivityContainer iContainer, TaskRecord inTask, String reason) {
    // Refuse possible leaked file descriptors
    if (intent != null && intent.hasFileDescriptors()) {
        throw new IllegalArgumentException("File descriptors passed in Intent");
    }
    ...
}
```
这个方法参考很多，方法体很长，先看说明一下各参数的值或含义，然后再到方法体里看具体过程。从前向后顺序，caller 为前文提到的 `ApplicationThread`；callingUid, callingPackage 和 intent
|参考|值|备注|
|:----|:----|:----|
|caller|`mMainThread.getApplicationThread()`|处理当前应用中的 Activity 展示，隐藏，销毁等|
|callingUid|-1|调用者进程的 uid|
|callingPackage|"com.iyh.example"|调用者进程包名，[获取方式](http://aospxref.com/android-8.0.0_r36/xref/frameworks/base/core/java/android/app/ContextImpl.java#2327)|
|intent|intent|前文创建的，用于存储调用者与被调用者基本信息|
|resolvedType|null|MIME 类型，用于系统后端（back-end），应用进程中基本不会使用到|
|voidceSession|null|作用未知|
|voiceInteractor|null|作用未知|
|resultTo|activity.mToken|当前 Activity 的 mToken 值，它代表 native 层的 activity|
|resultWho|activity.mEmbeddedID|此值在 Activity attach 方法中赋值|
|requestCode|-1|用于区分启动的不同 Activity 的返回信息，不需要返回信息时此值为 -1|
|startFlags|0|作用未知|
|profilerInfo|null|系统私有 API，用来传递 profiler 设置信息。作用未知|
|outResult|null|作用未知|
|globalConfig|null|作用未知|
|bOptions|前文提及的 `ActivityOptions`|Activity 间的非必要信息，如转场动画信息等|
|ignoreTargetSecurity|false|作用未知|
|userId|UserHandle.getCallingUserId()|调用者 userId|
|iContainer|null|作用未知|
|inTask|null|具体作用未知，TaskRecord 是 ActivityStackSupervisor-ActivityStack-TaskRecord-ActivityRecord 关系中的一个组件，记录 task 信息|
|reason|`"startActivityAsUser"`|作用未知|

进入代码之前要先清楚 task 是如何管理 Activity 的，快速浏览 [Activity launchMode、回退栈与 affinity](https://blog.csdn.net/weixin_46221133/article/details/104172642) 了解大概后，还需要了解以下实体类

|类|作用|
|---|---|
|Intent|代表一种意图，比如，启动一个 Activity，启动一个 Service，都是以 Intent 发起，根据 Intent 信息获取应用或系统中可以响应这个意图的组件|
|ResolveInfo|Resolve 代表解析的意思。粗略地讲，可以把它看作从 AndroidManifest 文件中解析出来的某个组件实体类。这个解析过程是由 PackageMangerService 提供，PMS 根据 intent 和 resolveType 从 manifest 信息中解析出来。 ActivityInfo、ServiceInfo、ProviderInfo 都是它的成员变量|
|ActivityInfo|Info 代表信息，是个静态概念，它是 activity 实体类。它是从 AndroidManifest 文件获取的信息，承载 `activity` 或 `receiver` 标签的所有属性。|
|ActivityRecord|Record 代表记录，是个动态概念（与 ActivityInfo 相比），它也是 activity 实体类。不过，它更偏向“记录”，信息是时时变化的，比如，activity 从哪个 intent 得来，需要将信息传回哪个 activity；activity 在 task 中的位置；activity 上次保存的状态等。|
|TaskRecord|与 ActivityRecord，TaskRecord 记录的是 task 的动态信息，它是 task 的实体类。它内部有个数据记录属于本 task 所有 ActivityRecord。|
|ActivityStack|Stack 代表栈，看名字就能看出，它起栈的作用，内部元素是 activity。它存放到设备中所有的 activity, task 以及它们的排列方式。因为记录的是所有的 activity，所以它知道所有 activity 的状态。|
|ActivityStackSupervisor|Supervisor 有管理员的意思，它的作用也就很显示了，操作 ActiviyStack，因为 ActivityStack 拥有所有的 activity，所以 ActivityStackSupervisor 就是管理所有 activity 了。它就是 AMS 的真正执行者|
|Activity|感觉它名字起错了，叫 Controller 才合适，它等待 ActivityRecord 把所有信息处理完成后，会初始化一个 PhoneWindow 对象，此对象用于显示画面。此后就将内部逻辑交给 AMS，显示交到 WMS，Activity 在中间起调节作用，最主要的是给开发者一个简单的模型，方便开发。|

现在可以看代码了
```java
// ActivityStarter.startActivityMayWait
final int startActivityMayWait(IApplicationThread caller, int callingUid,
        String callingPackage, Intent intent, String resolvedType,
        IVoiceInteractionSession voiceSession, IVoiceInteractor voiceInteractor,
        IBinder resultTo, String resultWho, int requestCode, int startFlags,
        ProfilerInfo profilerInfo, WaitResult outResult,
        Configuration globalConfig, Bundle bOptions, boolean ignoreTargetSecurity, int userId,
        IActivityContainer iContainer, TaskRecord inTask, String reason) {
    
    ...

    // 从 PMS 获取目标 intent 的解析信息（resolve info）
    ResolveInfo rInfo = mSupervisor.resolveIntent(intent, resolvedType, userId);
    
    ...

    ActivityInfo aInfo = mSupervisor.resolveActivity(intent, rInfo, startFlags, profilerInfo);

    ActivityOptions options = ActivityOptions.fromBundle(bOptions);
    // 根据上文可知 iContainer 为 null
    ActivityStackSupervisor.ActivityContainer container =
            (ActivityStackSupervisor.ActivityContainer)iContainer;
    synchronized (mService) {
        if (container != null && container.mParentActivity != null &&
                container.mParentActivity.state != RESUMED) {
            return ActivityManager.START_CANCELED;
        }
        final int realCallingPid = Binder.getCallingPid();
        final int realCallingUid = Binder.getCallingUid();
        int callingPid;
        if (callingUid >= 0) {
            callingPid = -1;
        } else if (caller == null) {
            callingPid = realCallingPid;
            callingUid = realCallingUid;
        } else {
            callingPid = callingUid = -1;
        }

        final ActivityStack stack;
        if (container == null || container.mStack.isOnHomeDisplay()) {
            stack = mSupervisor.mFocusedStack;
        } else {
            stack = container.mStack;
        }
        stack.mConfigWillChange = globalConfig != null
                && mService.getGlobalConfiguration().diff(globalConfig) != 0;
        if (DEBUG_CONFIGURATION) Slog.v(TAG_CONFIGURATION,
                "Starting activity when config will change = " + stack.mConfigWillChange);

        // 与下面 Binder.restoreCallingIdentity(origId) 成对出现，用于权限控制
        final long origId = Binder.clearCallingIdentity();

        if (aInfo != null &&
                (aInfo.applicationInfo.privateFlags
                        & ApplicationInfo.PRIVATE_FLAG_CANT_SAVE_STATE) != 0) {
            // 如果在 <activity> 的 android:process 属性添加不同于包名的值就会进入下面的条件
            if (aInfo.processName.equals(aInfo.applicationInfo.packageName)) {
                final ProcessRecord heavy = mService.mHeavyWeightProcess;
                if (heavy != null && (heavy.info.uid != aInfo.applicationInfo.uid
                        || !heavy.processName.equals(aInfo.processName))) {
                    int appCallingUid = callingUid;
                    if (caller != null) {
                        ProcessRecord callerApp = mService.getRecordForAppLocked(caller);
                        if (callerApp != null) {
                            appCallingUid = callerApp.info.uid;
                        } else {
                            Slog.w(TAG, "Unable to find app for caller " + caller
                                    + " (pid=" + callingPid + ") when starting: "
                                    + intent.toString());
                            ActivityOptions.abort(options);
                            return ActivityManager.START_PERMISSION_DENIED;
                        }
                    }

                    IIntentSender target = mService.getIntentSenderLocked(
                            ActivityManager.INTENT_SENDER_ACTIVITY, "android",
                            appCallingUid, userId, null, null, 0, new Intent[] { intent },
                            new String[] { resolvedType }, PendingIntent.FLAG_CANCEL_CURRENT
                                    | PendingIntent.FLAG_ONE_SHOT, null);

                    Intent newIntent = new Intent();
                    if (requestCode >= 0) {
                        // 调用者需要返回信息
                        newIntent.putExtra(HeavyWeightSwitcherActivity.KEY_HAS_RESULT, true);
                    }
                    newIntent.putExtra(HeavyWeightSwitcherActivity.KEY_INTENT,
                            new IntentSender(target));
                    if (heavy.activities.size() > 0) {
                        ActivityRecord hist = heavy.activities.get(0);
                        newIntent.putExtra(HeavyWeightSwitcherActivity.KEY_CUR_APP,
                                hist.packageName);
                        newIntent.putExtra(HeavyWeightSwitcherActivity.KEY_CUR_TASK,
                                hist.getTask().taskId);
                    }
                    newIntent.putExtra(HeavyWeightSwitcherActivity.KEY_NEW_APP,
                            aInfo.packageName);
                    newIntent.setFlags(intent.getFlags());
                    newIntent.setClassName("android",
                            HeavyWeightSwitcherActivity.class.getName());
                    intent = newIntent;
                    resolvedType = null;
                    caller = null;
                    callingUid = Binder.getCallingUid();
                    callingPid = Binder.getCallingPid();
                    componentSpecified = true;
                    rInfo = mSupervisor.resolveIntent(intent, null /*resolvedType*/, userId);
                    aInfo = rInfo != null ? rInfo.activityInfo : null;
                    if (aInfo != null) {
                        aInfo = mService.getActivityInfoForUser(aInfo, userId);
                    }
                }
            }
        }

        final ActivityRecord[] outRecord = new ActivityRecord[1];
        // 此处方法名为 xxxLocked 的含意是此方法处于 synchronized 块中
        int res = startActivityLocked(caller, intent, ephemeralIntent, resolvedType,
                aInfo, rInfo, voiceSession, voiceInteractor,
                resultTo, resultWho, requestCode, callingPid,
                callingUid, callingPackage, realCallingPid, realCallingUid, startFlags,
                options, ignoreTargetSecurity, componentSpecified, outRecord, container,
                inTask, reason);

        Binder.restoreCallingIdentity(origId);

        ...
        
        return res;
    }
}
```

---

看了两天 startActivity 的源码，提取了启动过程中关键步骤（开篇时的时序图），整个过程涉及茫茫多知识点，整理起来异常困难。关键是很多的仔细不知所以然，可能会误导读者，篇暂时定为开篇，等理清非主干细节后再来续写。

## 参考链接
> [Android Application Launch explained: from Zygote to your Activity.onCreate()](https://android.jlelse.eu/android-application-launch-explained-from-zygote-to-your-activity-oncreate-8a8f036864b)<br/>
> [Android系统启动-zygote篇](http://gityuan.com/2016/02/13/android-zygote/)<br/>
> [四大组件之ActivityRecord](http://gityuan.com/2017/06/11/activity_record/)