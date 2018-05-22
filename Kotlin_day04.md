## day04

[TOC]

### 中缀表达式

- 中缀表达式关键字是"infix" 

- 使用条件:

  必须是成员函数或扩展函数

  必须只有一个参数

  参数不能是可变参数或者默认参数

  ```kotlin
  fun main(args: Array<String>) {
      val 张三 = Person()
      
      张三 sayHelloTo "李四"

     //自定义操作符 区间 元组 二元 三元
      val pair = Pair<String,Int>("张三", 20)
      val pair2 = "张三" to 20
  }
  class Person{
      infix fun sayHelloTo(name:String){
          println("你好 $name")
      }
  }
  ```

  ​

### 类委托

```kotlin
fun main(args: Array<String>) {
    val smallHeadFather = SmallHeadFather()
    //小头爸爸洗碗
    smallHeadFather.wash()
}
//洗碗能力
interface WashPower{
    fun wash()
}
//大头儿子
class BigHeadSon:WashPower{
    override fun wash() {
        println("大头儿子开始洗碗了")
    }
}
//小头爸爸 将洗碗的能力委托给大头儿子
class SmallHeadFather:WashPower by BigHeadSon()
```

#### 类委托实现二

```kotlin
fun main(args: Array<String>) {
	//让小头儿子洗碗
    val smallHeadFather = SmallHeadFather(SmallHeadSon())
    smallHeadFather.wash()
}
//洗碗能力
interface WashPower{
    fun wash()
}
class SmallHeadSon:WashPower{  //小头儿子
    override fun wash() {
        println("小头儿子开始洗碗")
    }
}
class BigHeadSon:WashPower{  //大头儿子
    override fun wash() {
        println("大头儿子开始洗碗了")
    }
}
class SmallHeadFather(var washPower: WashPower):WashPower by washPower
```



#### 类委托功能加强

```kotlin
class SmallHeadFather(var washPower: WashPower):WashPower by washPower {
    override fun wash() {
        println("付给小头儿子1块钱")
        washPower.wash()
        println("干的很好 下次继续")
    }
}
```



### 属性委托

- 将属性的get和set方法委托给其他对象

  ```kotlin
  fun main(args: Array<String>) {
      val bigHeadSon = BigHeadSon()
      //爷爷奶奶给了100块钱
      bigHeadSon.压岁钱 = 100
      //取压岁钱
      println(bigHeadSon.压岁钱)
  }
  class BigHeadSon{
      //存钱罐
      var 压岁钱:Int by Mother()
  }
  class Mother{
      //儿子要压岁钱
      operator fun getValue(bigHeadSon: BigHeadSon, property: KProperty<*>): Int {
          return 儿子的压岁钱
      }
      //儿子存压岁钱  i设置的值  100
      operator fun setValue(bigHeadSon: BigHeadSon, property: KProperty<*>, i: Int) {
          儿子的压岁钱 += 50
          自己的小金库 += i-50
      }
      var 儿子的压岁钱 = 0
      var 自己的小金库 = 0
  }
  ```


### 惰性加载

- 用的时候再赋值
- 字段: val  不可变
- by lazy放到成员变量中 可以单独存在
- by lazy返回值就是最后一行
- by lazy线程安全 (同步)

```kotlin
val name1: String  by lazy {
    println("初始化了")
    "张三"
}
val age = 20

fun main(args: Array<String>) {
    println(name1)
    println(name1)
}
//class Person{
//    // 用的时候再赋值 用属性字段用by lazy
//    val name1:String  by lazy { "张三" }
```

### 延迟加载

- 关键字 "lateinit"

- 只能修饰类成员

- 修改var可变变量

- 不能使用在基本数据类型

  ```kotlin
  fun main(args: Array<String>) {
      val person  = Person()
  	println(person.name1)
  }
  class Person{
      //不确定 后面可能用的时候才会赋值 不知道具体是什么值
  	lateinit var name1:String
       fun setName(name:String){
          this.name1 = name
      }
  }
  ```

  **by lazy和lateinit区别**

  - by lazy:知道具体值,用的时候再初始化,不能修改,val修饰
  - Lateinit:不知道具体值,后面用的时候再赋值,可以修改,var修饰
  - by lazy 和 lateinit  都可以单独使用或者放到成员变量中使用

  ​

### 扩展函数

- 不改变已有类的情况下，为类添加新的函数

- 扩展函数主要是替代java的util类

- 可以给任意类添加扩展函数

- 扩展函数可以像成员函数一样访问对象(字段和方法)

- 父类定义的扩展函数子类也可以使用

  ```kotlin
  扩展函数格式: String类扩展 fun String.扩展函数名
  fun main(args: Array<String>) {
      val str:String? = null
      //str是否为空
      val myIsEmpty = str.myIsEmpty()
      println(myIsEmpty)
      val son = Son()
      son.sayHello()
  }
  fun String?.myIsEmpty():Boolean{
      return this==null||this.length==0
  }
  //扩展方法
  fun Father.sayHello(){
      println("爸爸在抽烟")
  }
  open class Father{
      fun haha(){
      }
  }
  class Son:Father(){
  }
  ```

### kotlin的object单例

- object单例  所有的字段都是static静态  方法不是

 * object试用条件: 字段不多的时候
 * java可以控制字段是静态还是非静态 static
 * kotlin没有static关键字

```kotlin
fun main(args: Array<String>) {
    Utils.name
    Utils.sayHello()
}
object Utils{  //关键字object
    var name = "张三"
    fun sayHello(){
        println("hello")
    }
}
```

### 伴生对象

- 伴生对象作用就是控制属性的静态
- 可以将定义成static静态的属性放在伴生对象里
- 外部类可以直接调用伴生对象里面的方法和属性

```kotlin
fun main(args: Array<String>) {
    val person = Person()
    person.age
    Person.name
}
class Person {
    var age = 20
    companion object {   //关键字companion
        //静态的name
        var name = "张三"
    }
}
```

### 和java一样的单例

```kotlin
fun main(args: Array<String>) {
    Utils.instan.age
}
class Utils private constructor(){ //第一步:私有构造函数
    //非静态的
    var age = 20
    companion object {
        //静态
        var name = "张三"
        //instan代表Utils的对象实例
        val instan by lazy { Utils() } //惰性加载  只会加载一次  线程安全
    }
}
```

### 枚举

```kotlin
枚举就是一一列举
enum class Week{  
	星期一,星期二,星期三,星期四,星期五,星期六,星期日
}
```

### 枚举加强 

- 枚举里面可以定义构造函数

```kotlin
fun main(args: Array<String>) {
    COLOR.RED.r
    COLOR.RED.g
    COLOR.RED.b
}
// 红  r 255 g 0 b 0
// 蓝  r 0 g 0 b 255
// 绿  r 0 g 255 b 0
enum class COLOR(var r:Int,var g:Int,var b:Int){
    RED(255,0,0),BLUE(0,0,255),GREEN(0,255,0)
}
```



### 数据类

- 数据类相当于提供了bean类的模板

- 只保存数据,没有其他任何逻辑操作,对应java的bean类

- "date"包含了构造函数, get set, toString, hashcode, equeals,参数,copy

  ```kotlin
  fun main(args: Array<String>) {
      val news = News("标题","简介","路径","内容")
      news.title
      news.desc
      news.component1() //第一个元素
      news.component2() //第二个元素
      //解构
      val (title,desc,imgPath,content) = News("标题","简介","路径","内容")
      println(title)
      println(desc)
  }
  data class News(var title:String,var desc:String,var imgPath:String,var content:String)
  ```

### 密封类

- 密封类和枚举差不多,枚举在意数据,密封类更在意类型

```kotlin
sealed class NedStark { //密封类封装的是类型 类型是确定的
    class RobStark : NedStark()

    class SansaStark : NedStark()

    class AryaStark : NedStark()

    class BrandonStark : NedStark()
}

class JonSnow : NedStark()
```

