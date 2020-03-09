## kotlin 范围函数（Scope Functions）
### 概述
范围函数是一种能作用在某个对象上的 block（代码块，也可以叫闭包或匿名函数），可以在代码块中方便的引用目标对象从而达到简化代码逻辑、减少中间变量的功效。Kotlin 标准库中共有 5 个范围函数 `let, run, with, apply, also`，不同的情境使用不同的函数可以减少很多体力活，不过对于我等英语水平不高的渣渣来说，仅从单词语义来看完全不知道怎么区分它们五个。如果实在分不开就不用管了它们了，直接丢掉，用最原始的 kotlin 语法照样可以把功能写出来。我之前就是这么干的，每次看完它们的定义就知道怎么用了，过一段时间不写 kotlin 又忘它们都是干啥的了，但是看许多开源代码都大量使用范围函数，所以就想写篇博客巩固一下。

### 如何区别
这五个范围函数在作法上十分相似，要想区分它们主要得从下面两个方面入手

1. 函数内引用目标对象的方式
2. 函数返回值

可以通过下面代码，观察 let 与 apply 在这两特性上的表现

```kotlin
@Test
fun callScopeFunctions_referenceAndReturnValue() {
    val personBob = Person(18, "bob", "China").let {
        // it 代表 personBob 对象
        it.name = "Bob"
        println(it)
        it // 在 block 最后写一个值代表此 block 的返回值
    }
    assertEquals(personBob.name, "Bob")

    val personAlice = Person(17, "alice", "USB").apply {
        // 可以在此 block 中直接访问 person 成员属性
        name = "Alice"
        print(this)
    }
    assertEquals(personAlice.name, "Alice")
}

// Person 定义
class Person(
        var age: Int,
        var name: String?,
        var address: String?
) {
    override fun toString(): String {
        return "I'm $name, $age years old, and come from $address"
    }
}
```
可以显示看出在 let 函数内部引用 personBob 时需要使用 `it` 来完成，即 `it` 相当于 `personBob` 对象，let 块结束位置中写了个 `it` 完全是为了说明 let 块的返回值是块中最后一个值，如果最后的 it 的话，personBob 就会成 `Unit` 对象了，因为 `println(it)` 没有返回值或返回值为 null，在 kotlin 中 null 就是 Unit。

在 apply 函数中，this 就代表 personAlice 对象，这里的 this 可以省略，所以就变成了在 apply 函数内可以直接操作 Person 的成员变量了，apply 块的最终返回值是 this 对象，不受最后一行代码的影响。

这里主要对引用方式和返回值上简要说明了范围函数的区别方式，下面以应用角度分析这些函数的作用。

### 应用场景
本节提及的示例仅是部分应用场景，仅供大家参考，为了明确区分函数，在每个函数开始都会说明如何引用对象以及返回值是什么。

#### let
**引用对象为 it，返回值为 block 最后一行代码的值（专业叫法为“lambda 返回值”）**

**场景 1：链式调用**

因为返回值为最后一行代表的值（记为 lambdaResult），所以可以在 block 外部直接引用 lambdaResult 了，就是说可以这链式调用了，比如
```kotlin
objA.let{
    // blabla
    objB 
}.let{ 
    // blabla
    objC 
}.let ...
```
拿个具体的例子就是以前这样写
```kotlin
val numbers = mutableListOf("one", "two", "three", "four", "five")
val resultList = numbers.map { it.length }.filter { it > 3 }
println(resultList)  
```
现在可以这样写了
```kotlin
val numbers = mutableListOf("one", "two", "three", "four", "five")
numbers.map { it.length }.filter { it > 3 }.let { 
    println(it)
    // 可以在此处添加另外一个对象，然后
}// 在这里接着用 .let{} 继续处理
```
看，中间变量 `resultList` 不见了。

**场景 2：对象非空才执行 block**

在 kotlin 中，为了规避空指针异常引入了可选型（optional），可选型避免出现以下语句
```kotlin
var someStr: String = null // 会出现编辑错误（"Null can not be a value of a non-null type String"）
```
这样就在编辑阶段必须给 `someStr` 赋值，从而保证 `someStr` 不会出现空指针异常，但是有时确实不能确定 `someStr` 的值，所以还得给它一个 null 值，可以使用可先型赋值，比如
```kotlin
var someStr: String? = null
```
下次在使用 `someStr` 时，比如想获取它的长度，直接调用 `someStr.length` 就会出现语法错误（"Only safe(?.) or non-null asserted calls are allowed on a nullable receiver of type String?"），这时 let 就可以排上用场了
```kotlin
someStr?.let {
    // 如果 someStr 为 null，则此 block 不会执行，且返回值为 null
    it.length
}
```
**注意** 在这种情况下链式调用的话，需要对 `?.` 以后的 `let` 全添加有 `?.` 或调用 `let` 块中的 `it` 时使用 `?.`，比如
```kotlin
someStr?.let {
    it.length
    personBob // Person 对象
}?.let {
    val name = it.name
    // blabla
}

// 或着下面这种写法
someStr?.let {
    it.length
    personBob // Person 对象
}.let {
    val name = it?.name
    // blabla
}

```

**场景 3：替换对象名，增强代码可读性**

没啥好说的，就是把换换变量名，把 `it` 换成其它名字，使其更贴合主义，比如
```kotlin
val p = Person(18, "Bob", "China")
p.let { bob -> // 使用 bob 代替 it，它比 p 主义更清楚（瞎写的，有可能没 p 主义清楚，但大概就是这么个意思） 
    val name = bob.name 
}
```

### with
**目标对象通过 `with(obj)` 传入，block 内部使用 `this` 引用，返回值为 lambda 返回值**

**场景 1：表示使用这个对象，然后做下面这些事情**

这时一般不会用到 lambda 返回值，比如 Android 初始化 WebView 然后使用 Settings 支持 JS 代码
```kotlin
val webView = WebView(ctx)
with(webView){
    settings.javaScriptEnabled = true
}
```

**场景 2：操作对象里的值并返回结果**
```kotlin
val numbers = mutableListOf("one", "two", "three")
val firstAndLast = with(numbers) {
    first() + last()
}
println(firstAndLast) // 输出结果为：onethree
```
就是简化了 block 内的对象公开变量的调用，感觉没啥大用。

### run
**引用对象为 this，返回值为 lambda 返回值**

**场景 1：基本与 let 一样**

与 let 的不同在于 `it` 变成了 `this`

**场景 2：作为非扩展函数（non-extension function）**
```kotlin
val hexNumberRegex = run {
    val digits = "0-9"
    val hexDigits = "A-Fa-f"
    val sign = "+-"

    Regex("[$sign]?[$digits$hexDigits]+")
}

for (match in hexNumberRegex.findAll("+1234 -FFFF not-a-number")) {
    println(match.value)
}
```
意思就是这个意思，感觉这个有点把代码搞不清晰了，我看着下面的代码更舒服些
```kotlin
for (match in hexNumberRegex().findAll("+1234 -FFFF not-a-number")) {
    println(match.value)
}

fun hexNumberRegex(): Regex {
    val digits = "0-9"
    val hexDigits = "A-Fa-f"
    val sign = "+-"

    return Regex("[$sign]?[$digits$hexDigits]+")
}
```

### apply
**引用对象为 this，返回对象也为 this**

参考 “如何区别” 小节的例子，这个也可以做到链式调用，不过调用对象都是原对象本身。

### also
**引用对象为 it，返回对象为 this**
```kotlin
val numbers = mutableListOf("one", "two", "three")
numbers
    .also { println("The list elements before adding new one: $it") }
    .add("four")
```
总感觉这个函数就是来凑数的，完全可以用 apply 哇。

### 总结
五种范围函数作用及应用场景分析完了，其实它们就完成了两个功能：第一，改变原对象引用，减少无关代码或增强可读性；第二，返回某个值供链式调用。它们各自的特点如下表所示
|函数|引用|返回值|是否支持扩展（链式调用）|
|:---|:---:|:---:|:---|
|let|it|lambda 返回值|是|
|run|this|lamdba 返回值|是|
|run（场景 2）|-|lambda 返回值|否|
|with|this|lambda 返回值|否|
|apply|this|this|是|
|also|it|this|是|

### 参考
> https://kotlinlang.org/docs/reference/scope-functions.html