# APPT2
[原文](https://developer.android.com/studio/command-line/aapt2)

AAPT2 全称 Android Asset Packaging Tool 属于构建工具，Android Studio 与 Gradle 插件使用它编译并打包 app 的资源文件。AAPT2 将资源文件解析、索引并编译成二进制格式，并将该二进制包进行 Android 平台的优化处理。

Android Gradle 插件 3.0.0 及以上版本默认支持 AAPT2，开发者无需直接调用它。但是，如果你不喜欢使用Android Studio，而更喜欢使用终端及自己的构建系统的话，可以从命令行使用 AAPT2。开发者可以在命令行调试构建中的错误，这样做后你会发现，AAPT2 是一个独立的工具（在 26.0.2 或更高版本）。

使用 `sdkmanager` 在命令行下载 Android SDK 构建工具：
```
sdkmanager "build-tools;build-tools-version"
```

下载完成后，你可以在 `android_sdk/build-tools/version/` 目录下找到 AAPT2 工具。由于较新版的 Android SDK 构建工具经常发布不够及时，AAPT2 又在 Android SDK 构建工具中，所以你使用的 AAPT2 不一定是最新版。想获取最新版的 AAPT2 的话，可以看 [Download AAPT2 from Google Maven](https://developer.android.com/studio/command-line/aapt2#download_aapt2)

在 Linux 或 Mac 在命令行中使用 `appt2` 命令，Windows 上使用 `aapt2.exe` 来运行 AAPT2 工具。打开增量编译后，AAPT2 支持快速编译资源。快速编译是通过将资源处理分成两步来实现的：

1. [编译](https://developer.android.com/studio/command-line/aapt2#compile): 将资源文件编译成二进制格式
2. [链接](https://developer.android.com/studio/command-line/aapt2#link): 合并所有编译后的文件，使其成为一个包

这个分割可以提升增量构建的性能，比如，如果只有一个文件发生了变化，可以只重新编译这一个文件就可以，不必重新整个工程。

## 从 Google Maven 下载 AAPT2 
通过以下步骤从 Google Maven 仓库中下载最新版本的 AAPT2 工具：
 1. 在[这里](https://dl.google.com/dl/android/maven2/index.html)找到 **com.android.tools.build > aapt2**
 2. 复制最新版本 AAPT2 的名字
 3. 将你复制的名字替换下面 URL 中的 **aapt2-version** 字符串，并根据你操作系统类型保留 [windows | linux | osx] 中的一个（不要保留 '[' 或 ']'）
```
https://dl.google.com/dl/android/maven2/com/android/tools/build/aapt2/aapt2-version/aapt2-aapt2-version-[windows | linux | osx].jar
```
以 Windows 系统为例下载 3.2.0-alpha18-4804415 版本的链接如下：
```
https://dl.google.com/dl/android/maven2/com/android/tools/build/aapt2/3.2.0-alpha18-4804415/aapt2-3.2.0-alpha18-4804415-windows.jar
```
 4. 将替换好的 URL 放入浏览器的导航栏，回车后一会就会下载了
 5. 解压刚下载好的 JAR 文件。这 JAR 文件应有一个 `aapt2` 可执行文件以及该执行文件所需的依赖文件。

## 编译

AAPT2 可以编译各种 [Android 资源类型](https://developer.android.com/guide/topics/resources/available-resources)，比如 drawables 文件和 XML 文件。调用 AAPT2 编译时，需要传入单个待编译的资源文件。AAPT2 将它解析并生成一个以 .flat 做扩展名的二进制中间产物。

可以使用 `--dir` 标识一次性编译一个目录的资源文件，但是这样做会丢失增量编译资源的优势。这是因为，如果传入一整个目录，即使只修改了一个文件，AAPT2 也会重新编译此目录中的所有文件。

根据传入文件的类型，编译出的文件类型可能是不相同的。原因如下表所示：

|Input|Output|
|---|---|
|XML 资源文件，如在 res/values/ 目录下的String, Style 文件|使用 *.asrc.flat 作为它的扩展名|
|其它所有资源文件|所有不在 res/values/ 目录下的文件，会转换成以 *.flat 为扩展名的二进制 XML 文件。此外，所有的 PNG 文件会以默认的方式压缩并添加 *.png.flat 扩展名。编译时可以使用 `--no-crunch` 选项来避免 PNG 文件被压缩|

经 AAPT2 输出的文件不是可执行文件，必须将这些二进制文件做为链接阶段的输入文件来产生 APK 文件。然而，因为还没有包含 DEX 文件（由字节码编译而来），没有签名，所以此时生成的 APK 文件不能马上运行到 Android 设备上。

## 编译（Compile）语法

`compile` 基本语法如下所示：
```
aapt2 compile path-to-input-files [options] -o output-directory/
```
**注意：** 对于资源文件来说，文件路径必须匹配这种结构：**path/resource-type[-cofnig]/file**

下面示例中，使用 AAPT2 分别编译 values.xml 和 myImage.png 两个资源文件：
```
aapt2 compile project_root/module_root/src/main/res/values-en/strings.xml -o compiled/

aapt2 compile project_root/module_root/src/main/res/drawable/myImage.png -o compiled/
```
正如上面表中描述的那样，输出文件的名称取决于输入文件的名称以及它所在的目录（资源类型和配置）。拿上文中的 `strings.xml` 来说，`appt2` 自动将它输出为 `values-en_string.arsc.flat`。此外，存放于 `drawable` 目录下的已编译的 drawable 文件会被命名为 drawable_img.png.flat 。

## 编译选项

`compile` 命令有多个选项供开发者使用，如下表所示：
|选项|描述|
|---|---|
|**-o** *路径*|为编译好的文件指定输出路径。<br/>这是个必选项，因为你必须指定一个文件夹路径去存放 AAPT2 输出的已编译过的资源文件|
|**--dir** *目录*|指定待编译资源文件目录。<br/>尽管可以使用此选项一次编译多个资源文件，但也去除了增量编译的优势，所以在编译大的工程时不要使用此选项。|
|**--pseudo-localize**|生成[仿本地化](https://developer.android.com/guide/topics/resources/pseudolocales)（ pseudo-localized）版的默认 strings，例如，en-XA，en-XB。|
|**--no-crunch**|不处理 PNG 资源<br/>如果是已经处理过 PNG 文件或创建一个不需要缩减文件大小的 Debug 版构建，可以使用这个选项来避免重复或不必要的处理。使用此选项会加快编译速度，但会增加输出文件的大小。|
|**--legacy**|兼容旧版本资源，将编译时的 错误 变成 警告。<br/>使用 AAPT2 编译时遇到意外的错误时，应该尝试使用此选项来解决 AAPT2 行为变更（相对于 AAPT）带来的已知问题。具体有哪些变化，请参考[这里](https://developer.android.com/studio/command-line/aapt2#aapt2_changes)|
|**-v**|输出详细日志|

## 链接（Link）

在链接阶段，AAPT2 将编译产生的中间文件，如，资源表（resource tables）、二进制 XML 文件和已处理的 PNG 文件及包（packages）合并成一个 APK 文件。另外，一些辅助文件，如 `R.java` 及 ProGuard 规则文件会在这个阶段产生。此时产生的 APK 不包括 DEX 字节码并且是未签名的。这意味着还不能将此 APK 安装到设备上。如果不使用 Android Gradle 插件[从命令行构建 app](https://developer.android.com/studio/build/building-cmdline) 的话，你可以使用其它命令行工具，比如 [d8](https://developer.android.com/studio/command-line/d8) 将 Java 字节码编译成 DEX 字节码，然后使用 `apksigner` 签名 APK 包。

## 链接语法

`link` 一般用法如下：
```
aapt2 link path-to-input-files [options] -o outputdirectory/outputfilename.apk --manifest AndroidManifest.xml
```
下面的示例，AAPT2 将合并两个中间文件 drawable_Image.flat、values_values.arsc.flat 以及 AndroidManifest.xml 文件。AAPT2 链接合并后的结果到 `android.jar`，此包含有 android 包中的资源：
```
aapt2 link -o output.apk
-I android_sdk/platforms/android_version/android.jar
    compiled/res/values_values.arsc.flat
    compiled/res/drawable_Image.flat --manifest /path/to/AndroidManifest.xml -v
```

## 链接选项

`link` 命令有如下选项：
|选项|描述|
|---|---|
|**-o** *path*|指定 APK 文件输出路径。<br/>必选项，必须指定一个路径来存放输出的 APK 文件。|
|**--manifest** *file*|指定构建时 Android manifest 文件所在的路径。<br/>由于 manifest 文件包含 app 所需的必要信息，如包名、应用 ID 等，所以必须指定此路径。|
|**-I**|指定编译时可能用到的平台文件，例如 android.jar 或其它 APKs，如 framework-res.apk。<br/>如果你在资源文件中使用了 android 名称空间里的属性，如 `android:id`，就必须使用这个选项。|
|**-A** *directory*|指定 APK 需要包含的资源文件夹。<br/>可以使用此目录来存储原始未处理的文件。点击[访问原始文件](https://developer.android.com/guide/topics/resources/providing-resources#OriginalFiles)了解更多详情。|
|**-R** *file*|链接一个单独的 .flat 文件，执行覆盖语义操作，而不需要使用 <add-resource> 选项。<br/>使用此选项去覆盖一个已经存在的文件，如果多次指定，则后面指定的文件会覆盖前面的文件。|
|**--package-id** *package-id*|为 app 指定包 ID。<br/>包 ID 必须不小于 0x7f ，除非配合 `--allow-reserved-package-id` 选项|
|**--allow-reserved-package-id**|允许用户使用保留包 ID（reserved package IDs）。<br/>保留包ID通常用在共享库中，取值范围为 0x02 到 0x7e。使用 `--allo-reserved-packaged-id` 可以将 ID 分配到保留包 ID 的范围内。<br/>此选项只能用在 SDK 最低版本（min-sdk）为 26 或以下的项目中。|
|**--java** *directory*|指定生成 **R.java** 的目录|
|**--proguard** *proguard_options*|根据 ProGuard 规则文件产生的输出文件|
|**--proguard-conditional-keep-rules**|根据 ProGuard 规则产生的主（main）dex 文件的输出文件|
|**--no-auto-version**|禁止自动维护 style 与 layout 的 SDK 版本|
|**--no-version-vectors**|禁止自动对矢量图进行版本控制。只有在构建使用矢量图库的 APK 时才会用到这个选项。|
|**--no-version-transitions**|禁止自动对过度资源进行版本控制。只有在构建使用到过渡支持库（Transition Support library）的 APK 时才会用到这个选项。|
|**--no-resource-deduping**|在兼容配置中，禁止自动去除具有完全相同值的重复资源|
|**--enable-sparese-encoding**|使用二分查找树来取代稀疏编码。这可以减少 APK 的体积，但会增加检索资源时的性能消耗。|
|**-z**|本地化那些使用”suggested“标记的字符串|
|**-c** *config*|提供一组使用逗号分割的配置。<br/>例如，如果你依赖了包含翻译多种语言的支持包（support library），|
|**--preferred-density** *density*|允许 AAPT2 使用最匹配密度资源并排除其它密度资源文件。<br/>在 app 中有多种像素密度质量的资源，比如 ldpi, hdip 和 xhdpi. 当指定一种密度时，AAPT2 会将与此密度最匹配的资源留下，并去除其它资源。|
|**--output-to-dir**|输出 APK 内容到 APK 所在的目录，由 **-o** 指定。<br/>如果使用此选项时出现错误，可以通过升级 [Android SDK Build Tools 28.0.0 or higher](https://developer.android.com/studio/releases/build-tools) 解决。|
|**--min-sdk-version** *min-sdk-version*|设置 AndroidManifest.xml 中的默认的最低 SDK 版本。|
|**--target-sdk-version** *target-sdk-version*|设置 AndroidManifest.xml 中默认目标 SDK 版本。|
|**--version-code** *version-code*|如果 AndroidManifest.xml 中没有设置版本号（一个整数值），就去注入一个值。|
|**--compile-sdk-version-name** *compile-sdk-version-name*|如果 AndroidManifest.xml 中没有版本名，就注入一个值。|
|**--proto-format**|生成 Protobuf 格式的编译资源。<br/>生成与输入到 [bundle tool](https://developer.android.com/studio/build/building-cmdline#bundletool-build) 中相匹配的 Android App Bundle。|
|**--non-final-ids**|生成资源 IDs 为非 final 类型的 R.java 文件（在 kotlinc/javac 编译期间，app 代码不会内联这些 ID）。|
|**--emit-ids** *path*|导出一个文件到指定路径，这个文件包括一组资源类型名与其 ID 的映射，通常与 **--stable-ids** 配合使用。|
|**--stable-ids** *outputfilename.ext*|使用 **--emit-ids** 生成的包含一组资源名与其 ID 的文件。<br/>这个选项允许在链接阶段使用之前保留的 ID，还可以删除或添加资源。|
|**--custom-package** *package_name*|指定生成 R.java 文件的自定义 Java 包。|
|**--extra-packages** *package_name*|生成 R.java 文件，但此文件拥有不同的包名。|
|**--add-javadoc-annotation** *annotation*|为所有已生成的 Java class 文件添加 JavaDoc 注解。|
|**--output-text-symbols** *path*|在指定文件中生成一个包含 R 类中资源符号的文本文件。<br/>必须指定输出文件的路径。|
|**--auto-add-overlay**|即使不使用 **<add-resource>** 标识，也允许以覆盖的形式添加新资源|
|**--rename-manifest-package** *manifest-package*|重命名 AndroidManifest.xml 中的包名。|
|**--rename-instrumentation-target-package** *instrumentation-target-package*|修改目标包 [instrumentation](https://developer.android.com/reference/android/app/Instrumentation) 的名称。|
|**-0** *extension*|指定不想压缩的文件的扩展名|
|**--split** *path:config[,config[..]]*|根据一组设置将资源分成不同版本的 APK。<br/>在这组配置之前必须指定 APK 的输出路径。|
|**-v**|输出详细日志。|

## Dump

使用 `dump` 命令打印从链接阶段生成的 APK 的资源与配置信息。可以使用以下命令将信息打印到控制台：
```
aapt2 dump output.apk
```

### Dump 语法

一般用法如下：
```
aapt2 dump finename.apk [options]
```

### Dump 选项（options）
`dump` 的选项如下表所示：
|选项|描述|
|----|---|
|**--no-values**|展示资源时，不显示输出值。|
|**--file** *file*|将 APK 的输出信息输入到指定文件。|
|**-v**|输出详细日志。|

## AAPT2 中行为变更

AAPT2 之前，AAPT 作为 Android 资源打包工具（Android Asset Packaging Tool）的默认版本，但现在已经过时了。尽管 AAPT2 应该可以直接应用到早期工程，本节还是要描述下开发者需要了解的变化。

### Android manifest 中元素的层级
在之前版本 AAPT 中，嵌入到 Android manifest 不正确位置的元素会被忽略或出现警告。例如以下情况：
```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
   package="com.example.myname.myapplication">
   <application
       ...
       <activity android:name=".MainActivity">
           <intent-filter>
               <action android:name="android.intent.action.MAIN" />
               <category android:name="android.intent.category.LAUNCHER" />
           </intent-filter>
           <action android:name="android.intent.action.CUSTOM" />
       </activity>
   </application>
</manifest>
```
之前版本的 AAPT 会粗暴地将放错位置的 `<action>` 标签忽略掉。但是，在 AAPT2 中，会报出以下错误：
```
AndroidManifest.xml:15: error: unknown element <action> found.
```
要解决此类问题，要确保 manifest 所有元素都在合适的位置。想了解更多信息，请阅读 [Manifest file structure](https://developer.android.com/guide/topics/manifest/manifest-intro#filestruct)。

### 声明资源

现在不能在 `name` 属性中指定资源的类型了。比如，下面展示了一个不正确的资源项 `attr` 声明：
```
<style name="foo" parent="bar">
    <item name="attr/my_attr">@color/pink</item>
</style>
```
这样声明会抛出以下构建错误：
```
Error: style attribute 'attr/attr/my_attr (aka my.package:attr/attr/my_attr)' not found.
```
要解决此问题，需要使用 `type="attr"` 来声明：
```
<style name="foo" parent="bar">
  <item type="attr" name="my_attr">@color/pink</item>
</style>
```
此外，声明 `<style>` 元素时，它的父结点必须为 style 资源类型。否则会得到类似以下的错误信息：
```
Error: (...) invalid resource type 'attr' for parent of style
```

### Android ForegroundLinearLayout 名称空间
[ForegroundLinearLayout](https://developer.android.com/reference/android/support/design/R.styleable#ForegroundLinearLayout) 包括三种属性：[foregroundInsidePadding](https://developer.android.com/reference/android/support/design/R.styleable#ForegroundLinearLayout_foregroundInsidePadding), [android:foreground](https://developer.android.com/reference/android/support/design/R.styleable#ForegroundLinearLayout_android_foreground) 与 [android:foregroundGravity](https://developer.android.com/reference/android/support/design/R.styleable#ForegroundLinearLayout_android_foregroundGravity)。注意 `foregroundInsidePadding` 与其它两个属性不同的是，它不在 `android` 名称空间下。

之前版本的 AAPT，如果将 `foregroundInsidePadding` 属性添加上 `android` 名称空间，编译器会忽略这个属性设置。当使用 AAPT2 时，编译器会捕获这个错误并抛出如下构建错误信息：
```
Error: (...) resource android:attr/foregroundInsidePadding is private
```
解决此问题的方法：使用 `foregroundInsidePadding` 替换 `android:foregroundInsidePadding` 就行了。

### 使用资源引用符 @ 不当
如果忽略或放置不当资源引用符（@），AAPT2 会抛出构建错误。比如下面的需要指定某种类型属性值时却没有使用 @：
```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
  ...
  <!-- Note the missing '@' symbol when specifying the resource type. -->
  <item name="colorPrimary">color/colorPrimary</item>
</style>
```
编译此模块时，AAPT2 会抛出以下构建错误：
```
ERROR: expected color but got (raw string) color/colorPrimary
```
此外，如果访问 `android` 名称空间的资源时，误用了 @ ，像下面这样：
```
...
<!-- When referencing resources from the 'android' namespace, omit the '@' symbol. -->
<item name="@android:windowEnterAnimation"/>
```
当编译此模块时，AAPT2 会抛出以下构建错误信息：
```
Error: style attribute '@android:attr/windowEnterAnimation' not found
```

### 库（library）配置错误
如果 app 依赖三方库并且使用较旧版本的 [Android SDK 构建工具](https://developer.android.com/studio/releases/build-tools) 构建的话，app 运行时可能会崩溃并且没显示任何错误或警告信息。这种崩溃有可能是这样产生的，在创建库时，`R.java` 中的字段是 `final` 类型的，即所以资源 ID 都内联到库的 class 中了。

当构建 app 时，AAPT2 需要一个可重分配 ID 的库。如果这个库将这此 ID 设置为 `final` 类型并将它们内联到 dex 库中，就会引起运行时不匹配问题。

解决此问题的方法，联系此库的让他使用最新版 Android SDK 构建工具重新打个包，重新发布一下这个库。
