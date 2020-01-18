[详细对比](https://www.cnblogs.com/jlutiger/p/10548291.html)
## 线程安全的实现方法

### 互斥同步
共享变量同一时刻只被同一个线程使用。现实互斥同步的方式有

1. 临界区
2. 互斥量
3. 信号量

Java 中最基本的互斥操作为 synchronized 关键字。在 JUC 包里的 ReentrantLock 与 synchronized 关键字一样也可以实现互斥同步操作，但 ReentrantLock 还有提供了其它功能：

1. 等待可中断（使用 reentrantLock.tryLock(...)，如何返回 false 证明加锁失败，此时可以选择去执行其它操作）
2. 公平锁与非公平锁（默认为公平锁，synchronized 只支持非公平锁）
3. 锁绑定多个条件（可能绑定多个 Condition 对象实现多个条件锁，synchronized 只支持一个条件）


### 注意

synchronized 修饰方法时，不会对方法参数对象加锁，所以如果有多个方法对一个外部对象操作，必须把所有方法都使用 synchronized 方法修饰，这种情况最好不要对方法加锁，要针对目标对象加锁。

对目标对象加锁时要确保该对象为 final 对象，非 final 对象有可能会造成该对象中途变化，比如对变量重新赋值或置为 null，这样会导致加锁对象不一致，使锁失效。