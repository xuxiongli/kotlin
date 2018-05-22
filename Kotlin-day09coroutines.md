## Kotlin-day10-coroutines

[TOC]

### JAVA线程的创建

**知识点：**创建线程的两种方式

​		通过Thread直接卖票的缺点

​		通过Runnable方式共享线程间数据

​		线程安全问题

**概念：**JAVA线程如何创建

**需求：** 创建各类线程前，工程准备：

​	——首先创建一个gradle工程

​	——不勾选java

​	——Groupld:com.itcast.coroutines

​		Artifactld:Coroutines

​	——勾选本地gradle版本

​	——修改build.gradle.kts

​	——关闭工程再打开

​	——group =  ""

​		version = ""

​	——src-main-java

​	——添加支持java开发的插件

​		plugins{

​			java				

​				}

​	——点选自动导入

​	——包：线程的创建

**代码:**

```kotlin
//主函数
class Main{
    public static void main(String[] args){
        System.out.println("主线程开始执行");
      /*---------------------------- 直接Thread创建 ----------------------------*/
//        MyThread thread = new MyThread();
//        thread.setName("线程1");
//        thread.start();
        /*---------------------------- 通过Runnable方式进行创建 ----------------------------*/
//      MyRunnable runnable = new MyRunnable();
//      Thread thread = new Thread(runnable);
//      thread.setName("线程2");
//      thread.start();
        /*---------------------------- 通过Thread卖票 ----------------------------*/
      /*  TicketThread thread1 = new TicketThread();
        TicketThread thread2 = new TicketThread();
        TicketThread thread3 = new TicketThread();
        thread1.setName("窗口1");
        thread2.setName("窗口2");
        thread3.setName("窗口3");
        thread1.start();
        thread2.start();
        thread3.start();
      */
      /*---------------------------- 通过Runnable方式卖票 ----------------------------*/
        TicketRunnable runnable = new TicketRunnable();
        Thread thread1 = new Thread(runnable);
        Thread thread2 = new Thread(runnable);
        Thread thread3 = new Thread(runnable);
        thread1.setName("窗口1");
        thread2.setName("窗口2");
        thread3.setName("窗口3");
        thread1.start();
        thread2.start();
        thread3.start();
        System.out.println("主线程结束执行");
    }
}

/*

class MyThread extends Thread {
    public void run(){
        for(int i=0;i<10;i++){
            System.out.println(getName()+"-打印了"+i);
            try{
                Thread.sleep(500L);
            }catch(InterruptedException e){
                e.printStackTrace();
            }
        }
    }
}
*/
class MyRunnable implements Runnable{

    @Override
    public void run() {
        for(int i=0;i<10;i++){
            System.out.println(Thread.currentThread().getName()+"-打印了"+i);
            try{
                Thread.sleep(3000L);
            }catch(InterruptedException e){
                e.printStackTrace();
            }
        }
    }
}

//TicketRunnable类
class TicketRunnable implements Runnable {
    private int tickets = 100;

    @Override
    public void run() {
        while (true){
            synchronized (this) {
                if (tickets > 0) {
                    System.out.println(Thread.currentThread().getName() + "卖出第" + tickets + "张票");
                    try {
                        Thread.sleep(500);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    tickets--;
                }
            }
        }
    }
}

//TicketThread类
class TicketThread extends Thread {
    private int tickets = 100;

    @Override
    public void run() {
        while (true){
            if (tickets>0){
                System.out.println(getName()+"卖出第"+tickets+"张票");
                tickets--;
            }
        }
    }
}

```

**运行结果**

```
主线程开始执行
主线程结束执行
窗口1卖出第100张票
窗口3卖出第99张票
窗口3卖出第98张票
	...
窗口1卖出第2张票
窗口3卖出第1张票
```

**小细节（注意事项）**

线程安全问题出现的原因:

 * 是否是多线程   
 * 是否有共享数据 
 * 是否有多条语句操作共享数据 
 * 解决重复卖出，方案:同步关键字

```java
synchronized (this){
    
}
```

------

### 线程join

**知识点：**普通线程执行没有顺序，并行执行

​	   	加入join方法后，变为串行执行

**概念：**join   必须要等到当前线程执行结束 才能结束

**需求：**thread.join()

**代码:**

```kotlin
class Main {
    public static void main(String[] args){
        //普通线程执行没有顺序 并行执行
        System.out.println("主线程开始执行");

        MyThread thread = new MyThread();
        thread.setName("线程1");
        thread.start();
        //join  必须要等到当前线程执行结束 才能结束  相当于把线程的并行执行 变成了串行执行
        try {
            thread.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("主线程结束执行");
    }
}
class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println(getName()+"-打印了"+i);
            try {
                Thread.sleep(500L);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

**运行结果**

```
主线程开始执行
线程1-打印了0
线程1-打印了1
线程1-打印了2
线程1-打印了3
线程1-打印了4
线程1-打印了5
线程1-打印了6
线程1-打印了7
线程1-打印了8
线程1-打印了9
主线程结束执行
```

------

### 守护线程

**知识点：**自带的的一些方法

**概念：** 用户线程 ：主线程执行结束 用户线程可以继续执行

​	    守护线程：在程序运行的时候在后台提供一种通用的服务的线程(垃圾回收线程)

**需求：**thread.setDaemon(true)

**代码:**

```kotlin
class Main {
    public static void main(String[] args){
		//普通线程
        //用户线程  主线程执行结束 用户线程可以继续执行
        //主线程执行结束之后 垃圾回收线程也应该终止掉 垃圾回收线程就是属于守护线程
        //执行没有顺序 并行执行
        System.out.println("主线程开始执行");

        MyThread thread = new MyThread();
        thread.setName("线程1");
        //设置为守护线程  必须在启动前调用
        //主线程执行结束之后 线程就停止了
        thread.setDaemon(true);

        thread.start();

        System.out.println("主线程结束执行");
    }
}
```

**运行结果**

```
主线程开始执行
主线程结束执行
```

**小细节（注意事项）**

设置为守护线程，主线程执行结束之后 线程就停止了，但有个停止过程

------

### 线程池

**知识点：**线程池如何创建使用

**概念：** 线程池是指线程池里的每一个线程代码结束后,并不会死亡,而是再次回到线程池中成为空闲状态,等待下一个对象来调用

**需求：** 运用线程池打印0-10

**代码:**

```kotlin
class Main {
    public static void main(String[] args){
        //创建线程池
        //创建线程池 指定线程数量
        ExecutorService service =  Executors.newFixedThreadPool(3);//单核cpu 真实开发可以设置成核心数-1
        //执行多线程操作
        //可以通过execute执行 执行对象就是Runnable对象
        MyRunnable runnable = new MyRunnable();
        service.execute(runnable);
        service.execute(runnable);
        
    }
}
```

**运行结果**

```
主线程开始执行
线程1-打印了0
线程1-打印了1
线程1-打印了2
线程1-打印了3
线程1-打印了4
线程1-打印了5
线程1-打印了6
线程1-打印了7
线程1-打印了8
线程1-打印了9
主线程结束执行
```

**小细节（注意事项）**

JDK5开始，java内置支持线程池

线程池线程执行完成之后比并没有走销毁流程 而是回到线程池中 成为初始状态 等待下次执行

**总结**

程序启动一个线程成本是比较高的,因为它涉及到要与操作系统进行交互.而使用线程池可以很好的提高性能,尤其是当程序中要创建大量生命期很短的线程时,更应该考虑使用线程池. 

------

### Kotlin协程:第一个协程启动

**知识点：**协程的启动

- launch是一个函数, 协程需要通过launch函数启动
- launch前三个参数都是默认参数, 参数值可以不指定
- 最后一个参数是函数类型, 调用的时候通过lambda表达式接收
- launch函数的返回值是Job类型, 就是协程的任务

**需求：**配置支持Kotlin插件：

github搜索Kotlinx——Gradle下复制仓库以及依赖粘贴回去

**代码1:**

```kotlin
group = "com.itcast.coroutines"
version = "1.0-SNAPSHOT"

plugins {
    //添加支持java开发的插件
    java
    //支持开发kotlin的插件
    kotlin("jvm")
}
//仓库
repositories {
    mavenCentral()
    //协程jar包的仓库
    jcenter()
}
//依赖
dependencies {
    compile(kotlin("stdlib"))
    //配置协程jar包依赖
    compile("org.jetbrains.kotlinx:kotlinx-coroutines-core:0.22.5")
}
```

**代码2:**

```kotlin
import kotlinx.coroutines.experimental.launch
fun main(args: Array<String>) {
    println("主线程开始执行")

    //开启协程
    launch {
        println("协程执行了")
    }

    println("主线程结束执行")

    //主线程睡眠
    Thread.sleep(1000L)
}
```

**运行结果**

```
主线程开始执行
主线程结束执行
协程执行了
```

------

### 协程打印数据

**代码:**

```kotlin
import kotlinx.coroutines.experimental.launch
fun main(args: Array<String>) {
    println("主线程开始执行")

    //开启协程
    launch {
        (1..10).forEach {
            println("打印了$it")
            //睡眠5秒钟
            Thread.sleep(500L)
        }
    }
    println("主线程结束执行")
    //主线程睡眠
    Thread.sleep(5000L)
}
```

**运行结果**

```
主线程开始执行
主线程结束执行
打印了1
打印了2
打印了3
打印了4
打印了5
打印了6
打印了7
打印了8
打印了9
打印了10
```

**小细节（注意事项）**

 配置gradle下载jar包时，可关闭https来提高下载速度

**总结**

协程创建三步走

第一步:创建gradle工程

第二步:添加依赖

​       compile'org.jetbrains.kotlinx:kotlinx-coroutines-core:0.22.5'

第三步:创建协程

------

### launch函数的参数和返回值

**需求：** Ctrl+launch   查看launch参数及返回值

**代码:**

```kotlin
import kotlinx.coroutines.experimental.launch

fun main(args: Array<String>) {
    //协程启动
    //launch是一个函数 协程需要通过launch函数启动
    //launch前三个参数都是默认参数 参数值可以不指定
    //最后一个参数是函数类型 调用的时候通过lambda表达式接收
    //launch函数的返回值是Job类型 就是协程的任务
    launch {
        println("协程执行了")
    }

    Thread.sleep(1000L)
}
```

------

### launch函数的第一个参数

**代码:**

```kotlin
import kotlinx.coroutines.experimental.launch

fun main(args: Array<String>) {
    //协程启动
   
    /**
     * launch函数第一个参数:协程上下文
     * context: CoroutineContext = DefaultDispatcher
     * context类型是CoroutineContext默认值就是DefaultDispatcher
     * DefaultDispatcher就是CommonPool
     * 第一个参数默认情况下就是赋值为CommonPool
     * CommonPool实现原理是通过ForkJoinPool实现的
     * ForkJoinPool就是线程池
     * 协程其实也是在线程里面执行
     * 第一个参数就是制定协程执行在哪个线程或者线程池
     */
    launch {
        println("协程执行了")
    }

    Thread.sleep(1000L)
}
```

------

### ForkJoinPool

**知识点：**

- 普通线程池, 在主线程执行结束之后, 可以继续执行


- ForkJoinPool 主线程执行完之后 ForkJoinPool里面的线程也结束了
- 普通的线程池创建的用户线程
- ForkJoinPool创建的是守护线程
- 通过launch函数启动协程, 运行在线程池中, 启动的线程池默认是守护线程

**代码:**

```kotlin
//常规的线程池执行

fun main(args: Array<String>) {

    println("主线程开始执行")

    //定义线程池
    val service = Executors.newFixedThreadPool(3)
    //创建Runable对象
    val runnable = MyRunable()
    //执行
    service.execute(runnable)
 
    println("主线程结束执行")
}

class MyRunable:Runnable{
    override fun run() {
        (1..5).forEach {
            println(it)
            Thread.sleep(500L)
        }
    }
}
```

**打印结果:**

```
主线程开始执行
主线程结束执行
1
2
3
4
5
```

**ForkJoinPool实现**

```kotlin
//ForkJoinPool实现

fun main(args: Array<String>) {

    println("主线程开始执行")
   
    val service = ForkJoinPool(3)
    //创建Runable对象
    val runnable = MyRunable()
    //执行
    service.execute(runnable)

    Thread.sleep(2000L)
    println("主线程结束执行")
}

class MyRunable:Runnable{
    override fun run() {
        (1..10).forEach {
            println(it)
            Thread.sleep(500L)
        }
    }
}
```

**打印结果:**

```
主线程开始执行
主线程结束执行
```

------

### 协程启动之后的处理

**知识点：**

解决方案1: 让主线程睡眠

解决方案2: 通过协程返回的Job任务执行join方法(main runblocking)

**代码:**

```kotlin
fun main(args: Array<String>) = runBlocking{
    println("主线程开始执行")

    val job = launch {
        println("协程执行了")
    }

    println("主线程结束执行")
    
    //解决方案1: 让主线程睡眠
    //Thread.sleep(100L)
  
    //解决方案2:通过协程返回的Job任务执行join方法(main runblocking)
    job.join()
}
```

------

### 协程实现原理

**知识点：**

1. 协程实现原理:

- 可以把耗时任务先挂起
- 等时间到了再从线程池中空闲的线程执行
- 必须是挂起函数才能挂起

**代码**

```kotlin
fun main(args: Array<String>) = runBlocking {
    //线程池
    val job = launch {
        println("协程执行前:${Thread.currentThread().name}")

        //等待 sleep() 非阻塞
        delay(2000L)
//        Thread.sleep(2000L) //不能被挂起

        println("协程执行后:${Thread.currentThread().name}")
    }
  
    //lunch是单例 开启之后用的还是同一个线程池
    launch {
        Thread.sleep(3000L)
    }
    launch {
        Thread.sleep(3000L)
    }
    launch {
        Thread.sleep(3000L)
    }
    launch {
        Thread.sleep(3000L)
    }
   
    job.join()
}

```

------

### 挂起函数

**知识点：**

- 可以被挂起执行, 到时间后从线程池中空闲的线程中恢复执行
- 挂起函数必须通过suspend进行修饰
- 挂起函数必须在协程中执行, 或者在其它挂起函数中执行

**代码:**

```kotlin
fun main(args: Array<String>) = runBlocking{
    val job = launch {
        println("线程开启")
        delay(1000L)
        println("线程结束")
    }
    job.join()
}

fun main(args: Array<String>) = runBlocking{ //主协程
    val job = launch {
    	myTask()
    }
    job.join()
}
suspend fun myTask(){
	println("协程开启")
	delay(2000L)
	println("协程结束")
}
```

------

### 线程和协程效率对比

**知识点：**

```kotlin
fun main(args: Array<String>) = runBlocking {
    /*---------------------------- 线程耗时 ----------------------------*/
    //开始时间
//    val startTime = System.currentTimeMillis()
//
//    val threadList = List(100000){
//        MyThread()
//    }
//    threadList.forEach { it.start() }
//    threadList.forEach { it.join() }
//    //结束时间
//    val endTime = System.currentTimeMillis()
//    //耗时
//    val time = endTime-startTime
//    println("线程耗时$time") //8962

    /*---------------------------- 协程执行 ----------------------------*/
    val startTime = System.currentTimeMillis()
    val coroutinesList = List(100000){
        launch {
            println(".")
        }
    }
    coroutinesList.forEach {
        it.join()
    }
    val endTime = System.currentTimeMillis()
    val time = endTime - startTime
    println("协程耗时$time") //918
}
class MyThread:Thread(){
    override fun run() {
        println(".")
    }
}
```

------

### 协程的取消

**知识点：**cancel关键字

**代码:**

```kotlin
fun main(args: Array<String>)= runBlocking {
    val job = launch {
        (1..10).forEach {
            println("打印$it")
            delay(500L)
        }
    }
    //定时2秒钟  停止协程
    delay(2000L)
  
    //取消协程
    job.cancel()

    job.join()
}
```

------

### 协程定时取消

**知识点：**withTimeout关键字

**代码:**

```kotlin
fun main(args: Array<String>) = runBlocking {
    val job = launch {
        withTimeout(2000L) {
            (1..10).forEach {
                println("打印$it")
                delay(500L)
            }
        }
    }
    job.join()
}
```

------

### 协程取消失效

**知识点：**不能取消阻塞的Thread.sleep

**代码:**

```kotlin
fun main(args: Array<String>) = runBlocking{
    val job = launch {
        (1..10).forEach {
            println("打印$it")
            Thread.sleep(500L)
        }
    }
    //定时2秒钟  停止协程
    delay(2000L)
  
    //取消协程
    job.cancel() //取消挂起 不能取消阻塞的Thread.sleep

    job.join()
}
```

------

### 协程取消失效解决方案

**视频源：** 23.协程取消失效的处理.avi

**知识点：**判断isActivie状态  根据这个状态再执行协程的代码

**代码:**

```kotlin
fun main(args: Array<String>) = runBlocking{
    val job = launch {
        //协程里面可以获取isActivie状态
        (1..10).forEach {
            if(!isActive)return@launch//返回协程
            println("打印$it")
            Thread.sleep(500L)
        }
    }
    //定时2秒钟  停止协程
    delay(2000L)

    //协程状态
    println("取消之前${job.isActive}")

    //取消协程
    job.cancel()//取消挂起  不能取消阻塞的Thread.sleep

    println("取消之后${job.isActive}")

    job.join()
}
```

------

### 协程async启动

**知识点：**

- 第一种启动协程方式: launch, 没有返回值


- 第二种启动协程方式: async, 有返回值

**代码:**

```kotlin
fun main(args: Array<String>) = runBlocking{
//    val job = launch {
//        job1()
//    }
//    println("获取到了job1返回值")

    /*---------------------------- async启动协程 返回值 ----------------------------*/
    //Deferred 是job的子类
    val job1 = async { job1() }
    val job2 = async { job2() }
    val result1 = job1.await()//执行完协程之后才能获取到数据
    val result2 = job2.await()
    println(result1)
    println(result2)

}
suspend fun job1():String{
    println("开始执行job1")
    delay(1000L)
    println("执行了job1")
    return "job1"
}
suspend fun job2():String{
    println("开始执行job2")
    delay(1000L)
    println("执行了job2")
    return "job2"
}
```

------

### 协程上下文

**代码:**

```kotlin
fun main(args: Array<String>)= runBlocking {
    //第一个代表协程上下文 指定协程代码在哪个线程池中运行 默认是通过Commpool实现的
    val job1 = launch {
        println("协程执行")
    }
    val job2 = launch(CommonPool) {
        println("协程执行")
    }
    val job3 = launch(Unconfined) {//运行在主线程中
        println("协程执行")
    }
    val job4 = launch(coroutineContext) {//运行在父协程的上下文中 当前运行在主线程中
        println("协程执行")
    }
    val job5 = launch(newFixedThreadPoolContext(5,"新线程")) {//自定义线程池执行
        println("协程执行")
    }
    job1.join()
    job2.join()
}
```

