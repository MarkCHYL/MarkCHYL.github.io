---
layout: post
title: Kotlin中可变参数
toc: true
reward: true
date: 2021-04-26 09:22:07
tags: [Kotlin基础]
categories: [Kotlin]
---
[原文简书](https://www.jianshu.com/p/174c0e254713)
***
### 对比 Java 中的可变参数
先看下 Java 的可变参数，用我们最熟悉的 main 函数
```Java
public static void main(String... args) {
}
```
从 Java5 开始引入了可变参数（varargs）
对应的 Kotlin 的代码，参数为可变参数：
```Kotlin
fun main(vararg args: String) {
}
```
<!-- more -->
Java代码中参数应该是数组：
```Java
public static void main(String[] args) {
}
```
对应的 Kotlin 的代码，也是两种方式，参数为数组：
```Kotlin
fun main(args: Array<String>){

}
```
***
### 可变参数的本质
```Kotlin
fun main() {
    foo("1", "2", "3")
}

fun foo(vararg args: String) {
    println(args::class)
    println(args.contentToString())
    for (i in args.indices) {
        println(args[i])
    }
}
```
打印：
```Kotlin
class kotlin.Array
val kotlin.reflect.KClass<T>.java: java.lang.Class<T>
[1, 2, 3]
1
2
3
```
我们可以清晰的看到 args 的类型为数组类型，并且可以直接调用数组的方法。
> 如果你的第一行打印结果是：
> 
> class [Ljava.lang.String; (Kotlin reflection is not available)
> 
> 需要在 build.gradle 中添加依赖：
> 
> implementation "org.jetbrains.kotlin:kotlin-reflect:$kotlin_version"

****

****准确的说 args 的类型是 Array<out String> 类型:****
```Kotlin
fun main() {
    foo("1", "2", "3")
}

fun foo(vararg args: String) {
    bar(args)
}

fun bar(args: Array<out String>) {
    println(args.contentToString())
}
```
*** 

### 可变参数的传参
我们再来改造一下 bar 函数的参数类型，看下可变参数的传参：
```Kotlin
fun main() {
    foo("1", "2", "3")
}

fun foo(vararg args: String) {
    bar(args)
}

fun bar(vararg args: String) {
    println(args.contentToString())
}
```
编译器提示类型不匹配，需要一个 String 类型的参数，而传入了数组类型。
然而在 Java 中可变参数是可以直接传递，并且可以和数组相互转换传递：
```Java
public static void main(String[] args) {
    foo("1", "2", "3");
}

private static void foo(String... args) {
    bar1(args);
    bar2(args);
}

private static void bar1(String... args) {
    System.out.println(Arrays.toString(args));
}

private static void bar2(String[] args) {
    bar1(args);
}
```
在 Kotlin 中如果想将数组类型传入到可变参数，就需要使用一个特定的符号 `* `：
```Kotlin
fun main() {
    foo("1", "2", "3")
}

fun foo(vararg args: String) {
    bar1(*args)
    bar2(args)
}

fun bar1(vararg args: String) {
    println(args.contentToString())
}

fun bar2(args: Array<out String>) {
    bar1(*args)
}
```
### 总结
我对 Kotlin 中d可变参数的理解是：
1. 可变参数会在函数体中，自动转变为数组类型
2. 数组类型不能作为参数，直接传递给可变参数
3. 在数组类型前面添加 * ，可以传递给可变参数
   
最后，我们可以反编译看下 Kotlin 中的 foo 函数，看看 * 到底做了什么：
```Kotlin
public static final void foo(@NotNull String... args) {
    bar1((String[]) Arrays.copyOf(args, args.length));
    bar2(args);
}
```
就是一个复制数组的操作，相比 Kotlin 还是 Java 做的更便捷，可以在数组和可变参数之间直接自由转换。

