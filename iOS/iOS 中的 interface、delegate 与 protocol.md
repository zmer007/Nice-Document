## 概要
两年前一次偶然的机会，公司分配给我了一台 Macbook，感觉特别新鲜。写 Android 代码之余偶尔也打开 xCode 写写 OC 代码体验一下不同语言（其实就是大概了解 OC 语法）。今天学习 kotlin 语言中的 by 关键字作用时，官方文档这样说

- delegates the implementation of an interface to another object
- delegates the implementation of accessors for a property to another object

这个 delegate 突然勾起了我对 OC 的回忆，然后发现我对 OC 中的 delegate 概念相当模糊，只记得和 interface、protocol 有关，反正也是学习，先把 OC 中的这三个概念搞清楚再来看 by 吧，所以就有了这篇文章。

## Objective-c 中的 interface 是什么
我主要从事 Android 开发，主语言是 Java，Java 中的 interface 是一个接口，是一种高度抽象类型，代表一种能力，比如某个类实现了某个接口，那么这个类就拥有了这个接口的功能，在上层调用中可以直接使用接口引用，从而实现了“程序要依赖抽象接口而不依赖具体实现”的原则。

OC 也有个 interface 的关键字，概念与 java 中的 interface 基本相同，但在实现上，它更像把 Java 中的普通类分成两部分（.h 和 .m），把抽象的头文件提供给外部使用，内部实现文件隐藏起来，比如下面是 OC 中的一个简单类
```obj-c
// Persion.h
#import <Foundation/Foundation.h>

@interface Person : NSObject

@property NSString *firstName;
@property NSString *lastName;

- (void) sayHello;

@end


// Persion.m
#import "Person.h"

@implementation Person
- (void)sayHello {
    NSLog(@"Hello, I'm %@, %@", _firstName, _lastName);
}

@end
```
这里的 `@interface` 与 `@implementation` 是相对应的，不能单独出现。在应用程序中需要使用 Persion 类时，只需要引用 `Persion.h` 文件就行了，初始化后调用 `sayHello` 方法时真正执行的是 `Persion.m` 文件中的 `sayHello`。

## protocol 和 delegate
protocol 与 Java 里的 interface 十分相似，代表一种协议，只要一个类遵守某个协议，就可以在调用端就可以直接使用协议通信，而不需要知道执行者是哪个。protocol 基本语法如下
```obj-c
@protocol ProtocolName
// 方法或属性，默认为必需（required），代表遵守 ProtocolName 协议的类必需实现这个方法，否则编译时会报警告，运行时极有可能会崩溃（调用者程序未做容错处理时）
@optional
// 可选方法，可以不实现可选方法，不会报警告
@end
```
protocol 可以放到任意头文件中，也可以独自一个头文件。比如以下自定义协议
```obj-c
// NotifyProtocol.h
@protocol NotifyMethodCalled <NSObject>
- (void)methodCalled;
@end
```
可以利用这个协议向 Persion 类中添加一个功能，比如，调用 `sayHello` 方法后向实现了此协议的对象发出一个调用结束的通知。Persion 类个性成以下样子
```obj-c
// Persion.h
#import <Foundation/Foundation.h>
#import "NotifyProtocol.h"

@interface Person : NSObject

@property id <NotifyMethodCalled> sayHelloCalled;

@property NSString *firstName;
@property NSString *lastName;

- (void) sayHello;

@end

// Persion.m
#import "Person.h"

@implementation Person

- (void)sayHello {
    NSLog(@"Hello, I'm %@, %@", _firstName, _lastName);
    
    // 避免空异常，避免出现调用未实现的可选协议方法
    if(_sayHelloCalled && [_sayHelloCalled respondsToSelector: @selector(methodCalled)]){
        [_sayHelloCalled methodCalled];
    }
}

@end
```
测试代码如下
```obj-c
@interface MyProtocolImpl : NSObject <NotifyMethodCalled>
@end

@implementation MyProtocolImpl

- (void)methodCalled {
    NSLog(@"did say Hello.");
}

@end

@implementation LearnObjCTests

- (void)testBob {
    _bob = [[Person alloc] init];
    [_bob setFirstName:@"Lee"];
    [_bob setLastName:@"Bob"];
    _bob.sayHelloCalled = [[MyProtocolImpl alloc]init];
    [_bob sayHello];
    
    XCTAssertEqual([_bob firstName], @"Lee");
}
```
输出如下
```
Hello, I'm Lee, Bob
did say Hello.
```
这个例子我都看吐了，不过 protocol 就是这么使用的。它不依赖任何指定类，任何类都可以实现它。它主要有两种用途：第一提供数据，比如约定好一个可能获取时间的协议，调用者只需要通过这个协议获取时间就可以了，不需要操心这个协议的实现者是从本地获取的时间还是在网络上获取的时间；第二个作用就是通知回调，比如上面的例子，还有系统中的 `UIApplicationDelegate` 协议。

下面说说 delegate 是什么个回事， delegate 不是一个关键字，它只是一种命名规范， protocol 用于通知时一般会命令 delegate。

**注意** protocol 在做数据源属性（Data source properties）和通知时，为了避免循环引用问题需要使用 `weak` 关键字修饰属性，比如 Persion 类中的 `sayHelloCalled` 属性就不合格，它应该这样写
```objc
@property (weak) id <NotifyMethodCalled> sayHelloCalled;
```

## 总结
通过代码示例清楚了 interface 在 OC 和 Java 中的区别，并且知道了如何定义 protocol，而 delegate 呢，它则是 protocol 在某个场景（方法回调）下的惯用名称。


## 参考
> [Working with Protocol](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithProtocols/WorkingwithProtocols.html#//apple_ref/doc/uid/TP40011210-CH11-SW1)