# Gradle 插件入门
[原文地址](https://guides.gradle.org/writing-gradle-plugins)

本文将全程介绍如何创建可重用的构建逻辑，即 Gradle 插件。该插件源码位于工程的 `buildSrc` 目录，此目录可单独导出一个工程，用于发布或应用到其它 Gradle 构建脚本中。

插件提供了普适的约定（convention），任务类型（task type）和其它构建逻辑，使开发者可以专注于自己项目中独有的元素，而无需关注插件可自动完成的部分。例如，[Java 插件](https://docs.gradle.org/4.10.3/userguide/java_plugin.html)提供一个标准的目录结构和任务（tasks）来执行一些编译源码、导出 java 文档、运行单元测试等类似的工作。

本文介绍的只是一个非常简单的插件，使用的功能很少，只用于理解编写插件的流程。记清一点，任何在 Gradle 构建脚本（`build.gradle`）中可实现的功能，在插件中都可以实现。

## 构建什么
编写一个简单的插件，向此插件中添加新的任务类型并在任意项目中创建该任务类型的任务（task）。体会插件是如何工作以及插件是如何在宿主构建脚本中起作用的。

## 需要什么环境
- 大约 12 分钟
- 一个文本编译器或 IDE
- 一个不低于 1.8 版本的 [JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
- 一个不低于 4.10.3 版本的 [Gralde](https://gradle.org/install/)

## 创建项目
创建一个目录作为工程的根目录，并进入该目录
```sh
$ mkdir greeting-plugin
$ cd greeting-plugin
```

之后，使用以下命令创建插件源码目录

```sh
$ mkdir -p buildSrc/src/main/java/org/example/greeting
```

将插件源码放入 `buildSrc` 目录是封装插件的一种方式，这样可以将它与工程内部构建脚本隔离。这样做无论对自定义任务类型还是插件都非常有用。更多关于此方面的内容请参考[用户手册](https://docs.gradle.org/4.10.3/userguide/organizing_gradle_projects.html#sec:build_sources)

> 如果想跨多个构建脚本使用插件，就需要发布此插件。参考[下一步]()获取更多此方面信息的链接。应该将插件源码放到单独的目录中，即不将插件源码放到其它工程的 `buildSrc` 目录下，这样方便配置插件的构建脚本，方便将打包好的插件上传到 Gradle Plugin Portal 或其它仓库。

## 创建插件
在前面建的目录（`buildSrc/src/main/java/org/example/greeting`）下创建 GreetingPlugin 类。内容如下

**buildSrc/src/main/java/org/example/greeting/GreetingPlugin.java**
```java
package org.example.greeting;

import org.gradle.api.Plugin;
import org.gradle.api.Project;

public class GreetingPlugin implements Plugin<Project> {
    public void apply(Project project) {
        project.getTasks().create("hello", Greeting.class, (task) -> { // tag-1
            task.setMessage("Hello");
            task.setRecipient("World");                                // tag-2
        });
    }
}
```
tag-1：创建一个名为 hello 的 Greeting 类型（稍后将定义）的任务

tag-2：为此任务设置默认值

此处是插件入口点，Gradle project 对象提供完整 Gradle API，可以通过此对象做 build.gradle 脚本中能做的事情。此处示例表示在目标工程中创建了一个名为 hello 的简单任务。

> 使用 Gradle [DSL Reference](https://docs.gradle.org/4.10.3/dsl/) 和 [Javadocs](https://docs.gradle.org/4.10.3/javadoc/overview-summary.html) 了解 Gradle API 用途。以 Project 为起点了解整个 API 功能。也可以通过[下一步]()中链接了解更多进阶内容。

下一步要创建插件中用到的任务类型的类。添加 Greeting 类到插件包下：

**buildSrc/src/main/java/org/example/greeting/Greeting.java**
```java
package org.example.greeting;

import org.gradle.api.DefaultTask;
import org.gradle.api.tasks.TaskAction;

public class Greeting extends DefaultTask {
    private String message;
    private String recipient;

    public String getMessage() { return message; }
    public void setMessage(String message) { this.message = message; }

    public String getRecipient() { return recipient; }
    public void setRecipient(String recipient) { this.recipient = recipient; }

    @TaskAction
    void sayGreeting() {
        System.out.printf("%s, %s!\n", getMessage(), getRecipient()); // tag-1
    }
}
```
tag-1：运行 task 时，打印配置好的 greeting 内容。

> 查看[用户手册](https://docs.gradle.org/4.10.3/userguide/custom_tasks.html)了解更多关于创建自定义任务类型的信息。

现在已经完成插件功能了，但是单独的插件成不了事，它必须应用到工程中才有用。下面会介绍如何将它运用到工程中。

## 将插件配置到宿主工程
在宿主工程（greeting-plugin）目录下创建一个构建脚本，内容如下：
```groovy
apply plugin: org.example.greeting.GreetingPlugin // tag-1
```
tag-1：这样就将当前 Project 实例传到插件中了，并将 hello 任务添加到构建逻辑中。

> **（注意）** `apply plugin` 语法与你之前可能用过的 `plugins {}` 块不同。因为插件源码在 `buildSrc` 目录下并没有提供 id（稍后会添加）。前往[用户手册](https://docs.gradle.org/4.10.3/userguide/plugins.html#sec:binary_plugins)了解这两种方式在配置插件时的不同之处。

在主构建中运行 hello 任务验证插件是否可以正常工作：
```sh
$ gradle hello
:buildSrc:compileJava
:buildSrc:compileGroovy UP-TO-DATE
:buildSrc:processResources UP-TO-DATE
:buildSrc:classes
:buildSrc:jar
:buildSrc:assemble
:buildSrc:compileTestJava UP-TO-DATE
:buildSrc:compileTestGroovy UP-TO-DATE
:buildSrc:processTestResources UP-TO-DATE
:buildSrc:testClasses UP-TO-DATE
:buildSrc:test UP-TO-DATE
:buildSrc:check UP-TO-DATE
:buildSrc:build
:hello
Hello, World!
```

以上输出内容表示 `buildSrc` 中的文件已经被 Java 工程识别了。一但出现这些内容，就代表插件中的类已经在主构建中生效，主构建脚本可以执行指定的任务（插件中的 task） 了。

当前构建中使用 `greeting` 中默认的属性值，所以都会打印 `Hello, World!`。可以在构建脚本中修改这些默认值，如下：

**build.gradle**
```groovy
hello { // tag-1
    message = "Hi"
    recipient = "Gradle"
}
```
tag-1：设置 `hello` 任务的多个属性值

> 查看[用户手册](https://docs.gradle.org/4.10.3/userguide/more_about_tasks.html#sec:configuring_tasks)了解更多任务设置的语法。

在运行 `hello` 任务时可以添加 `-q` 参考来隐藏 `buildSrc` 输出细节，如下：
```sh
$ gradle -q hello
Hi, Gradle!
```

现在插件功能已经齐全，从上面的构建输出可以看出这些功能都起作用了。还有一个事件需要了解--添加插件 id，添加插件 id 可以使用构建脚本更整洁点，同时对发布插件也有裨益。

## 声明插件 id
多数情况下，开发者使用 ID 添加插件，因为相对于全限定类名，ID 更容易记忆。这样做也会使构建脚本文件更整洁。所以将自定义插件声明 id 就成了一件很有意义的事情。

创建以下属性文件，并添加内容：

**buildSrc/src/main/resources/META-INF/gradle-plugins/org.example.greeting.properties**
```
implementation-class=org.example.greeting.GreetingPlugin
```

Gradle 使用这个文件检测哪个类实现了 `Plugin` 接口。此属性文件名（不包含后缀名 `.properties`）将作用插件的 id 。

> **（警告！）** 必须将属性文件放入 `META-INF/gradle-plugins` 目录下，因为 Gradle 将从插件 JAR 包的指定位置中解析此文件。

以上是插件需要做的全部内容，现在可以将构建脚本中下面的一行替换掉了：

```groovy
apply plugin: org.example.greeting.GreetingPlugin
```

使用插件 ID：
```groovy
plugins {
  id 'org.example.greeting'
}
```

注意属性文件名（`org.example.greeting.properties`）必需与插件 ID 相匹配。

> 请始终确保插件（包括名称空间）名的唯一性，而不是使用示例中的 "org.example"。这样做可以避免插件冲突。可以在[用户手册](https://docs.gradle.org/4.10.3/userguide/custom_plugins.html#sec:creating_a_plugin_id)查找更多关于插件 ID 的细节描述。

## 摘要
大功告成！你现在已经成功创建一个插件并且在构建中还使用了它。回顾一下个中知识点，想一下它们是如果实现的：

- 将构建逻辑放入插件中
- 使用 `buildSrc` 目录存放插件类文件
- 为插件添加 ID 并在构建脚本中使用此 ID

本文主要描述了插件的本质是什么，功能相当简单，但大多数插件提供的功能都非常强大。下面的章节会介绍插件的更多用例，并且指导如何实现这些插件。

## 下一步
现在你已经熟悉构建基本的 Gradle 插件了，可能会对以下课题有兴趣：

- [使用 Java Gradle Plugin 开发插件简化插件开发](https://docs.gradle.org/4.10.3/userguide/java_gradle_plugin.html)
- [将插件发布到 Gradle Plugin Portal](https://guides.gradle.org/publishing-plugins-to-gradle-plugin-portal/)
- [使用扩展为 domain 建模](https://docs.gradle.org/4.10.3/userguide/custom_plugins.html#sec:getting_input_from_the_build)
- [测试插件](https://docs.gradle.org/4.10.3/userguide/test_kit.html)
- [为新任务添加增量构建支持](https://docs.gradle.org/4.10.3/userguide/more_about_tasks.html#sec:up_to_date_checks)