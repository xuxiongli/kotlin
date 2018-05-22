#Kotlin

[TOC]

基于JVM（规范，虚拟机）的编程语言

安卓第一编程语言

##Kotlin特点：

1. 简洁
2. 空值安全
3. 100%兼容java
4. 函数式编程jdk1.8 lambda表达式
5. 协程（thread相比线程更加轻量）
6. DSL（领域特定语言）

##Kotlin前景
1. Kotlin scripy（gradle）.kts
2. java虚拟机应用
3. 前端开发
4. 安卓开发
5. IOS开发
6. kotlin Native（完全抛弃JVM虚拟机）
7. 全栈工程师

##参考资料
1. 官方文档
2. Kotlin源码（github）
3. Kotlin的官方博客

##开发工具
1. IDEA
2. eclipse
3. Android Studio

#你好，世界

```kotlin
fun main(args array:String){

	println("Hello World")

}
```

**fun 是函数声明**

**main 主函数、入口、顶层函数**

**args 变量名**

**array：String 字符串数组**

**println 输出语句**


##kotlin的基本数据类型


```kotlin
fun main(args: Array<String>) {
    println("Byte型最大值："+Byte.MAX_VALUE)
    println("Byte型最小值："+Byte.MIN_VALUE)
    var b:Boolean=true
    println("布尔类型："+b)
    var char:Char='a'
    println("字符型："+char)
    println("短整型最大值："+Short.MAX_VALUE)
    println("短整型最小值"+Short.MIN_VALUE)
    println("整型最大值："+Int.MAX_VALUE)
    println("整型最小值"+Int.MIN_VALUE)
    println("单精度浮点型最大值："+Float.MAX_VALUE)
    println("单精度浮点型最小值："+-Float.MAX_VALUE)
    println("长整型最大值："+Long.MAX_VALUE)
    println("长整型最小值："+Long.MIN_VALUE)
    println("双精度浮点型最大值："+Double.MAX_VALUE)
    println("双精度浮点型最小值："+-Double.MAX_VALUE)
}
```

**这里需要注意的是浮点型类型，最小值是他们的最大值加一个负号，而不是.MIN_VALUE。**

**kotlin里面没有包装数据类型，系统自动判断需要使用哪种。**

##定义一个变量

```kotlin
fun main(args:Array<String>){
	var a:Int=10
	var b=10
	var c="张三"
	var d='a'
	var e:Byte=10
	var f=10L//Long
}
```

kotlin编译器自动推断是什么类型，智能类型推断，他是类型安全的语言：类型一旦确定不能改变。

##不同数据类型的转换
**JAVA可以，但是Kotlin不可以：小的可以赋值给大的，大的不能赋值给小的**

###String转换Int

```kotlin
var m=10
var s="10"
println(s.toInt())
s.toByte
```

通过to方法转换成基本数据类型或相互转换

###总结语言特性

kotlin：最严格的类型检查

java：小的赋值给大的

js：最宽松

###变量与常量

var变量，可以修改

```kotlin
var a:Int=10
a=20
```

val不可变变量，不可以修改,运行时常量，编译器在编译时不能知道它的值，在编译时会用他的变量名。加上const val就能得到编译期常量，在编译时会把对他的所有引用变成它的值。
​	
```kotlin
val b:Int=20（不可以修改，只可以在反射中进行修改，类似于java中的private static final）
```

项目开发中尽量使用val，实在不能用val再使用var。

##字符串
###原样输出字符串
```kotlin
fun main(args:Array<String>){
	val s="""
		广东省
		深圳市
		宝安区
	""".trimIndent
	println(s)
}
```
###字符串函数
1. trim() 删除空格
2. trimMargin("") 删除xx和前面空格
3. equals() 比较字符串值是否相等
4. == 比较字符串值是否相等
5. === 比较地址值
6. split(，) 多条件切割，集合接收
7. subString()
8. subStringBefore()
9. subStringBeforeLast()
10. subStringAfter()
11. subStringAfterLast()

##元组数据
定义二元组

```kotlin
val pair=Pair<String,Int>("张三",20)
println(pair.first)
println(pair.second)
```

定义三元组

```kotlin
val triple=Triple<"李四"，20，"12345678">
println(triple.first)
println(triple.second)
println(triple.third)
```

##空值处理
val str:String 非空类型
val str:String? 可空类型
str!!.toInt 告诉编译器不要检查了，一定不为空，还是会报空指针异常，不建议使用
str?.toInt() 空安全调用符
类似于：

```kotlin
if(str!=null){
	return str.toInt()
}else{
	return null
}
```

```kotlin
Elvis操作符 ?:
```

```kotlin
val b:Int=str?.toInt?:-1
```

类似于


```kotlin
if(str!=null){
	return str.toInt()
}else{
	return -1
}
```

##输入输出

println输出
readline输入

#函数

无参无返回值
有参无返回值
无参有返回值
有参有返回值

Unit相当于void

##顶层函数和嵌套函数

顶层函数：独立于对象存在，不依赖于class类单独存在的函数
嵌套函数：函数中可以再有函数

##字符串模板

占位符：${XXX}

##条件控制语句

* 普通if/else语句

```kotlin
	if(a>b){
		return a-b
	}else{
		return b-a
	}
```

* 去掉{}的if/else


```kotlin
	if(a>b)
		return a-b
	else
		return b-a
```

* 精简if/else

```kotlin
	if(a>b) return a-b else return b-a
```

* if语句返回值

```kotlin
	return if(a>b) a-b else b-a
```

**java是声明式语法，无返回值**

**kotlin是表达式语法，有返回值**

##For循环和Foreach循环

* **字符串遍历 数组遍历 集合遍历**

* 普通for循环

```kotlin
	for(c in s){
		println("c=$c")
	}
```

* 加上index角标的for循环

```kotlin
	for((index,c) in s.withIndex()){
		println("index=$index c=$c")
	}
```

* 普通foreach

```kotlin
	s.forEach{
		print("it=$it")
	}
```

* 加上index角标的foreach


```kotlin
	s.forEachIndexed{
		println("$c的角标是$index")
	}
```

##continue和break
* Continue跳出这次循环，后面的还会执行
* 不可以在foreach中使用，只能用在for循环中

```kotlin
	var newstr="abcde"
	for(c in newstr){
		if(c=='c') continue
		println(c)
	}
```

输出abde 不输出c

* Break跳出循环，循环停止


```kotlin
	for(c in newstr){
		if(c=='c') break
		println(c)
	}
```

只输出ab后面不会输出

##标签处返回
* 多重循环
* abc和123所有的组合


```kotlin
	loop@ for (i:Char in s1) {
		for (j:Char in s2){
			if(i=='b' && j=='f'){
				//break到指定标签
				break@loop
			}
			println("i = $i, j = $j")
		}
	}
```

##while和do while
* 打印小于100的整数
* while循环（满足条件才会执行）

```kotlin
	while(x>0){
		println("x=$x")
		x--
	}
```

* do while循环（不满足条件也会执行一次）

```kotlin
	do{
		println("x=$x")
		x--
	}while(x>0)
```

##区间Range
* 任何程序的本质都是对数据的处理
* 区间：一段实数的范围 1到100
* 开区间（1,100）不包含1和100
* 闭区间[1,100] 包含1和100
* 半开半闭区间[100,100）包含1不包含100
* kotlin区间：1到100 'a'到'z'
* 定义数据类型保存1到100的数据
* 常见区间：IntRange CharRange LongRange
* 区间创建

intRange(1,10]
1..100[1,100]
1 until 100[1,100)
charRange（'a','z'）
a.Range To('z')

* 区间遍历：for foreach

##反转区间和区间反转
* 反转区间：10 downTo 5
* 区间反转：IntRange.reversed()

##数组的定义
* 创建数组：arrayOf("haha",10,10)
* 创建基本数据类型
* 数组类型可以是any
* IntArray（size 10）
* IntArray（size 10）{30}初始化
* 八种基本数据类型的数组可以通过Array创建也可以通过自己的数据类型Array创建

```kotlin
	IntArrayOf(10,20,30)
	BooleanArrayOf(true,false,true)
	ByteArrayOf(10,20)
	注意：没有StringArrayof
```

* 基本数据类型数组类型

```kotlin
	val intArr =IntArray(10)//定义元素个数10个的Int数组
	val charArray = CharArray(10)
```

* 定义几根数据类型数组并初始化

```kotlin
	val intArr = IntArray(10){
		30
	)//定义了一个长度为10的Int数据类型并且所有元素都为30
```

数组遍历

* for循环遍历

```kotlin
	val array = arrayOf("张三","李四","王五")
	for(s in array){
		println(s)
	}
```

* for循环遍历元素以及角标

```kotlin
	for((index,s) in array.withIndex()){
		println("角标$index s=$s")
	}
```

* foreach循环

```kotlin
	array.forEach{
		println(it)
	}
```

* foreach遍历元素及角标

```kotlin
	array.forEachlndexed{index,s->
		println("角标=$index 元素=$s")
	}
```

* 数组元素修改

```kotlin
	var newArr = arrayOf(1,2,3,4,2,3,5)
	newArr[0] = 20//将角标0元素值设置为20
	newArr.set(4,20)//将角标元素设置为20
```

##查找元素角标

```kotlin
	arrayOf("张三","李四","张四","王五","张三","赵六")
```

* 查找第一个“张三”角标

```kotlin
	array.indexOF("张三")
```

* 查找最后一个“张三”角标

```kotlin
	array.lastIndexOf("张三")
```

* 查找第一个姓“张”的人的角标

```kotlin
	array.indexOfFirst{
		it.startWith("张")（Boolean值）
	}
```

* 查找最后一个姓“张”的人的角标

```kotlin
	array.indexOfLast{
		it.startWith("张")(Boolean值)	
	}
```

##when表达式
* when表达式是多分支的条件控制语句

```kotlin
	when(age){
		7->return"开始读小学"
		12->{
			println("开始读中学")
			return"开始读小学了"
		}
		else ->return "读社会大学"
	}
```

* when表达式加强

```kotlin
	when(ele){
		10->println("它10岁")
		is Int->println("传递的是年龄")
		in 1..10->println("在10岁以内的年龄")
		!in 1..10->println("不在10岁以内")
		else->println("所有情况都不满足")
	}
```

* when表达式不带参数

```kotlin
	when{
		ele == 10->return "它十岁"
		ele is Int->return "传递的是年龄"
		ele in 1..10->return "在10岁以内"
		ele ！in 1..10->return "不在10岁以内"
		ele ->return"所有情况都不满足"
		}
```

* when表达式返回值
* when表达式返回值在{}的最后一行

```kotlin
	val s=when{
		ele == 10->"它10岁"
		ele is Int->"传递的是年龄"
		ele in 1..10->"在10岁以内"
		ele !in 1..10->"不在10岁以内"
		else -> "所有情况都不满足"
	}
	return s
```


