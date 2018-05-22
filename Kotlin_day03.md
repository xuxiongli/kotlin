#day 03

[TOC]

##函数表达式
* Statement & Expression
* java 声明式语法  声明 no return value
* kotlin表达式语法  表达式 return value
* 如果函数是一个表达式就可以用"="连接
* 如果是多行的语句块，就不能用“=”
* Scala “万物皆表达式” 语句块、表达式、函数
* 对于一个普通函数

```kotlin
	fun add(a:int, b:Int):Int {
		return a + b
	}
```

* 函数体只有一行，可以简写

```kotlin
	fun add(a:Int, b:Int) = a + b
```

* 还可以更简洁

```kotlin
	fun add(a:Int, b:Int) = a + b
```

* 定义函数变量保存函数引用

```kotlin
	val addFun1:(Int, Int)->Int = ::add
```

* 执行这个函数

```kotlin
	addFun1(10,20) // 第一种
	addFun1?.invoke(10,20) // 第二种
```

* 定义函数变量时定义函数（这个函数也是求a+b的和）

```kotlin
	var addFun:(Int,Int) -> Int = {a, b -> a + b}
```

* 执行这个函数

```kotlin
	val result = addFun(10, 20) // 第一种
	val result = addFun?.invoke(10, 20) // 第二种
```

##函数默认参数和具名参数
* 发送网络请求：path请求方式GET
* 默认参数：定义函数变量时可以指定默认值

```kotlin
	fun info(name:String="zhangsan", age:Int) {
		println("name = $name, age = $age")
	}
```

* 具名参数：调用函数时可以指定参数（没用顺序）

```kotlin
	info(age = age.name = "李四")
```

* 可变参数
* 关键字 vararg

```kotlin
	fun add(vararg arr:Int):Int { // arr是数组
		var result = 0
		arr.forEach {
			result += it
		}
		return result
	}
```

##异常处理
* 程序抛出异常

```kotlin
	fun add(a:Int,b:Int):Int {
		if(b == 0) {
			throw Exception("我是异常")
		} else {
			return a/b
		}
	}
```

* kotlin没有受检异常
* 处理异常

```kotlin
	try {
		add(10, 0)
	} catch(e:Exception) {
		println("不能为0")
	} finally {
		println("一定执行的代码")
	}
```

##递归
* 递归：把一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解
* 只需少量的程序就可描述出解题过程所需要的多次重复计算

###尾递归优化
* 尾递归：函数在调用自己之后没有执行其他任何的操作就是尾递归
* 实现尾递归优化
* 第一步将递归转化成尾递归
* 第二步加  tailrec
* 尾递归优化的核心原理是将递归转化成迭代

```kotlin

	tailrec fun Add(n:Int， result:Int):Int {
		if(n == 1) {
			return result + 1
		} else {
			return gsTaAdd(n - 1, result + n)
		}
	}
```

##面向对象
* 用基本的数据类型描述复杂的事物
* 描述矩形
* 描述妹子

###面向对象入门

```kotlin
	class Person1 {
		var name:String = "zhangsan"
		override fun toString():String {
			return "Person(name = $name age = $age)"
		}
	}
```

##运算符重载
* 基本运算符：+-*/
* 运算符重载：对于+-*/等运算来说，其实就是一个函数
* kotlin中每一个运算符就是一个方法的简写

a+b a.plus(b)
a-b a.minus(b)
a*b a.times(b)
a/b a.div(b)
....

```kotlin
	class BeautifulGirl {
		var name:String = ""
		var age:Int = 0
		operator fun plus(a:Int):BeautifulGirl {
			age = age + a
			return this
		}

		override fun toString():String {
			return "BeautifulGirl(name ='$name', age = $age)"
		}
	}
```

##类成员的get和sey方法
* 对于kotlin成员变量来说，已经默认实现了get和set方法
* 修改访问器的可见性

```kotlin
	var name:String="张三"
		private set
```

* 自定义访问器

```kotlin
	var name:String = ""
		get() {
			return "李四"
		}
		set(value) {
			field = value // 这里field代表name字段
		}
```

* 定义Person对象：name age
* 主构函数

```kotlin
	class Person(var name:String, var age:Int) {
	}
```

##init

```kotlin
	class Person() { //这里如果没有()就代表没有无参构造函数
		init {
			println("执行了init方法")
		}
		constructor(name:String):this()
	}
```

* 调用主函数

```kotlin
	val person = Person()
	会执行init
```

* 调用次构函数

```kotlin
	val person = Person("张三")
```

也会执行init

* 保存主构中的参数
  * 在init方法中保存

  ```kotlin
  	class Person(name:String, age:Int) {
  		var name = ""
  		var age = 0
  		init {
  			this.name = name
  			this.age = age
  		}
  	}
  ```

##主构函数参数的var和val
* 参数没有var和val修饰，参数在其他地方不能使用

```kotlin
	class Person8(name:String, age:Int) {
		override fun toString():String {
			return "Person(name = $name age = $age)"
		}
	}
```

* 参数有var修饰，可以使用，可以修改

```kotlin
	class Person(var name:String, var age:Int) {
		override fun toString():String {
			return"Person(name = $name age = $age)"
		}
	}
	val person = Person8("张三"， 20)
	person.name = "李四"
```

* 参数有val修饰，可以使用，不能修改

```kotlin
	class Person(val name:String, val age:Int) {
		override fun toString():String {
			return "Person(name = $name age = $age)"
		}
	}
```

* 参数有val修饰，可以使用，不能修改

```kotlin
	class Person(var name:String,val age:Int) {
		override fun toString():String {
			return"Person(name = $name age = $age)"
		}
	}
```

###次构函数
* 次构函数（次构函数必须要调用主构）

```kotlin
	class Person(name:String,age:Int) {
		constructor(name:String, age:Int, phone:String):this(name, age)
	}
```

* 次构函数间的调用

```kotlin
	class Person12(name:String,age:Int) {
		constructor(name:String, age:Int, phone:String):this(name, age)
		constructor(name:String. age:Int, phone:String, email:String):this(name,age,phone)
	}
```

* 次构中参数不能加var或者val修饰
* 次构函数参数的使用

```kotlin
	class Person11(var name:String, var age:Int) {
		var phone:String = ""
		constructor(name:String, age:Int, phone:String):this(name, age) {
			this.phone = phone
		}
	}
```

###主构 次构init调用顺序

```kotlin
	class Person(name:String, age:Int) {
		init{
			println("执行了init方法")
		}
		constructor(name:String, age:Int, phone:String):this(name, age) {
			println("执行了次构函数")
		}
	}
```

* 无论调用主构还是次构都会执行init
* 调用次构先执行次构中的操作

##面向对象的三大特征
* 封装
  * 隐藏内部实现的细节，只保留功能接口

```kotlin
	fun sendRequest(path:String, method:String) {
		val url = URL(path)
		val conn:HttpURLConnection = url.openConnection() as HttpURLConnection
		val input:InputStream != conn.inputStream
	}

	class Circle(var dotX:Int, var dotY:Int, var radius:Int) {
		private val PI=3.1415926
		//获取圆周长
		fun GetCircumference():Double {
			return 2*PI*radius
		}
	}
```

* 继承
  * 按照法律或遵照遗嘱接收死者的财产、职务、头衔、地位等。
  * 继承是指一个对象直接使用另一个对象的属性和方法
  * 类的继承：kotlin的类都是final的, 不能被继承, 要在类前加"open"关键字, 如果要修改父类中的属性或方法, 也要在属性或方法前加上"open", 子类加的关键字是"override"(会默认自动添加)

```kotlin
 	open class Father {
		var name:String = ""
		var age:Int = 0
		open fun sayHello() {
			println("父类Hello")
		}
	}
	class Son:Father() {	
	}
```

* 方法继承

```kotlin
	open class Father2 {
		open var name = "小头爸爸"
		var age = 30
		open fun sayHello() {
			println("Hello")
		}
	}
	class Son2:Father2() {
		override var name = "大头儿子"
		override fun sayHello() {
			super.sayHello()
		}
	}
```

* 继承父类主构 

```kotlin
 	//子类主构继承父类主构
	open class Human(val name:String, var age:Int)
	class Man(name:String, age:Int):Human(name, age)

	//子类次构继承父类主构
	open class Human(var name:String, var age:Int)

		class Woman:Human{
			constructor(name:String, age:Int):super(name, age)
			constructor(name:String, age:Int, phone:String):super(name, age)

			constructor(name:String, age:Int,phone:String, email:String):this(name,age,phone)
		}
```

###抽象类和接口
* 抽象类以abstract表示,不用添加"open", 抽象类可以没有抽象字段和抽象方法
* 抽象类反映的是事物的本质，只能单继承
* 抽象类也可以继承抽象类
* 抽象类反映的是事物的本质，接口反映的是事物的能力
* 类只能单继承，接口可以多实现

```kotlin
 	interface DriveCarl {
		var liscence:String
		fun drive() {
			println("挂档  踩油门  走")
		}
	}
```

###多态
* 多种功能，不同的表现形态
* 通过父类接受子类类型，调用的还是子类的方法

```kotlin
	fun main(args: Array<String>) {   
        val dog: Animal = Dog()
        val cat = Cat()
        dog.call()
        cat.call()
    }
    abstract class Animal {
        var color: String = ""
        open fun call() {
            println("动物叫")
        }
    }
    class Dog : Animal() {
        override fun call() {
            println("狗汪汪叫")
        }
    }
    class Cat : Animal() {
        override fun call() {
            println("猫喵喵叫")
        }
    }
```

### 智能类型转换

- 当判断出类型后就自动将类型转换成该类型了

- ```kotlin
  if(shepHerdDog is ShepHerdDog){ //判断完之后已将将shepHerdDog类型由Dog类型转换为ShepHerdDog类型
  	shepHerdDog.herdSheep()
  }
  ruralDog.watchDoor()
  ```

###嵌套类和内部类

```kotlin
	//嵌套类
	class OutClass {
		var outName="张三"
			class InClass{
				fun sayHello(){
					println(outName) //无法访问外部类中字段
				}
			}
		}

	//内部类inner
	class OutClass1{
		var outName="张三"
			inner class InClass{
				fun sayHello(){
					println(outName) //可以访问外部类中字段
				}
			}
		}
```

##Kotlin的class
* 定义class默认都是final
* 类里面定义的嵌套类都是静态的（static）
* 加上ninner之后变成内部类

###内部类中使用this
* 可以指定内部this和外部this

```kotlin
	class OutClass2{
		var name="张三"
		inner class InClass{
			var name="李四"
			fun sayHello(){
				println(this@OutClass2.name)
			}
		}
	}
```

##泛型
* 在强类型程序设计语言中编写代码时定义一些可变部分
* 使用范型类需要指定泛型具体类型
* 继承范型类可以指定泛型类型
* 继承泛型类不知道具体类型可以使用泛型传递
* 泛型类

```kotlin
	class Box<T>(var value:T) //不知道具体传递的类型
	
	fun main(args: Array<String>) {
    	val box =Box<String>("香蕉")
    	println(box.thing)
	}
	//箱子
	open class Box<T>(var thing:T)//物品类型不确定  定义泛型和使用泛型

    //水果箱子
    class FruitBox(thing:Fruit):Box<Fruit>(thing)

    //不知道具体放什么东西
    class SonBox<T>(thing:T):Box<T>(thing)//第一个是申明  后面两个都是使用

    abstract class Fruit

    class Apple:Fruit()

    class Orange:Fruit()
```

* 泛型函数

```kotlin

	fun <T> printlnfo(content:T) {
		when(content) {
			is Int->println("传入的$content,是一个Int类型")
			is String->println("传入的$content,是一个String类型")
			else->println("传入的$content,不是Int也不是String")
		}
	}
```

* 泛型上限

```kotlin
	// T:Fruit  泛型上限  泛型智能是Fruit类型或者Fruit类型的子类
	// 泛型作用: 放任何类型 限制存放的类型

	fun main(args: Array<String>) {
   		 val apple = Apple()
    	 val thing = Thing()
         val fruitBox = FruitBox(thing)
    }
    //箱子
    open class Box<T>(var thing:T)
    //水果箱子
    class FruitBox<T:Fruit>(thing:T):Box<T>(thing)

    open class Thing
    //水果
    open class Fruit:Thing()
    //苹果
    class Apple:Fruit()
    //橘子
    class Orange:Fruit()
```

* 泛型擦除  在泛型内部，无法获得任何有关泛型参数的信息

  解决泛型擦除方案:

   * 第一步: 泛型前加reified关键字
   * 第二步: 方法前加上inline关键字

```kotlin
例1 inline fun <reified T>parseType(thing:T) {
        //获取传递的thing的class类型
        val name = T::class.java.name
        println(name)
    }
    
例2 inline fun <reified T>getType(t:T):String {
		val class:Class<T> = T::class.java
		return class.name
	}
```

- 泛型类型投射
  - out: 接收当前类型或它的子类 相当于java的  ? extents
  - in: 接收当前类型或者它的父类  相当于java的  ? super


- 星号投射

  - "*"可以传递任何类型  相当于java的 "?"

      ```kotlin
      fun main(args: Array<String>) {
          val list = ArrayList<Int>()
          setFruitList(list)
      }

      // ArrayList里面可以接收任何类型
      fun setFruitList(list:ArrayList<*>){
          println(list.size)
      }
      ```

###类型协变（in out）

* 指定类型参数在该类型或者接口的用途

* 指定参数是用来消费的还是用来返回的

* in T：消费T类型，而不能返回T类型

* out R：只能返回R类型，而不能消费R类型

* List<out Fruit>:相当于java的java的List<?Extends Fruit>

* List<in Fruit>:相当于java的List<?super Fruit>

* 泛型上限是在定义泛型时指定的

* 类型投射是在使用泛型的时候指定的

  ​


