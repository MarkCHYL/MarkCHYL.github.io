---
layout: post
title: kotlin学习笔记第一章
toc: true
reward: true
date: 2019-11-19 09:28:16
tags: [Android,Kotlin,学习笔记]
categories: [Android]
---
## 第一章 搭建 Kotlin 开发环境
****
### 一、Kotlin简介：
Kotlin是一种基于JVM的新型编程语言，它完全兼容Java语言。
[慕课网电子书](https://songyubao.com/book/primary/lesson-summary.html)

### Kotlin 优先意味着什么？
在构建新的 Android 开发工具和内容（例如 Jetpack 库、示例、文档和培训内容）时，google会在设计层面考虑到 Kotlin 用户

| |	Java 语言	|Kotlin|
|-|-|-|
|平台 SDK 支持|	是	|是|
|Android Studio 支持|	是	|是|
|Lint	|是	|是|
|==引导式文档支持|	是|	是|
|API 文档支持	|是	|是|
|AndroidX 支持	|是	|是|
|AndroidX Kotlin 特有 API（KTX、协程等）|	无	|是|
|在线培训	|尽力而为	|是|
|示例|	尽力而为|	是|
|多平台项目|	否	|是|
|Jetpack Compose|	否	|是|

<!-- more -->

### Kotlin能做什么?
如果对 Kotlin 的能力仅仅停留在 JVM平 台，那是片面的。如今的 Kotlin 已经从当初的更好 Java 目标完成了它华丽的大变身，他们的目标已经瞄准了星辰大海。目前 Kotlin 可以适用于**移动端跨平台、原生 JVM、服务端开发、Web 开发、Android 开发、数据科学**等多个领域。此外近年来 Kotlin 团队已经将重心转移到了语言层面的跨平台，多平台的支持。
![](https://songyubao.com/book/primary/kotlin/kotlin-ablility.png)

### Kotlin与Java的异同
**打印日志**
> Java 

```java
System.out.print("hello world"); 
System.out.println("hello world");
```
> Kotlin 
```kotlin
print("hello world") println("hello world") 
```

**定义变量与常量常**
> Java 
```java
String name = "hello world";
final String name = "hello world";
```
> Kotlin 
```kotlin
var name = "hello world" 
val name = "hello world" 
```
**null声明**
> Java 
```java
String otherName; 
otherName = null;
```
> Kotlin 
 ```kotlin
 var otherName : String? 
 otherName = null 
 ```
**空判断**
> Java 
 ```java
if (text != null) { 
    int length = text.length();
}
```
> Kotlin 
 ```kotlin 
text?.let { 
    val length = text.length
} // 
or 
simply val length = text?.length 
```
**字符串拼接**
> Java 
 ```java 
String firstName = "Android"; 
String lastName = "Architect"; 
String message = "My name is: " + firstName + " " + lastName; 
```
> Kotlin
 ```kotlin
 val firstName = "Android" 
 val lastName = "Architect" 
 val message = "My name is: $firstName $lastName" 
```
**换行** 
> Java 

 ```java 
 String text = "First Line\n" + "Second Line\n" + "Third Line";
 ```
> Kotlin 

 ```Kotlin
 val text = """ |First Line |Second Line |Third Line """.trimMargin() 
 ```
**三元表达式**
> Java 

```java
String text = x > 5 ? "x > 5" : "x <= 5";
 ```
> Kotlin 
```kotlin 
val text = if (x > 5) "x > 5" else "x <= 5" 
```
**操作符**
> java 
```
final int andResult = a & b; 
final int orResult = a | b; 
final int xorResult = a ^ b; 
final int rightShift = a >> 2;
final int leftShift = a << 2; 
final int unsignedRightShift = a >>> 2;
```
>  Kotlin 
```
val andResult = a and b 
val orResult = a or b 
val xorResult = a xor b 
val rightShift = a shr 2 
val leftShift = a shl 2 
val unsignedRightShift = a ushr 2 
```
**类型判断和转换 (声明式)**
> Java 
```
Car car = (Car) object;
```
> Kotlin 
```
var car = object as Car 
```
**类型判断和转换 (隐式)** 
> Java 
```java
if (object instanceof Car) {
    Car car = (Car) object; 
} 
```
> Kotlin
```kotlin
if (object is Car) {
    var car = object // 自动识别 
}
```
**多重条件** 
> Java 
```java
if (score >= 0 && score <= 300) { } 
```
> Kotlin 
```
if (score in 0..300) { } 
```
**更灵活的case语句**
> Java
```java
int score = // some score;String grade; 
switch (score) { 
    case 10: 
    case 9: 
    grade = "Excellent"; 
    break; 
    case 8: 
    case 7: 
    case 6: 
    grade = "Good"; 
    break; 
    case 5: 
    case 4: 
    grade = "OK";
    break; 
    case 3:
    case 2:
    case 1:
    grade = "Fail";
    break;
    default:
    grade = "Fail";
} 
```
> Kotlin
```kotlin
var score = // some score 
var grade = when (score) { 
    9, 10 -> "Excellent"
    in 6..8 -> "Good" 
    4, 5 -> "OK" 
    in 1..3 -> "Fail"
    else -> "Fail"
}
```

**for循环**
> Java
 ```java
 for (int i = 1; i <= 10 ; i++) { } 
 for (int i = 1; i < 10 ; i++) { } 
 for (int i = 10; i >= 0 ; i--) { } 
 for (int i = 1; i <= 10 ; i+=2) { } 
 for (int i = 10; i >= 0 ; i-=2) { } 
 for (String item : collection) { }
 for (Map.Entry entry: map.entrySet()) { } 
 ```
> Kotlin
 ```lotlin
 for (i in 1..10) { }
 for (i in 1 until 10) { }
 for (i in 10 downTo 0) { }
 for (i in 1..10 step 2) { }
 for (i in 10 downTo 0 step 2) { }
 for (item in collection) { }
 for ((key, value) in map) { }
 ```
**更方便的集合操作**
> Java 
 ```java
 final List listOfNumber = Arrays.asList(1, 2, 3, 4); 
 final Map keyValue = new HashMap();
 map.put(1, "Android"); 
 map.put(2, "Ali");
 map.put(3, "Mindorks"); // Java 9 
 final List listOfNumber = List.of(1, 2, 3, 4);
 final Map keyValue = Map.of(1, "Android", 2, "Ali", 3, "Mindorks");
 ```
> Kotlin
 ```koltin
 val listOfNumber = listOf(1, 2, 3, 4)
 val keyValue = mapOf(1 to "Android", 2 to "Ali", 3 to "Mindorks")
 ```
**遍历**
> Java
 ```java
 // Java 7 and below 
 for (Car car : cars) { 
     System.out.println(car.speed); 
 }
 // Java 8+ 
 cars.forEach(car -> System.out.println(car.speed)); 
 // Java 7 and below 
 for (Car car : cars) {
     if (car.speed > 100) {
         System.out.println(car.speed);
     }
 }
 // Java 8+ 
 cars.stream()
     .filter(car -> car.speed > 100)
     .forEach(car -> System.out.println(car.speed)); 
 ```
> Kotlin
 ```kotlin
 cars.forEach { println(it.speed) } 
 cars.filter { it.speed > 100 } 
     .forEach { println(it.speed)} 
 ```
**方法定义**
> Java 
 ```java
 void doSomething() { 
    // logic here
 }
 void doSomething(int... numbers) { 
    // logic here 
 }
 ```
> Kotlin

 ```kotlin
 fun doSomething() { 
     // logic here 
 } 
 fun doSomething(vararg numbers: Int) {
     // logic here 
 }
 ```

**带返回值的方法**

> Java 
 ```java
 int getScore() { 
    // logic here 
    return score; 
 }
 ```
> Kotlin
 ```kotlin
 fun getScore(): Int {
    // logic here 
    return score 
 }
 // as a single-expression function 
 fun getScore(): Int = score
 ```
**无结束符号**
> Java 
 ```java
 int getScore(int value) { 
    // logic here 
    return 2 * value; 
 }
 ```
 > Kotlin
 ```kotlin
 fun getScore(value: Int): Int {
    // logic here 
    return 2 * value 
 } 
 // as a single-expression function 
 fun getScore(value: Int): Int = 2 * value 
 ```
**constructor 构造器** 
> Java 
 ```java
 public class Utils { 
    private Utils() {
         // This utility class is not publicly instantiable 
    } 
    public static int getScore(int value) {
        return 2 * value; 
    } 
 }
 ```
> Kotlin
 ```kotlin
 class Utils private constructor() {
    companion object {
        fun getScore(value: Int): Int {
            return 2 * value
        }
    }
 } 
 // another way 
 object Utils {
    fun getScore(value: Int): Int {
        return 2 * value
    }
 }
 ```
 **Get Set 构造器**
 > Java 
  ```java
  public class Developer {
    private String name;
    private int age;
    public Developer(String name, int age) {
        this.name = name; this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
 }
 ```
> Kotlin
 ```kotlin
 data class Developer(val name: String, val age: Int)
 ```