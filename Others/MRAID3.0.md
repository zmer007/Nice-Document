[原文](https://iabtechlab.com/wp-content/uploads/2018/06/MRAID_3.0_FINAL_June_2018.pdf)

 * [移动端富媒体广告接口标准（MRAID）](#移动端富媒体广告接口标准mraid)
      * [移动端富媒体广告接口标准（MRAID）3.0 版本](#移动端富媒体广告接口标准mraid30-版本)
      * [关于 IAB 技术实验室](#关于-iab-技术实验室)
      * [概要](#概要)
      * [周知](#周知)
      * [介绍](#介绍)
         * [1.1 定义](#11-定义)
         * [1.2 范围](#12-范围)
         * [1.3 MRAID 是如何工作的](#13-mraid-是如何工作的)
         * [1.4 版本](#14-版本)
            * [1.4.1 MRAID 3.0 版本中的更新](#141-mraid-30-版本中的更新)
      * [2 概述](#2-概述)
         * [2.1 Web 技术支持](#21-web-技术支持)
            * [2.1.1 广告服务器（Ad Server）](#211-广告服务器ad-server)
            * [2.1.2 渲染广告（Ad Rendering）](#212-渲染广告ad-rendering)
         * [2.2 广告控制（Ad Control）](#22-广告控制ad-control)
         * [2.3 接口（Interface）](#23-接口interface)
         * [2.4 离线请求与指标（Metrics）](#24-离线请求与指标metrics)
         * [2.5 DAA 广告标记实现（DAA Ad Marker Implementation）](#25-daa-广告标记实现daa-ad-marker-implementation)
      * [3 初始化及设置](#3-初始化及设置)
         * [3.1 初始化概要](#31-初始化概要)
            * [3.1.1 检测 MRAID 广告加载完成](#311-检测-mraid-广告加载完成)
            * [3.1.2 声明 MRAID 环境细节（Environment Details）](#312-声明-mraid-环境细节environment-details)
            * [3.1.3 识别（Identification）](#313-识别identification)
            * [3.1.4 实现 MRAID 事件](#314-实现-mraid-事件)
            * [3.1.5 加载并显示插屏广告](#315-加载并显示插屏广告)
            * [3.1.6 使用 iframe](#316-使用-iframe)
            * [3.1.7 视窗（Viewport）与默认容器设置](#317-视窗viewport与默认容器设置)
            * [3.1.8 初始展示的标准图像（Standard Image for Initial Display）](#318-初始展示的标准图像standard-image-for-initial-display)
            * [3.1.9](#319)
      * [4 特性与操作（Operation）](#4-特性与操作operation)
         * [4.1 可见性（Viewability）](#41-可见性viewability)
            * [4.1.1 轮询速率（Polling Rates）与事件阈值](#411-轮询速率polling-rates与事件阈值)
            * [4.1.2 实现注意事项（Implementation Considerations）](#412-实现注意事项implementation-considerations)
         * [4.2 控制广告展示](#42-控制广告展示)
            * [4.2.1 广告状态以及状态是如何改变的](#421-广告状态以及状态是如何改变的)
            * [4.2.2 检查屏幕与广告的位置及尺寸](#422-检查屏幕与广告的位置及尺寸)
            * [4.2.3 改变广告的尺寸](#423-改变广告的尺寸)
            * [4.2.4 open(), expand() 与 resize() 之前的不同点](#424-open-expand-与-resize-之前的不同点)
      * [5 MRAID 中的方法](#5-mraid-中的方法)
         * [5.1 getVersion()](#51-getversion)
         * [5.2 addEventListener()](#52-addeventlistener)
         * [5.3 removeEventListener()](#53-removeeventlistener)
         * [5.4 open()](#54-open)
         * [5.5 close()](#55-close)
         * [5.6 unload()](#56-unload)
         * [5.7 useCustomClose()（已过时）](#57-usecustomclose已过时)
         * [5.8 expand()](#58-expand)
         * [5.9 isViewable() （已过时）](#59-isviewable-已过时)
         * [5.10 playVideo()](#510-playvideo)
         * [resize()](#resize)
         * [5.12 storePicture()](#512-storepicture)
         * [5.13 createCalendarEvent()](#513-createcalendarevent)
         * [5.14 VPAID 方法](#514-vpaid-方法)
            * [5.14.1 initVpaid()](#5141-initvpaid)
            * [5.14.2 vpiadObject.subscribe()](#5142-vpiadobjectsubscribe)
            * [5.14.3 vpaidObject.startAd()](#5143-vpaidobjectstartad)
            * [5.14.4 vpaidObject.unsubscribe()](#5144-vpaidobjectunsubscribe)
            * [5.14.5 vpaidObject.getAdDuration()](#5145-vpaidobjectgetadduration)
            * [5.14.6 vpaidObject.getAdRemainingTime()](#5146-vpaidobjectgetadremainingtime)
      * [6 属性](#6-属性)
         * [6.1 supports](#61-supports)
         * [6.2 getPlacementType](#62-getplacementtype)
         * [6.3 get/set orientationProperties](#63-getset-orientationproperties)
         * [6.4 getCurrentAppOrientation](#64-getcurrentapporientation)
         * [6.5 getCurrentPosition](#65-getcurrentposition)
         * [6.6 getDefaultPosition](#66-getdefaultposition)
         * [6.7 getState](#67-getstate)
         * [6.8 get/set expandProperties](#68-getset-expandproperties)
         * [6.9 getMaxSize](#69-getmaxsize)
         * [6.10 getScreenSize](#610-getscreensize)
         * [6.11 get/set resizeProperties](#611-getset-resizeproperties)
         * [6.12 getLocation](#612-getlocation)
      * [7 事件](#7-事件)
         * [7.1 错误（error）](#71-错误error)
         * [7.2 ready](#72-ready)
         * [7.3 sizeChange](#73-sizechange)
         * [7.4 staeChange](#74-staechange)
         * [7.5 exposureChange](#75-exposurechange)
         * [7.6 audioVolumeChange](#76-audiovolumechange)
         * [7.7 viewableChange(过时)](#77-viewablechange过时)
      * [使用设备特性](#使用设备特性)
         * [8.1 设备方向](#81-设备方向)
         * [8.2 存储图片](#82-存储图片)
         * [8.3 日历事件](#83-日历事件)
         * [8.4 视频](#84-视频)
      * [9 VPAID 事件与方法](#9-vpaid-事件与方法)
         * [9.1 MRAID 广告中的 VPAID  交互](#91-mraid-广告中的-vpaid--交互)
            * [9.1.1 在 MRAID 上下文中初始化 VPAID](#911-在-mraid-上下文中初始化-vpaid)
            * [9.1.2 发送与接收 VPAID 事件](#912-发送与接收-vpaid-事件)
            * [9.1.4 VPAID AdClickThru 事件](#914-vpaid-adclickthru-事件)
            * [9.1.5 VPAID AdPause, AdPlaying 事件](#915-vpaid-adpause-adplaying-事件)
            * [9.1.6 运行视频自动播放](#916-运行视频自动播放)
         * [9.2 点击链接行为及可视性](#92-点击链接行为及可视性)
         * [9.3 展示记数（Counting Impressions）](#93-展示记数counting-impressions)
      * [10 术语](#10-术语)
      * [11 附录：W3C CalendarEvent 接口](#11-附录w3c-calendarevent-接口)

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

如果没有 MRAID，广告开发者不得不为每个投放广告的系统专门设计交互式广告。这些开发花费的时间将会降低品牌在移动应用上制作出高效广告的可能性。

MRAID 提供了一个包含多个调用方法的库，广告使用这些调用方法与兼容 MRAID 的应用进行交互。当广告开发者使用 MRAID 时，移动应用支持 MRAID 库中的方法越多，广告体验的可预见性越高。

为提升移动广告执行效果，MRAID 3.0 更新了众多特性，比如，辅助跟踪广告可见度，明确广告初始化及就绪状态，对含有视频交互的广告集成了 IAB VPAID (Video Player Ad Interface Definition) 以及其它新增特性。

随着使用手机的用户日益增长，对高质量广告的需求也逐渐增长。广告主想要改进跟踪方法，应用发布者想要提升广告体验。在高质量的广告中接入或升级到 MRAID 3.0 会拥有更好的表现，也会有更优良的广告跟踪。

## 周知

MRAID 是一个广告设计者与开发者在兼容 MRAID 的应用上开发出具有交互能力的广告的协议。应用开发者或 SDK 提供者必须为应用提供一个技术构件，使应用可以设置一个广告容器或响应广告发出的 MRAID 调用。

虽然这个规范是为广告开发者、应用发布者或 SDK 提供者提供的技术指导，但是所有参与移动广告制作的人都应该熟悉此文档。

## 介绍
MRAID 规定一组 API，该 API 为富媒体广告体验与展示该广告的原生移动应用之间提供交互支持。一个应用中的 MRAID 的实现通常是由 SDK 提供，该 SDK 创建一个可以展示广告的容器和一个广告用户与应用容器交互的控制器。

符合 MRAID 标准的广告根据本规范调用方法，提供信息与其它行为。符合 MRAID 标准的原生应用同样使用本规范来提供信息，响应 MRAID 调用及其它行为。

### 1.1 定义
MRAID 包含一些移动部件（moving parts），开发者们使用这些部件制作可交互广告体验。为了清晰地描述这些部件，下面列出了文档中使用到的关键字的定义。

**host：** 移动应用中的组件，该组件提供空间（容器）来展示广告，同时提供服务（控制器），广告通过 MRAID API 可以访问这些服务。host 的实现方式可以是一个 SDK，也可以是直接在 app 内部实现。

**MRAID 实现（MRAID Implementation）：** host 给广告提供 MRAID 方法的特性。包含 JavaScript 属性，广告可以通过它检验 MRAID 的状况，开启 MRAID 并且可以调用 MRAID 服务。

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

同理，对于应用来说，提供商开发的这这些交互组件包含一个容器，容器里有简单的 JavaScript 启动标签。这容器就是一个类浏览器环境或 webview 的组件，广告可以在容器里执行标准化的交互协议。应用可能会初始化一个 "listener" 来监听广告发出的某种跟踪交互或响应事件。

下面的图片展示了部分 MRAID 系统的工作示意：
![MARID system](../resource/parts-of-mraid-system.png)

### 1.4 版本
保证完全兼容旧版本是 MRAID 项目的目标之一。在 MRAID 3.0 更新版本中，工作组保留了 2.0 版本中确立的 6 个关键结果：

* **高通用性（High interoperability）：** 已制作完成的广告在一个 MRAID 容器中可以正常运行，那么该广告在其它平台和操作系统上的 MRAID 容器中也可以正常运行。
* **优雅降级（Graceful degradation）：** 已制作完成的广告在使用所有 MRAID 中高级功能的同时，也能在需要的时候优雅地使用低版本功能。这对 MRAID 版本升级过程中持续加入可使用设备来说尤为重要。
* **进阶性复杂度（Progressive complexity）：** 使用简单的 API 进行广告设计，在必要的时候才使用复杂 API。
* **广告操作的一致性（Consistent means for ad operation）：** 无论是需要扩展还是打开一个应用里的内置浏览器（或没有内置浏览器情况下打开设备浏览器），MRAID 都会提供给广告一个统一的交互方式。
* **退出控制的一致性（Consistent exit controls）：** MRAID 广告保证会有一个统一的方式让用户退出广告并返回应用或内容页。
* **对发布者的灵活性（Flexibility for publishers）：** 尽管符合 MRAID 的 SDK 必须支持所有 MRAID 功能，但是应用发布者或广告主可以自由地选择使用或禁止广告使用 MRAID 特性。即，MRAID 支持富媒体广告，但是不强制所有的富媒体广告主必须支持所有 MRAID 特性。

**1.0 版本**

每 1 版本的 MRAID 包含的方法及事件仅提供了最低要求的富媒体广告（所需的信息），主要展示可以在一个固定的容器中改变大小的 HTML 广告（比如，从 banner 尺寸扩展成较大尺寸或全屏尺寸）和插屏广告。

**2.0 版本**

MRAID 2.0 版本对 1.0 版本进行扩展，通过 `resize()` 方法，给广告设计者在可扩展广告上提供更多的控制力。2.0 版本同时提供了 `supports()` 方法，作为一个查询设备某些功能的标准方式。其它特性包括：统一处理视频创意，添加设备日历入口，在设备相册中存储图片。

**MRAID 视频附加项**

2.0 版本发布后，工作组在 MRAID 基础上起草了一份关于使用 IAB 视频播放接口定义（VPAID）的附录。这个视频附加项对如何初始化 VPAID 以及 host 如何使用 VPAID 中的事件跟踪这些交互提供了指导。

#### 1.4.1 MRAID 3.0 版本中的更新
MRAID 3.0 版本是一次重大修改，集成了 2.0 版本附录中的 VPAID，可视性变更，及其它加强富媒体广告体验的特性。MRAID 3.0 更新内容如下：

* **可视性（Viewability）：** 可视性的度量必须考虑多个因素，而不能仅仅判断当前 webview 是否正在显示。MRAID 3.0 将 `isViewable()` 方法标记为过时，保留它是为了保证版本兼容性。推荐使用 *exposureChange* 事件取代 `isViewable()` 来获取广告更好的可视性交互。
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
MRAID 是为了使原生应用可以利用标准化的 web 技术而设计的一个标准。本章将讲述 MRAID 是如何利用这些技术的背景信息。

### 2.1 Web 技术支持
为了通用性，MRAID 应该只适用于符合 web 标准的标记语言与脚本语言。虽然 MRAID 是代码无关的，但是我们假设开发者使用了 HTML, JavaScript 和 CSS。 广告设计者应保证广告可以在 web 浏览器中开发和测试。如果设计者使用只被某个或某些浏览器支持的标签，样式或方法，比如，WebKit 中的 CSS3，那么就应该标明此广告只兼容某些设备。

当有新的跨浏览器的 web 标准出现时，我们鼓励广告设计者们使用它们。比如示例中出现的协议，像 sms: 和 tel:

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
* 响应点击、摇晃、定位或其它用户约定的活动行为

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
|getState|广告请求当前广告容器的状态，包括：正在加载（loading），默认（default），扩张（expand），重置大小（resized），隐藏（hidden）|
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
当设备没有连接网络时，富媒体广告可以存储用户如何及何时与广告进行的交互，之后（连接后）再处理这个指标。

MRAID 通过可以集成普通的 API（common APIs）来实现存储并转发广告展示，浏览或其它广告活动等指示，但是，这些指标是如何被监测以及如何上报的细节实现已经超出了 MRAID 的范围。

### 2.5 DAA 广告标记实现（DAA Ad Marker Implementation）
数字广告联盟（the Digital Advertising Alliance, DAA）对数字广告为用户隐私建立了一些原则。为了将这此原则用在移动环境，DAA 开发出实现了一些指南，包括标记布置，通过此标记用户可以获取关于广告的信息以及选择退出。

[DAA 移动端广告标记实现指南](http://www.aboutads.info/resource/Ad_Marker_Guidelines_Mobile.pdf)发布于 2014 年 4 月，2015 年 9 月 1 日起，移动环境下强制使用隐私原则。

MRAID 已经并一直都支持在广告上展示一个小图标（icon）且可以打开一个窗口提供给用户更多关于广告的信息，所以 MRAID 3.0 无需对这方面内容做升级。广告开发者和移动供应商应该审查 Interest-Based Advertising（IBA）原则，这些原则是广告制作与投放策略的一部分。

## 3 初始化及设置
在 host 中初始化一个可以执行或展示广告的容器后，MRAID 就可以在原生应用环境下完成广告中的复杂交互。下面将介绍初始化程序。

### 3.1 初始化概要
MRAID 通过实现一个可以展示广告的容器来管理广告与应用之前的交互。广告设计者必须使用 `marid.js` 脚本，但 host 则需要使用 webview 来支持 JavaScript 库。

webview 需要保证调用 `mraid.js` 引用时，相应的 JavaScript 库是都是可以的才行。webview 通过发送 `ready` 事件来确保这些库已准备好了。

下面分步总结这些行为，包括广告与 MRAID 容器是如何参与广告初始化的，还有 MRAID API 库的注册。
 1. host 创建 webview 用来执行广告
 2. 在广告加载之前，host 初始化 *MRAID_ENV* 对象，广告可以根据些对象识别 MRAID 规范。
 2. 广告加载完成之前，广告通过调用 MRAID 脚本标签来识别 MRAID 规范。详情看 3.1.1
 2. （可选）host 检测 MRAID 脚本调用。
 2. host 为广告提供 MRAID 桥接器。
 2. host 提供一个带有限制的 MRAID 对象，此对象状态为加载中（state = 'loading'）并且可以查询状态
 2. 广告必须等到 `mraid.js` 加载完成后，才能访问 mraid 对象，才能使用`mraid.js` 中的 `createElement` 方法。广告需要确定 `mraid.js` 是可用的，必须监听 `ready` 事件来监测 `mraid.js` 完全可用。
 2. 广告执行 `mraid.getState() == 'loading'` 为 `true` 之后，使用 `mraid.addEventListener('ready')` 来监听 `ready` 事件。
 2. host 将 MRAID 库加载至 webview
    1. 将 MRAID 状态置为 'default'，并且发送 `stateChange` 事件
    2. 触发 MRAID `ready` 事件。
 2. 广告事件监听对象收到 `ready` 事件，并且可以访问 MRAID 中广告所需的特性了。

#### 3.1.1 检测 MRAID 广告加载完成
当 webview 中包含广告的页面（document）被解析并且它的所有子资源（包含广告物料）被加载完成后，此时广告就处于加载完成状态了。这时，`ocument.readyState` 被置成 `complete`，`load` 事件会被发送到 `window` 对象。

此检测不一定被所有广告类型所需要，比如，小尺寸的 banner 广告可能不需要这个。但是所有大尺寸广告都必须检测，比如，插屏或其它渲染前需要加载重多物料的广告。

host 可以通过下面的方法检测 webview 以及它所包含广告物料的组件已经加载完成了：
 1. 在 Android 端，使用 onPageFinished() 处理器（handler）。
 2. 在 iOS* 通过轮询（polling）webview 广告页（document）的 `document.readyState` 来检测，直到它被置为 `complete` 并且 `load` 事件被发送到 `window` 对象上。

host 在 iOS 上也可以使用 `webViewDidFinishLoad()` 方法，但是此方法有时触发的太早了。所以在 iOS 也必须同时使用 `document.readyState` 来验证广告加载状态。

当未知原因导致广告未被加载时，广告需要通过 `mraid.unload()` 方法通知 SDK 广告没有展示。

#### 3.1.2 声明 MRAID 环境细节（Environment Details）
之前版本的 MRAID，尽可能早地给广告提供识别一个 MRAID 广告的指导（guidance，用 `mraid.js` 识别），但是广告不能访问它将要填入的容器的信息。某些情况下，广告供应商同时提供一个有 MRAID 的版本与一个无 MRAID 的版本，广告服务器收集环境的详情后就不去在不支持 MRAID 的环境上调用 `mraid.js` 特性了。

MRAID 3.0 版本的广告容器提供了一个 `MRAID_ENV` 对象，此对象可以检验该容器是否符合 MRAID 标准并包含环境的信息，如 MRAID 版本，SDK 版本以及其它可以给广告提供便利的信息。即使广告没有加载 `mraid.js` 脚本，`MRAID_ENV` 对象也是可以使用的。

`MRAID_ENV` 对象声名了 MRAID 环境、SDK 容器的信息，并且只要创意一旦上传后，这些信息就可以使用。广告可以使用这些详细信息去发布更好体验的广告并提升数据分析效果。

下面的脚本是 `MRAID_ENV` 对象的一种示例：
```
<script>
window.MRAID_ENV = {
    version: '3.0',
    sdk: 'SDK Name',
    sdkVersion: '1.0.0',
    appid: 'com.iab.myapp',
    ifa: '01234567-89ab-cdef-0123-456789abcdef',
    limitAdTracking: true,
    coppa: false
</script>
```

**MRAID_ENV 属性（Attribute）：**

下面是广告中使用到的 MRAID_ENV 对象中的属性，此对象是当广告请求时，由 host 提供的。

|属性|描述|
|---|---|
|version*|（string）此 SDK 实现的 MRAID 的版本。它必须与 mraid 接口的 getVersion 方法返回的值一致。|
|sdk*|（string）运行此 webview 的 SDK 的名称|
|sdkVersion*|（string）SDK 的版本号。如果没有可用的版本号的话，可以设为空值。|
|appId|（string）运行广告的应用的包名或应用 ID。通常被叫作 BundleID。|
|ifa|（string）广告目的的用户 ID。对 iOS来说，此值必须为 IDFA（the Identifier for Advertising）。对 Android 来说，此值必须为 Google 广告 ID（AID，the Google Advertising ID）|
|limitAdTracking|（Boolean, 设置 IFA 后使用）如果是 true 则禁止广告跟踪行为，否则，跟踪。|
|coppa|（Boolean，请求儿童类广告）设置为 ture 则为儿童类广告，否则，不是。|

*代表必须

为了方便起见，不用的可选字段可能会从对象中省略或提供一个默认值（string 类型中的空字符串，数字类型中的 0）。

这个可选字段必须设置默认值或不设置，而且*只有在应用发布者明确选项后才提供*。选项可以是应用的全局选项，或基于曝光广告的一次展示（an impression by impression basis，比如一个发布者同时支持直投 'direct sold' 和匿名投放 'anonymous inventory'）。

对于 2-part 类广告，host 只保证 MRAID_ENV 对象在广告初始化的页面（第一部分）有效。如果 2-part 广告第二部分需要这些详情的话，它必须使用在第一部分接收到的详情。

#### 3.1.3 识别（Identification）
正如 3.1.1 小节提到的那样，当加载 HTML 广告时，广告必须请求 `mraid.js` 脚本才能使用 MRAID。

可以通过以下方式请求 `marid.js`：

 * 在 HTML 广告创意里使用 `<script src="mraid.js"></script>` 标签（tag）。
 * 在创意里使用 JavaScript 的 `document.write()` 脚本标签。
 * 在创意里使用 JavaScript 脚本向 DOM 元素中插入标签。

在上面的例子，使用 JavaScript 代码可以动态插入脚本标签。脚本标签必须在广告加载完成之前插入，到 3.1.1 小节查看广告加载的细节。

这份以 MRAID 标准制作的[资源](http://www.iab.com/mraid)，展示了脚本标签在如何被指定的。

广告设计者必须避免使用 `mraid.js` 标识，此标识只能用在标识 MRAID 上（即，不要随便给一个 js 文件起一个 mraid.js 名）。多次使用 `mraid.js` 可能会引出多个 MRAID 库，导致文件体积的增加以及降低广告性能。

下面示例图解释了应用请求 `mraid.js` 的时机：

![interval-request-mraid](../resource/interval-request-mraidjs.png)

 1. host 应用将广告加载到广告容器
 2. 在广告容器内，host 在页内同步加载 JavaScript 以及其它内面资源。
 2. 广告必须在容器加载完成之前调用 `mraid.js` 。
 2. 广告应该监听 `window.load()` 事件，用来确定容器已经加载完成（广告）。

 非 MRAID 广告在一个符合 MRAID 标准的 webview 中调用或不调用 `mraid.js` 都应该可以正常打开广告。一个简单的广告为了保证其超链接的行为一致性也应该使用 MRAID，当使用 MRAID 时，所有的超链接必须使用 `mraid.open()` 来打开。

 #### 3.1.4 实现 MRAID 事件
 在 MRAID 广告中，用户交互可能触发一系列事件。为了减少广告创意的负担，MRAID 3.0 将托管这些在容器操作中发生的用户交互或创意行为的链式事件。

 一般来说，容器在发送任何事件之前，已经执行完容器所必需做的事情，如容器大小变化以及值的变化。这样广告就可以只监听一个事件而不是监听多个事件了。

 比如，以扩展类广告（广告容器可以放大缩小的广告）举例，广告将会请求 `stateChange`，`exposureChange` 和 `sizeChange` 事件来执行创意的扩展。在之前版本的 MRAID 中，广告不仅要检查状态是否改变以便‘放大’广告，而且还要检查容器大小变化是否真的是 `sizeChange` 事件。

 在 MRAID 3.0 中，容器必须执行完所有必需改变后才触发链式事件。在可扩展的示例中，将会触发 `stateChage`, `exposureChange` 和 `sizeChange` 事件。这样，广告可以只需要监真正是它需要的事件就好了，比如：`stateChange`。

 本节将会讲解可扩展广告与插屏广告示例的更多细节信息。

**可扩展广告（Expandable Ad）**

MRAID 的一种典型用法是，根据用户操作，广告可以由 banner 状态扩展成全屏状态。
![lifecycle-of-a-mraid-expandable-ad](../resource/lifecycle-of-a-mraid-expandable-ad.png)

当用户点击广告时，广告借助 MRAID javascript 层调用 `mraid.expand()` 方法去请求扩展。MRAID 请求扩展广告，容器通知应用广告正在扩展，容器暂停一切活动来防止用户此时交互。之后容器重置大小，webview 在右上角保留一个响应关闭事件的位置，并放到一个关闭按钮。

当用户点击关闭按钮或广告调用 `mraid.close()` 方法时，广告重置到原来大小，容器通知应用它可以响应正常方法了。

一般会触发 `exposureChange` 事件，但是如果广告未注册监听器，为了节省计算资源，SDK 可能选择不触发此事件。

**插屏广告**

插屏广告操作与可扩展广告非常相似，但点击插屏广告的关闭按钮后，它不会重置容器大小，并且广告状态会变成 `hidden`。因为广告只显示一次，所有关闭时要移除注册的事件监听器。

![lifecycle-of-an-mraid-interstitial-ad](../resource/lifecycle-of-an-mraid-interstitial-ad.png)

#### 3.1.5 加载并显示插屏广告
插屏广告有时会在所有资源加载完成之前展示给用户，有可能会展示一个加载了一部分的广告或一个空白广告。在这种情况下，这种不好的用户体验可能导致用户在浏览广告之前就关掉它。为了防止展示一个不完整的广告，host 必须保证等到广告完全加载完成后才展示给用户。

广告必须加载到一个游离在屏幕外或一个隐藏的 webview 中，直到所有资源被加载完成为止。查看 3.1.1 小节介绍了识别广告已加载完成的方法。

host 必须在同一个 webview 里加载并展示插屏广告。host 不应将广告加载到一个临时的 webview 中，之后将广告再加载到一个新的未在屏幕上的 webview 中再展示它。这样做会导致广告被重复加载，进而可能会导致广告请求和像素跟踪器（pixel trackers）的重复计数，进而增加的动态广告的额外的按每个广告计费（per-ad）的服务成本。

#### 3.1.6 使用 iframe
由于一个广告可能使用到多个平台的服务，比如，有时一个 MRAID 广告是一个容器，它里面使用 `iframe` 加载另外一个 MRAID 广告。这可能会导致广告在一系列嵌套的 iframe 里。

当 MRAID 方法嵌套在 iframe 里时，访问原生 MRAID 实现技术上来说非常困难或不可能实现。MRAID 不能直接支持嵌入在 iframe 中的广告。如果想在嵌套于 iframe 中的广告访问 MRAID 方法的话，可以使用广告最外层的 frame 为嵌套的 iframe 提供一套专属的机制。

嵌套的 iframe 解决方案已超出 MRAID 的范围。

#### 3.1.7 视窗（Viewport）与默认容器设置
MRAID 方法需要知道广告容器的默认设置，并可以覆盖这些默认设置去适应适当的广告。广告可以像查询网页设置一样去查询容器的默认设置。MRAID 默认设置包括：容器的宽和高，缩放程度（scale）和用户是否可以缩放容器。

当 MRAID 没有为容器创建任何新参数（parameters）或控制时，广告必须检查并判断初始展示的参数是否满足需求。

#### 3.1.8 初始展示的标准图像（Standard Image for Initial Display）
广告设计者应该提供一个初始展示的标准图片。可以通过 `<img>` 标签提供这个初始资源，当其它资源在后台加载时可以显示这个资源。一旦所有额外资源都就绪了，广告的任何元素都可以替换掉该简单图片。

#### 3.1.9
事件处理（Event handing）是 MRAID 功能的关键。自然情况下，web 层与原生层的交互是异步的。通过事件处理，广告可以监听某些事件并在需要的时候响应这些事件。MRAID 提倡广播式（broadcast-style）的事件来支持大量特性与高度一致的灵活性。

## 4 特性与操作（Operation）
MRAID 可以使富媒体广告在手机进行交互，比如，扩展，重置大小以及跟踪多种指标。为了保证所有的 MRAID 都可以流畅操作，开发者必须理解广告与应用的操作的顺序和相应的响应。

本节将描述这些功能与操作的细节。第 3 节讲了初始化与设置。第 5 － 7 节讲指定方法、属性和事件的详细情况。第 8 节讲在原生层与设备特性协调的细节，比如创建日历事件与存储图片。第 9 节讲集成 IAB VPAID 广告在原生播放器 IAB VPAID 中的操作与跟踪。

### 4.1 可见性（Viewability）
当广告展示在 web 浏览器中时，广告中的 JavaScript 代码会检查 DOM 图层的属性并收集图形尺寸信息来决定广告的可见性。然而，这种技术不适用在原生应用容器里展示广告。JavaScript 代码不能检查原生 UI 中的尺寸信息，即，它不知道用户是否可以看到 weview。

对于可见性更完整的描述涉用户交互过程中检测展示的变化情况。MRAID 3.0 引入 `exposureChange` 事件，而不是跟踪当前活动 webview 的可见性。在 7.5 小节可以看到这个事件的详细情况。

MRAID 3.0 更新了可见性相关的内容，其指导原则如下所示：
 
 * **性能（Performance）：** 减少测量给用户端用户体验带来的影响，包括动画的流畅性与电量损耗。
 * **最小化影响（Minimal impact）：** 只向容器添加可以获取更好可见效果所必需的附件（additions）.
 * **永不过时的（Future-proof）：** 避免指定明确阈值，以支持即将到来的测量标准，应该提供最大限度的创意灵活性。
 * **向后兼容（Backwards compatible）：** 当前符合 MRAID 标准的容器必须保持兼容，比如，已存在 API 的含意不应该被重新定义新的含意。

可见性是一个测量指标，用来检测广告是否处于一个用户可以在他们设备上看到的位置。

媒体评级委员会（MRC, the Media Rating Council）在 2016 年公布了测量移动广告可见性的指南。应该使用[这些指南](http://mediaratingcouncil.org/062816%20Mobile%20Viewable%20Guidelines%20Final.pdf)在移动 web 和应用内为跟踪可见广告的展示制定策略。对于移动广告测量来说，MRAID 3.0 的 `exposureChange` 事件与 MRC 的指导是一致了。

`isViewable()` 方法与 `viewableChange` 事件在 MRAID 3.0 中已经过时。在当前版本中保留这些功能只是为了向后兼容性。

#### 4.1.1 轮询速率（Polling Rates）与事件阈值
升级的一贯准则就是要保持性能，MRAID 3.0 对 `exposureChange` 事件的处理必须不能影响 UI 动画的流畅性或加剧电量消耗。

host 平台升级 MRAID 3.0 实现的同时必须避免与 JavaScript 进行过多的交流。这种结合升级要保证在广告可见区域报告 `expsureChange` 事件后，下次该事件的报告间隔不少于 200 毫秒。当使用轮询时，视图层必须足够频繁去获取展示中的改变，轮询频率不能小于 200 毫秒/次。

**使用示例**

下面程序展示了 `exposureChange` 事件的一次完整过程：
 1. 广告容器注册一个监听器监听 `exposureChange` 事件。比如：
 ```
 mraid.addEventListener('exposureChange', handleExposureChange);
 ```
 2. `exposureChange` 事件调用广告事件处理器并比较容器框（the contianer rectangle）广告创意的大小，比如：
 ```
 function handleExposureChange(exposedPercentage, visibleRectangle, occlusionRectangles) {
     if (exposedPercentage >= 50.0) {
         // log visible time ...
     }
 }
 ```
 3. 如果广告在可见区域取样时间过长超出了可见性阈值（the viewability threshold），就必须考虑广告是否可见。如果不需要更多的测试信息，要移除 `exposureChange` 监听器，比如：
 ```
 mraid.removeEventListener(`exposureChange`, handleExposureChange);
 ```

#### 4.1.2 实现注意事项（Implementation Considerations）
用户可应用开发者想要他们有应用有最好的性能表现。广告测量不应该降低应用的性能。MRAID 关于可见性的实现必须将以下内容纳入考虑范围：
  
**避免使用 JavaScript 进行耗时工作**

应用 webview 里展示广告是运行在两个处理器线程中的：一个线程用在原生 UI 另一个用在 webview 中的 JavaScript。当原生线程处理等待状态时，要避免 JavaScript 耗时操作。

比如，在 iOS WKWebview 使用 Objective-C 方法：
```
evaluateJavaScript:completionHandler
```
*此方法没有阻塞 UI 线程的副作用。推荐使用这个方法来替代 webview。*

**使用低延时（low-latency）的 API**

原生平台含有 UI 发生变化时高效低延时地发送通知的 API。实现 MRAID 新事件 `exposureChange` 时，必须使用此类通知，特别是在低轮询率或低功率（back-off）作用下。在 Android 平台下，可以 `ViewTreeObserver` 事件就是这么一个高效的 API。

**防止阻塞操作**

不要在操作系统应用生命周期回调中执行阻塞操作。

在 iOS 中，包括以下 `UIApplicationDelegate` 方法：

 * applicationWillResignActive
 * applicationDidEnterBackground
 * applicationWillEnterForeground
 * applicationDidBecomeActive

NSNotificationsCenter 相应事件也添加到阻塞操作。

在 Android 中，阻塞操作包含以下的 Activity 方法：
 
 * onPause
 * onStop
 * onStart
 * onResume

如果这些方法在几秒内没有返回值，操作系统会终止应用。

**在不阻塞原生操作下将事件加入队列**

有时一个广告中的 JavaScript 可能是错误的或是恶意的。当这些代码因为太长而执行不了时，后面的事件会被延时或阻塞。 MRAID 实现在入队事件（enqueueing events）时一定要避免阻塞原生线程。无论在原生线程还是在 webview 中，入队事件只可以在之前的 MRAID 事件监听器返回结果后再加入。

### 4.2 控制广告展示
除初始化显示外，广告设计者应该有理由来控制广告的展示：

 * 应用可能加载后台加载广告视图来缓解延时问题，此时广告已经请求了，但是用户可不到广告。
 * 广告可能超出应用内容默认尺寸
 * 用户交互完成后，广告可能会返回默认尺寸

4.2.1 到 4.2.4 小节将解释如何控制高级广告的展示，比如扩展广告和插屏广告。

#### 4.2.1 广告状态以及状态是如何改变的

每个执行广告的 webview 都会处于以下的某个状态：
 
 * **loading：** webview 还未准备好与 MRAID 实现进行交互
 * **default：** 初始状态，容器的尺寸就是被应用和 SDK 放置的尺寸。
 * **expanded：** 广告容器被扩展到视图层顶部，覆盖在应用内容之上。
 * **resized：** 广告容器已使用 `resize()` 方法改变过尺寸。
 * **hidden：** 可交互广告的关闭状态。如果支持的话，banner 广告关闭时也会被置为隐藏。

调用：`expand()`, `resize()`, `close()`, 或 `unload()` 方法时，webview 的状态会发生改变。调用这些方法的影响大概如下表所示：
|初始状态|expand()|resize()|close()|unload()|
|---|---|---|---|---|
|loading|不变|不变|不变|不适用*|
|default<br/>(banner)|变为 “expanded”|变为 “resized”|变为 “hidden” (如果支持的话)|不适用|
|expanded|不变|不变|变为 “hidden”|不适用|
|resize|变为 “expanded”|变为 “resized” 并且更新了值|变为 “default”|不适用|
|hidden|不变|不变|不变|不适用|

*不适用 因为 `unload()` 方法是用在广告不再展示给用户时的。在这种情况下，host 可以隐藏（dismiss）或移除 webview，然后使用其它的文档（document）来替换它，或使用另一个广告刷新 webview。

two-part 扩张（expandable）类的广告以及插屏广告状态流程会有些许不同：

 **可扩张 Two-Part 广告**

 可扩张 Two-Part 广告使用两个不同的 webview ，广告组件可以彼此独立扩展。因为 MRAID 广告同一时间只有种状态，可扩张 two-part 广告只要被扩张的视图在屏幕上，它就会一直处于 expanded 状态。

 新特性，扩张的 webview 会处于 loading 状态，直到 MRAID 可用时。当触发 ready 事件时，广告状态变为 expanded。对于 banner 来说，two-part 广告的第一部分也会改变它的状态，从 default 变为 expanded。

 **插屏广告**
 
 对于插屏广告，webview 从 loading 变为 default，当插屏广告关闭时，状态变为 hidden。

`getState()` 方法可以获取当前状态，该状态是由最后触发 stateChange 事件的行为确定的。此特性分别会在 6.7 和 7.4 做进一步介绍。

#### 4.2.2 检查屏幕与广告的位置及尺寸
MRAID 包含可以使广告检测它位置、大小及可扩展的最大尺寸的方法。广告设计者使用这些方法增强广告的灵活性，可以在不同设备或不同尺寸屏幕上有不同的行为。

详细信息请查看：
 
 * 6.5 getCurrentPosition
 * 6.6 getDefaultPosition
 * 6.10 getScreenSize
 * 6.9 getMaxSize
 * 7.3 sizeChange

#### 4.2.3 改变广告的尺寸

MRAID v2 版本包含三个独立的方法用于改变广告的大小：`open()`，`expand()` 或 `resize()`.

改变尺寸最简单的方法是使用 `open()` 方法。试图打开超链接时，此方法也会打开一个新的 webview 来展示广告的不同尺寸的新组件。这种形式的广告称为 two-part 可扩展广告。

`expand()` 用来扩展广告，是一种相当简单的方式，即，直接覆盖应用的内容。

`resize()` 方法可以更精细扩大或缩小广告，该广告处于应用的某个对话框（dialogue）里。这个方法给设计者提供更多自由与控制权，但是，对广告、应用或 webview 来说，为了能正确响应需要在不同地方添加额外的方法和监听器。

#### 4.2.4 `open()`, `expand()` 与 `resize()` 之前的不同点
尽管这些方法是相关联的，但它们也促进了一种渐进的复杂性方法。讨论 `open()`、`expand()` 和 `resize()` 会帮助广告设计者为他们的需求选择最合适的方法。

所有这些方法必须是用户发起的并且是基于 IAB 新广告组合标准（IAB New Ad Portofolio）的。用户发起就是用户不连接的操作，如点击（click）或敲击（tap）.

广告必须确保这些方法的调用只能是由用户发起的。用户没有发起行动，广告一定不能主动触发这些请求。

 **open()**
    
    * 最基本的操作（Lowest common denominator）
    * 广告打开落地页或站点
    * 使用设备默认浏览器打开一个新 url
    * 总是全屏的
    * 没有其它额外属性

**expand()**
    * 简单的接口
    * 维护广告体验（Maintains ad experience）
    * 全屏
    * 少量额外属性
    * 支持 one-part 或 two-part 创意
    * MRAID 强制要求在固定的位置（右上角）放置关闭按钮
    * 创意相对统一（Relative aligment for creative）
    * 扩展之前检查屏幕尺寸（getScreenSize）

**resize()**
    * 灵活的接口
    * 持续，无模式的广告（non-modal ad）
    * 无默认值，可以变大或变小
    * 需要额外属性和方法
    * 只支持 one-part 创意
    * MRAID 强制指定关闭区域，但是设计者可以改变关闭按钮在创意中的位置。
    * 绝对定位可能性（Absolute positioning possible）
    * 支持指定方向的重置大小
    * 在重设大小前检查广告允许的最大尺寸
    * host 不能拒绝广告的 `resize()` 请求

下表列出了这些方法的主要不同点：
|属性|open()|expand()|resize()|
|---|---|---|---|
|Modal|Y|Y|N|
|MRAID 强制广告|N|Y|Y|
|观看者停留在广告内|N|Y|Y|
|two-part 创意|n/a|Y|N|
|one-part 创意|n/a|Y|Y|
|对齐到屏幕|n/a|Y|N|
|为小创意提供背景|n/a|Y|N|
|尺寸变大|n/a|Y|Y|
|尺寸变小|n/a|N|Y|
|应用限定最大区域|n/a|N|Y|
|调用完成所需的回调|n/a|N|Y|
|支持方向|n/a|N|Y|
|创意可以控制重置大小的广告的位置|n/a|N|Y|
|应用可以返回默认状态|n/a|N|Y|

当扩展广告小于全屏时，调用 `expand()` 会导致 webview 留有空白或模糊广告下的应用内容，这样可以清晰地看到扩展广告是做为原生应用中的一个模态（modal）存在的。模态 MRAID 不支持竖屏扩展，但可以使用 `resize()` 在竖屏中创建非模态扩展广告。

||模态|非模态|
|---|---|---|
|全屏|支持（使用 expand()）|不支持|
|竖屏|不支持|支持（使用 resize()）|

## 5 MRAID 中的方法
MRAID 方法通常伴随广告请求广告容器适配或传递信息等操作使用。比如，调用 `expand()` 促使 SDK 停止广告操作并扩展广告容器。一些方法包括检查或更新属性（第 6 节）和使用一个事件（第 7 节）来上报或确认某种行为。

### 5.1 getVersion()

广告调用 `getVersion()` 方法来查询 host 支持的 MRAID 版本。host 会返回一个字符串（"3.0" 代表 MRAID 3.0）。此版本号代表 host 支持的 MRAID 的版本号（1.0, 2.0 或 3.0 等），不是供应商 SDK 的版本号。

|语法|getVersion()|
|---|---|
|参数|无|
|返回值|String|
|相关事件|无|

注意：MRAID 与 SDK 的版本在 MRAID_ENV 对象里有定义，详情查看 3.1.1 节。

### 5.2 addEventListener()

广告调用 `addEventListener()` 来某个事件注册某个监听器。广告可以注册多个监听器，每个对应一个事件。某个事件触发时，host 将该事件分发给对应监听器。广告也可以为多个事件注册同一个监听器，而不是每个事件都注册一个。

MRAID 3.0 中支持的事件：

 * **ready：** 初始化完成时上报
 * **error：** 当有错误发生时上报
 * **stateChange：** 当状态改变时上报
 * **exposureChange：** 展示发生改变时上报
 * **viewableChange(deprecated)：** 可见性发生变化时上报
 * **sizeChange：** 可见部分发生变化时上报

|语法|addEventListener(event, listener)|
|---|---|
|参数|event: 需要监听的事件的事件名，string 类型<br />listener: 可执行方法|
|返回值|无|
|相关事件|无|

### 5.3 removeEventListener()

当广告不再需要某个事件的通知时，使用 `removeEventListener()` 方法注销该事件。为避免错误，事件监听器必须在不需要的时候才能注销。如果在调用的 `listener` 属性里没有指定监听函数，那么这个事件对应的所有函数者会被移除。

|语法|removeEventListener(event, listener)|
|---|---|
|参数|event: string，代表要移除的事件的事件名<br/>listener: 将要被移除的函数|
|返回值|无|
|相关事件|无|

### 5.4 open()

广告可以调用 open() 方法来促使 host 在一个浏览器窗口打开一个外部网站。此方法的目的是为了处理广告中的点击（clickthroughs）。所有 MRAID 广告都得用 open() 方法处理点击。所有 MRAID SDK 都要确保 open() 方法可以正确处理 "deep links"，也被称为 "universal links"(Apple) 或 "Android App links"(Android)。最简单的方法就是使用系统默认浏览器来处理 open() 回调，使用应用内置 webview 会比较麻烦。如果使用应用内置 webview，SDK 需要确保使用正确的应用来响应 deep link 链接。

**常规实现注意点**

非 MRAID 广告的外部 web 页必须使用 open() 打开。调用 open() 打开的展示页不能加载一个额外 MRAID 实现的实例，意味着 close() 方法在已打开的浏览器里会出现异常。打开的窗口只能使用打开的浏览器的某个控制实现来关闭。

原生浏览器总在已打开的窗口中控制回退、前进、刷新、关闭。SDK 提供者认为 open() 方法是一个可上报的事件。

|语法|open(URL)|
|---|---|
|参数|URL：string，代表要打开的网页的 URL|
|返回值|无|
|相关事件|无|

**超链接**

当用户在 MRAID 广告点击一个 HTML 超链接（使用 `<a href="">` 标签定义），将会有两个可能：链接的页面在已存在的 webview 中加载，或打开另一个浏览器窗口加载链接。

打开 MRAID 广告中的 URL 时，open 方法必须使用原生设备或操作系统行为（OS behavior）或用户设置来打开。这样可以保证此响应是用户预期的，并打开必要的设备控制，用户需要这些控制从外部链接切换过去。

### 5.5 close()

广告使用 `close()` 方法来回退容器的状态。host 通过状态改变做出响应，此状态依赖使用stateChange 事件（7.4 描述）测量的当前状态。

对于处于扩展或重置尺寸过的状态，`close()` 方法将广告转换到默认状态。对于处于默认状态的插屏广告来说，`close()` 方法将基转换到 hidden 状态。这些广告状态及调用 `close()` 后的状态在 4.2.1 节有描述。

拿处于默认状态的广告来说，广告开发者必须避免使用 `mraid.close()`。使用 `mraid.unload()` 通知容器广告需要消失（dismiss）。

如果广告在调用 `expand()` 之后一次或多次调用 `resize()` 方法，`close()` 会返回容器的状态为 'default'。 在这些实例中调用 `close()` **不会**简单的撤销最近调用的 `resize()` 或 `expand()`。

|语法|close()|
|---|---|
|参数|无|
|返回值|无|
|相关事件|StateChange（7.4 节）|

**可重置尺寸广告的关闭控制**

可扩展广告重置广告尺寸必须以一种用户可看到返回广告默认状态的方式。MRAID 有两种不同的 “close” 特性：

 * 关闭事件区域：关闭事件区域是一个用户点击就能关闭或折叠广告到默认状态的广告创意区域。 host 为所有 MRAID 可变尺寸广告在创意指定的位置提供关闭区域。

 * 关闭指示器：关闭指示器是一个可见的开关来为用户指明关闭事件的区域。对可变尺寸广告来说，广告提供一个关闭指示器的图片，host 提供关闭区域。

host 必须总要包含一个 50x50 密度无关像素在关闭按钮区域。推荐放到广告容器的右上角。

关闭指示器至少为 50x50 密度无关尺寸。推荐放在右上角。当用户点击指示器时，广告返回到它的默认状态。推荐使用一个 “X” 号按钮当作关闭指示器。

`useCustomClose()` 方法已经过时了，广告一定不要提供自定义关闭指示器。关闭指示器总是由 host 提供的。这可以确保广告在应用中扩展或重设广告尺寸中有一致的体验。

host 也可以跟随原生应用决定关闭或消失广告。这样做可以会在某个应用中保持浏览内容的原生用户体验。比如，在某些应用内容导航是通过在全屏范围内滑动（iOS 右滑关闭退出页面）或点击屏幕的左边或右边。同理，这种行为也可以用在展示在内容间的待关闭的广告上。可扩展或可变尺寸广告不能使用这个行为。它们必须使用 host 提供的关闭指示器。

当 MRAID 2.0 或更老的版本的广告在 MRAID 3.0 容器里时，`useCustomClose()` 请求会被 host 忽略。

一个可变尺寸广告必须在屏幕放置一个可见的可以关闭整广告的区域。如果 host 检测到广告变化会导致关闭事件区域移出屏幕，host 必须返回一个错误并忽略这个变大请求，使广告停留在当前状态。此限制也意味着可变广告必须不小于 50x50 密度无关像素，保证可变创意有空间放置关闭区域。

关闭控制是被所有形式的 host 强制提供的。

在 Android 操作系统上，用户点击“返回”按钮必须理解为（调用）广告关闭方法。

### 5.6 unload()
这个方法主要目的是允许广告通知 host 广告不再显示给用户了。广告请求 host 使用 `unload()` 方法完全关闭广告。host 则关闭这个广告，之后移除 webview 或使用另一个文档替换或使用一个新广告来刷新 webview。

当广告决定不再展示给用户时，它可以在其生命周期任何时间调用此方法，并且它不一定会返回到默认状态。

广告应用在遇到错误或运行时异常导致广告不能被正确渲染时调用此方法。下面是一些广告可以决定使用 `unload()` 方法的例子：

 * 初始化期间，遇到与广告服务器通讯错误或不能正确加载所有必需的资源或不想在当前所处的环境展示。
 * 在生命周期后面的阶段中，加载一个必需的资源时可能遇到错误或决定不再展示给用户。
 * 一个预加载的广告可能决定不向用户展示了。

|语法|unload()|
|---|---|
|参数|无|
|返回值|无|
|相关事件|StateChange（7.4 节）|

`unload()` 为广告提供一个优雅的退出机制，host 无需在屏幕上显示一个错误或向用户显示一个空白屏幕。

### 5.7 useCustomClose()（已过时）
此方法在 MRAID 3.0 中已过时

### 5.8 expand()
广告使用 `expand()` 支持扩展，可以通过改变当前 weview 的的宽或高（one-part 创意）或在顶层打开一个新的扩展尺寸的 webview （two-part 创意）。扩展的 view 可以包含一个新的 HTML 文档或可以在重用之前同一个文档。

这种操作使广告设计者提供 one-part 广告（banner 和 面板是同一个创意）或 tow-part 广告（banner 或面板是不同的 HTML 创意）的扩展。

 **One-Part 创意（不指定 URL）**
 
 当广告调用 `expand()` 而不指定 URL 时，host 扩展已存在的 webview。 这个方法简化广告设计和上报，因为原始创意没有重新加载且额外的展示需要记录。

 **Two-Part 创意（指定 URL）**

 当广告调用 `expand()` 并指定 URL 时，host 打开一个新的 webview。这个 URL 必须是一个完整的 HTML 页，不能是一个代码片段。如果广告创意的第二部分需要使用 MRAID，该广告必须在新 webview 中请求 `mraid.js`。无论广告的扩展部分请求或不请求 mraid.js，host 都必须提供一个关闭区域。如果广告在 `expandProperties` 指定一个关闭指示器图案，那么 host 将使用这个提供的图案指示关闭。否则，host 将提供（默认）关闭图案。

**host 响应**

调用 `expand()` 时，host 使用 stateChange 事件将其状态从 'default' 变成 'expanded'，stateChange 事件在 7.4 节有讲述。host 会忽略多次调用 `expand` 并将状态维持在 `expand`。

即使扩展创意可能比屏幕小，host 也会扩展容器来覆盖所有可用屏幕区域。广告容器未填充广告创意的部分会被置为透明或不透明。扩展广告总是模态的，处于扩展状态时，容器必须阻止加载新的广告。

**竖屏问题**

扩展广告在扩展状态可能涉及多种窗口对象，或有时改变内容的 z 轴（图层位置）。这此复杂的应用环境可能会影响用户关闭广告的功能。SDK 开发者必须考虑这些问题并根据指定 MRAID 实现做调整。

**扩展创意位置**

广告设计者决定在屏幕的哪块区域放置扩展广告，特别是当广告可以被放在多个不同的位置时。对于全屏扩展来说，所有 MRAID 实现都为广告提供一个全屏的 view ，并把广告完整显示出来。

当扩展创意尺寸大于或小于设备屏幕尺寸时，host 调节容器到应用和设备所允许的最大尺寸。host 可能不会缩放广告来配合屏幕。广告可能在扩展容器里通过 CSS 把扩展创意放到希望的位置上。

![placement-of-expaned-creative](../resource/placement-of-expaned-creative.png)

|语法|expand([URL])|
|---|---|
|参数|URL（可选）：此 URL 代表一个可在新 view 中展示的文档。如果为 null 或一个非 URL 参数，当前广告的主体会在当前 webview 中展示。|
|返回值|无|
|相关事件|StateChange|

### 5.9 isViewable() （已过时）
`isViewable()` 方法在 MRAID 3.0 中已过时，并会在将来版本中移除。MRAID 3.0 中保留这个方法只是为了向后兼容。

`isViewable()` 方法返回值表示广告容器当前是否显示在屏幕上。 当广告从在屏幕上到不在屏幕上（或反过来）转换时，会触发 `viewableChange` 事件。

对于 two-ppiece 扩展广告来说，当广告状态是 expanded 时，`isViewable()` 根据扩展广告的可见性返回结果。

任何广告可能会被移出屏幕的情况，广告都必须在所有行为之前检查它的可见状态是否注册了 `viewableChange`。

注意 MRAID 没有定义一个广告必须有某个最小百分比阈值或像素值数在屏幕上才认定为可见的方法。

|语法|isViewable()|
|---|---|
|参数|无|
|返回值|true：容器在屏幕上，用户可以看到。<br/>false：容器不在屏幕上并看不到|
|相关事件|viewableChange|

### 5.10 playVideo()
当一个创意的视频组件需要在设备原生播放器播放时，广告调用 `playVideo()`。在内置 webview 播放视频而非使用原生播放器时，广告设计者必须使用 HTML5 的视频标签。

|语法|playVideo(URI)|
|---|---|
|参数|URI：string, 视频或视频流的 URI|
|返回值|无|
|相关事件|无|

### resize()
调用 `resize()` 请求使容器尺寸改变，容器里创意的尺寸也会改变。重置尺寸被使用在连续的变化或非全屏范围的变化，它不影响应用操作。

调用 'resize()' 之前，广告必须使用 `setResizeProperties()` 指定期望的宽和高尺寸。在 `setResizeProperties` 之前调用 `resize()` 会报错。

调用 resize 之后，host 将广告容器调整至广告期望的尺寸。

在一个比应用内容更高 z 轴上进行 resize 操作，因此它不会移动或重排应用内容。如果应用希望支持内容移动（content-shifting）广告，比如 “push-downs”，应用必须实现应用重排特性来支持这个功能。

**产生的事件**

如果 resize 正常执行，host 发送两个事件：`stateChange`（7.4 节）和 `sizeChange`（7.3 节）。`stateChange` 从 'default' 更新为 'resized'。如果调用 `resize()` 之前广告状态为 'expanded' ，host 会发送一个错误事件（7.1 节讨论）。`sizeChange` 事件会携带重置大小后的容器的宽和高。

*常规实现注意（Genral Implementation Note）*

使用 `expand()` 而非 `resize()` 方法来使广告扩展到全屏或大尺寸。因为全屏或大尺寸扩展会遮盖应用内容，`expand()` 天然会停止其它应用操作。resize 总会导致一个非模态尺寸变化，且用户总会看到应用的某一部分。

|语法|resize()|
|---|---|
|参数|无|
|返回值|无|
|相关事件|sizeChange<br/>stateChange|

### 5.12 storePicture()
广告向用户提供保存图片的选项，如果支持的话，可以使用 `storePicture()` 来将图片存放到设备相册里。此图片可能是广告的一部分也可能是从服务器下载。

广告可以使用 6.1 节讲得 `supports` 方法来向 host 查询设备是否支持 `sotrePicture`。如果不支持 `storePicture`，广告一定要避免调用 `storePciture()`。

如果支持，host 响应一个初始控制程序去请求用户存储图片的权限。控制程序是一个系统级的模态组件，提供给用户确认或取消的选项。如果调用缺少这样的处理器，host 在 supports 对象的 `storePicture` 特性上设置一个 false 值。

MRAID 实现必须支持使用 HTTP 重定向（以跟踪为目的）添加图片；但是，实现不必需支持元重定向（meta-redirects）。

无论什么原因导致添加图片失败或被用户取消，host 都要发送一个错误。

|语法|storePicture(URI)|
|---|---|
|参数|string, 图片或其它媒体资源的 URI|
|返回值|无|
|相关事件|无|

### 5.13 createCalendarEvent()

如果支持的话，广告可以使用 `createCalendarEvent()` 打开设备日历 UI 来添加一个事件。host 将会在原生设备上打开一个日历事件表。如果没有找到事件表，host 会在 supports 对象的日历特性上将值置为 false。

当相日历 UI 打开时，任何广告操作都要暂停。日历事件数据必须使用 W3C 日历规范的 JavaScript 对象写入。查看附录来了解 W3C 日历规范。

无论什么原因导致添加日历事件失败或被用户取消，host 都要发送一个错误。

|语法|createCalendarEvent(parameters)|
|---|---|
|参数|URI：string，图片或其它媒体资源的 URI（**译者注：** 原文就是这个意思，但感觉是有问题的）|
|返回值|无|
|返回值|无|

### 5.14 VPAID 方法
MRAID 方法可能包含设计报告给 VPAID 事件的视频。如下 MRAID 方法被用于在 MRAID 环境中设置 VPAID 事件，同样包含展示视频广告的最基本信息。查看第 9 节来了解 VPAID 在 MRAID 应用实现中的工作细节。

#### 5.14.1 initVpaid()
只要广告检查 `mraid.supports()` 方法支持 VPAID，它就需要给一个 `vaidObject` 控制器，容器使用此控制器传递关于视频播放的事件和信息。VPAID 对象允许 host 订阅 VPAID 事件，开始广告以及调用 VPAID 的一组有限方法集。调用 `initVpaid()` 之后，为了使视频开始播放，控制器必须调用 `vpaidObject.startAd()`。广告在调用 `videoObject.startAd()` 之前一定不能播放。

|语法|initVpaid(vpaidObject)|
|---|---|
|参数|vpaidObject - 代表展示在广告中的 VPAID 的 JavaScript 对象|
|返回值|无|
|相关事件|无|

#### 5.14.2 vpiadObject.subscribe()

调用 `initVpaid()` 之后，host 可以使用 `vpaidObject.subscript()` 选择性的注册一些 VPAID 事件。VPAID 支持与不支持的列表会在 9.1.3 节中列出。

#### 5.14.3 vpaidObject.startAd()

当 host 检测到应用已准备好去播放视频广告时，它会调用 `vpaidObject.startAd()` 来通知 MRAID 广告 VPAID 创意现在可以播放了。

#### 5.14.4 vpaidObject.unsubscribe()

当 host 不再需要监听它所订阅的事件时，可以使用不再使用的事件名做为参数调用 `vpaidObject.unsubscribe()` 来注销。

#### 5.14.5 vpaidObject.getAdDuration()

host 使用 `vpaidObject.getAdDuration()` 来询问广告视频广告播放时长为多少。广告返回这个广告总共持续的秒数。

#### 5.14.6 vpaidObject.getAdRemainingTime()

host 使用 `vpaidObject.getAdRemainingTime()` 来查询广告剩余时间。广告会返回查询请求时广告剩余秒数。

## 6 属性
此节讲解广告通过 host 查询容器属性的方法。某些情况下，广告可以设置属性来指引 host 更新容器来适应广告操作。例如，广告调用 resize 方法之前，必须要设置 resize 属性。host 使用设置的属性相应的调整容器。

### 6.1 supports
广告可以使用 supports 功能来查询 host 获取应用支持哪些设备原生特性。了解支持哪些原生特性可以帮助广告使用可用的特性来弥补它不支持的特性。

广告向 host 查询以下原生特性：
|特性|描述|
|---|---|
|sms|设备支持 sms 协议：发送 SMS 消息|
|tel|设备支持 tel 协议：打电话|
|calendar|设备可以创建一个日历记录|
|storePicture|设备支持 MRAID storePicture 方法|
|inlineVideo|设备可以使用 `<video>` 标签播放 HTML5 视频，优化指定 video 标签的尺寸。这不一定是使 video 全屏播放。|
|vpaid|设备容器支持 VPAID 通信，VPAID 事件会在第 9 节介绍|
|location|设备支持访问 GPS 坐标|

在任何支持这些属性的设备上，除非 app 发布者检测到的特性与发布政策冲突，app 实现 MRAID 必须传递所有这些功能，

|语法|supports(feature)|
|---|---|
|参数|feature：String。特性名，上面表格中列举的|
|返回值|true: 支持此特性并 get 方法与事件都可用<br/>false: 此设置不支持这个特性|
|相关事件|无|

### 6.2 getPlacementType
广告调用 `getPlacementType()` 来检测它是否被加载到一个内置的位置或插屏位置。

为效率起见，广告供应商有时在 banner 与插屏位置使用同一个创意。这些广告可能被设计为根据所放位置不同而有不同的表现。

对于 two-part 可扩展广告来说，第二部分的扩展创意也必须向 host 查询位置类型。

host 返回 'inline' 还是 'interstitial' 可以看下表说明：
|语法|getPlacementType()|
|---|---|
|参数|无|
|返回值|**inline:** 广告位置默认值为 inline，即将广告内置（banner）到内容中<br/>**interstitial:** 广告位置在内容之上|
|相关事件|无|

### 6.3 get/set orientationProperties
广告通过 `orientationProperties` 查询 host 当前设备的方向，使用 `orientationProperties` 来调整扩展和屏幕广告的显示。默认状态的 banner 不能使用 `orientationProperties` 属性。可变大小的广告可以使用 `orientationProperties` 属性，但是不会有任何效果。

下面为 `orientationProperties` 对象的示例代码：
```
orientationProperties object = {
    "allowOrientationChange" : boolean,
    "forceOrientation" : "portrait|landscape|none"
}
```
这两个方向属性含义如下：
* **allowOrientationChange:boolean**

其值为 true 时，代表容器支持设备级方向变化；其值为 false 时，容器会忽略设备级方向变化的请求（比如，即使设备的方向改变了，webview 的方向也不会发向变化）。默认为 true。任何时刻，广告都可能绕过 `allowOrientationChange` 使用 `forceOrientation` 变量请求改变方向。

* **forceOrientation:string**

其值为 portrait, landscape 或 none 。如果设置了 `forceOrientation` 不管设备是什么方向都得按要求的方向打开一个视图（view）。例如，当用户在 landscape 模式点击一个 portrait 模式的扩展广告，无论设备或之前广告的方向是什么，打开的广告必须是 portrait 方向的。默认值为 none。

为了更好的控制广告行为，在广告处于扩展状态后，广告应该调整方向对象的属性。这种形式的广告开始为 portrait 但是指导用户改变方向来玩游戏。用户完成游戏之前必须保持方向固定。host 必须可以接收改变，以便满足用户交互与可扩展广告的扩展属性。

广告必须同时设置这两个属性来确保正确控制广告的方向。比如，强制使用 landscape 方向时，要将 `allowOrientationChange` 设为 false， 将 `forceOrientation` 设为 landscape.

代码示例：
```
mraid.setOrientationProperties ( {"allowOrientationChange":true} );
mraid.expand()

/* 用户变成 landscape，开始游戏 */
mraid.setOrientationProperties ( {"allowOrientationChange":false} );

/* 用户结束游戏 */
mraid.setOrientationProperties ( {"allowOrientationChange":true} );
```

`getOrientationProperties` 方法会返回整个 `orientationProperties` 对象。
|语法|`getOrientationProperties()`|
|---|---|
|参数|无|
|返回值|JavaScript 对象：包含方向属性|
|相关事件|无|

`setOrientationProperties` 方法设置 `orientationProperties` 对象中的属性。
|语法|setOrientationProperties(properties)|
|---|---|
|参数|Properties：一个 JavaScript 对象，包含 `allowOrientationChange` 和 `forceOrientation` 值|
|返回值|无|
|相关事件|无|

### 6.4 getCurrentAppOrientation
广告调用 `getCurrentAppOrientation` 向 host 查询 app 当前方向。

无论方向是否被锁定，host 都会返回当前 app 的方向，portrait 或 landscape。如果锁定了，`setOrientationProperties` 中的 `forceOrientation` 选项就无效了。
|语法|getCurrentAppOrientation()|
|---|---|
|参数|无|
|返回值|JavaScript 对象 {orientation, locked} 代表：<br/>**orientation:** 可以为 portrait 或 landscape<br/>**locked:** 布尔值，代表当前位置方向是否被锁定，如果值为 true，则代表 `setOrientationProperties` 中的 `forceOrientation` 无效。|
|相关事件|无|

### 6.5 getCurrentPosition
广告调用 `getCurrentPosition` 向 host 查询广告容器当前位置。

host 返回广告容器当前位置及密度无关像素尺寸。

|语法|getCurrentPosition()|
|---|---|
|参数|无|
|返回值|JavaScript 对象：{x, y, width, height} 代表：<br/> x 为相对左边偏移密度无关像素值，最大值定义在 `getMaxSize()` 中<br/> y 为相对上边偏移密度无关像素值，最大值定义在 `getMaxSize()` 中<br/> width 为当前容器的宽度的密度无关像素值<br/> height 为当前容器高度的密度无关像素值|
|相关事件|无|

### 6.6 getDefaultPosition
广告调用 `getDefaultPosition` 向 host 查询广告默认容器的位置与尺寸，位置与尺寸单位为密度无关像素。

host 返回广告容器的默认位置与尺寸，此值与调用者 view 所处的状态无关。

|语法|getDefaultPosition()|
|---|---|
|参数|无|
|返回值|JavaScript 对象：{x,y,width,height} 代表：<br/> x 为相对矩形左边偏移的距离，单位为密度无相像素，此矩形定义了 `getMaxSize()` 方法<br/> y 为相对矩形上边偏移的距离，单位为密度无相像素，此矩形定义了 `getMaxSize()` 方法<br/> width 当前容器的宽度，单位为密度无相像素<br/> height 为当前容器的高度，单位为密度无关像素。|
|相关事件|无|

### 6.7 getState
广告可能随时使用 `getState` 向 host 查询广告容器的状态以确保正确地调用请求。

host 返回广告容器状态值，这些值可以描述广告容器是处于默认状态及固定位置，或是处于扩展状态，或是处于重置尺寸状态、更大尺寸状态，或是隐藏状态。

|语法|getState()|
|---|---|
|参数|无|
|返回值|String: "loading", "default", "expanded", "resized" or "hidden"|
|相关事件|stateChange|

### 6.8 get/set expandProperties
广告使用 `getExpandProperties` 向 host 查询当前扩展设置，使用 `setExpandProperties` 设置扩展的宽高以及可以指定自定义关闭指示器的使用方式。

调用 `expand()` 之前，广告需要使用 `setExpandProperties` 指定想要扩展的宽高。广告可能会指定一个自定义的关闭指示图形来代替 host 的默认关闭指示器。host 调用 `expand()` 方法之后，会忽略所有 expand 属性的设置

下面是一个 `expandProperties` 对象示例代码：
```
expandProperties object = {
    "width":interger,
    "height":interger,
    "useCustomClose":boolean,
    "isModal":boolean(只读)
}
```
|属性|描述|
|---|---|
|width|integer, 创意的宽度，默认与全屏等宽|
|height|integer, 创意的高度，默认与全屏等高。<br/>注意在设置 expand 属性之前获取它，宽和高的值将会反映屏幕的实际值。这将允许想要使用应用或设备值的广告设计者去做必要的调用。|
|useCustomClose|已过时，MRIAD 3.0 的 host 会忽略此请求<br/><br/> boolean, true 代表容器将停止使用默认关闭图形并在广告创意上放一个自定义的关闭指示器；默认为 false 代表容器使用默认闭关图形。此属性与 5.6 节提到的 `useCustomClose` 方法完全一样，它给可扩展广告的创造者提供了方便。|
|isModal|boolean, true 代表容器对于扩展的广告是模态的，在扩展时，其它操作都是停止的；false 代表容器对于扩展的广告不是模态的，扩展时，其它操作可能会继续进行。对于广告来说此属性是只读的，不可设置的。|

当广告调用 `getExpandProperties` 时，host 返回完整的 `expandProperties` 对象。
|语法|getExpandProperties()|
|---|---|
|参数|无|
|返回值|JavaScript 对象：包含广告所有的 expand 属性，如上表所述。|
|相关事件|无|

广告使用 `setExpandProperties` 来先设置 `expandProperties` 对象，然后使用 `expand()` 方法开始广告扩展。

|语法|setExpandProperties(properties)|
|---|---|
|参数|JavaScript 对象：{width, height, useCustomClose} 包含可扩展广告的宽和高。|
|返回值|无|
|相关事件|无|

### 6.9 getMaxSize
广告调用 `getMaxSize` 向 host 查询广告可能重置的最大尺寸（密度无关像素）。

如果应运行在全屏设备上（覆盖状态栏），host 返回全屏维度。如果应用运行在比全屏小的设备上，此屏通用会为状态栏或其它应用之外的元素留有空间，host 将会返回包含此应用的视图的尺寸。

|语法|getMaxSize()|
|---|---|
|参数|无|
|返回值|JavaScript 对象：{width, height}，包含 webview 的最大宽高，此宽高为 webview 可以重置的最大的宽高。|
|相关事件|无|

### 6.10 getScreenSize
广告调用 `getScreenSize` 向 host 查询设备屏幕的尺寸，尤其是在扩展广告之前（如果是重置尺寸使用 `getMaxSize`）。host 返回当前设备屏幕的宽度和高度，单位为密度无关像素。

设备方向改变时，此大小也会变化。比如，一个 640x960 的竖屏旋转后变为 960x640 的横屏。

返回整个屏幕的尺寸，包含为状态或系统栏保留的区域和其它应用控制的区域。广告必须使用 6.9 节介绍的 `getMaxSize()` 查找应用有多少可用于展示广告的区域。

|语法|getScreenSize()|
|---|---|
|参数|无|
|返回值|JavaScript 对象：{width, height} 包含基于设备方向的设备屏幕的宽度和和高度，单位为密度无关像素|
|相关事件|无|

### 6.11 get/set resizeProperties
在广告执行重置尺寸之前，它可以调用 `getResizeProperties` 向 host 查询广告容器为当前重置尺寸操作的相关设置。然后广告可以调用 `setResizeProperties` 去更新 `resizeProperties` 对象。当广告调用 `resize()` 时，host 使用此对象中的属性集来调用广告容器。

如果展示的是 `resizeProperties` 对象的一种示例：
```
resizeProperties object = {
    "width": integer,
    "height": integer,
    "offsetX": integer,
    "offsetY": integer,
    "customClosePosition": string,
    "allowOffscreen": boolean
}
```
`resizeProperties` 对象中的属性含意如下：
|属性|描述|
|---|---|
|width*|integer: 宽度，单位为密度无关像素，代表广告容器必须重置的宽度|
|height*|intger: 高度，单位为密度无关像素，代表广告容器必须重围的高度|
|offsetX*|integer: 水平方向上，当前左上角位置相对目标广告容器左上角位置的偏差。正整数为向右移动，负整数为向左移动。|
|offsetY*|integer: 垂直方向上，当前左上角位置相对目标广告容器左上角位置的偏差。正整数代表向下移动，负整数代表向上移动|
|customClosePosition|在 MRAID 3.0 中已过时。host 总会将关闭指示器放到右上角|
|allowOffscreen|boolean: 指明重置大小的广告容器是否一定被允许显示部分或完全不显示。<br/>**true:** 允许超出屏幕；尽管会导致广告位置会在屏幕外，但 host 必须避免复位广告容器。**false:** 禁止超出屏幕，（当超出屏幕时）host 会尝试复位广告容器，之后广告会在 maxSize 属性指定的区域内展示。|
*：代表必选项。如果广告调用 `resize()` 之前没有使用 `setResizeProperties` 设置值的话，host 会保持原始设置并发送一个错误（error）。

**当重置尺寸导致超出屏幕时**

当 `allowOffscreen` 属性设置为 'false' 时，host 会尝试在 `maxSize` 对象定义的尺度下复位重置尺寸的创意。例如，如果广告放到了屏幕的上方，之后广告向上扩展 50 像素，在执行重置尺寸之前，host 会将广告向下移动 50 像素。

相同的情况下，当 `allowOffscreen` 设置为 'true' 时，重置广告的上边会超出屏幕外 50 像素。

`allowOffscreen` 属性不能解决所有的放置问题。比如，一个广告在成功在横屏方向重置后，接着为（展示）更大的广告改变屏幕方向，此时，设置 `allowOffscreen` 为 'false' 是无用的。

可重置尺寸的广告在正式投放到应用展示之前一定要测试其质量。如果 host 无法正确执行广告传回的重置数值，host 会发送一个错误。

**检查并设置重置尺寸属性**

广告使用 `getResizeProperties` 向 host 查询当前重置属性值。host 返回 `resizeProperties` 对象。

|语法|getResizeProperties()|
|---|---|
|参数|无|
|返回值|JavaScript 对象：{width, height} 包含重置尺寸的属性|
|相关事件|无|

广告使用 `setResizeProperties` 来改变 `resizeProperties` 对象中的值。host 使用广告提供的值为替换 `resizeProperties` 对象的值。

|语法|setResizeProperties(properties)|
|---|---|
|参数|JavaScript 对象：包含重置广告的宽度、高度、关闭位置、偏移方向（单位均为密度无关像素）以及广告是否可以超出屏幕。|
|返回值|无|
|相关事件|无|

### 6.12 getLocation

当广告激活时知道设备的定位信息可能会增强广告创意，但是并发所有应用都支持获取定位详情。MRAID 3.0 的 `supports` 功能（详情看 6.1 节） 标记是否支持定位（location）。

如果支持，定位信息会在 location 对象相应属性中，相应属性如下：
|属性|描述|
|---|---|
|lat|设备地理位置的纬度坐标|
|lon|设备地理位置的经度坐标|
|type|定位数据资源；当传递经纬度时推荐<br/>1: GPS/定位服务<br/>2: IP 地址<br/>3: 用户提供信息（比如，注册数据）|
|accuracy|估算定位，精度为米；当指定经纬度并可以从设备定位服务中导出时推荐使用（比如，type = 1）。注意这里的精度指的是设备获取的精度。查阅特定操作系统（比如，Android, iOS）文档来了解详细信息。|
|lastfix|自从获取有效定位信息后的秒数。注意在多次获取定位信息中设备可能缓存定位信息。理想时，此值必须为真实获取定位信息后的秒数。|
|ipservice|服务或提供者，如果可用的使用的话，它可以从 IP 地址来确定定位信息（比如，type = 2）|

如果支持的话，广告可以使用 `getLocation` 向 host 查询设备定位信息。host 会返回定位对象，此对象携带设备定位数据和来源。

|语法|getLocation()|
|---|---|
|参数|无|
|返回值|JavaScript 对象：包含上表描述的定位信息。|
|相关事件|无|

一定 **不要** 使用 HTML5 API 中的定位内容替换上面说的内容。

当经纬度属性不可用或根据用户要求不允许使用时，getLocation() 方法必须设法传递 "-1" 错误信息。

## 7 事件
事件用来上报行为，此行为表示 webview 状态发生改变或确认 webview 状态发生改变。host 将事件发送给 activity 继而给广告。广告必须创建特定事件监听器来监听事件。

### 7.1 错误（error）
当 host 无法执行广告调用的方法时，host 发送一个错误，表示方法调用失败。广告必须注册监听器才能接收错误事件。可以为不同类型的错误设置监听器，以便广告可以响应所需要的事件。

一个 `error`  事件提供两个参数：一个是错误信息，一个是未遂行为在什么时间发生的。这两个参数都是可选的并且错误信息项经常在创意调试阶段使用。

以下任何 MRAID 行为都可能会发生错误：

* addEventListener
* createCalendarEvent
* close
* expand
* getCurrentPosition
* getDefaultPosition
* getExpandProperties
* getLocation
* getMaxSize
* getPlacementType
* getResizeProperties
* getScreenSize
* getState
* getVersion
* isViewable
* open
* playVideo
* removeEventListener
* resize
* setExpandProperties
* setResizeProperties
* storePicture
* supports
* useCustomClose

尽管广告可以为多种上述中提到的行为注册错误监听，但是一般这三个方法最有可能出现错误： `resize()`, `storePhoto()` 和 `createCalendarEvent()`。广告必须为这些错误注册监听器并且可以正确响应这些错误。

因为 host 可以同步或异步处理错误，广告开发者必须考虑当错误发生时用哪种方式处理。

|语法|"error" funtion(meesage, action)|
|---|---|
|参数|message: String, 发生的错误的描述信息<br/>action: String，错误发生时，未遂的 MRAID 行为的名称|
|被触发|任何可以发生错误的事|

### 7.2 ready
广告加载之前，host 必须确保 MRAID 库可用，如此广告才能在 host 发送事件上调用或注册监听器。至少，广告容器必须尽早支持 `getState()` 和 `addEventListener()` 方法。没有这两个函数，广告不能为 host 的 ready 事件注册监听器。

理想情况下，ready 事件只在所有 MRAID 方法在容器中都被支持并且 host 能从广告接收 任何 MRAID 的调用之后再发送。

广告必须等到 host 发送 ready 事件后，然后在执行任何富媒体操作之前都检查容器是否准备就绪。在此种情况，host 在广告注册监听之前就已经发送 ready 事件，广告必须使用 `getState()` 配合 ready 事件，以下是示例：
```
function showMyAd() {
    ...
}

if (mraid.getState() === 'loading') {
    mraid.addEventListener('ready', showMyAd);
} else {
    showMyAd();
}
```

当广告容器加载好，初始化完并可以接收来自广告的请求时，host 发送 `ready` 事件。此时代表广告可以使用 MRAID JavaScript 库了。

|语法| "ready" |
|---|---|
|参数|无|
|被触发|容器完全加载好，初始化完且准备好接收任何来自广告的调用|

### 7.3 sizeChange

无论何时广告容器为响应转换方向，广告重围尺寸请求或广告扩展请求而改变尺度时，host 都会发送 `sizeChange` 事件。事件包含新的宽度和高度，单位为密度无关像素。
|语法|"sizeChange" function(width, height)|
|---|---|
|参数|width: 数字，view 的宽度<br/>height: 数字，view 的高度|
|被触发|由于重围尺寸，扩展，关闭，转换或应用注册一个 "size" 事件监听器导致广告容器宽或高发发生变化时触发|

### 7.4 staeChange
无论何时，广告容器状态为响应来电或环境改变而发生改变时，host 都会发送 `stateChange` 事件。

广告容器可能存在的状态：

* loading
* default
* expanded
* resized
* hidden

广告容器状态可能会由以下原因而改变：host 初始化（加载）；用户发起关闭或其它应用交互，如 window 重置、旋转；广告调用 `expand()`, `resize()` 或 `close()`。查看 4.2.1 小节了解广告容器状态以及它们如何改变的。

广告可以使用 `getState` （6.6 节有介绍）向 host 查询广告容器的当前状态。

|语法|"stateChange" function(state)|
|---|---|
|参数|state: String 代表 "loading", "default", "expanded", "resized" 或 "hidden"|
|被触发|`expand()`, `close()` 或应用改变 webview 的状态|


### 7.5 exposureChange
`exposureChange` 事件是 MRAID 3.0 加入的，为了能更准确反映可见性。

two-part 广告不支持 exposureChange 事件。

广告注册 `exposureChange` 事件监听器时，host 此广告的向所有监听器异步发送初始曝光状态事件。初始事件之后，host 会在任何曝光区域发生变化时发送 `exposureChange` 事件。 `exposureChange` 值 0.0（0）代表当前广告容器不在 view 中或移到后台了。广告容器或其它父 view 滚动，移动或遮挡时，会发生 `exposureChange` 事件。由于应用或 UI 变化导致广告可见性发生变化时，host 也会上报 `exposureChange` 事件。

即使可见性百分比没有变化，只要广告曝光区域发生变化时，host 就必须发送 `exposureChange` 事件。比如，重置尺寸，展示或隐藏插屏广告，banner 扩展到全屏，banner 广告被添加到 window 上。这此曝光变化提升广告重新计算基于更新广告尺寸的可见性测量。

host 可以通过原生事件句柄(handling)或在原生层轮询的方式实现 `exposureChange` 事件。

|语法|"exposureChange" function(exposedPercentage, visibleRectangle, occlusionRectangles)|
|---|---|
|参数|exposedPercentage: 广告展示在屏幕上的百分比，一个 0.0 到 100.0 浮点型数字，0.0 代表不可见<br/>visibleRenctangle: 广告容器的可见部分，为 null 时不可见。它包含 {x, y, width, height}，x 和 y 代表相对于广告容器左上角的可见部分的左上角位置，width 和 height 代表可见区域的尺寸。如果可见区域不是矩形，这个参数代表可见区域的最大矩形区，`occlusionRectangle` 参数描述此范围的不可见区域<br/><br/>`occlusionRectangles`: 一个矩形数组，包含 `visibleRectangle` 中不可见的部分，如果为 null 代表未使用遮挡检查。数组中每个元素包含 {x, y, width, height}，x 和 y 代表相对于当前存在的广告容器左上角的不可见区域的左上角位置，width 和 height 代表不可见区域的尺寸。这些矩形不能重叠，它们必须按面积从大到小排列。在常见场景中，可见区域为矩形的，此参数为 null。如果实现可以检测非矩形曝光，那么此参数会被设置。|
|被触发|webview 的曝光区域发生变化。看下面如果触发 `exposureChange` 事件了解详情|

示例方法
```
{
    "exposedPercentage": 78,
    "viewport": {
        "width": 375,
        "height": 667
    },
    "visibleRectangle":{
        "x": 27.5,
        "y": 65,
        "width": 300,
        "height": 50,
        "occlusionRectangle": {
            "x": 27.5,
            "y": 65,
            "width": 50,
            "height": 50
        }
    
```

**触发 exposureChange 事件**

在这些属性都为 true 时，广告认为被曝光：

* 设置屏幕开起，未处理睡眠或锁屏状态
* 应用在前台显示。前台应用是设置屏幕上活动的应用，用户可以交互。在 Android 上，Activity 在 `Activity.onResume()` 和 `Activity.onPause()` 之间处于前台状态；在 iOS 应用在 `UIApplicationDidBecomActiveNotification` 和 `UIApplicationWillResingActiveNotification` 事件之间处于前台状态。
* 广告 view 至少有 1 像素在屏幕上，考虑到遮挡和被应用图层滚动。（此计算不需要考虑图层内 z 轴遮挡。）注意此与 MRAID 2.0 中指定的不同，2.0 中没有为可见广告设置任何像素阈值。
* 没有模态接口阻塞广告。与其它应用内置模态屏幕一样，模态接口可能包含 MRAID 功能比如使用 `mraid.open()` 打开应用内容浏览器或应用市场，使用 `mraid.playVideo()` 播放视频，使用 `mraid.storePicture()` 访问相册以及使用 `mraid.createCalendarEvent()` 访问日历。

如果这些情况有一个没有包括的，广告移出 view 且不可见。

host 上报曝光变化，如下所示
|情况|曝光变化|
|---|---|
|广告容器被曝光|exposureChange 事件携带一个非零百分比参数|
|广告容器移出 view 或在其它 activity view 下面|exposureChange 事件携带一个零百分比参数|
|广告容器从曝光变到移出 view|host 发送携带零（0）百分比参数的 exposureChange事件|
|广告容器从移出 view 到曝光|host 发送携带非零百分比参数的 exposureChange 事件 |
|广告容器状态或大小改变，但是曝光百分比不有变化|host 发送一个 exposureChange 事件，携带与上次上报一样的参数。|

### 7.6 audioVolumeChange

host 使用 `audioVolumeChange` 上报音频声音百分比变化。音频音量可能许多不同控制因素控制，可能是应用自动调节，应用设定音量大小，设备静音或其它因素。如果应用不能播放音频，或它不能检测音量大小，此事件上报 null 而非一个百分比。

|语法|"audioVolumeChange" function(volumePercentage)|
|---|---|
|参数|volumePercentage: 最大音量的百分比，是 0.0 到 100.0 之间的浮点型数字，0.0 代表不允许出声，null 代表无法检测到音量|
|被触发|广告音量变化时触发|

当广告注册 `audioVolumeChange` 事件后，初始化完音频音量后，host 向广告的所有监听器异步发送此事件。初始化事件之后，当音量发生变化时，host 上报 `audioVolumeChange` 事件。

音量变化的典型实例是当用户改变设备音量或静音设备。当应用或 UI 转变导致音量焦点发生变化时，host 也要上报音量变化。多个音频变化事件可能使用同一个百分比值上报。

host 可能通过原生事件句柄或轮询原生播放器的方式检测音量。如果原生播放器使用轮询方式，当音量变化时，它发送 `audioVolumeChange` 事件的间隔不能多于一秒。

音量变化事件不受广告的控制。广告可以对声音进行一定控制，如静音。直接发生在广告上的音量控制不会反映到 `audioVolumeChange` 事件。

在某些情况下，当广告没有播放音频时，host 可能无权使用音量。另外，host 可能不能得知音频是静音或没有播放，比如，当静音键打开但耳麦还在工作。在此情况下，在广告播放之前，host 上报一个 null `volume_Percentage` 事件，之后再上报另一个携带音量数字的 `audioVolumeChange` 事件，在播放开始之后 host 就可以监测音量。

**使用示例**

当 host 提供除 MRAID 中的其它特性的 `audioVolumeChange` 事件时，广告将能检测可听性。广告可能使用如下伪代码：

1. 注册 `audioVolumeChange` 事件监听器。比如，
```
mraid.addEventListener('audioVolumeChange', handleVolumeChange);
```
2. `audioVolumeChange` 处理器可能使用一个阈值对比音量，当到达阈值时记录时间：
```
function handleVolumeChange(volume_percentage) {
    if (volumePercentage && volumePercentage >= 10.0) {
        // 记录听到音量的时间 。。。
    }
}
```
3. 当广告不再需要监听声音时，移除 `audioVolumeChange` 监听器。比如：
```
mraid.removeEventListener('audioVolumeChnage', handleVolumeChange);
```

**实现注意项**

JavaScript 事件驱动 API 覆盖一个状态 API（如，`getAudioVolume()`），原因如下：

* 在 JavaScript 中调用 `getAutioVlume()` 状态立即从原生层获取数值可能阻塞 webview 执行线程，影响原生性能。
* 为了防止调用 `getAudioVolume()` 状态阻塞 JavaScript 线程，原生层应用检查音量（并缓存结果）无论创意是否使用此值。通过使用事件接口，只有对需要音量控制的广告才会承担检测声音带来的损耗。
* 此事件可避免在 JavaScript 层轮询
* 在 `audioVolumeChange` 事件之外添加 `getAudioVolume()` 状态 API 是多余的。创意的 JavaScript 脚本可以基于它的此事件的句柄轻易实现这种方法

**Android**

在 Android 上，`android.media.AudioManager` 类提供了访问设备音量的方法。此值必须在展示广告的 webview 获取焦点时进行控制。下面伪代码将返回设备音量。

```
Double getAudioVolumePercentage() {
    if (!adHasAudioFocus()) return null;
    AudioManager audioManager = (AudioManager) getContext().getSystemService(Context.AUDIO_SERVICE);
    int currentVolume = audioManager.getStreamVolume(AudioManager.STREAM_MUSIC);
    int maxVolume = audioManager.getStreamMaxVlume(AudioManager.STREAM_MUSIC);
    return new Doule((100.0 * currentVolume) / maxVolume);
}
```
伪代码中的 `adHasAudioFocus()` 方法可以调用 MRAID 实现的检测音频聚焦和 activity 处于后台的处理句柄。

**iOS**
在 iOS 上，`AVAudioSession` 类提供了访问设备音量的方法。然而，此音量只有当应用音量聚焦时才有效。下面伪代码将返回设置音量。
```
-(NSNumber*)getAudioVolumePercentage {
    if (![self adHasAudioFocus]) return nil;

    return @(100.0 * [AVAudioSession sharedInstance].outputVolume);
}
```
伪代码中的 `adHasAudioFocus` 方法将调用 MRAID 实现检测音频会话（session）与应用后台的处理句柄。

### 7.7 viewableChange(过时)

`viewableChange` 事件在 MRAID 3.0 中过时，并有可能在未来版本的 MRAID 移除。然而，在 MRAID 3.0 的 host 实现中必须支持 `viewableChange` 事件以维持向后兼容性。当广告容器从在屏幕上移到不在屏幕上或不在屏幕上移到在屏幕上时，host 发送 `viewableChange` 事件。任何情况下，容器都有可能被卸载出屏幕，广告在执行任何造型时，都应该注册并通过 `viewableChange` 检测它的可见状态。
|语法|"viewableChange" function(boolean)|
|---|---|
|参数|true: 容器在屏幕上，用户可见；false: 容器不在屏幕上，不可见|
|被触发|应用 view 容器改变时触发|

## 使用设备特性

MRAID 可以提供移动设备特性，如定位，方向，存储图片，日历和视频播放器等。下面章节将介绍使用这些特性开发广告的指南和示例。

### 8.1 设备方向

广告不应使用 window.orientation 来检测方向，也不应用使用 orientationChange 事件来检测方向变化。在新的系统版本上，host 不能控制 webview 提供的方向更新信息，有时 webview 提供的方向信息是不准确的。

广告必须使用 mraid.getCurrentPosition() 而非 windown.orientation 来检测方向，并且对比宽高值确定横竖屏布局；在布局发生变化时，广告必须使用 mraid.getCurrentPosition() 在 sizeChange 事件中上报的宽高来验证广告的方向，而非使用 orientationChange 事件。

当广告没有控制尺寸时，应当使用 MRAID 3.0 提供的 getOrientationProperties 特性来检测方向。6.3 节有 orientationProperites 对象的讲解。

### 8.2 存储图片
富媒体广告设计者可能希望向广告运行的设备的相册中保存图片。这样可能对很多的特性都有帮助，包括保存之后要使用的优惠券。

**storePicture** 方法

storePicture 方法将会向设备相册中存入一张图片。此图片可能来自本地或网络。为保证用户知道有张图片添加到相册中，每存一张图片 MRAID 都需要 SDK / 容器使用系统级的句柄（hanmdler）来向用户展示一个模态对话窗，用户使用对话窗确认或取消保存。如果设备没有原生 “添加图片” 的确认句柄，SDK 必须认定这些设备为不支持 storePicture 功能。

此方法将存储指定 URI 代表的图片或其它媒体类型。

符合 MRAID 标准的容器支持通过 HTTP 重定向（为了统计）的方式保存图片，然而容器可能不支持元重定向。

由于用户取消或其它任何原因导致添加图片失败了，都应该反馈一个错误。

storePicture(URI)

参数：
 * URI String: 图片或其它媒体资源的 URI
 * 相关事件：无

### 8.3 日历事件
createCalendarEvent 方法会打开设备相关界面创建一个日历事件。当日历界面展示时，广告应被暂停。符合 MRAID 标准的容器必须调用设备原生的 “创建日历事件” 面板，添入广告提供的数据，以保证创建日历事件是用户发起并授权的。如果设备不支持 “创建日历事件” 面板，SDK 必须认定此设备不支持添加日历事件。

日历事件数据必须按 W3C 日历指定的 JavaScript 对象的格式传递。看附录。

如果用户取消或创建日历事件失败，必须要报一个错误。

createCalendarEvent(parameters)

参数:
 * parameters: JavaScript Object{...}：此对象包含日历实体的参数，根据 W3C 指定的日历实体写就的。看附录。

返回值：
 * 无

相关事件：
 * 无

例如，以下将添加一个日历事件代表：玛雅启示 / 起始于东部时间 2012 年 12 月 21 日午夜，终止于东部时间 2013 年 12 月 22 日午夜发生在每个角落的时世界末日。

```
createCalendarEvent({description: "Mayan Apocalypse/End of World", location: "everywhere", start: "2012-12-21T00:00-05:00", end: "2012-12-22T00:00-05:00"})
```


### 8.4 视频
设备中的视频可以使用内联播放（webview 或系统浏览器播放），也可以使用原生播放器播放。对许多广告来说，内联播放更可取：它破坏观看者体验较少，并且在 webview 中播放支持 HTML5 上报创意播放进度指标。当视频在原生播放器中播放时，这些指标一般很难获取，或不能获取。

广告设计者必须牢记设备/系统限制可能阻止内联播放（这些情况时发生在 Android 2.x 及更早版本）。

然后，如果可以的话，支持 MRAID 的容器必须支持内联播放，允许广告设计者指定使用内联或分离的播放器播放。广告设计者可用 "supports("inlineVideo")" 方法判断运行的设备是否支持创意使用内联方式播放视频。

为了可以使用内联播放并自动播放视频，符合 MRAID 标准的 SDK 必须向 webivcew 插入任何必须的依赖设备操作系统的标签。

对于 iOS 设备，如下标签必需设置：

 * webView.mediaPlaybackRequiresUserAction = NO;
 * webView.allowsInlineMediaPlayback = YES;

对于 Android（3.0 及以上版本） 来说， SDK 必需调用硬件加速功能。将硬件加速功能添加到 WindowManager:

 * getWindow().setFlags(WindowManager.LayoutParams.FLAG_HARDWARE_ACCELERATED, WindowManager.FLAG_HARDWARE_ACCELERATED);
 
对于 Android 2.x 及之前版本，不能使用内联方式播放视频。原生播放器会被 playVideo 方法调起。

**playVideo** 方法
使用此方法在设备的原生外置播放器播放视频。注意，此方法纯粹为存在外置播放器的系统提供方便，不支持分离的基于 SDK 的视频播放器。使用 HTML5 video 标签内联播放视频（设备支持此特性）

playVideo(URI)

参数：
 * URI String，视频或视频流的 URI

返回值：
 * 无

## 9 VPAID 事件与方法

MRAID 处理富媒体广告与应用或 webview 的交互。当广告包含视频组件时，host 可以指示如何处理广告中的视频播放事件，追踪视频进度和用户体验。

IAB 的数字流视频播放广告接口标准（VPAID，Video Player-Ad Interface Definition）是一个在广告与视频播放器之前的一个接口。MRAID 2.0 附录中介绍如何初始化 VPAID 并上报某些事件。MRAID 3.0 将此内容及一些其它可选规范集成到 MRAOID 3.0 上了，支持 MRAID 3.0 的 host 将同时支持开发广告的 MRAID 和 VPAID 标准。

VPAID 与 MRAID 集成标准不处理广告与播放器之间的交互。此集成标准使 MRAID host 可以监听 VPAID 事件。

### 9.1 MRAID 广告中的 VPAID  交互

下图解释了 VPAID 视频在 MRAID 创意中的交互进程。
![vpaid-in-mraid-progress](../resource/vpaid-in-mraid-progress.png)

#### 9.1.1 在 MRAID 上下文中初始化 VPAID

MRAID/VPAID 广告使用第 3 节讲的机制进行初始化。一旦容器返回 'ready' 事件或设置容器状态为 'default' 时，广告就可以检查容器是否支持 vpaid 标准了。

可以使用 mraid.supports() 方法以 'vpaid' 作值来检查是否支持 VPAID，格式如下

```
mraid.supports("vpaid")
```

MRAID 3.0 中支持 VPAID 是可选的，但是支持 MRAID 3.0 的应用需要返回一个布尔值：true 代表支持 VPAID，false 代表不支持。

#### 9.1.2 发送与接收 VPAID 事件

只要广告验证 host 支持 VPAID，广告必须调用 `mraid.initVpaid(vpaidObject)` 将 VPAID 对象传送给容器。之后 host 会订阅 VPAID 事件并且调用 `vpaidObject.startAd()` 方法。之后广告可以开始视频创意并发送相关 VPAID 事件。

MRAID 上下文并非支持全部的 VPAID 事件。为避免与 MRAID 指令重复或冲突，只有以下 VPAID 方法与事件会被支持。当 host 支持 VPAID 集成时，VPAID 方法可以使用 vpaidObject 调用（如，`vpaidObject.subscribe()`）。在 MRAID 中使用 VPAID 方法的细节请查看 5.13 节。

|支持 VPAID 方法|不支持 VPAID 方法|
|---|---|
|* subscribe()<br/>* unsubscribe()<br/> * getAdDuration()<br/> * getAdRemainingTime()<br/> * startAd()|* handshakeVersion<br/> * initAd()<br/> * resizeAd()<br/> * stopAd()<br/> * pauseAd()<br/> * resumeAd()<br/> * expandAd()<br/> * collapseAd()<br/> * skipAd|

|支持 VPAID 事件|不支持 VPAID 事件|
|---|---|
|* AdClickThru<br/> * AdError<br/> * AdImpression<br/> * AdPaused<br/> * AdPlaying<br/> * AdVideoComplete<br/> * AdVideoFirstQuartile<br/> * AdVideoMidpoint<br/> * AdVideoStart<br/> * AdVideoThirdQuartile|* AdDurationChange<br/> * AdExpandedChange<br/> * AdInteraction<br/> * AdLinearChange<br/> * AdLoaded<br/> * AdLog<br/> * AdRemainingTimeChange(Deprecated in VPAID 2.0)<br/> * AdSizeChange<br/> * AdSkippableStateChange<br/> * AdSkipped<br/> * AdStopped<br/> * AdUserAccept Invitation<br/> * AdUserClose<br/> * AdUserMinmize<br/> * AdVolumeChange|

任何未在此列出的 VPAID 方法与事件都不会被 MRAID 所支持。列举的这些方法与事件应用辅助 MRAID 创意的任意视频部分的交互。

#### 9.1.4 VPAID AdClickThru 事件
在 VPAID 中，广告创意使用 `AdClickThru` 事件传递点击某一链接（clickthrough）URL，此事件必须能被处理。VPAID 提供三个对数来定义 URL(url)，一个追踪用的 ID，一个布尔值，用来定义播放器或广告创意是否必须打开此 URL(playerHandles)。MRAID 容器必须忽略这些参数并且广告要用 host 中的 `mraid.open()` 方法来处理视频点击某链接事件。

#### 9.1.5 VPAID AdPause, AdPlaying 事件

VPAID 中，`AdPause` 和 `AdPlaying` 事件用来响应播放器命令 `pauseAd()` 和 `resumeAd()`。然后，在 MRAID 上下文中，还没有此命令，所以当 `AdPaused` 和 `AdPlaying` 事件发生时，会被通知给 host。

#### 9.1.6 运行视频自动播放

为支持最佳视频体验，webview 必须支持内联并自动播放视频。广告创意中的视频元素必须做到无用户交互也可以自动内联播放。

iOS 设置参数以支持自动开始的说明已经包含在 MRAID 说明书中了，在提供额外信息来内联播放的下面。容器支持此附件必须向 webview 添加自动开始支持。

如果给定的设备，操作系统或应用不支持自动开始，那么容器必须返回 `mraid.supports("vpaid")` 值为 'false', 如此广告创意可以播放视频内容的无交互版本，允许用户手动或采取其它方法初始化视频播放。

然后要注意，视频广告需要手动播放应该当作一个极端例子，广告商必须确定发布者、网络广告服务商投放到系统版本不支持自动开始时才能采取手动播放。

### 9.2 点击链接行为及可视性

当用户点击广告时，容器使用 MRAID 的 `open()` 方法在应用中打开一个新浏览器容器，如 iOS 中的SFSafariViewController 或 WKWebView 或 Android 中的自定义标签或发送到向用户设备默认浏览器。在新窗口中，容器没有任何方法从广告的 webview 向新浏览器窗品传递信息。同样地，当用户关闭新浏览器窗口返回到起初窗口时，容器也获取不到任何指示。因为新浏览器窗口的盲点，创意可能不知道什么时候暂停动画或视频元素。MRAID 3.0 使用 `exposureChange` 事件来上报曝光度变化。此事件通过 `exposedPercentage` 和 `occlusionRectangle` 参数上报曝光变化。

链接点击事件流程应如下：

1. 请求 URL 来展示（此可以来自主广告或来自某个交互状态），广告调用 `mraid.open()` 方法
2. 容器在内应内打开一个新浏览器窗口或打开设备默认浏览器并触发 `exposureChange()` 方法。新浏览器窗口应用包含一个关闭按钮，可能包含其它控制器，如返回按钮或返回到应用的指示器。
3. 调用 `mraid.open()` 方法，如果创意向 host 注册过，则必须触发 VPAID `AdClickThru` 事件，因此集成层可以使用 VPAID 追踪点击。
4. 如果广告暂停视频，广告必须触发 VPAID `AdPaused` 事件。
5. 当新浏览器窗口关闭或用户从默认浏览器返回到应用时，容器必须发送 `exposureChange()` 来报告新的 `exposedPercentage` 和新的 `occlusionRectangle` 值。创意可以恢复执行。如果视频恢复播放了，必须发送 VPAID `AdPlaying` 事件

### 9.3 展示记数（Counting Impressions）

MRAID 和 VPAID 标准是为了管理广告与移动应用或移动 web 应用之间交互的技术规范。都没提供具体的测量记数的规范指导。但是，它们都说明了如何设计支持记数的测量指南。

当广告是一个使用 VPAID 事件来记展示数的视频广告时，上报必须与最新 IAB 记数数据视频广告展示的说明一致。

如下是一段来自 [Digital In-Stream Video Impression Measurement Guidelines](https://www.iab.com/wp-content/uploads/2015/06/dig_vid_imp_meas_guidelines_finalv2.pdf) 的摘要：

> 一个有效的数据视频广告展示应该只在广告计数器（日志服务器）接收并响应来自客户端的追踪资源的 HTTP 请求时才被记数。此记数必须发生在初始化（视频）流，缓冲完成之后，而非刚链接到数字视频内容时。
>
> 特别指出，测量不应用发生在刚开始缓冲时。测量应该发生在广告刚开始展示在用户浏览器上，用户第一眼看到时。

必须使用 VPAID 中 `AdImpression` 事件来记数流视频广告展示。如果广告是富媒体广告，可以使用 MRAID 中的 `adload` 来记数。在 MRAID 记数时，不需要 VPAID 的 `AdImpression` 事件，广告容器一定不要等着这个事件。

下表展示了 6 种常见场景，包括 MRAID 广告视频组件及可能使用 VPAID 来记数视频展示的说明。

|格式|描述或举例|MRAID placementType|是否报告 VPAID 事件|
|---|---|---|---|
|插页视频，不可跳过（non-dismissible）|比如，在游戏升级时的 pre-app 广告或插页视频广告|interstitial|是|
|可跳过的 pre-roll|比如，一个 iPad 播放视频前且占据屏幕的 3/4 的 640x480 的区域|interstitial|是|
|带结束页的 post-roll|比如，带有可交互结束页的15 秒 post-roll 视频展示完后停在屏幕上直到用户完毕广告|interstitial|是|
|点击后可播放视频的简单 banner|比如，在新闻应用上的 300x250 的 banner 广告|inline|否|
|点击后会扩展并播放视频的 banner|比如，点击 320x50 的 banner 后，调用 mraid.expand() 方法并在一个 `<video>` 元素中播放视频。视频结束后，广告结束。对于用户来说，这几乎与使用在 banner 中使用 mraid.playVideo() 有相同的体验；基础的 tap-to-video|inline|否|
|不带点击扩展功能并提供网格视频的 banner|与上面的相似，但是当扩展时，取代创意展示立即播放视频的是展示一个网格，格子里有图片，每个图片代表一个视频。用户在关闭之前可以选择播放多次|inline|否|

## 10 术语
如下部分适用于整个 MRAID 文档。

**广告 View 或容器（Ad View/Container）：** 一个指定的用来展示创意的区域。发布者既可以在内容内放置广告容器（内联位置）也可以之内容之上（插屏位置）展示广告创意。容器在屏幕上提供一片区域，MRAID 控制器和基于 web 的 view 来展示广告。广告容器通常（不是必须）是由 SDK 提供的。一个应用可能包含多个来自同一个 SDK 的多个广告容器。

**关闭事件区域：** 关闭事件区域是广告创意中的可点击区域，点击后使广告返回到默认状态（在可扩展/可重置大小广告中）或在屏幕上移除（在插屏广告中）。

**关闭指示器：** 关闭指示器是个可见东西，位于关闭事件区域。

**控制器：** 广告设计者通过 JavaScript 代码访问 MRAID 方法和事件。广告创意使用此控制器根据广告容器，应用和设备的交互展示广告。

**密度无关像素：** 整个 MRAID API 所有容器和创意长度值都使用的是密度无关像素。

密度无关像素是从物理屏幕像素抽象出的，用来简化各各设备不同屏幕密度的应用和内容开发。

比如，使用密度无关像素表示视新款 iPhone 手机的网膜屏和旧款 iPhone 手机将返回相同的测量值，即使它们的物理像素值不同。1 密度无关像素大概相当于 1/160 英寸（在设备上，1 英寸设备像素大概为 160 DPI）。

在 iOS 上，此值必须转换成 ‘点’；在 Android 上，要转换成 ‘dp’。

注意：1 密度无关像素只有在 viewpost 缩放率为 1.0 时，才能与 1 CSS 像素相匹配。创意进行 CSS 像素与密度无关像素转换必须使用如下表达式：
```
css_pixels * viewport_scale = density_independent_pixels
```

**内联广告：** 一个伴随其它内容一同展示在屏幕上的广告，比如，在 web 页或应用内的 banner。

**插屏广告：** 一个全屏模态广告，在内容上面展示。广告必须可以关闭，关闭后返回应用内容。此广告可以展示在游戏等级之间，或在视频裁切或其它动态内容前后展示（广告包含内联 MRAID 广告，像许多杂志应用那样在页面之间的广告）。

**物理像素：** 在设备上的真实的像素。比如，一个 iPhone 视网膜屏有 960x640 个物理像素。MRAID API 长度值是以密度无关像素计算的，**不是**根据物理像素计算。

**SDK：** Software Development Kit. 一块集成在发布者应用中的可重用的代码（库），用来提供广告/广告容器。对 SDK 自身而言是个不可见的组件。

**Webview：** 基于 HTML 的指示器（viewer），可以展示广告创意。webivew 用来渲染包含 HTML 及 Javascript-enabled 的广告。

![screen-container-webview-adcreative](../resource/screen-container-webview-adcreative.png)

## 11 附录：W3C CalendarEvent 接口

来自：W3C Calendar API，4.3 及 4.4 节。

W3C 工作草案 2011 年 4 月 19 日

这个版本：
```
http://www.w3.org/TR/2011/WD-calendar-api-20110419/
```

最新发布版本：
```
http://www.w3.org/TR/calendar-api/
```

最新编辑器草案：
```
http://dev.w3.org/2009/dap/calendar/
```

编辑器：

[Richard Tibbett](http://richt.me/), [Opera Software ASA](http://www.opera.com/), Suresh Chitturi, [Research in Motion(RIM)](http://www.rim.com/)

版权 @ 2011 [W3C](http://www.w3.org/)([MIT](http://www.csail.mit.edu/),[ERCIM](http://www.rim.com/), [Keio](http://www.keio.ac.jp/))，保留所有权。W3c [liability](http://www.w3.org/Consortium/Legal/2002/ipr-notice-20021231#Legal_Disclaimer), [trademak](http://www.w3.org/Consortium/Legal/2002/ipr-notice-20021231#W3C_Trademarks) 和 [document use](http://www.w3.org/Consortium/Legal/2002/ipr-notice-20021231#W3C_Trademarks) 应用规则。
----
4.3 CalendarEvent 接口

此 CalendarEvent 接口捕获一个日历事件对象。

当前使用 DOMString 数据和时间来代表时区事件[被认为是不够的](http://www.w3.org/2009/dap/track/issues/81)。作用于位置的组限制，寻找 TZData 对象的开发将在这里。
```
[NoInterfaceObject]

interface CalendarEvent {
    readonly attribute DOMString        id;
    attribute DOMString     description;
    attribute DOMString?        location;
    attribute DOMString?        summary;
    attribute DOMString     start;
    attribute DOMString?        end;
    attribute DOMString?        status;
    attribute DOMString?        transparency;
    attribute CalendarRepeatRule?       recurrence;
    attribute DOMString?        reminder;
}
```

4.3.1 属性

**description** DOMString 类型

描述一个事件。
```
{description: "Meeting with Joe's team"}
```
无它（没有其它参数）

**end** DOMString 类型，可为空
事件结束日期，是一个有效数据或日间串。
```
{end:'2011-03-24T 10:00:00-08:00'} // 事件结束于 2011 年 5 月 24 日下午 6 点（UTC）
```
无它

**id** DOMString 类型，只读

一个全局唯一标识给定 CalendarEvent 对象的标识。每个引用日历的 CalendarEvent 都必须有一个非空的 id 值。

当一个日历事件添加到或展示在日历中时，id 必须保证全局唯一。

可以使用 IANA 注册的标识格式作为一种实现。此值也可以是一个非标准的格式。

无它

**location** DOMString 类型，可为空

一个描述事件地点的纯文件。
```
{location: 'Conf call #+44020000001'}
```

无它

**recurrent** CalendarRepeatRule 类型，可为空。

此事件周期或重复规则
```
{recurrence: {frequency: 'daily'}} // Event occurs every day and never expires
{recurrence: {frequency: 'weekly', // Event occurs weekly...
daysInWeek: [2, 4], // ...every Tuesday and Thursday
expires: '2011-06-11T12:00:00-04:00'}} // Event expires on or before June 11,
2011 @ 4pm (UTC)
{recurrence: {frequency: 'weekly', // Event occurs weekly...on every
Wednesday
// (if we say the 'start' attribute is March 24, 2011 @ 2pm
(Wednesday) as
// shown above and no daysInWeek attribute is
provided)
expires: '2011-06-11T11:00:00-05:00'}} // Event expires on or before June 11,
2011 @ 4pm (UTC)
{recurrence: {frequency: 'monthly', // Event occurs monthly...
daysInMonth: [-5], // ...5 days before the end of each month
expires: '2011-06-11T20:00:00+04:00'}} // Event expires on or before June 11,
2011 @ 4pm (UTC)
{recurrence: {frequency: 'monthly', // Event occurs monthly...on the 24th day of
every month
// (if we say the 'start' attribute is March 24, 2011 @ 2pm
as
// shown above and no daysInMonth attribute is
provided)
expires: '2011-06-11T20:00:00+04:00'}} // Event expires on or before June 11,
2011 @ 4pm (UTC)
{recurrence: {frequency: 'yearly', // Event occurs yearly...on the 24th day of
every March
// (if we say the 'start' attribute is March 24, 2011 @ 2pm
as
// shown above and no daysInMonth attribute is
provided)
expires: '2011-06-11T16:00:00+00:00'}} // Event expires on or before June 11,
2011 @ 4pm (UTC)
{recurrence: {frequency: 'yearly', // Event occurs yearly...
daysInMonth: [24], // ...every 24th day...
monthsInYear: [3, 6], // ...in every March and June
expires: '2011-06-11T16:00:00Z'}} // Event expires on or before June 11, 2011 @
4pm (UTC)
{recurrence: {frequency: 'yearly', // Event occurs yearly...
daysInYear: [168], // ...every 168th day of each year
expires: '2011-06-11T21:45:00+05:45'}} // Event expires on or before June 11,
2011 @ 4pm (UTC)
```
无它

**start** DOMString 类型

事件开始日期
```
{start:'2011-03-24T09:00-08:00'}
```
无它

**status** DOMString 类型，可为空

一种指示此事件的用户状态。

此参数有如下几种值：'pending', 'tentative', 'confirmed', 'cancelled'。
```
{status: 'pending'} // 事件正在等待用户动作
```
无它

**summary** DOMString 类型，可为空

事件摘要
```
{summary:'Agenda:\n\n\t* Introductions\n\t* AoB'}
```
无它

**transparency** DOMString 类型，可为空

展示事件设置的状态的一个指示

此参数有如下几种值：'transparent', 'opaque'。
```
{freebusy: 'transparent'} // 标记事件在日历中透明
```
无它