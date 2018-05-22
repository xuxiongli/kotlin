###for标签

**视频源：** 02.for循环和foreach循环.avi

**知识点：**for标签

**概念：** 循环的一种写法


**需求：** 循环遍历一个字符串中的每个字符

**代码:**

```
fun main(args: Array<String>) {
	val str  = "abcd"
	for (a in str) {
    	println(a)
    }
}
```


**运行结果**

```
a
b
c
d
```

**小细节（注意事项）**

 注意：java中不可以仿造kotlin的写法对集合进行循环遍历，在编写for代码时str无法编译通过

```
 public static void main(String[] args){
     String str  = "abcd";
     for (char a:str) {
        System.out.println(a)；
     }
 }
```

---

### foreach标签

**视频源：** 02.for循环和foreach循环.avi

**知识点：**foreach标签

**概念：** 循环的一种写法

**需求：** 循环遍历每个字符

**代码:**

```
fun main(args: Array<String>) {
	val str  = "abcd"
	str.forEach {
        println(it)
    }
}
```

**运行结果**

```
a
b
c
d
```

---

### foreach标签获取循环角标

**视频源：** 02.for循环和foreach循环.avi

**知识点：**foreach标签

**概念：** 循环获取角标

**需求：** 循环遍历每个字符，并获取角标

**代码:**

```
fun main(args: Array<String>) {
	val str  = "abcd"
	str.forEachIndexed { index, c ->
        println("index=$index c=$c")
    }
}
```

**运行结果**

```
index=0 c=a
index=1 c=b
index=2 c=c
index=3 c=d
```

---

### continue关键字

**视频源：** 03.continue和break.avi

**知识点：**continue关键字

**概念：** continue关键字跳出本次循环

**需求：** 跳过字符‘c’的打印

**代码:**

```
fun main(args: Array<String>) {
    val str = "abcde"
    for (c in str) {
        if(c=='c'){
            continue
        }
        println(c)
    }
}
```

**运行结果**

```
a
b
d
e
```

**总结**

结果说明，在执行for循环的过程中，如果匹配上字符‘c’则运行执行continue关键字，跳出本次循环，进入下一次循环，所以在打印结果的时候，不会打印字符c

---

### break关键字

**视频源：** 03.continue和break.avi

**知识点：**break关键字

**概念：**break关键字跳出循环

**需求：** 只打印字符a和b

**代码:**

```
fun main(args: Array<String>) {
    val str = "abcde"
    for (c in str) {      
        if(c=='c'){
            break
        }
        println(c)
    }
}
```

**运行结果**

```
a
b
```

**小细节（注意事项）**

break和continue只能用在for循环中，不可以用在foreach循环中

---

### 双重for循环

**视频源：** 04.标签处返回.avi

**知识点：**双重for循环

**需求：** 打印abc和123所有的组合

**代码:**

```
fun main(args: Array<String>) {
    val str1 = "abc"
    val str2 = "123"
	for (c1 in str1){
        for (c2 in str2){
            println("$c1 $c2")
        }
    }
}
```

**运行结果**

```
a 1
a 2
a 3
b 1
b 2
b 3
c 1
c 2
c 3
```

**小细节（注意事项）**

break和continue只能用在for循环中，不可以用在foreach循环中

---

### return关键字

**视频源：** 04.标签处返回.avi

**知识点：**return关键字

**概念：**return关键字返回，结束整个函数

**需求：** 熟悉return关键字的作用

**代码:**

```
fun main(args: Array<String>) {
	val str1 = "abc"
    val str2 = "123"
	for (c1 in str1){
        for (c2 in str2){
            println("$c1 $c2")
            if (c1 == 'b' && c2 == '2'){
                return
            }
        }
    }
	println("最终执行的代码")
}
```

**运行结果**

```
a 1
a 2
a 3
b 1
b 2
```

**总结**

确实在得到b2后，后续的字符串没有再打印，但是“最终执行的代码”这句话也没有打印，return关键字结束了main方法，也就是触发return后后续代码都不执行，所以不满足目前需求，使用break或者continue关键字同样得到的是错误结果

---

### tag@使用

**视频源：** 04.标签处返回.avi

**知识点：**tag@使用

**概念：**tag@用于指定循环在何处跳出

**需求：** 获取b2后，后面的元素不再打印，并且确保循环后续的代码能够执行

**代码:**

```
fun main(args: Array<String>) {
	val str1 = "abc"
    val str2 = "123"
	tag@for (c1 in str1){
        for (c2 in str2){
            println("$c1 $c2")
            if (c1 == 'b' && c2 == '2'){
                break@tag
            }
        }
    }
	println("最终执行的代码")
}
```

**运行结果**

```
a 1
a 2
a 3
b 1
b 2
最终执行的代码
```

**总结**

@tag添加在break后，代表的是break关键字要跳出的循环是那个，按照要求，需要跳出的事外层的for循环，所以tag@关键字添加在第一个for循环前面，在循环执行完后，还顺序打印"最终执行的代码"字符串。

---

### while循环

**视频源：** 05.while和do while.avi

**知识点：**while循环

**概念：**while循环写法

**需求：** while循环从1打印到100

**代码:**

```
fun main(args: Array<String>) {
    var a= 0
    while (a<=100){
        println(a)
        a++
    }
} 
```

**运行结果**

```
1
2
...
100
```

**总结**

只要满足while括号内的条件，则while中的代码会一直执行，所以打印从1到100

---

### do while循环

**视频源：** 05.while和do while.avi

**知识点：**do while循环

**概念：**do while循环写法

**需求：** do while循环从1打印到100

**代码:**

```
fun main(args: Array<String>) {
    var a= 0
    do {
        println(a)
        a++
    }while (a<=100)
} 
```

**运行结果**

```
1
2
...
100
```

**小细节（注意事项）**

while循环在执行的时候，先考核是否满足条件，如果满足条件在执行，而do while循环则是先执行循环内部的代码，执行完后再判断while中的条件，如果条件满足则再次触发下一次循环操作

**总结**

只要满足while括号内的条件，则while中的代码会一直执行，所以打印从1到100

---

### 区间Range

**视频源：** 06.区间定义.avi

**知识点：**区间Range

**概念：**从起点a到终点b的一个范围

**需求：**编写多种数据类型区间

**代码:**

```
fun main(args: Array<String>) {
    /*---------------------------- 定义1到100 ----------------------------*/
    val range1 = 1..100
    val range2 = IntRange(1,100)
    val range3 = 1.rangeTo(100)
    /*---------------------------- 长整形区间 ----------------------------*/
    val range4 = 1L..100L
    val range5 = LongRange(1L,100L)
    val range6 = 1L.rangeTo(100L)
    /*---------------------------- 字符区间 ----------------------------*/
    val range7 = 'a'..'z'
    val range8 = CharRange('a','z')
    val range9 = 'a'.rangeTo('z')
}
```

**总结**

..代表的就是范围

---

### 区间遍历

**视频源：** 07.区间的遍历.avi

**知识点：**区间遍历

**概念：**区间遍历用法

**需求：**for循环遍历1到100区间

**代码:**

```
fun main(args: Array<String>) {
    val range = 1..100
    for (i in range) {
        println(i)
    }
}
```

**运行结果**
```
1
2
...
100
```
---

### 区间遍历打印索引

**视频源：** 07.区间的遍历.avi

**知识点：**区间遍历打印索引

**概念：**区间遍历用法

**需求：**for循环遍历1到100区间，并打印索引

**代码:**

```
fun main(args: Array<String>) {
    val range = 1..100
    for ((index,i) in range.withIndex()) {
        println("index=$index i=$i")
    }
}
```

**运行结果**

```
index=0 i=1
index=1 i=2
......
index=99 i=100
```

---

### 反向区间

**视频源：** 08.反向区间和区间的反转.avi

**知识点：**反向区间

**概念：**反向区间，如由100到1的区间

**需求：**for循环遍历100到1区间

**代码:**

```
fun main(args: Array<String>) {
    val range = 100 downTo 1
    range.forEach {
        println(it)
    }
}
```

**运行结果**

```
100
...
3
2
1
```

------

### 循环步长

**视频源：** 08.反向区间和区间的反转.avi

**知识点：**循环步长

**概念：**设定每次循环间隔的大小

**需求：**for循环遍历100到1区间，并设定步长为20

**代码:**

```
fun main(args: Array<String>) {
    val range = 100 downTo 1
    for (i in range step 20) {
       println(i)
    }
}
```

**运行结果**

```
100
80
60
40
20
```

**小细节（注意事项）**

step约定步长为20，则以20为间隔进行循环

---

### 区间反转

**视频源：** 08.反向区间和区间的反转.avi

**知识点：**区间反转

**概念：**比如，将1到100的区间，反转为100到1

**需求：**将1到100的区间，反转为100到1

**代码:**

```
val range1 = 1..100
val range2 = range1.reversed()
```



### 数组的创建

**视频源：**09.数组创建.avi

**知识点：**数组的概念，数组的定义，数组的创建

**概念：**

```
首先数组是个容器，容器就是一个对象可以存储多条数据；
有序，存储元素有先后顺序，先存储的顺序靠前；
可重复，数组可以存储重复的数据；
数组长度创建就已经固定。
```

**需求：** 定义数组并赋值

**代码:**

```
//我们可以通过arrayOf方法创建数组，也可以通过Array类的构造方法创建。

//方式一： 使用arrayOf定义数组并赋值：
fun main(args: Array<String>) {
	/*--- 定义数组并赋值 ------*/
    //张三  李四  王五
    val arr = arrayOf("张三","李四","王五")
    //10 20 30
    val arr1 = arrayOf(10,20,30)
    //a b c
    val arr2 = arrayOf('a','b','c')
    //"张三"  10  'a'
    val arr3 = arrayOf("张三",10,'a')  // 这里是Any类型
}


//方式二：通过Array类的构造方法创建：
/*---- 8种基本数组类型数组 --------*/
eg:
//	 IntArray
//    BooleanArray
//    ByteArray
//    ShortArray
//    CharArray
//    FloatArray
//    DoubleArray
//    LongArray
//保存年纪信息
val arr4 = IntArray(10)//new int[10]

//把数组里面每一个元素都初始化为30
val arr5 = IntArray(10){
    30
}
......

```

**小细节（注意事项）**

 注意：

- 字符串数组只能通过Array创建

```
//    StringArray	//不能用 ,因为字符串数组只能通过Array创建
```

**总结**

- ​8种基本数据类型的数组可以通过Array创建或者通过自己的数据类型Array创建


- 字符串数组只能通过Array创建



### 数组的遍历

**视频源：** 无（本节为补充内容）

**知识点：**数组的遍历

**概念：**获取数组的每一个元素

**需求：** 打印出每一个数组每一个元素的值

**代码:**

```
//方式一：for 
fun main(args: Array<String>) {
	val array = arrayOf(1,2,3,4,5)

    //for循环遍历
    for (s in array) {
           println(s)
    }
    //for循环遍历元素以及角标
    for ((index,s) in array.withIndex()) {
         println("角标$index s=$s")
    }
}

//方式二：foreach
fun main(args: Array<String>) {
	val array = arrayOf(1,2,3,4,5)

    //foreach高级for循环
    array.forEach {
        println(it)
    }

    //foreach遍历元素及角标
    array.forEachIndexed { index, s ->
            println("角标=$index 元素=$s")
    }
}
```

**运行结果**

```
1
2
3
4
5
```



### 数组元素的修改

**视频源：** 10.数组元素的修改.avi

**知识点：**数组元素的修改

**概念：** 数组修改某一个元素的值

**需求：** 修改数组指定角标的值

**代码:**

```
//数组修改某一个位置上的元素主要有两种方式：

fun main(args: Array<String>) {
	val array = arrayOf(1,2,3,4,5)

     方式一：  使用下标修改：
      array[2] = 6     //将角标2元素值设置为6

     方式二：  使用set方法修改：
      array.set(2,9)   //将角标2元素值设置为9
}
```

**运行结果**

```
//方式一：
1
2
6
4
5

//方式二：
1
2
9
4
5
```

**小细节（注意事项）**

 注意：数组起始位置从0开始，第一个元素的位置为0，第二个元素位置为1，以此类推。

**总结**

数组修改指定位置元素可以用下标修改、set方法修改



### 数组元素角标的查找

**视频源：** 11.数组元素角标的查找.avi

**知识点：**查找数组元素的角标

**概念：** 数组获取指定元素的角标

**需求：** 查找指定元素的角标

```
arrayOf("张三","李四","张四","王五","张三","赵六")
查找第一个”张三”角标
查找最后一个”张三”角标
查找第一个姓”张”的人的角标
查找最后一个姓”张”的人的角标
```

**代码:**

```
val array = arrayOf("张三","李四","张四","王五","张三","赵六")

//查找第一个”张三”角标，返回第一对应的元素角标  如果没有找到返回-1
val index1 = array.indexOf("张三")
println(index1)

//查找最后一个”张三”角标，找不到 返回-1
val index2 = array.lastIndexOf("张三")
println(index2)

//查找第一个姓”张”的人的角标
val index3 = array.indexOfFirst {
    it.startsWith("张")
}
println(index3)

//查找最后一个姓”张”的人的角标
val index4 = array.indexOfLast { it.startsWith("张") }
println(index4)
```

**运行结果**

```
0
4
0
4
0
```

**小细节（注意事项）**

 注意：使用indexOf的方式查找元素，如果有该元素，返回第一对应的元素角标 ，如果没有找到返回-1。

```
val array = arrayOf("张三","李四","张四","王五","张三","赵六")

//查找第一个”王麻子”角标，返回第一对应的元素角标  如果没有找到返回-1
val index1 = array.indexOf("王麻子")
println(index1)  // 这里会返回-1
```

**总结**

```
array.indexOf()   	//返回第一对应的元素角标  如果没有找到返回-1
array.lastIndexOf()	//查找最后一个对应元素的角标，如果找不到返回-1
array.indexOfFirst {   }  // 查找第一个符合条件的元素角标
array.indexOfLast {  }    // 查找最后一个符合条件的元素角标
```

### java的switch

**视频源：** 12.java的switch语句回顾.avi

**知识点：**java的switch

**概念：** 多分支语句

**需求：** 根据学生成绩等级判断优秀与否

**代码:**

```
class SwitchDemo {
    public static void main(String[] args) {
        String result = level('A');
        System.out.println(result);
    }
    public static String level(char grade) {
        switch (grade) {
            case 'A':
                return "优秀";
            case 'B':
                return "优秀";
            default:
                return "一般";
        }
    }
}
```

**运行结果**

```
优秀
```

**小细节（注意事项）**

 分支如果不需要返回结果,用break结束分支处理

------

### switch多分支合并处理

**视频源：** 12.java的switch语句回顾.avi

**知识点：**switch多分支合并处理

**概念：** 多个分支统一处理

**需求：** 根据学生成绩等级判断优秀与否

**代码:**

```
class SwitchDemo {
    public static void main(String[] args) {
        String result = level('A');
        System.out.println(result);
    }
    public static String level(char grade) {
        switch (grade) {
            case 'A':
            case 'B':
                return "优秀";
            default:
                return "一般";
        }
    }
}
```

**运行结果**

```
优秀
```

**小细节（注意事项）**

 对于'A'和'B'的处理相同,可以省略'A'的break或者return

------

### when表达式

**视频源：** 13.when表达式.avi

**知识点：**when表达式

**概念：** 多分支语句,类似java的switch

**需求：** 判断给定的年龄要做什么事情

**代码:**

```
fun main(args: Array<String>) {
    val age = 12
   
    val result = todo(age)
    println(result)
}

fun todo(age:Int):String{
    when(age){
        7-> return "开始上小学"

        12->{
            return "开始上中学"
        }
        15->{
            return "开始上高中"
        }
        18->{
            return "开始上大学"
        }
        else->{
            return "开始上社会大学"
        }
    }
}
```

**运行结果**

```
开始上中学
```

**小细节（注意事项）**

 每个分支通过{}里面处理,如果只有一行可以省略{}

------

### when表达式加强

**视频源：** 14.when表达式加强.avi

**知识点：**when表达式加强

**需求：** 判断给定的年龄要做什么事情

**代码:**

```
fun main(args: Array<String>) {
    val age = 20
    val result = todo(age)
    println(result)
}

fun todo(age:Int):String{
    when(age){
        //在区间里面
        in 1..6-> return "没有上小学"

        7-> return "开始上小学"
        in 8..11->return "在上小学"

        12->{
            return "开始上中学"
        }
        13,14->return "正在上中学"
        15->{
            return "开始上高中"
        }
        16,17->return "正在上高中"
        18->{
            return "开始上大学"
        }
        in 19..22->return "正在上大学"
        else->{
            return "开始上社会大学"
        }
    }
}
```

**运行结果**

```
正在上大学
```

------

### when表达式原理

**视频源：** 15.when表达式原理.avi

**知识点：**when表达式原理

**概念：**对于普通的when表达式,最终会转化为switch语句执行,对于复杂的when表 达式会转化为if else语句执行

**需求：** 查看when表达式对应的java代码

------

### when表达式不带参数

**视频源：** 16.when表达式不带参数.avi

**知识点：**when表达式不带参数

**概念：**when表达式后面可以不用跟参数

**需求：** 判断给定的年龄要做什么事情

**代码:**

```
fun main(args: Array<String>) {
    val age = 12
    
    val result = todo(age)
    println(result)
}

fun todo(age:Int):String{
    when{
        age==7->{
            return "开始上小学"
        }
        age==12->{
            return "开始上小学"
        }
        age==15->{
            return "开始上小学"
        }
        age==18->{
            return "开始上小学"
        }
        else->{
            return "开始上社会大学"
        }
    }
}
```

**运行结果**

```
开始上小学
```

------

### when表达式返回值

**视频源：** 17.when表达式返回值.avi

**知识点：**when表达式返回值

**概念：**when表达式返回值为每个分支{}最后一行

**需求：** 判断给定的年龄要做什么事情

**代码:**

```
fun main(args: Array<String>) {
    val age = 7
    
    val result = todo(age)
    println(result)

}

fun todo(age:Int):String{
    val result =  when(age){
        7-> {
            println("找到了分支")
            10
             "开始上小学"
        }

        12->{
             "开始上中学"
        }
        15->{
             "开始上高中"
        }
        18->{
             "开始上大学"
        }
        else->{
             "开始上社会大学"
        }
    }
    return result
}
```

**运行结果**

```
开始上小学
```









