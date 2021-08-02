---
layout: post
title: Android混淆
toc: true
reward: true
date: 2020-11-07 16:38:19
tags: [混淆]
categories: [Android]
---
>代码混淆（Obfuscated code）亦称花指令，是将计算机程序的代码，转换成一种功能上等价，但是难于阅读和理解的形式的行为。

> 为什么要加代码混淆?
> 不想开源应用，为了加大反编译的成本,但是并不能彻底防止反编译

### 开启混淆
* 通常我们需要找到项目路径下app目录下的build.gradle文件
* 找到minifyEnabled这个配置,然后设置为true即可.
```
 release{
            minifyEnabled true//是否启动混淆 ture:打开   false:关闭
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
```
proguard-rules.pro文件的作用
* 只要在工程应用目录的gradle文件中设置minifyEnabled:true即可。然后我们就可以到proguard-rules.pro文件中加入我们的混淆规则了
  <!-- more -->
### proguard是什么?
* Proguard是一个集文件压缩,优化,混淆和校验等功能的工具
* 它检测并删除无用的类,变量,方法和属性
* 它优化字节码并删除无用的指令.
* 它通过将类名,变量名和方法名重命名为无意义的名称实现混淆效果.
* 最后它还校验处理后的代码

### 混淆的常见配置
* Proguard关键字

| Proguard关键字 | 描述 |
|--|----|
| dontwarn| dontwarn是一个和keep可以说是形影不离,尤其是处理引入的library时. |
| keep| 保留类和类中的成员，防止被混淆或移除 |
| keepnames| 保留类和类中的成员，防止被混淆，成员没有被引用会被移除|
| keepclassmembers| 只保留类中的成员，防止被混淆或移除|
| keepclassmembernames| 只保留类中的成员，防止被混淆，成员没有引用会被移除 |
| keepclasseswithmembers| 保留类和类中的成员，防止被混淆或移除，保留指明的成员|
| keepclasseswithmembernames| 保留类和类中的成员，防止被混淆，保留指明的成员，成员没有引用会被移除|

如：
(1)保留某个包下面的类以及子包
`-keep public class com.droidyue.com.widget.**`

(2)保留所有类中使用otto的public方法
```
# Otto
-keepclassmembers class ** {
    @com.squareup.otto.Subscribe public *;
    @com.squareup.otto.Produce public *;
}
```

(3)保留Contants类的BOOK_NAME属性
```
-keepclassmembers class com.example.admin.proguardsample.Constants {
     public static java.lang.String BOOK_NAME;
}
```

(4)dontwarn：
引入的library可能存在一些无法找到的引用和其他问题,在build时可能会发出警告,如果我们不进行处理,通常会导致build中止.因此为了保证build继续,我们需要使用dontwarn处理这些我们无法解决的library的警告.
```
#比如关闭Twitter sdk的警告,我们可以这样做
-dontwarn com.twitter.sdk.**
```
* Proguard通配符
  
| Proguard通配符 | 描述 |
|--|----|
| `<field>` | 匹配类中的所有字段|
| `<method>` | 匹配类中所有的方法 |
| `<init>` | 匹配类中所有的构造函数|
| * | 匹配任意长度字符，不包含包名分隔符(.)|
| **| 匹配任意长度字符，包含包名分隔符(.)|
| ***| 匹配任意参数类型|
| ...| ...|