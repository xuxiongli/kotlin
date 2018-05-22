## Kotlin_day06_DSL笔记

### day06练习题-开房数据

**视频源：**02.开房数据库.avi

**知识点：**集合的各种操作方法的练习

1. 对开房数据中的数据做多种条件操作

2. 单条数据示例

   > ```
   > 丁青  xxxxxx19821205xxxx 重庆市    女  尚客优    1453042287949  1453072490949
   > ```

**代码:**

```kotlin
fun main(args: Array<String>) {
    val list = getPersonRepository()
    /*---------------------------- 计算广东省人数 ----------------------------*/
    val list1 = list.filter { it.province=="广东省" }
    println(list1.size)
    println(list.count { it.province == "广东省" })
  
    /*---------------------------- 统计广东省男性人数 ----------------------------*/
    println(list.count { it.province == "广东省" && it.sex == "男" })

    /*---------------------------- 获取年纪在20到35岁的 女性人数 ----------------------------*/
    //今年是哪一年 2018-20  2018-35
    //获取今年是哪一年
    val year = Calendar.getInstance().get(Calendar.YEAR)
    val max = year - 20
    val min = year - 35

    val count = list.count {
        val boneYear = it.idNum.substring(6..9).toInt()
        it.sex=="女"&&boneYear>=min&&boneYear<=max
    }
//    println(count)
    /*---------------------------- 2016年2yue13到15开房人信息 ----------------------------*/
    //2016年2yue13  2016-02-13
    //2016年2月15
    val formater = SimpleDateFormat("yyyy-MM-dd")
    val min1 = formater.parse("2016-02-13").time
    val max1 = formater.parse("2016-02-15").time
    println(list.count { it.startTime >= min1 && it.startTime <= max1 })

    /*---------------------------- 范冰冰在2016年2yue13到15开放人信息 ----------------------------*/
    println(list.count { it.startTime >= min1 && it.startTime <= max1&&it.name=="范冰冰" })

    /*---------------------------- 范冰冰在2016年2yue13到15开放人信息 按照酒店分类 ----------------------------*/
    println(list.filter { it.startTime >= min1 && it.startTime <= max1 && it.name == "范冰冰" }.groupBy { it.hotel })

    /*---------------------------- 范冰冰在2016年2yue13到15开放人信息按时间排序 ----------------------------*/
    println(list.filter { it.startTime >= min1 && it.startTime <= max1 && it.name == "范冰冰" }.sortedBy { it.startTime })

}
```

**小细节（注意事项）**

1. 使用Calendar获取当前时间(年份)
2. 使用SimpleDateFormat解析出 格式时间 的 Date 值

```java
//1
 val year = Calendar.getInstance().get(Calendar.YEAR)

//2
val formater = SimpleDateFormat("yyyy-MM-dd")
val min1 = formater.parse("2016-02-13").time
val max1 = formater.parse("2016-02-15").time
```

**总结**

熟练使用 list.filter  list.count   groupBy    sortedBy



### day06练习题-黑马商店

**视频源：**03.黑马商店练习.avi

**知识点：**集合的各种操作方法的练习

**需求：**对黑马商店的交易数据做操作

**代码:**

```kotlin
fun main(args: Array<String>) {
  //变换
    println("--------黑马商店,用户来自哪些城市？--------")
    val result = heimaShop.customers
            .map { it.city }//将城市重新组合
            .distinct()//去重
    println(result)

    println("--------黑马商店,小王买过的所有商品？--------")
    //order1  List1  order2 List2
    val result1 = heimaShop.customers
            .find { it.name=="小王" }
            ?.orders//所有的订单
            ?.flatMap { it.products }//将所有的商品组合在一起
            ?.distinct()//去重
    println(result1)
    println("--------黑马商店,所有用户买过的所有商品？--------")
    val result2 = heimaShop.customers
            .flatMap { it.orders }
            .flatMap { it.products }
            .distinct()
    println(result2)
  
  //变换
  println("--------黑马商店,小王买东西花了多少钱？--------")
    val result = heimaShop.customers.find { it.name=="小王" }
            ?.orders//所有的订单
            ?.filter { it.isDelivered }//过来出所有已经付钱的订单
            ?.flatMap { it.products }//所有的商品
            ?.sumByDouble { it.price }//统计商品的价钱
    println(result)
  
  //拆分
   println("---黑马商店,过滤用户,过滤条件 购买的商品 未发货 < 已发货---")
    val result = heimaShop.customers.filter {
        //已发货的订单数量
//        val hasSend = it.orders.filter { it.isDelivered }
//        //没有发货的订单数量
//        val hasNotSend = it.orders.filter { !it.isDelivered }
        val (hasSend,hasNotSend)= it.orders.partition { it.isDelivered }
        hasNotSend.size<hasSend.size
    }
    println(result)

    println("---黑马商店,所有订单里面，已发货的，最贵的商品---")
    val result1 = heimaShop.customers
            .flatMap { it.orders }
            .filter { it.isDelivered }
            .flatMap { it.products }
            .distinct()
            .maxBy { it.price }
    println(result1)
}
```

**总结**

1. 熟练使用 list.map   list.distinct    filter  sortedBy  flatMap  sumByDouble  maxBy


1. 注意map方法与 flatmap方法之间的区别



### DSL简介

**视频源：**04.DSL简介.avi

**知识点：**DSL的介绍

**概念：** 

1. 什么是DSL
   - DSL(Domain-Specific Language,领域特定语言)指的是专注于特定问题领域的计算机语言
   - 不同于通用的计算机语言(GPL  general purpose language),领域特定语言只用在某些特定的领域
   - 领域可能是某一种产业,保险 教育 航空 医疗,也可能是一种方法或技术,比如数据库SQL  html…


1. Kotlin中构建DSL所需要的技术
   - 扩展函数
   - 中缀调用
   - 带接收者的函数字面值(高阶函数  lambda表达式)    fun <T> T.apply(block:T.()->Unit):T{}

**需求：** 循环遍历每个字符

------

# 普通类实现人物信息的登记

**视频源：**05.class类的普通实现.avi

**知识点：**class的定义已经对象创建

**概念：** 类的定义与对象创建

**需求：** 定义一个普通类并创建对象

**代码:**

```kotlin
fun main(args: Array<String>) {
	val address = Address("深圳市","宝安街道",114) //地址类的实例
    val person = Person("林青霞",30,address)  //person类的实例
}
class Address(var city:String,var street:String,var number:Int) // 地址
class Person(var name:String,var age:Int,var address: Address) // 人
```



# 用DSL搭建框架-人名的赋值

**视频源：**06.实现person函数.avi  07.实现属性name的赋值

**知识点：**带接受者函数字面值的理解

**需求：** 实现name属性的赋值

**代码: **

------

```kotlin
fun main(args: Array<String>) {
	val person: Person =
            person {
                name= "徐熊丽"
            }
    println(person)
}			
fun person(block:Person.()->Unit):Person{
	val person = Person()
	person.block()
	return person
}
data class Address(var city: String?=null, var street: String?=null, var number: Int?=null)
data class Person(var name: String?=null, var age: Int?=null, var address: Address?=null)
```

**运行结果**

```kotlin
name=徐熊丽，age=null，address=null
```

### address函数实现

**视频源：**08.address函数实现.avi

**代码:**

```kotlin
	val person:Person =
		person {
  			name = "徐熊丽"
         	 age = 30
         	 address {    
               
        	 }
		}
fun Person.address (block:() -> Uint){
  	val address = Address()
    this.address = address
}

data class Address(var city:String?=null, var street:String?=null, var number:Int?=null)
```

**运行结果**

```
Person(name=徐熊丽, age=30, address= Address(city=null, street=null, number=null))
```

------

### address里面字段的使用

**视频源：**09.address里面字段的使用.avi

**代码:**

```kotlin
	val person:Person =
		person {
  			name = "徐熊丽"
         	 age = 30
         	 address {
         		 city = "深圳市"
                  street = "宝安街道"
               	  number = 114
        	 }
		}
fun Person.address (block:address.() -> Uint){
  	val address = Address()
  	address.block()
    this.address = address
}

data class Address(var city:String?=null, var street:String?=null, var number:Int?=null)
```

**运行结果**

```
Person(name=徐熊丽, age=30, address= Address(city="深圳市", street="宝安街道", number=114))
```

------

### dsl流程

**视频源：**10.dsl流程.avi

**代码:**

```kotlin
	val person:Person =
		person {
  			name = "徐熊丽"
         	 age = 30
         	 address {
         		 city = "深圳市"
                  street = "宝安街道"
               	  number = 114
        	 }
		}
//address高阶函数
//相当于在person里面又定义了一个address函数
fun Person.address (block:address.() -> Uint){
  	//需要创建Address对象,赋值给person的address字段
  	val address = Address()
  	//执行block函数
  	address.block()
    //必需要将address字段赋值给person函数里面定义的person对象
    this.address = address
}

data class Address(var city:String?=null, var street:String?=null, var number:Int?=null)
```

**运行结果**

```
Person(name=徐熊丽, age=30, address= Address(city="深圳市", street="宝安街道", number=114))
```

### DSL优化

**视频源：**11.dsl优化.avi

**知识点：**dsl中哪些代码可以进行优化

**目的：** 优化使代码更简洁

**需求：** Person里面又定义了一个address函数

​	     实现person函数

**代码（优化前）:**

```kotlin
fun Person.address(block:Address.()->Unit){
    val address = Address()
    address.block()
    this.address = address
}

fun person(block:Person.()->Unit):Person{
    val person = Person()
    person.block()
    return person
}
```

**代码（apply优化后）:**

```kotlin
fun Person.address(block:Address.()->Unit){
	this.address = Address().apply(block)
}

fun person(block:Person.()->Unit):Person{
    return Person().apply(block)
}
```



------

### DSL总结

**视频源：**12.dsl总结.avi

**知识点：**dsl总结

**总结**

- DSL只是问题解决方案模型的外部封装，这个模型可能是一个API库，也可能是一个完整的框架
- 没有具体的标准来区分DSL和普通的API，DSL代码更易于理解，不仅对于开发人员而且对于技术含量较低的人员来说也更易于理解

------

### 普通实现html标签打印

**视频源：**13.普通实现html标签打印.avi

**知识点：**html对象抽取（先抽取共同特性）；代码的一小步优化

**需求：** 纯代码标签打印

**代码:**

```kotlin
fun main(args: Array<String>) {
 	val html = Html()//"<html></html>"
    val head = Head()//"<head></head>"
    val body = Body()//"<body></body>"
    val div = Div()//"<div></div>"
    
    //先放入head
    html.setTag(head)
    //div放到body中
    body.setTag(div)
    //body放到html中
    html.setTag(body)

    println(html.toString())
}

//div标签
class Div : Tag("div")

//body标签
class Body : Tag("body")

//head标签
class Head : Tag("head")

//Html标签类
class Html : Tag("html")

	//任何标签的父类
    class Tag(var name:String){
        //用容器保存其他的标签
        val list = ArrayList<Tag>()
        //定义方法传递标签
        fun setTag(tag:Tag){
            list.add(tag)
        }
        override fun toString():String{
            val sb = StringBuilder()
            //前面的标签
            sb.append("$name")
            //子标签
            list.forEach{
                //子标签的toString
                sb.append(it.toString())
            }
            //后面标签
            sb.append("</$name>")
            return sb.toString()
        }
}
    
```

**运行结果**

```
<html><head></head><body><div></div></body></html>
```

**小细节（注意事项）**

 注意：只打印了父类对象，子类对象未打印；

​	    StringBuffer与StringBuilder的选用（StringBuffer更安全）；

​	    标签可能写错；

```java
 //前面的标签
            return "<$name></$name>"
                
//这样写可能写错标签
    val html = Tag("head")
    val head = Tag("html")
    html.setTag(head)
    println(html)
    "<html><head></head><html>"
```

**这样写的代码中存在不妥的地方**

思路得时刻保持清晰（稍有不慎写错可能会看不出来）以及导致后期可维护性比较差

```
    //先放入head
    body.setTag(html)
    //div放到body中
    body.setTag(div)
    //body放到html中
    html.setTag(body)
```

##### 更好的思路：

```kotlin
html{
         head{
             title{
                 ""
             }
         }
         body{
             div{

             }
         }
     }
```



------

### html实现

**视频源：**14.html实现.avi

**知识点：**自带的的一些方法

**概念：** 实现html

**需求：** 第一步 (实现html)

**代码:**

```kotlin
fun main(args: Array<String>) {
	val result:Html = 
               html {

                }
}

fun html(block:() ->Unit):Html{
    val html = Html()
    return html
}
```

**运行结果**

```
<html><html>
```

### head实现

**视频源：**15.head实现.avi

**需求：** 第二步 (实现head)

**知识点：**head 高阶函数    Html的扩展函数不能直接调用,需要在html函数传参那里  Html.()  调用head函数

```kotlin
fun main(args: Array<String>) {
	val result:Html = 
               html{
					head{
                        
						}
                	}
}
//把head设为扩展函数的时候  上面的head会报错红
//Html的扩展函数不能直接调用
fun Html.head(block:()->Unit){
    //创建head标签
    val head = Head()
    setTag(head)
    //添加到html对象 
}


fun html(block:Html.() ->Unit):Html{
    val html = Html()
    //执行block函数
    html.block
    return html
}
```

**运行结果**

```
<html><head><head><html>
```

### body实现

**视频源：**16.body实现.avi

**需求：** 第三步 (实现body)

**知识点：**body是一个高阶函数

```kotlin
fun main(args: Array<String>) {
	val result:Html = 
               html{
					head{
                        
						}
					body{
                        
						}
                	}
}
fun body(block:()->Unit){
    //body标签
    val body = Body()
    //添加到html对象中(在html中创建对象)
    setTag(body)
}
//把head设为扩展函数的时候  上面的head会报错红
//Html的扩展函数不能直接调用
fun Html.head(block:()->Unit){
    //创建head标签
    val head = Head()
    setTag(head)
    //添加到html对象 
}


fun html(block:Html.() ->Unit):Html{
    val html = Html()
    //执行block函数
    html.block
    return html
}
```

运行结果

```
<html><head><head><body><body><html>
```

### DSL实现div标签

**视频源：**17.div标签.avi

**知识点：**html如何用kotlin实现

**概念：** DSL实现html

**需求：** 搭好框架让不懂代码的人也会操作

代码:

```kotlin
fun main(args: Array<String>) {
    val result: Html =
            html {
                head {
                }
                body {
                    div {
                    }
                }
            }
    println(result)
}
//div高阶函数
fun Body.div(block: () -> Unit) {
    //创建Div标签
    val div = Div()
    //添加到Body里面
    setTag(div)
}
//body高阶函数
fun Html.body(block: Body.() -> Unit) {
    //Body标签
    val body = Body()
    //执行block函数
    body.block()
    //添加到Html对象中(在html函数中创建的对象)
    setTag(body)
}
//head高阶函数
fun Html.head(block: () -> Unit) {
    //创建Head标签
    val head = Head()
    //添加到html中html对象
    setTag(head)
}
//html
fun html(block: Html.() -> Unit): Html {
    val html = Html()
    //执行block函数
    html.block()
    return html
}
//div标签
class Div : Tag("div")
class Body : Tag("body")
class Head : Tag("head")
class Html : Tag("html")
//任何标签的父类
open class Tag(var name: String) {
    //还应该有容器保存其他的标签
    val list = ArrayList<Tag>()
    //定义方法传递标签
    fun setTag(tag: Tag) {
        list.add(tag)
    }
    override fun toString(): String {
        val sb = StringBuilder()
        //前面标签
        sb.append("<$name>")
        //子标签
        list.forEach {
            //子标签的toString
            sb.append(it.toString())
        }
        //后面标签
        sb.append("</$name>")
        return sb.toString()
    }
}
```

**运行结果**

```kotlin
<html><head></head><body><div></div></body></html>
```



### DSL构建器模式

**视频源：**18.构建器模式.avi

**知识点：**Person可以等到所有的字段都有了之后再创建

**概念：** 使对象赋值之后不能修改

**需求：** 让Person对象赋值后不能被修改

**代码:**

```kotlin
fun main(args: Array<String>) {
    val person: Person =
            person {
                name= "徐熊丽"
                age = 30
                address{
                    city = "深圳"
                    street = "宝安街道"
                    number = 114
                }
            }
    println(person)
}
fun PersonBuilder.address(block:Address.()->Unit){
    val address = Address()
    address.block()
    this.address = address
}
fun person(block:PersonBuilder.()->Unit):Person{
    val person = PersonBuilder()
    person.block()
    return person.build()
}
data class Address(var city: String?=null, var street: String?=null, var number: Int?=null)
data class Person(val name: String?=null, val age: Int?=null, val address: Address?=null)
class PersonBuilder{
    var name:String? = null
    var age:Int? =null
    var address:Address? = null
    fun build():Person{
        return Person(name,age,address)
    }
}
```

**运行结果**

```
Person(name=徐熊丽, age=30, address=Address(city=深圳, street=宝安街道, number=114))
```



### 多个address的使用

**视频源: ** 21.多个address使用.avi

**知识点: ** 关于address集合作为person的

**概念:** 将原来的address函数转变为address集合,并将集合看作一个整体

**需求:**person有多个地址(address),该如何实现

**代码:**

```kotlin
	addresses {  
			address {       
                  city = "深圳"       
                  street = "宝安街道"       
                  number = 114   
			} 
			address {        
                  city = "广东"        
                  street = "流线街道"        
                  number = 110  
			}
	}			  
```

**运行结果:**

```kotlin
Person(name=徐熊丽,age=30,address=[Address(city=深圳,street=宝安街道,number=114),Address(city=广东,street=流线街道,number=110)])
```

**小细节(注意事项)**

**注意**:多个地址需要将原来是单个address的地方全都变成address集合,赋值的时候也是集合

```kotlin
	fun PersonBuilder.addresses(block: MYLIST.() -> Unit) {
    //需要创建Address对象 赋值给person的addres字段
    val list = MYLIST()
    //执行block函数
    list.block()
    //必须要讲address字段赋值给person函数里面定义的person对象
    this.address = list
```

### 缩小作用域

**视频源：**22.缩小作用域

**知识点：**使用一种写法来限制html的作用域

**概念：**限制html作用域的一种写法

**需求：** 严格限制html的作用域

**代码:**

```kotlin
@MYTAG
open class Tag(var name: String) {

    //还应该有容器保存其他的标签
    val list = ArrayList<Tag>()

    //定义方法传递标签
    fun setTag(tag: Tag) {
        list.add(tag)
    }

    override fun toString(): String {
        val sb = StringBuilder()
        //前面标签
        sb.append("<$name>")
        //子标签
        list.forEach {
            //子标签的toString
            sb.append(it.toString())
        }
        //后面标签
        sb.append("</$name>")
        return sb.toString()
    }
}
@DslMarker
annotation class MYTAG
```

**小细节（注意事项）**

 注意：在每个需要缩小作用域的类前都要加注解



### dsl的作用

**视频源：**23..dsl的作用.avi

**知识点:**dsl在工作中的作用

**概念：**提供一套问题解决方案模型的外部封装,让代码更容易理解

**需求：** 不直接写具体的业务代码,而是写一套框架

**代码:**

```kotlin
fun main(args: Array<String>) {
    val url = URL("http:www.baidu.com")
    val conn = url.openConnection() as HttpURLConnection
    val code = conn.responseCode
    if(code==200){

    }else{

    }
}
```

**小细节（注意事项）**

 注意：

DSL(Domain-SpecificLanguage,领域特定语言)指的是专注于特定问题领域的计算机语言

不同于通用的计算机语言(GPL  general purpose language),领域特定语言只用在某些特定的领域

领域可能是某一种产业,保险 教育航空 医疗,也可能是一种方法或技术,比如数据库SQL  html…

DSL只是问题解决方案模型的外部封装,这个模型可能是一个API库,也可能是一个完整的框架

没有具体的标准来区分DSL和普通的API,DSL代码更易于理解，不仅对于开发人员而且对于技术含量较低的人员来说也更易于理解。

------

