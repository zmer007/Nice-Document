## 1 背景
Android 开发平时都是看的都是 SDK 中的源码，SDK 中有很多代码都被隐藏了，看不了。mac 上没有 source insight 工具，折腾半天 vscode 插件，也没能设置通代码跳转功能。后来，看到有在线版 aosp 预览网站，第一个是 [androidxref](http://androidxref.com)，体验还不错，就是加载慢，后来出了个[国内版本 androdxref](http://aospxref.com)，加载快了很多，但有时死活加载不出来大文件，比如 Activity.java 只加载到几百行之后就一片空白了，越想看越加载不出来，憋的很难受。我有个不经常使用的笔记本电脑，正好用来搭建一个 aosp 预览服务，还省下 mac 100 多 G 硬盘空间。

将电脑装上 ubuntu 系统，这里选择 14.04 版本，因为 aosp 各版本都是在 ubuntu 14.04 上[测试](https://source.android.com/setup/build/initializing#setting-up-a-linux-build-environment)的。

## 2 获取源码
从国内下载 aosp 镜像（清华大学开源软件镜像站）：https://mirror.tuna.tsinghua.edu.cn/help/AOSP 

1. 访问 https://mirrors.tuna.tsinghua.edu.cn/aosp-monthly/aosp-latest.tar 开始下载最新月包，我是 2020 一月份下载的最新包，有 68G
2. tar -xvf aosp-latest.tar 并进入 aosp 目录（在我电脑上目录地址为 /home/lgd/aosp）
3. 执行 `repo sync -l` checkout 代码，当前 checkout 出的为 master 分支代码
4. 执行 `repo init -b android-8.0.0_r27` 将当前分支切换到 `android-8.0.0_r27`（只是将 repo 配置转换到 `android-8.0.0_r27`，下一步会切换源码；其它分支看[这里](https://source.android.com/setup/start/build-numbers#source-code-tags-and-builds)）
5. 执行 `repo sync -c --no-clone-bundle --no-tags -j4` 将源码切换到 `android-8.0.0_r27`

[repo 介绍](https://duanqz.github.io/2015-06-25-Intro-to-Repo)

## 3 建立源码索引
1. 执行 `sudo apt-get install openjdk-8-jdk` 安装 openjdk-8，如果提示找不到包，就执行 `sudo add-apt-repository ppa:openjdk-r/ppa` 添加 apt 库，update 后再进行安装
2. 下载 [tomcat 8](http://mirror.downloadvn.com/apache/tomcat/tomcat-8/v8.5.50/bin/apache-tomcat-8.5.50.tar.gz)，解压后进入 bin 目录，执行 `./startup.sh` 启动 tomcat
3. 安装 [universal-ctags](https://github.com/universal-ctags/ctags)
4. [设置](https://github.com/oracle/opengrok/wiki/How-to-setup-OpenGrok) OpenGrok
   1. [下载](https://github.com/oracle/opengrok/releases)最新版 OpenGrok
   2. 创建 OpenGrok 工程目录，创建根目录 `mkdir grok`，进入 grok 执行 `mkdir src data dist etc log` 创建作业目录
   3. 将下载的 opengrok-x.x.x.tar.gz 解析，将 opengrok-x.x.x 中的文件全部转移到步骤 2 中的 dist 目录下
   4. 将 ./doc/logging.properties 复制到 ./etc/
   5. 创建 aosp 目录的软链接 `ln -s /home/lgd/aosp android-8.0.0_r27`，并将该软链接添加到 grok/src 目录中，下一步中会将根据这个软链创建索引，这样做缺点是查找源码时会搜索整个 aosp 项目，代码列表显的很多。可以为 aosp 目录下每个子目录创建一个软链，然后放到 grok/src 目录下，这样可以通过选择目录过滤搜索文件
5. 创建 aosp 索引文件，执行 `./indexer.sh`，脚本文件在下面，这个过程很长并且会有许多警告抛出，但不影响主体功能
6. 访问 http://localhost:8080/source 应该可以看到 OpenGrop 预览首页，并会有 android-8.0.0_r27 源码目录

indexer.sh 内容如下
```sh
# /home/lgd/grok 为 OpenGrok 根目录，应该替换成自己的目录再执行
java -Xmx12g \
    -Djava.util.logging.config.file=/home/lgd/grok/etc/logging.properties \
    -jar /home/lgd/grok/dist/lib/opengrok.jar \
    -c /usr/bin/ctags \
    -s /home/lgd/grok/src \
    -d /home/lgd/grok/data -H -P -S -G \
    -W /home/lgd/grok/etc/configuration.xml \
    -U http://localhost:8080/source \
    -m 256
```

## 遇到的问题
以下是在导入 Ide 中出现的问题及解决方法（未完待续）

**所有命令都是在 aosp 根目录下执行**

### 问题 1
执行 `source build/envsetup.sh` 命令
```
Traceback (most recent call last):
  File "/media/lgd/d/aosp/aosp/.repo/repo/main.py", line 531, in <module>
    _Main(sys.argv[1:])
  File "/media/lgd/d/aosp/aosp/.repo/repo/main.py", line 507, in _Main
    result = repo._Run(argv) or 0
  File "/media/lgd/d/aosp/aosp/.repo/repo/main.py", line 158, in _Run
    copts, cargs = cmd.OptionParser.parse_args(argv)
  File "/media/lgd/d/aosp/aosp/.repo/repo/command.py", line 67, in OptionParser
    self._Options(self._optparse)
  File "/media/lgd/d/aosp/aosp/.repo/repo/subcmds/sync.py", line 189, in _Options
    self.jobs = self.manifest.default.sync_j
  File "/media/lgd/d/aosp/aosp/.repo/repo/manifest_xml.py", line 360, in default
    self._Load()
  File "/media/lgd/d/aosp/aosp/.repo/repo/manifest_xml.py", line 428, in _Load
    self._ParseManifest(nodes)
  File "/media/lgd/d/aosp/aosp/.repo/repo/manifest_xml.py", line 537, in _ParseManifest
    project = self._ParseProject(node)
  File "/media/lgd/d/aosp/aosp/.repo/repo/manifest_xml.py", line 802, in _ParseProject
    relpath, worktree, gitdir, objdir = self.GetProjectPaths(name, path)
  File "/media/lgd/d/aosp/aosp/.repo/repo/manifest_xml.py", line 852, in GetProjectPaths
    worktree = os.path.join(self.topdir, path).replace('\\', '/')
  File "/usr/lib/python2.7/posixpath.py", line 80, in join
    path += '/' + b
UnicodeDecodeError: 'ascii' codec can't decode byte 0xe5 in position 11: ordinal not in range(128)
```

解决方法： 在 main.py 文件， import 语句之后添加 
```py
reload(sys)
sys.setdefaultencoding('utf8')
```

### 问题 2
执行 `sudo apt-get install openjdk-8-jdk` 提示找不到安装包

解决方法：
```sh
sudo add-apt-repository ppa:openjdk-r/ppa # 添加 openjdk 库
sudo apt-get update

sudo apt-get install openjdk-8-jdk
```

### 问题 3
编译 build/envsetup.sh 完成后，执行 `lunch aosp_x86_64-eng` 提示 文件权限错误。

解决方法：之前是在移动硬盘上操作到，某个 .bash 文件权限为 -rx------，使用 `sudo chmod a+x file.bash` 后也改变不了文件属性。最后将文件全部考到电脑中就此问题就消失了。

### 问题 4
执行 `make` 时，提示：
```sh
build/make/core/base_rules.mk:589: warning: ignoring old commands for target `out/target/product/generic_x86_64/testcases/simpleperf_unit_test/testdata/wrong_ip_callchain_perf.data'
[ 99% 931/932] glob tools/apksig/src/main/java/**/*.java
[  0% 459/81763] Check module type: out/target/common/obj/APPS/Browser2_intermediates/link_type
packages/apps/Browser2/Android.mk: warning: Browser2 (java:sdk) should not link to legacy-android-test (java:platform)
[  0% 793/81763] build out/target/common/obj/JAVA_LIBRARIES/core-oj_intermediates/annotated/timestamp
WARNING: duplicate annotation of type libcore.util.NonNull
WARNING: duplicate annotation of type libcore.util.NonNull
[  3% 2941/81763] Yacc: ss <= external/iproute2/misc/ssfilter.y
FAILED: out/target/product/generic_x86_64/obj/EXECUTABLES/ss_intermediates/ssfilter.c 
/bin/bash -c "prebuilts/misc/linux-x86/bison/bison -d  --defines=out/target/product/generic_x86_64/obj/EXECUTABLES/ss_intermediates/ssfilter.h -o out/target/product/generic_x86_64/obj/EXECUTABLES/ss_intermediates/ssfilter.c external/iproute2/misc/ssfilter.y"
/bin/bash: prebuilts/misc/linux-x86/bison/bison: 没有那个文件或目录
[  3% 2949/81763] target  C++: secdiscard <= system/vold/FileDeviceUtils.cpp
56 warnings generated.
[  3% 2950/81763] target  C++: secdiscard <= system/vold/secdiscard.cpp
58 warnings generated.
ninja: build stopped: subcommand failed.
20:58:10 ninja failed with: exit status 1

#### failed to build some targets (06:48 (mm:ss)) ####
```

解决方法：
日志中有这一句： prebuilts/misc/linux-x86/bison/bison: 没有那个文件或目录 解决方法如下：
```sh
sudo apt-get install g++-multilib gcc-multilib lib32ncurses5-dev lib32readline-gplv2-dev lib32z1-dev
```
国内网络下载缓慢，需要替换 apt 仓库源，备份 `/etc/apt/sources.list` 文件，并将文件内全部内容清空，添加阿里 apt 源
```
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
```

### 问题 5
```
[  0% 24/78813] Yacc: ss <= external/iproute2/misc/ssfilter.y
FAILED: out/target/product/generic_x86_64/obj/EXECUTABLES/ss_intermediates/ssfilter.c 
/bin/bash -c "prebuilts/misc/linux-x86/bison/bison -d  --defines=out/target/product/generic_x86_64/obj/EXECUTABLES/ss_intermediates/ssfilter.h -o out/target/product/generic_x86_64/obj/EXECUTABLES/ss_intermediates/ssfilter.c external/iproute2/misc/ssfilter.y"
external/iproute2/misc/ssfilter.y: conflicts: 35 shift/reduce
[  0% 33/78813] Check module type: out/target/product/generic_x86_64/obj/EXECUTABLES/tc_intermediates/link_type
ninja: build stopped: subcommand failed.
21:51:18 ninja failed with: exit status 1

#### failed to build some targets (01:07 (mm:ss)) ####
```
解决方法：
从 .repo 中使用 `repo sync -l` checkout 出源码，此时的源码是 master 分支，编译会遇到奇奇怪怪的问题，应该切换到某个发布分支，再做编译，比如切换到 android-8.0.0_r27

```
repo sync -l
repo init -b android-8.0.0_r27
repo sync -c --no-clone-bundle --no-tags -j4
```

### 问题 6
没有报春错误日志，错误内容印象是 m4 编译错误，发现环境中没有安装 m4，安装后继续执行。
```sh
# 安装 m4
sudo apt-get install m4
```

### 问题 7
```
2 warnings generated.
[ 16% 9889/61548] Copy xml: out/target/product/generic/system/etc/apns-conf.xml
FAILED: out/target/product/generic/system/etc/apns-conf.xml 
/bin/bash -c "(xmllint device/generic/goldfish/data/etc/apns-conf.xml >/dev/null ) && (mkdir -p out/target/product/generic/system/etc/ ) && (rm -f out/target/product/generic/system/etc/apns-conf.xml ) && (cp device/generic/goldfish/data/etc/apns-conf.xml out/target/product/generic/system/etc/apns-conf.xml )"
/bin/bash: xmllint: 未找到命令
ninja: build stopped: subcommand failed.
09:06:00 ninja failed with: exit status 1
make: *** [run_soong_ui] 错误 1
```
解决方法：
```
sudo apt-get install libxml2-utils
```

### 问题 8
找不到 mmm 命令

解决方法：
重新执行 
```sh
source build/envsetup.sh
```

### 问题 9
运行 `repo sync -b --no-clone-bundle --no-tags -j4` 命令时出现以下错误

```
	28/system/api/com.android.media.tv.remoteprovi
正在终止
prebuilts/sdk/: discarding 1 commits
error: prebuilts/sdk/: platform/prebuilts/sdk checkout 2ba6c3807598071c98c37eb6fb5601493748041c 
error: Cannot checkout platform/prebuilts/sdk
prebuilts/tools/: discarding 1 commits
system/bt/: discarding 253 commits
system/chre/: discarding 14 commits
system/connectivity/wificond/: discarding 36 commits
system/core/: discarding 197 commits
system/extras/: discarding 80 commits
system/gatekeeper/: discarding 1 commits
system/hardware/interfaces/: discarding 7 commits
system/hwservicemanager/: discarding 29 commits
system/keymaster/: discarding 12 commits
system/libfmq/: discarding 15 commits
system/libhidl/: discarding 67 commits
system/libhwbinder/: discarding 15 commits
system/libufdt/: discarding 8 commits
system/libvintf/: discarding 37 commits
system/media/: discarding 49 commits
system/netd/: discarding 28 commits
system/nfc/: discarding 45 commits
system/nvram/: discarding 2 commits
system/security/: discarding 27 commits
system/sepolicy/: discarding 180 commits
system/tools/hidl/: discarding 90 commits
system/update_engine/: discarding 10 commits
system/vold/: discarding 21 commits
test/vts/: discarding 91 commits
test/vts-testcase/fuzz/: discarding 15 commits
test/vts-testcase/hal/: discarding 63 commits
test/vts-testcase/hal-trace/: discarding 5 commits
test/vts-testcase/kernel/: discarding 25 commits
test/vts-testcase/performance/: discarding 19 commits
test/vts-testcase/security/: discarding 4 commits
test/vts-testcase/vndk/: discarding 5 commits
tools/apksig/: discarding 5 commits
tools/external/gradle/: discarding 8 commits
tools/loganalysis/: discarding 12 commits
tools/test/connectivity/: discarding 94 commits
tools/tradefederation/contrib/: discarding 3 commits
tools/tradefederation/core/: discarding 82 commits
Checking out projects:  99% (587/588), done.

error: Exited sync due to checkout errors
Failing repos:
prebuilts/sdk
```
解决问题：
可能是之前操作时中途强行中断导致文件出现不一致情况，[网上查找](https://groups.google.com/forum/#!topic/android-building/ClJoLAeBlXA)说有删除 `.repo/prebuilts/sdk.git` 删除再重新 sync 的，但是我 .repo 目录中没有这个文件。删除所有文件，从原 .tar 包中重新解压了一份重新执行命令就 ok 了