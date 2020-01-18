所以源码基于 Android 6.0 分析
## Activity 启动流程
起点：
Activity 
    startActivity(intent)
    startActivityForResult(intent, requestCode, option:Bundle)

Instrumentation 
    execStartActivity(context, applicationThread:IBinder, mToken:IBinder, who:Activity, intent, requestCode, option)

IActivityManager (通过 ActivityManagerNative.getDefault() 获取 IActivityManager，过程：通过 `SystemManager.getService("activity")` 获取 IBinder 对象，通过 IBinder.queryLocalInterface("android.app.IActivityManager") 序列化得到 IActivityManager 对象，如果 IActivityManager 为空，则创建 ActivityManagerProxy 封装 IBinder 对象，使用 mRemote:IBinder 来处理 startActivity 等方法)
    startActivity(appThread, packageName, intent, resolveType, token, mEmbeddedId|null, requestCode, 0, null, options)

ActivityManagerService extends ActivityManagerNative

StackSupervisor
    startActivityMayWait(...)
    startActivityLocked(...)
    startActivityUncheckedLocked(...)