### Android 3.0

1. 添加 Fragment
2. Action Bar 取代传统 title Bar
3. 应用可以粘贴复制系统范围剪切板中的内容。支持 txt文本，URI 和 intent
4. 添加新 API，View 间可以通过拖拽传递数据
5. 添加新组件 GridView，ListView，StackView， ViewFlipper 和 AdapterViewFlipper
6. 扩展状态栏通知，
   1. 使其支持大 icon，
   2. 支持自定义布局
   3. 支持 PendingIntent，比如，可以在通知栏中播放或暂停音乐
7.  添加 Loader 类，配合 ContentProvider 异步加载数据
8.  添加蓝牙 A2DP 和耳机 API，现在应用可以监听蓝牙 A2DP 与耳机连接状态。
9.  新动画框架：ValueAnimator 和 ObjectAnimator
10. 扩展 UI 框架
11. 硬件加速
    1.  开启 OpenGL 渲染，加速 2D 画面渲染速度
    2.  硬件或软件层支持 View 渲染
    3.  添加 Renderscript 3D 图形渲染引擎
12. 媒体
    1.  延迟录像
    2.  SurfaceTexture 支持以 OpenGL ES 纹理显示图片字节流，比如可以使用 SurfaceTexture 显示相机预览图片
    3.  实时 HTTP 流，支持处理 M3U 格式的 URL
    4.  支持 EXIF 数据
    5.  丰富摄像相配置参数，比如使用 QUALITY_1080P, QULITY_720P, QULITY_CIF 等设置摄像品质
    6.  支持数码文件传输，可以使用 USB 传输线将媒体文件传输到宿主电脑上，或从宿主电脑传输到手机
    7.  增加可扩展数字版权管理（DRM）框架，用来检查和执行数字版权
    8.  新增音频流格式：ADTS、AAC、FLAC
13. 件盘支持
    1.  支持 control 键，大小写锁定，数字锁定及滚动锁定等功能
    2.  支持桌面全键盘功能，比如支持 Esc, Home, End, Delete 及其它键
    3.  TextView 支持键盘级操作，比如 ctrl + C/V/A 表示 复制/粘贴/全选 
14. 支持多点触摸，之前版本同一时间只能有一个 View 响应触摸事件，现在默认可以多个 View 同时接收到触摸事件。此功能可以通过特定属性关闭。
15. WebKit 功能
    1. 新增 WebViewFragment 类，此类中组合了一个 WebView
    2. 新增 WebSettings, WebView 类成员方法
    3. CookieManager 支持 `file:` URI
    4. 支持自动登录功能，需要登录时会触发 `onReceivedLoginRequest()` 通知
    5. 移除了一些之前标记过期的类和接口
16. 浏览器
    1.  HTML 中支持 `<input type="file" accept="image/*;capture=camera"/>` 标签，用于获取设备中的文件或调用相机
    2.  可以监听设备方向和运动状态
    3.  支持 CSS 3D 渲染
    4.  支持 HTML5 `<video>` 标签，并可以支持硬件加速
    5.  支持固定（fixed）放置元素
17. JSON 新增 JsonReader 和 JsonWriter，辅助读写 JSON 流。
18. 新平台技术
    1.  存储：ext4 文件系统支持板载 eMMC 储存；FUSE 文件系统支持 MTP 驱动；USB 支持键盘和 USB hubs；支持 MTP/PTP
    2.  升级 Linux 内核至 2.6.36
    3.  Dalvik 虚拟机：支持 SMP；提升 JIT 基础架构；提升 GC 性能，支持 learge heap，统一处理 bitmap 和 byte 缓存
    4.  Dalvik 核心库：添加 NIO；优化异常信息；其它
19. 网络
    1. 高性能 Wi-Fi 锁，使锁屏后依然保持 Wi-Fi 传输的高效性能
    2. 更多网络传输状态，可以获取或监听指定 UID 的 UDP 状态，数据包数量，TCP 传输/接收到数据的 byte
    3. 支持 SIP 授权
20. 下载管理器
    1. 支持下载完成通知
    2. 支持应用向下载数据库中添加文件
21. 新增工具类：LruCache
22. 增加 MTP/PTP API，支持应用直接连接相机或其它 PTP 设备
### Android 4.0