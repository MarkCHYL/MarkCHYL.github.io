---
layout: post
title: Kotlin基础入门学习笔记
toc: true
reward: true
date: 2019-12-03 09:51:56
tags: [kotlin]
---
若不留下点啥笔记，感觉对不住自己一个星期半的学习加复习。
### 一、如何实现单列模式？
使用关键字 **object** 修饰的类,替换掉class关键字，便会在整套程序中生成一个且是唯一的对象实例。
譬如如下代码：
```
object BigHeadSon:IWash {
    override fun wash() {
        println("大头儿子洗完收费1元钱")
    }
}
```
那样之后该类的实例变量整个项目中有且只有一个。
<!-- more -->
***
### 二、如何实现枚举？
实现枚举的代码如下：
```
enum class Week {
    星期一, 星期二, 星期三, 星期四, 星期五, 星期六, 星期日
}

fun main(args: Array<String>) {
    println(Week.星期一.ordinal)
}
```
使用关键字 enum 修饰类
***
### 三、如何实现印章类？
举个例子一匹母驴、一屁公马和一匹公驴一碗酒后乱性，过了几天母驴怀孕了，不只是公马的还是公驴的。那么问题来了马和驴的后代是骡子，母驴和公驴的后代当然还是驴。
就像盖了章一样，使用**sealed**关键字修饰类,可以指定子类和个数。
```
// 印章sealed类
//指定子类种类和个数

sealed class Son {
    fun sayHello(){
        println("大家好！！！")
    }
    class smallBlackMule():Son()

    class smallDonkey():Son()
}

fun main(args: Array<String>) {
    var s1: Son = Son.smallBlackMule()

    var s2: Son = Son.smallBlackMule()

    var s3 = Son.smallDonkey()

    var list = listOf<Son>( s1, s2, s3 )

    for ( h in list){
        if(h is Son.smallDonkey) {
            h.sayHello()
        }
    }
}
```
***
### 四、如是实现委托和代理？
利用关键字 **by** 实现，示例代码如下：
```
class SmallHeadFather : IWash by BigHeadSon() {
    override fun wash() {
        println("小头爸爸洗完收费10块钱！！！")
        BigHeadSon().wash()
        println("小头爸爸看见儿子把碗洗好了")
    }
}
```
***
### 五、如何实现多态和继承？
kotlin是面向对象的语言，子类可以继承父类的方法和属性。
**抽象类**：如父类，同一类具有同样基本属性和方法的类事物的描述，事物的本质描述。
**接口**：指具有同样基本属性和方法的能力，事物的能力描述。
```
//抽象类
abstract class Human(var name:String) {
    abstract fun eat()

    abstract fun pee()
}

//男人
class Man(name: String) : Human(name) {
    override fun eat() {
        println("${name}大口的吃饭,暴力")
    }

    override fun pee() {
        println("${name}站着尿尿---")
    }

}
//女人
class Woman(name: String) :Human(name) {
    override fun pee() {
        println("${name}蹲着尿尿---")
    }

    override fun eat() {
        println("${name}小口慢慢的吃饭，文雅")
    }
}
//测试
fun main(args: Array<String>) {
    var person1 = Man("金三胖")
    var person2 = Woman("波多野结衣")
    var person3 = Woman("貂蝉")
    var person4 = Man("吕布")

    var humanList = listOf<Human>(person1,person2,person3,person4)

    for (h in humanList){
        h.name
//        h.eat()
        h.pee()
    }

}
```
***
### 六、when的表达式：
```
//考试成绩评级10分 9分A 8分B 7分C

fun scoreMatch(score:Int):String{
	when(score){
		10 -> return "恭喜你拿了满分S"
		9 -> return "恭喜你拿了A"
		8 -> return "恭喜你拿了B"
		7 -> return "你拿了C，还需更加努力"
		else -> return "对不起你被OUT啦！"
	}
}

fun main(agrs:Array<String>){
	println(scoreMatch(10))
}
```
***
### 七、空值判断：
加上问好代表参数可以为空
```
//接收一个参数，参数为非空的String类型   
fun heat(str:String?):String{
	return "热${str}"
}

fun main(agrs:Array<String>){
	var result1 = "油"
	println(heat(result1))
	
	var result2 = null
	println(heat(result2))
}
```
***
### 八、函数表达式
```
fun main(args:Array<String>){
	//函数
	var result = add(3,4)
	println(result)
	
	//函数表达式
	var i = {x:Int,y:Int -> x+y}
	var result2 = i(3,4)
	var temp = result2==(7)
	println("${temp}")

}

fun add(a:Int,b:Int):Int = a+b
```
***
### 九、递归演示：
**BigInteger**函数是Java数值运算大值时使用的属性，特殊的超级**Int**
```
//演示一
import java.math.BigInteger

// 递归演示
// 阶乘  5的阶乘：5*4*3*2*1
fun main(args:Array<String>){
	///BigInteger是一个不可描述超大的数字
	println("请输入数字：")
	var a = readLine()
    try{
		var b:BigInteger = a!!.toBigInteger()
		//阶乘
		println(fact(b))
	}catch(e:Exception){
		println("输入数字有误！！")
	}
}

fun fact(a:BigInteger):BigInteger{
	if(a==BigInteger.ONE)
	    return BigInteger.ONE
	else
		return a*fact(a-BigInteger.ONE)
}
```
尾递归 **tailrec**修饰方法,譬如超过电脑运算周期范围的计算
```

fun main(args:Array<String>){
	var a = readLine()!!.toInt()
	var num:Int = 0
	var result:Int = 0
	println(add(a,num,result))
}

//尾递归 tailrec修饰方法
tailrec fun add(nuk:Int,nn:Int,result:Int):Int{
	var nns = nn+1
	var results = result + nuk
	println("计算机第${nns}次运算,结果为：${results}")
	if(nuk==1)
		return results
	else
		
		return add(nuk-1,nns,results)
}
```
***
### 十、List和Map
List演示：
```
 fun main(args:Array<String>){
	 var lists = listOf("买鸡蛋","买可乐","买猪肉","买大米")
	 
	 for(a in lists){
		 println(a)
		}
	 
	 for((i,e) in lists.withIndex())
		 println("$i,$e")
	
}
```
Map演示：
```
import java.util.TreeMap

fun main(args:Array<String>){
	//TreeMap 同一key的value不能重复存在
	var map = TreeMap<String,String>()
	var list = listOf("好","好","学习","天","天","向上")
	
	map["好"] = "Good"
	map["学习"] = "Study"
	map["天"]  = "Day"
	map["向上"]  = "Up"
	
	for(a in list){
		print("${map[a]}  ")
	}
}
```
***
### 十一、人机交互
```
fun main(args:Array<String>){
	println("请输入第一个数字：")
	var c = readLine()
	println("请输入第二个数字：")
	var d = readLine()
	
	try{
		var a = c!!.toInt()
	    var b = d!!.toInt()
		println("${a} + ${b} = ${a + b}")
	}catch(e:Exception){
		println("输入数据有误！，请重新输入")
	}
	
}
```
**!!.**确保不为空
**?.**可为空

