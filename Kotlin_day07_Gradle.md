## Kotlin-Gradle-day07笔记

##### 

### gradle课程内容+介绍原始的工程构建方式

**视频源：**02.原始人的工作方式.avi

**知识点：**集合的各种操作方法的练习

**概念：** 

1. gradle课程内容介绍
   - 使用gradle构建工具
   - 使用kotlin编写gradle脚本
   - 看懂groovy写的gradle脚本
2. 使用eclipse构建工程经历的步骤
   - 编译
   - 测试
   - 手动依赖管理
   - 打包
   - 上传服务器

**小细节（注意事项）**

1. eclipse添加Junit测试框架步骤 .  
   1. 在测试类方法上添加@Test 注解后 ,CTRL+1 自动导入junit框架
2. 使用export导出包时,注意选择主类

**总结**

eclipse构建工程很麻烦,需要gradle来解决



### 三种构建工具对比

**视频源：**03.三种构建工具对比.avi

**知识点：**ant maven gradle三种构建工具对比

**概念：** 

1. 三种工具出现年份对比: ant 2000年   ,  maven 2007年 ,  gradle 2012年
2. 功能对比: 
   1. ant: 编译测试
   2. maven: 编译测试 依赖管理  (使用XML进行项目配置,容易出错)
   3. gradle: 编译测试 依赖管理 DSL自定义拓展任务
3. gradle是什么,有什么优点:
   1. 可以用kotlin代码控制的一种智能的自动化构建工具
   2. 用代码控制工具
   3. 不是xml配置来控制工具
   4. 很牛逼的构建工具,构建一切可构建的内容

### gradle官网初览

**视频源：**04.gradle简介.avi

**知识点：**查看gradle官网   https://gradle.org

**概念：** 

1. 官网介绍中的gradle宣传语
   - Build Anything(构建一切)
   - Automate Everything(自动化一切)
   - Deliver Faster(超快交互)
2. 官网中gradle与maven运行速度进行了对比, gradle优势体现在三个方面
   1. gradle跟踪输入,输出文件,所以只会对有变化的文件做处理
   2. 对有相同输入源的其他gradle工程进行重用其输出文件,包括在机器之间
   3. Gradle守护进程:通过守护进程,使构建信息长时间保留在内存之中

------



### gradle下载和配置

**视频源：**05.gradle下载和配置.avi

**知识点：**gradle官网下载   https://gradle.org

**概念：** 

1. 官网下载安装包
2. 解压到 自定义路径(不可含有中文 空格)
3. 配置安装路径的bin 路径 到 电脑环境变量PATH中(与配置JDK的做法一致 )
4. 打开命令窗口 , 输入命令  gradle -v  ,如果出现版本信息,则说明安装成功



### gradle项目初步构建

**视频源：**06.gradle项目初始化.avi

**概念：** 

1. idea中new一个 gradle project
2. 第一次初始化时选择使用推荐的gradle版本,不使用本地已有版本(老师是这么讲的,但是实际操作时还是选择本地已有版本,因为如果选择推荐版本,由于我们之前没有下载过对应版本,导致第一次sync项目时,idea会自动下载最新版本,下载十分慢)
3. 进入工程时,首先在弹出对话框中选择 auto-import
4. 打开build.gradle文件,点击右上角
5. 发现项目根目录多了一个gradle文件夹
6. 打开gradle/wrapper/gradle-wrapper.properties文件,修改distributionUrl属性值可以设置项目使用的gradle版本,
7. 将distributionUrl设置为file:///C:/.../gradle-4.1.zip的形式,指定其为我们的本地gradle
8. 将build.gradle文件后缀改为kts, 然后重开项目(由于idea的bug造成)

------

------

##### 

# gradle打包

**视频源：**07.gradle打包.avi

**知识点：**配置java环境属性

**需求：** 用kotlin编程配置java环境的脚本-打包项目

**代码:**

```kotlin
plugins {
    application //配置java环境
}

//主类名
application {
    mainClassName = "Main"
}
```

**小细节（注意事项）**

 ![1525918351290](C:\Users\lenovo\AppData\Local\Temp\1525918351290.png)

**1：**先把build.gradle改为 build.gradle.kts(shift+f6)

![1525918560495](C:\Users\lenovo\AppData\Local\Temp\1525918560495.png)

**2：**打开build.gradle.kts  把原来用grooy写的脚本删除或者改写为kotlin编写。



**3：**在当前工程目录下新建src目录->新建main目录->java目录

**4：**在build.gradle.kts 里面书写以上代码   刷新后 main文件图标和java文件图标由灰色变亮后可以再java目录下新建javaClass及java项目。

**5：**项目写好后刷新![1525920392401](C:\Users\lenovo\AppData\Local\Temp\1525920392401.png)

**6：**再右侧点开Gradle->gradle02->Tasks->distribution->双击distZip

**7:**打包完成后再左侧build目录下找到distributions 打开找到gradle02.Zip。

![1525920593753](C:\Users\lenovo\AppData\Local\Temp\1525920593753.png)



# 动态语言和静态语言

**视频源：**08.动态语言和静态语言.avi

**知识点：**动态语言的定义与静态语言的定义

**概念：** 

静态语言（Kotlin， Java）： 类型确定后不能修改  

动态语言（python  groovy  javascript）： 类型确定后可以修改



# gradle支持Kotlin开发

**视频源：**09.gradle支持Kotlin开发.avi

**知识点：**配置Kotlin环境属性

**需求：** 用kotlin编程配置Kotlin环境的脚本-打包项目

**代码：** 

```kotlin
plugins {
    application
    kotlin("jvm") //支持kotlin jvm
}

//指定主类名
application {
    mainClassName = "MainKt"
}
repositories {
    mavenCentral()
}
dependencies {
   compile(kotlin("stdlib"))
}

```

**小细节（注意事项）：** 过程和java类似  



# Project和task

**视频源：**10.project和task.avi

**知识点：** 

- project和task 是Gradle本身领域主要的两个对象
- Project为task提供了执行的容器和上下文

**概念：**

- Project就是接口

- gradle构建的时候:首先根据build.gradle配置文件创建一个Project实例  执行project实例

- 配置文件中所有的代码都会通过task任务的方式插入到project中

- project实例可以在配置文件中通过project隐式调用

- task任务：每一个操作都可以定义成一个任务  前面学的application插件里面已经打包好了很多任务  可以直接使用

  **代码：**

  ```
  task("编译java文件"){
      println("开始编译java文件")
  }
  ```

------

------

##### 

### 第一个task任务

**视频源：**11.第一个task任务.avi

**知识点：**task中所有任务都会插入到project里面

**代码:**

```kotlin
task("编译java文件"){
    println（"开始编译java文件"）
}
```

**运行结果**

```
开始编译java文件P-TO-DATE
:编译:java文件
```

------

### task依赖

**视频源：**12.任务的依赖avi

**知识点：**Maven依赖管理用xml文件而Gradle用一个函数就行

​		.dependsOn()

**需求：** File-New-Project-Gradle(其中选项都不勾选)-Next-					 		Groupld:com.itcast.gradle

Artifactid:GradleDemo04_task_depedence

-勾选Use local gradle distrubution-Next-Finish

-修改build.gradle为：build.gradle.kts

-关闭工程再打开-删除代码

**代码:**

```kotlin
task("opendoor"){
    
}
task("putelephant"){
    
}.dependsOn("opendoor")
task("closedoor"){
    
}.dependsOn("putelephant")
```

**运行结果**

```
:opendoor UP-TO-DATE
:putelephant UP-TO-DATE
:closedoor UP-TO-DATE

BUILD SUCCESSFUL in 0s
```

**小细节（注意事项）**

可用于distrZip依赖  编译   class  jar  普处理文件

------

### task生命周期

**视频源：**13.task生命周期.avi

**知识点：**Task的两种生命周期

**概念：** 

扫描生命周期：gradle会首先找到每一个任务  执行每一个任务中闭包中的逻辑 

执行生命周期：即非扫描生命周期。可以用doFirst(先执行) , doLast(后执行)

**需求：** 循环遍历每个字符

**代码:**

```kotlin
task("putelephant") {
    doFirst {
        println("放入大象")
    }
}.dependsOn("opendoor")

//关闭冰箱门依赖于放入大象
task("closedoor") {
    doFirst {
        println("关闭冰箱门")
    }
}.dependsOn("putelephant")

//distrZip  编译  class  jar  普处理文件
task("opendoor") {
    doLast {
        println("打开冰箱门")
    }
}
```

**运行结果**

```
:opendoor
打开冰箱门
:putelephant
放入大象
:closedoor
关闭冰箱门
```

------

### task任务集

**视频源：**14.task任务集.avi

**知识点：**把任务放到任务集中表示 用group

**代码:**

```kotlin
tasks{
    "opendoor"{
        //任务组
        group = "大象放冰箱"
        doFirst {
            println("打开冰箱门")
        }
    }
    "putelephant"{
        group = "大象放冰箱"
        doFirst {
            println("放入大象")
        }
    }.dependsOn("opendoor")
    "closedoor"{
        group = "大象放冰箱"
        doFirst {
            println("关闭冰箱门")
        }
    }.dependsOn("putelephant")
}
```

**运行结果**

```kotlin
:opendoor
打开冰箱门
:putelephant
放入大象
:closedoor
关闭冰箱门
```

------

### 默认属性

**视频源：**15.默认属性.avi

**知识点：**gradle帮我们定义好的默认任务及默认属性

**代码:**

```kotlin
//获取默认属性
task("打印默认属性"){
    doFirst {
        project.properties.forEach { t, any ->
            println("$t ---- $any")
        }
    }
}
```

**运行结果**

```kotlin
:打印默认属性
parent ---- null
classLoaderScope ---- org.gradle.api.internal.initialization.DefaultClassLoaderScope@19683b67
.........等等很多键值对
```

------

##### 

### 依赖的配置阶段

**视频源：**26.依赖的配置阶段

**知识点：**depenendcies函数里面使用compile对依赖库坐标进行匹配

**概念：** gradle里面使用固定的函数关键字对jar包进行导入

**需求：** 导入jar包

**代码:**

```kotlin
dependencies {
    compile(kotlin("stdlib"))
    // https://mvnrepository.com/artifact/junit/junit
//    testCompile group: 'junit', name: 'junit', version: '4.12'
    compile("junit:junit:4.12")
}
```

**小细节（注意事项）**

 注意：依赖配置阶段分为两类,上面代码展示的是编译时依赖 ,在编辑时(上面代码的最后一行)如果时编译时需要在compile前加test

```java
dependencies {
    compile(kotlin("stdlib"))
    // https://mvnrepository.com/artifact/junit/junit
//    testCompile group: 'junit', name: 'junit', version: '4.12'
    testCompile("junit:junit:4.12")
}
```

------

### 版本冲突第一种解决方法

**视频源：**27.版本冲突第一种解决方法.avi

**知识点：**Gradle自动依赖最高版本的依赖

**概念：** 两个项目分别需要依赖一个jar包的两个不同的版本该怎么解决

**需求：** 解决项目版本冲突的问题

**代码:**

项目需要C和D两个依赖

C需要依赖A(haha.jar) 1.0版本

D需要依赖A(haha.jar)2.0版本

调用A里面的sayHello方法

究竟执行的是1.0版本的还是2.0版本的

**运行结果**

执行的时2.0版本,因为Gradle自动依赖最高版本的依赖

------

### 依赖冲突的解决

**视频源：**28.依赖冲突的解决.avi

**知识点：**关闭gradle依赖冲突默认处理方案以提示冲突,然后选择将某一个库依赖的版本去掉或者强制指定一个版本号

**概念：** 两个项目分别需要依赖一个jar包的两个不同的版本该怎么解决

**需求：** 解决项目版本冲突的问题

**代码:**

```kotlin
//将gradle依赖冲突默认处理方案关闭
compile("commons-httpclient", "commons-httpclient", "3.1") {
    exclude("commons-httpclient","commons-httpclient")
    }

//将某一个库依赖的版本去掉
compile("commons-httpclient", "commons-httpclient", "3.1") {
    exclude("commons-httpclient","commons-httpclient")
    }

//强制指定一个版本号
configurations.all {
    resolutionStrategy {
    failOnVersionConflict()

    force("commons-logging:commons-logging:1.2")
    }
}
```

**小细节（注意事项）**

 注意：理论上高版本的logging的jar包应该支持低版本的jar包,如果遇到不支持的情况,可以选择关闭gradle依赖冲突默认处理方案以提示冲突,然后选择将某一个库依赖的版本去掉或者强制指定一个版本号

------

### 拓展gradle任务

**视频源：**29.拓展gradle任务.avi

**知识点：**task的运用

**概念：** gradle的任务拓展

**需求：** 对目前已经创建好的文件经行拷贝,删除等操作

**代码:**

```kotlin
//拷贝文件
task("拷贝文件",Copy::class){
    from("src/main/java")
    into("temp")
}
//删除文件
task("删除文件",Delete::class){
    delete("temp")
}
```

**小细节（注意事项）**

 注意：https://gradle.org/官网上 -> docs -> Groovy DSL Reference ->在页面上找到Task types里面有更多的关键字,可以对文件进行更多的操作

------

------

##### 

### 分模块开发

**视频源：**30.多模块构建简介.avi

**知识点：**

- 模块的全局配置
- 整个工程的构建
- 当前模块的gradle构建
- gradle构建需要依赖的插件

**概念：** 一个工程里面有多个模块

**要求：**掌握插件的管理, 脚本之间的依赖

**补充:**  软件设计原则: 高内聚，低耦合

**代码:**

整个工程的gradle (buile.gradle.kts)

```kotlin
//负责整个工程的配置
//给每一个子项目配置

//查找插件, gradle构建的时候需要的配置, 有两种方式,一种是plugins,但idea支持不太好,另一种是apply方式会更好

//支持不太好
//    plugins{
//        application
//        kotlin("jvm")
//    }

//buildscript{}表示配置此项目的构建脚本类路径
buildscript {
    repositories {
        maven {
            setUrl("https://plugins.gradle.org/m2/")
        }
    }
    dependencies {
        classpath("org.jetbrains.kotlin:kotlin-gradle-plugin:1.2.41")
    }
}

//allprojects{} 所有子模块和当前模块的配置
//subprojrcts{} 所有子模块的配置,不包括当前工程的模块
allprojects { //只要在模块里面可以配置的属性都可以配置
    
   apply{
       plugin("org.jetbrains.kotlin.jvm")
   }
}

```

Core模块

```java
buile.gradle.kts
负责当前模块的配置
//plugins{
//    application
//    kotlin("jvm")
//}

//库
repositories {
    mavenCentral()
}
dependencies {
    compile(kotlin("stdlib"))
    // https://mvnrepository.com/artifact/junit/junit
    // testCompile group: 'junit', name: 'junit', version: '4.12'
    // 测试时依赖
    testCompile("junit","junit","4.12") 
}

```

```kotlin
TestNetUtils.ks

import org.junit.Assert
import org.junit.Test

class TestNetUtils {
    @Test
    fun testSendRequest() {
        val utils = NetUtils()
        val msg = utils.sendRequest()
        Assert.assertEquals("hello", msg)
    }
}
```

```kotlin
NetUtils.ks

class NetUtils {
    fun sendRequest():String{
        return "hello"
    }
}
```

App模块

```kotlin
buile.gradle.kts
负责当前模块的配置
//plugins{
//    application
//    kotlin("jvm")
//}

//库
repositories {
    mavenCentral()
}
dependencies {
    compile(kotlin("stdlib"))
    
    //导入Core工程依赖
    compile(project(":Core"))
}
```

```kotlin
Main.kt

fun main(args: Array<String>) {
    val utils = NetUtils()
    val msg = utils.sendRequest()
    println(msg)
}
```

**备注: **

idea的每一个project相当于eclipse的工作空间,module相当于eclipse的project工程

在多模块中, 比如有一个App和Core模块,每个模块中都有buile.gradle.kts负责当前模块的配置, 外面还有一个buile.gradle.kts是本工程的gradle,负责整个工程的配置