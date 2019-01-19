[原文](https://iabtechlab.com/wp-content/uploads/2018/06/MRAID_3.0_FINAL_June_2018.pdf)

# 移动端富媒体广告接口标准（MRAID）
版本 3.0
更新于 2018.06.21

## 移动端富媒体广告接口标准（MRAID）3.0 版本
MRAID 3.0 版本（MRAID 3.0）引入了许多提升用户广告体验的新特性。新版本 MRAID 广告通过测量视听能力，检测 MRAID 环境并获取定位信息来向用户展示尽可能好的体验。此外，还有关于提前加载和广告准备向用户展示的手册。MRAID 附属标准--视频播放器广告接口标准（VPAID）现在已经完全囊括在 MRAID 说明中了。

此版本 MRAID 由 IAB 技术实验室移动端广告接口标准（MRAID）工作组开发完成。

## 关于 IAB 技术实验室
IAB 技术实验室是一个独立的，国际的，研究与发展，为了生产或帮助公司实现全球工业技术标准而成立的联盟。IAB 由数字出版商、广告技术公司、营销商、代理商以及其它对交互市场有兴趣的公司组成，它的目标是降低数字广告与市场供应链联接障碍，同时致力于该工业健康、安全的增长。更多关于 IAB 技术实验室内容请看[这里](https://www.iabtechlab.com/)

此文档在 IAB 的网址为：<a>https://www.iab.com/mraid</a>

下面是前面提到的工作组里的部分 IAB 成员公司：

1. AccuWeather.com 
2. AdColony
3. Adform
4. AdGear 
5. Technologies, Inc.
6. Adobe
7. Adsiduous Media ADTECH AdTheorent ADVR
8. AerServ
9. Alliance for Audited Media (AAM)
10. Amazon
11. AOL
12. BabyCenter
13. Bazaarvoice
14. Bonzai
15. Cedato Technologies Ltd
16. Celtra
17. Conversant Media
18. Cyber Communications Inc.
19. Digital Advertising Consortium Inc.
20. Dominion Enterprises
21. DoubleVerify Flashtalking
22. FOX Networks Group FreeWheel
23. Google
24. Gruuv Interactive
25. Hulu
26. IAB
27. Improve Digital International B.V.
28. Innovid
29. Integral Ad Science
30. Leaf Group
31. Liquidus
32. LogoBar Enterprises MGID
33. Microsoft Advertising MoPub/ Twitter Inc.
34. mPlatform NinthDecimal PadSquad
35. Pixalate RhythmOne
36. Shazam Sizmek Taboola
37. The Media Trust Company
38. The New York Times Company
39. Thinknear by Telenav
40. Time Inc.
41. Tremor Video
42. Turner Broadcasting System
43. Vdopia
44. Vertebrae
45. Verve
46. ViralGains Visible Measures Westwood One White Ops
47. Yahoo Yieldmo YuMe
48. Zynga

## 概要

MRAID 是 Mobile Rich Media Ad Interface Definition 的首字母缩写。它使广告与移动 app 可以间接地交换信息，从而使广告拥有交互式体验。

如果没有 MRAID，广告开发者不得不为每个投放广告的系统专门设计交互式广告。这些开发花费的时间将会降低品牌在移动应用上制作出高效广告的可能性。

MRAID 提供了一个包含多个调用方法的库，广告使用这些调用方法与兼容 MRAID 的应用进行交互。当广告开发者使用 MRAID 时，移动应用支持 MRAID 库中的方法越多，广告体验的可预见性越高。

为提升移动广告执行效果，MRAID 3.0 更新了众多特性，比如，辅助追踪广告可见度，明确广告初始化及就绪状态，对含有视频交互的广告集成了 IAB VPAID (Video Player Ad Interface Definition) 以及其它新增特性。

随着使用手机的用户日益增长，对高质量广告的需求也逐渐增长。广告主想要改进追踪方法，应用发布者想要提升广告体验。在高质量的广告中接入或升级到 MRAID 3.0 会拥有更好的表现，也会有更优良的广告追踪。

## 周知

MRAID 是一个广告设计者与开发者在兼容 MRAID 的应用上开发出具有交互能力的广告的协议。应用开发者或 SDK 提供者必须为应用提供一个技术构件，使应用可以设置一个广告容器或响应广告发出的 MRAID 调用。

虽然这个规范是为广告开发者、应用发布者或 SDK 提供者提供的技术指导，但是所有参与移动广告制作的人都应该熟悉此文档。

## 介绍
MRAID 规定一组 API，该 API 为富媒体广告体验与展示该广告的原生移动应用之间提供交互支持。一个应用中的 MRAID 的实现通常是由 SDK 提供，该 SDK 创建一个可以展示广告的容器和一个广告用户与应用容器交互的控制器。

符合 MRAID 标准的广告根据本规范调用方法，提供信息与其它行为。符合 MRAID 标准的原生应用同样使用本规范来提供信息，响应 MRAID 调用及其它行为。

### 1.1 定义
MRAID 包含一些移动部件（moving parts），开发者们使用这些部件制作可交互广告体验。为了清晰地描述这些部件，下面列出了文档中使用到的关键字的定义。

**host：** 移动应用中的组件，该组件提供空间（容器）来展示广告，同时提供服务（控制器），广告通过 MRAID API 可以访问这些服务。host 的实现方式可以是一个 SDK，也可以是直接在 app 内部实现。

**MRAID 实现（MRAID Implementation）：** host 给广告提供 MRAID 方法的特性。包含 JavaScript 属性，广告可以通过它检验 MRAID 的状况，开启 MRAID 并且可以调用 MRAID 服务。

**SDK：** 是 Software Developtment Kit 的首字母缩写。以 MRAID 为例，SDK 指的是广告提供者提供给移动应用发布者的代码或说明的实现。

**SDK 提供者（SDK provider）：** 对于本文档来说，SDK 提供者代表为移动应用发布者开发 MRAID 代码实现的移动广告技术服务提供商。

**广告容器 (ad container）：** 应用中特意保留的一块展示广告的区域。应用发布者可以将广告容器放到应用内容里面（内置），也可以把这个容器放到应用内容的上面（插屏）。一个应用中可以有多个容器。

**webview：** 移动应用系统中用来展示 web 内容并执行 JavaScript 脚本的组件。MRAID 不强制使用 webview，但是在典型的 MRAID host 中 ad 容器通常含有一个 webview，广告在 webview 中作为 HTML 文档的顶级结点。

**原生层（native layer）：** 在原生应用内部进行沟通与操作的技术层。

**广告（ad）：** 对于本文档的主旨来说，广告通常表示一个移动环境下的 MRAID 广告，它包括所有的创意，包含任意 MRAID 方法的依赖库（代码及其它文件）。

### 1.2 范围
每个 MRAID 实现都为开发者提供一份唯一的功能集。本文档将解释该如何对这些功能做出响应，包括设置，初始化，方法，属性，事件和可预见的行为，但不包括操作的一些细节。以下功能的示例就超出了本文档的讨论范围：

* 从广告服务器，广告网络或本地资源中检索（获取）广告。
* 上报
* IDE 集成
* 安全与隐私
* 国际化
* 错误上报
* 日志
* 账单与支付
* 广告尺度与广告行为
* 下载资源到本地文件系统，以备缓存或离线使用

SDK 提供者必须能够在广告位区域渲染 web 内容。对于大多数情况来说，这些功能已经被 webview 所支持，但是开发者可能会开发额外的方法来支持该规格说明中的特性。

MRAID 不仅限于本文档中描述的特性。我们鼓励开发者创新或添加文档中没有的特性。但是，为了维护基准特性的互通性，额外的特性集必须在 MRAID 名称空间外实现。可能需要有添加额外功能集或集成实现的意识来完成文档中提到的功能。

### 1.3 MRAID 是如何工作的
作为一个两系统之间的协议，每个系统必须配备另一系统匹配的调用及响应。简单来说，这两个系统就是广告与移动应用，一个系统主动调用另外一个，然后被调用方作出回应。但实际中，广告与应用不会直接交互。

移动广告服务商（提供商）构建一套组件，该组件将广告对移动应用开发者受到影降到最小。广告技术员代表一个可以实现 MRAID 中特定方法的人或平台，他帮助使用 MRAID 的广告设计者或广告开发者实现交互功能。广告设计者或广告开发者制作包含所有预置功能广告之后，广告技术员使用脚本将这些方法规整为 MRAID 规定的方法。提供商可能会提供一些简化技术设计的其它方法。

同理，对于应用来说，提供商开发的这这些交互组件包含一个容器，容器里有简单的 JavaScript 启动标签。这容器就是一个类浏览器环境或 webview 的组件，广告可以在容器里执行标准化的交互协议。应用可能会初始化一个 "listener" 来监听广告发出的某种追踪交互或响应事件。

下面的图片展示了部分 MRAID 系统的工作示意：
![MARID system](../resource/parts-of-mraid-system.png)

### 1.4 版本
保证完全兼容旧版本是 MRAID 项目的目标之一。在 MRAID 3.0 更新版本中，工作组保留了 2.0 版本中确立的 6 个关键结果：

* **高通用性（High interoperability）：** 已制作完成的广告在一个 MRAID 容器中可以正常运行，那么该广告在其它平台和操作系统上的 MRAID 容器中也可以正常运行。
* **优雅降级（Graceful degradation）：** 已制作完成的广告在使用所有 MRAID 中高级功能的同时，也能在需要的时候优雅地使用低版本功能。这对 MRAID 版本升级过程中持续加入可使用设备来说尤为重要。
* **进阶性复杂度（Progressive complexity）：** 使用简单的 API 进行广告设计，在必要的时候才使用复杂 API。
* **广告操作的一致性（Consistent means for ad operation）：** 无论是需要扩展还是打开一个应用里的内置浏览器（或没有内置浏览器情况下打开设备浏览器），MRAID 都会提供给广告一个统一的交互方式。
* **退出控制的一致性（Consistent exit controls）：** MRAID 广告保证会有一个统一的方式让用户退出广告并返回应用或内容页。
* **对发布者的灵活性（Flexibility for publishers）：** 尽管符合 MRAID 的 SDK 必须支持所有 MRAID 功能，但是应用发布者或广告主可以自由地选择使用或禁止广告使用 MRAID 特性。即，MRAID 支持富媒体广告，但是不强制所有的富媒体广告主必须支持所有 MRAID 特性。

**1.0 版本**

每 1 版本的 MRAID 包含的方法及事件仅提供了最低要求的富媒体广告（所需的信息），主要展示可以在一个固定的容器中改变大小的 HTML 广告（比如，从 banner 尺寸扩展成较大尺寸或全屏尺寸）和插屏广告。

**2.0 版本**
MRAID 2.0 版本对 1.0 版本进行扩展，通过 `resize()` 方法，给广告设计者在可扩展广告上提供更多的控制力。2.0 版本同时提供了 `supports()` 方法，作为一个查询设备某些功能的标准方式。其它特性包括：统一处理视频创意，添加设备日历入口，在设备相册中存储图片。

**MRAID 视频附加项**

2.0 版本发布后，工作组在 MRAID 基础上起草了一份关于使用 IAB 视频播放接口定义（VPAID）的附录。这个视频附加项对如何初始化 VPAID 以及 host 如何使用 VPAID 中的事件追踪这些交互提供了指导。

#### 1.4.1 MRAID 3.0 版本中的更新
MRAID 3.0 版本是一次重大修改，集成了 2.0 版本附录中的 VPAID，可视性变更，及其它加强富媒体广告体验的特性。MRAID 3.0 更新内容如下：

* **可视性（Viewability）：** 可视性的度量必须考虑多个因素，而不能仅仅判断当前 webview 是否正在显示。MRAID 3.0 将 `isViewable()` 方法标记为过时，保留它是为了保证版本兼容性。推荐使用 *exposureChange* 事件取代 `isViewable()` 来获取广告更好的可视性交互。
* **MRAID 检测与初始化（MRAID detection and initialization）：** 之前版本的 MRAID 不论是不是运行在符合 MRAID 标准的 webview 中，都不知道广告何时进行初始化。3.0 版本更新中引入了 *MRAID_ENV* 对象促使在广告初始化时，尽早地传入版本号及其它重要属性。
* **修改 MRAID 事件实现（Revised MRAID events implementation）：** 该版本对 host 与广告以及明确的事件序列实现之间的适当交互提供了指导。
* **可听性度量（Audibility measurement）：** 加入新的事件，些事件可以检查音频是否能被听到以及音量变化情况。
* **定位（Location）：** `supports` 功能中的一个新特性，用来指示定位功能是否可用，如果可用的话，还能提供位置信息。
* **预加载（Pre-loading）以及广告就绪（ad readiness）：** 之前版本的 MRAID 缺少广告资源是否已被加载以及广告是否可以播放的交互的指导，有时会导致展示空白屏的糟糕体验。MRAID 3.0 对 host 如何在播放前检测广告是否就绪及如何正确预加载插屏广告提供了指导。
* **卸载方法（unload method）：** *添加新方法*为广告运行出现异常时提供一种了优雅的退出的机制。当广告不想继续展示给用户时，此机制允许广告去通知 host 去关闭 webview 。
* **过时 Two-part 广告（Two-part ads deprecated）：** MRAID 3.0 依旧会保留 MRAID 2.0 中的 two-part 广告的特性，所以依然可以在 MRAID 3.0 中使用 two-part 广告，但是 MRAID 3.0 新增特性不再支持 two-part 形式的广告。
* **VPAID 事件（VPAID Events）：** 为支持初始化 VPAID 及上报某些事件而添加到 MRAID 2.0 的一个附录。MRAID 3.0 集成了这个附录并且为 host 提供可选集成，使 host 可以支持同时使用 MRAID 和 VPAID 标准制作的广告。
* **useCustomClose()：** 此方法在 MRAID 3.0 中会被标记为过时。
  
## 2 概述
MRAID 是为了使原生应用可以利用标准化的 web 技术而设计的一个标准。本章将讲述 MRAID 是如何利用这些技术的背景信息。

### 2.1 Web 技术支持
为了通用性，MRAID 应该只适用于符合 web 标准的标记语言与脚本语言。虽然 MRAID 是代码无关的，但是我们假设开发者使用了 HTML, JavaScript 和 CSS。 广告设计者应保证广告可以在 web 浏览器中开发和测试。如果设计者使用只被某个或某些浏览器支持的标签，样式或方法，比如，WebKit 中的 CSS3，那么就应该标明此广告只兼容某些设备。

当有新的跨浏览器的 web 标准出现时，我们鼓励广告设计者们使用它们。比如示例中出现的协议，像 sms: 和 tel:

MRAID 2.0 发布时，HTML5 已经被正式发布了，所以要尽可能使用 H5。广告设计者应该知道，尽管通用性一直在增长，但已知的协议和实现依然不能保证可以在所有设备和平台上正常运行。

#### 2.1.1 广告服务器（Ad Server）
用来投放移动富媒体广告的服务器应该支持使用 JavaScript 的 HTML 广告

#### 2.1.2 渲染广告（Ad Rendering）
符合 MRAID 标准的应用必须可以展示任何形式的 HTML 广告。这个程序包括一个 HTML 文件，此文件中有用来初始化广告渲染引擎的 JavaScript 标签。在这个 HTML 文件里，引擎就是显示 HTML 的 "webview"。

尽可能地将 webview 包含设备 web 浏览器的所有功能。比如，iOS 开发者可以使用 WKWebView。手机应用可以初始化多个相互独立表现的 webview。一个广告可以制作多个组件，这些组件展示在不同的 webview 里。

### 2.2 广告控制（Ad Control）
希望使用 MRAID 标准制作广告的设计者们在广告加载之前必须调用 `mraid.js` 脚本（详情看第 3 节初始化部分）。一旦调用脚本后，广告容器就可以将 MRAID JavaScript 注入到广告文件中了。

之后 host 就在后台工作了，广告设计者可以通过 host 控制广告的播放。当广告需要与应用交互时，host 将会处理这个交互。由 host 对广告与 MRAID 之间的交互进行管理后，可将其对广告设计者或应用开发者影响降至很低或无影响。

如果广告没有使用到任何设备特性的话，就没有必要使用 MRAID，但是如果没有 MRAID 的话，host 会把广告当作一个简单的播放类广告。下面是广告可能会利用到的 MRAID API 提供的特性：

* 打开一个内置的 web 浏览器
* 监测展示状况及广告交互
* 将 banner 广告扩展到一个更大的尺寸
* 响应点击、摇晃、定位或其它用户约定的活动行为

广告设计者应该依赖 MRAID 提供的功能去处理上面的行为。即使是简单展示类型的广告也要考虑使用 MRAID 标准，这样如果广告中含有超链接的话也能获得一致性的表现。

### 2.3 接口（Interface）
广告设计者可以访问如下方法、属性和事件：
|Methods|Description|
|---|---|
|getVersion|广告检查 MRAID 的版本<br/>实现些方法后备 host 使用。|
|addEventListener|广告为某个事件注册监听方法|
|removeEventListener|广告移除某个事件的监听方法|
|open|广告使用一个新的 webview 打开一个指定 URL|
|close|广告回退广告容器的状态|
|useCustomClose|MRAID 3.0 中已过时<br/>在 MRAID 3.0 版本的 host 中调用此方法将被忽略。|
|unload|由于无法加载或渲染的原因，广告调用此方法去关闭或移除 webview。Host 可能会移除这个 webview，也可能会用另一个 HTML 文档或重新请求一个广告来替换它。|
|expand|广告请求广告容器放大|
|isViewable（过时）|广告询问 host 屏幕上的广告容器的状态|
|playVideo|广告请求在原生播放器播放视频|
|resize|广告请求改变广告容器大小来适应新广告尺寸|
|storePicture|广告提示用户将一个图片保存到设备上|
|createCalendarEvent|广告提示用户添加一个事件到设备日历上|
|VPAID 中的方法|在 MRAID 环境下管理 VPAID 类视频，其方法集如下：<br/><ul><li>initVpaid</li><li>vpaidObject.subscribe</li><li>vpaidObject.startAd</li><li>vpaidObject.unsubscribe</li><li>vpaidObject.getAdDuration</li><li>vpaidObject.getAdRemainingTime</li></ul>|
|Properties||
|supports|广告请求获取 host 支持的特性的细节|
|getPlacementType|广告请求确认广告位置是一个内置位置还是插屏位置|
|getOrientationProperties|广告请求屏幕方向的细节|
|getCurrentAppOrientation|广告请求当前应用的方向|
|getCurrentPosition|广告请求当前广告容器的坐标位置|
|getDefaultPosition|广告请求广告容器的默认坐标位置|
|getState|广告请求当前广告容器的状态，包括：正在加载（loading），默认（default），扩张（expand），重置大小（resized），隐藏（hidden）|
|getExpandProperties|广告请求当前扩张的属性|
|setExpandProperties|广告设置新的扩张属性|
|getMaxSize|广告请求广告容器最大支持的尺寸|
|getScreenSize|广告请求设备屏幕的尺寸|
|getResizeProperties|广告请求当前广告容器重置大小后的尺寸|
|setResizeProperties|广告设置广告容器的重置尺寸|
|getLocation|广告请求当前设备的坐标位置|
|Events||
|error|host 反馈一个错误（error）|
|ready|host 反馈 MRAID 库已被加载|
|sizeChange|host 反馈广告容器尺寸发生改变|
|stateChange|host 反馈广告容器的状态发生改变|
|exposureChange|host 反馈广告容器露出的百分比发生变化|
|audioVolumeChange|host 反馈音量发生变化|
|viewableChange（过时）|host 反馈广告容器的可见度百分比发生变化|

### 2.4 离线请求与指标（Metrics）
当设备没有连接网络时，富媒体广告可以存储用户如何及何时与广告进行的交互，之后（连接后）再处理这个指标。

MRAID 通过可以集成普通的 API（common APIs）来实现存储并转发广告展示，浏览或其它广告活动等指示，但是，这些指标是如何被监测以及如何上报的细节实现已经超出了 MRAID 的范围。

### 2.5 DAA 广告标记实现（DAA Ad Marker Implementation）
数字广告联盟（the Digital Advertising Alliance, DAA）对数字广告为用户隐私建立了一些原则。为了将这此原则用在移动环境，DAA 开发出实现了一些指南，包括标记布置，通过此标记用户可以获取关于广告的信息以及选择退出。

[DAA 移动端广告标记实现指南](http://www.aboutads.info/resource/Ad_Marker_Guidelines_Mobile.pdf)发布于 2014 年 4 月，2015 年 9 月 1 日起，移动环境下强制使用隐私原则。

MRAID 已经并一直都支持在广告上展示一个小图标（icon）且可以打开一个窗口提供给用户更多关于广告的信息，所以 MRAID 3.0 无需对这方面内容做升级。广告开发者和移动供应商应该审查 Interest-Based Advertising（IBA）原则，这些原则是广告制作与投放策略的一部分。

## 3 初始化及设置
