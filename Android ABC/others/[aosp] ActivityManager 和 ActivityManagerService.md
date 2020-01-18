[原谅地址](https://androidreverse.wordpress.com/2018/03/11/aosp-activitymanager-and-activitymanagerservice/)

Android 操作系统里面包含了很多服务，比如：AlarmManagerService，InputMethodManagerService，TrustManagerService，WindowManagerService，ServiceManager，PermissionController，SystemServer 当然，还有 ActivityManagerService(查看：https://android.googlesource.com/platform/frameworks/base/+/android-8.1.0_r18/services/core/java/com/android/server)。

## ActivityManagerService 的生命周期
### 启动（start up）
ActivityManagerService 起始于 SystemServer，下面是初始化方法
```java
// 源码位置：https://android.googlesource.com/platform/frameworks/base/+/android-8.1.0_r18/services/java/com/android/server/SystemServer.java : 510
startBootstrapServices(){
    [...] 
    traceBeginAndSlog("StartActivityManager");
    mActivityManagerService = mSystemServiceManager.startService( ActivityManagerService.Lifecycle.class).getService()
}
```
其它一些核心服务也在这个位置初始化，它们都同属于一个进程。输入下面的命令可以看到 system_server 相关信息：
```sh
generic_x86:/ $ ps -A | grep system_server 
USER PID PPID VSZ RSS WCHAN ADDR S NAME   
system 2151 2030 1559736 182860 SyS_epoll_wait 0 S system_serve
```
SystemServiceManager 是 SystemServer 的一个组件，它用来处理所有运行在 SystemServer 中的服务。未来会介绍更多 SystemServer 的相关内容。SystemServer 使用不同的值初始化 ActivityManagerService 并将它与其它服务如 WindowManagerService 关联。一般来说，WindowManagerService 与 ActivityManagerService 关系比较近，因为如果没有 Window 的话也就不会有 Activity。Android 中一个 Window 可以有持有多个 activity，activity 可以持有多个 fragment.
```java
// 位置：https://android.googlesource.com/platform/frameworks/base/+/android-8.1.0_r18/services/java/com/android/server/SystemServer.java : 845
traceBeginAndSlog("SetWindowManagerService"); 
mActivityManagerService.setWindowManager(wm);
```

ActivityServiceManager 和其它系统服务初始化后，SystemServer 通知 ActivityServiceManager 启动其它三方服务或应用。`systemReady(...)` 方法中有个回调方法，可以通过这个回调方法知道 ActivityManagerService 是否准备就绪。SystemService 现在启动了，其它重要组件如系统 UI 也启动了。
```java
// 位置：https://android.googlesource.com/platform/frameworks/base/+/android-8.1.0_r18/services/java/com/android/server/SystemServer.java : 1673
// 现在我们通知 activity manager 可以启动三方代码了。
// 一但等到三方代码真正执行的状态时（在三方代码启动初始化 application 之前），
// activity manager 会通知我们，此时就可以完成我们初始化工作。
 mActivityManagerService.systemReady(() -> {
   Slog.i(TAG, "Making services ready");
   [...]
   startSystemUi(context, windowManagerF);
   [...]
 }, BOOT_TIMINGS_TRACE_LOG);
```

通过查看 ActivityManagerService.systemReady(...) 方法，可以知道 systemReady(...) 获取设备序列号，并将它提供给 activity 并开启 home activity。这里的 home activity 代表启动 launcher(就是系统桌面) 的 activity。正常是启动 launcher 的，除非是类似首次开机进入欢迎设置页面，因为此时 home activity 还未获取到系统用户授权。
```java
// 位置 https://android.googlesource.com/platform/frameworks/base/+/android-8.1.0_r18/services/core/java/com/android/server/am/ActivityManagerService.java : 14148
public void systemReady(final Runnable goingCallback, TimingsTraceLog traceLog) {
    [...]
    sTheRealBuildSerial = IDeviceIdentifiersPolicyService.Stub.asInterface(ServiceManager.getService(Context.DEVICE_IDENTIFIERS_SERVICE)).getSerial();
    // Enable home activity for system user, so that the system can always boot. 
    // We don't do this when the system user is not setup since the setup wizard should be the one to handle home activity in this case.   
    [...]
    if (UserManager.isSplitSystemUser() && Settings.Secure.getInt(mContext.getContentResolver(), Settings.Secure.USER_SETUP_COMPLETE, 0) != 0) {
        ComponentName cName = new ComponentName(mContext, SystemUserHomeActivity.class);
        try {
            AppGlobals.getPackageManager().setComponentEnabledSetting(cName, PackageManager.COMPONENT_ENABLED_STATE_ENABLED, 0, UserHandle.USER_SYSTEM);
        } catch (RemoteException e) {
            throw e.rethrowAsRuntimeException();
        }
    }
    startHomeActivityLocked(currentUserId, "systemReady");
    [...]
}
```

### 关闭（shurt down）
ActivityManagerService 现在已经启动进入可用状态，并会一直运行到系统关闭为止。 `ActivityManagerService.shutDown(...)` 方法可以看出 ActivityManagerService 是如果完成关闭任务的。
首先检查调用者是否拥有相应的权限（android.Manifest.permission.SHUTDOWN 权限）。进入 `checkCallingPermission` 查看，root 用户和 system_server pid 会有 SHUTDOWN 权限。通过权限检查后，Activity 栈会收到系统将要关闭的通知。之后，所有的子服务也会被关闭。
```java
@Override
public boolean shutdown(int timeout) {
    if (checkCallingPermission(android.Manifest.permission.SHUTDOWN) != PackageManager.PERMISSION_GRANTED) {
        throw new SecurityException("Requires permission " + android.Manifest.permission.SHUTDOWN);
    }
    boolean timedout = false;
    synchronized (this) {
        mShuttingDown = true;
        mStackSupervisor.prepareForShutdownLocked();
        updateEventDispatchingLocked();
        timedout = mStackSupervisor.shutdownLocked(timeout);
    }
    mAppOpsService.shutdown();
    if (mUsageStatsService != null) {
        mUsageStatsService.prepareShutdown();
    }
    mBatteryStatsService.shutdown();
    synchronized (this) {
        mProcessStats.shutdownLocked();
        notifyTaskPersisterLocked(null, true);
    }
    return timedout;
}
```

## 沟通（communication）
### ActivityManager
ActivityManagerService 可以被“客户端”进程 ActivityManager 访问。ActivityManager 代表 ActivityManagerService 并提供接口用于协调 ActivityManagerService。ActivityManager 与 ActivityManagerService 之前的数据传递是通过 Binder 接口来实现的。以后会对 Binder 做介绍。通过 ActivityManager 可以看到一个简洁整完的存在于 ActivityManagerService 中的事件（译的有点别扭，主要是说 ActivityManager 类似于一个接口，事件方法都是全的，但是真正的实现全在 ActivityManagerService 中，所以 ActivityManager 看起来很 nice）。要注意，ActivityManager 中的方法并非全部对应用开发者可见，它里面有一些方法是隐藏 api，不能通过 Android SDK 直接调用，需要使用反射调用。另一个可以获取所有预览事件的地方是 IActivityManager.java 文件，它是通过 IActivityManager.aidl 生成的。看这里：https://android.googlesource.com/platform/frameworks/base/+/c80f952/core/java/android/app/IActivityManager.java

在文件末尾可以看到许多这样的 integer，比如：
```java
START_ACTIVITY_TRANSACTION
```
这些值定义了事件 code，这些值必须通过 Binder 接口传递。下面会介绍事件参数，并实验验证其功能！

### 发送 Activity 关闭事件
我们会向 ActivityManagerService 发送一个 FINISH_ACTIVITY_TRANSACTION 事件，希望它可以关闭我们的 activity，或者至少返回一个正数返回值。代表是用 kotlin 写的，因为现在写 Android 代表都用 kotlin！

第一，定义 data parcel 和 reply Parcel。Parcel 是一个可以使用 Binder 传送的对象。使用 `writeInt(...)`，`writeString(...)` 或者其它类似的方法将数据序列化并存储到 Parcel 中。

