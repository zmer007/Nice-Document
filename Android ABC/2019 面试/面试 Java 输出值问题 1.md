面试问题：<br/>
以下程序输出值是什么？
```java
public class Main {
    static class Base {
        String name = "base";

        Base() {
            sayHello();
        }

        void sayHello() {
            System.out.println(name);
        }
    }

    static class Sub extends Base {
        String name = "sub";

        @Override
        void sayHello() {
            System.out.println(name);
        }
    }

    public static void main(String[] args) {
        new Sub();
    }
}
```

看似简单的问题，却包含着以下 Java 基础知识：

1. 类加载过程
2. 类初始化过程
3. 方法分派调用

如果感兴趣的话可以先思考一下输出值是什么~

好，现在我们一步步看程序是如何运行的。

第一阶段：对象初始化之前的所有操作

当程序遇到 `new Sub()` 时会去方法区找是否有一个叫 Sub 的 Class 对象，因为是第一次调用肯定不会存在该对象。自此开始了 **类加载** 之旅，类加载器会根据 `Sub` 对应的字符常量(com.package.Sub)去 classpath 里找 `com.package.Sub.class`，找到后加载该 Sub.class 字节文件，在类加载过程中的解析环节，发现 Sub.class 父类还没有被加载，然后接着去加载 Base.class。类加载完成后，在方法区就存在 Base 类对象和 Sub 类对象了，它们的版本、字段、方法等信息都存放在运行时常量池中了。

第二阶段：对象初始化

经过上一段操作，现在对象已经在准备阶段进行过一次系统初始化（把字段值置零）了，现在轮到在 java 程序中可见的初始化了。初始化过程如下：

1. 先执行父类 `<clinit>()` 方法，再执行子类中的 `<clinit>()` 方法。`<clinit>()` 是虚拟机自动生成，`<clinit>()` 方法里包括类静态变量，类静态块内容（按 java 代码顺序依次排列）。
2. 接着执行父类 `<init>()` 方法，然后再执行子类 `<init>()` 方法。`<init>()` 方法包括类成员变量，还有构造器块中的内容

现在回归到问题，串行看对象初始化：

1. `new Sub()`
2. `Base.<clinit>`
3. `Sub.<clinit>`
4. `Base.<init>()`
    - name = "base"
    - sayHello()
5. `Sub.<init>()`
   1. name = "sub"

现在大概可以确定，不是输出 "base" 就是输出 null，确定这个问题需要了解方法分派调用机制，方法分派分为 **静态分派** 与 **动态分派**，分别对应多态的 **重载** 与 **重写** 。静态分派就是在编译阶段就可以确定的一种机制，比如
```java
public class Dispatch {
    static class A {
    }

    static class B extends A {
    }

    static class C extends A {
    }

    void sayHi(A a) {
        System.out.println("a");
    }

    void sayHi(B b) {
        System.out.println("b");
    }

    void sayHi(C c) {
        System.out.println("c");
    }

    public static void main(String[] args) {
        Dispatch dispatch = new Dispatch();
        dispatch.sayHi(new A());
        dispatch.sayHi(new B());
        dispatch.sayHi(new C());
    }
}
```
此处输出为 
```
a
b
c
```
因为在 dispatch.sayHi 传入的对象过程中，知道谁是调用者（**dispatch**），所以只要知道 dispatch 中应该匹配哪一个方法就可以了，匹配过程如下：找完全匹配的方法，即方法参数一致的，找不到找其父类或接口，基本类型如果可以安全转换也算配置成功，如果找不到就会有编译错误。

动态分派就比较复杂点，因为不编译时不清楚哪个是调用者，比如调用 `Base()` 方法里的 `sayHello()` 方法，这个时期是不清楚谁是调用者的，直到运行时，才会知道有一个 `new Sub()` 对象，所以此时知道了调用者 Sub，然后应该先在 Sub 里进行方法匹配，如果匹配不到再去其父类找。巧了，这个例子 Sub 类中有 `sayHello()` 方法，所以会执行 Sub 中的方法，但是此时 name 还未到赋值阶段，所以最终输出为 null。

