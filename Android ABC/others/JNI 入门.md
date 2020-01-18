1. JNI 基本概念
3. Java 调用 Native 注意点
4. Native 调用 Java 注意点

JNI（Java Native Interface）是连通 Java 和 C/C++/汇编 程序的桥梁。主要用于以下场景：

1. 我搞到一个 c 库，功能很强大，但是我项目是用 java 写的，把这个库添加到项目中就得使用 jni 技术
2. 项目某块功能对计算能力要求很高，Java 库中方法达不到要求，此时就可以使用 c 库完成这项任务，一般游戏或物理模拟会这么做

