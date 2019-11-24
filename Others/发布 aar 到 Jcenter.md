

本文介绍如何将自己的 aar 包上传到 jCenter 并发布至 public 仓库。

## 1 私有项目发布到 jCenter public 仓库

### 1.1 背景
源码在 Github 上以私有库的形式存放，并想将打出的 aar 包上传到 jCenter 公有库中，一直被拒审。总是提示使用的 VCS url 不可访问。

### 1.2 思路
创建一个新仓库并加些源码，新建与私有项目同名的 module ，
使用 [release-aar 脚本](https://raw.githubusercontent.com/zmer007/ReleaseZip2Jcenter/master/release-aar.gradle)将 module 打出 zip，手动将此 zip 发布到 jCenter 审核通过后再将自己私有项目发布到通过审核的仓库中。这样就避免了 private 库导致的审核问题。

### 1.3 具体步骤

以发布 targetsdk 为例

1. 在 jCenter 创建仓库，库名为 targetsdk，VCS url 为 `https://github.com/your-user-name/your-public-repo-name`
2. 创建 targetsdk module 并配置 release-aar 变量后执行 ```./gradlew targetsdk:generateRelease``` 获取 zip 包
3. 将 zip 上传到步骤 1 创建的仓库，上传时文件时，要勾选 "Explode this archive" 选框，上传完成并点击 publish 后，就将 aar 发布到 jCenter 私有库了
4. 最后，点击 add Jcenter 将此私有库发布到公有库，审核（下午的话，一个小时就会有结果）通过后，就代表成功了。

首次发布成功后，后续可以使用[脚本](https://raw.githubusercontent.com/blundell/release-android-library/master/android-release-aar.gradle)实现自动上传 jCenter 公有库。


## 2 将三方 aar 发布到 jCenter public 仓库

### 2.1 背景
接入三方 SDK 时，对方提供的是 aar（记为 A.aar） 没有提供远程依赖的方法，我们发布的 SDK 集成了 A.aar，但是想将自己的 SDK 发布到 jCenter public 仓库。如果将 A.aar 添加到 lib 目录下，是不会将 A.aar 一块上传到 jCenter 的，其它开发者接入我们的 SDK 就必须手动将 A.aar 添加到他们 lib 里，十分麻烦。我们想将 A.aar 原样发布到 jCenter 公有库，然后使用远程依赖 A.arr 就避免了上面手动添加 A.aar 的情况。

### 2.2 思路
在自己 jCenter 仓库下创建一个 A.aar 同名项目，将 A.aar 上传到此项目中。

### 2.3 具体步骤
aar 是不能直接放到 jCenter 仓库的，还得有一些其它文件，如 pom 文件，源码 jar 文件等，还有它们对应的 md5/sha1 文件。

以 A.aar 为例

1. 在 jCenter 创建仓库，库名为 A，VCS url 为 `https://github.com/your-user-name/your-public-repo-name`
2. 配置 A mudule 的 release-aar 脚本执行 ```./gradlew A:generateRelease``` 获取 zip 包
3. 解压 zip 包找到包中的 aar 及 pom 文件，将 A.aar 重命名后替换包中的 aar，如果 A.aar 有其它依赖就将其添加到 pom 文件中
4. 修改 aar、pom 文件的 md5 值及 sha1 值（在命令行中执行 ```md5 your-file-path``` 获取 your-file 的 md5 值，执行 ```shasum your-file-path``` 获取 your-file 的 sha1 值）
5. 将解压的目录重新打成 zip 包，然后上传到步骤 1 创建的仓库
6. 按照 1.3 小节步骤将此 zip 上传到 jCenter，并发布
7. 通过审核后，就可以直接使用远程依赖的方式了