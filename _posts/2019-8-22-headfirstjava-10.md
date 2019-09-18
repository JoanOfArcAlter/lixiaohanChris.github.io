---
layout: post
title:  "headfirstjava第十章笔记"
date:   2019-08-22 17:55:00 +0800
categories: Java
tags: Java HeadFirstJava
author: lxh
mathjax: true
---

* content
{:toc}
head first Java第十章



## 静态方法

### Math方法：最接近全局的方法

> Java中没有东西的全局（global）的，但是`Math`的方法似乎不受到实例变量的影响，例如`round()`方法，你就算是创建10000个`Math`实例，`round(114.514)`的值也是不会变的（115），它永远都执行相同的工作——取出浮点数（方法的参数）的最接近整数值（四舍五入），唯一能够影响`round()`行为的只有传入的参数。哪还有必要在宝贵的堆上创建Math实例吗？

> 事实上不会有`Math`的实例。因此也不会有空间被它的实例所占用。因为你无法创建`Math`。如果你硬要`new`一个`Math`实例，编译器会报错，`Math`的构造函数被标记为私有的，这代表你不能新建`Math`的对象。

> 在Math这个类中的所有方法丢不需要实例变量值。因为这些方法都是静态的，所以你无需Math的实例。你会用到的只有它的类本身。

```
int x = Math.round(114.514);
int y = Math.min(114,514);
int z = Math.abs(-777);
//这些方法无需实例变量，因此也不需要特定对象来判别行为。
```

### 静态方法和非静态方法的区别

> `static`这个关键词可以标记出不需类实例的方法。一个静态的方法代表“一种不依靠实例变量也就不需要对象的行为”

###### 非静态方法

```
public class Song{
    String title;//实例变量的值会影响到play()方法的行为
    public Song(String t){
        title = t;
    }
    public void play(){
        SoundPlayer player = new SoundPlayer();
        player.playSound(title);//title的值会决定play()方法所播放的内容
    }
}
```

###### 静态方法

```
public static int min(int a,int b){//用static标记
    //返回a与b中较小的值
}
//调用时直接用类的名字Math.min(114,514)
//没有对象！
```

###### 以类的名称调用静态方法

```
Math.min(114,514);
```

###### 以引用变量的名称调用非静态方法

```
Song t2 = new Song();
t2.play();
```

> 类用`abstract`标记为抽象类可以防止被初始化，你也可以用标记构造函数为`private`来限制非抽象类被初始化。要记得，被标记为`private`的方法代表只能被同一类的程序所调用。构造函数也是同样的意思，这也是`Math`如何防止被初始化的方法。

> 任何非静态的方法都必须通过某种实例来操作，取得新对象的方法只有`new`或者序列化（`deserialization`）以及我们不会讨论的`Java Reflection API`。

###### 静态的方法不能调用非静态的变量

> 静态方法是通过类的名称来调用，如果静态方法调用非静态的实例变量，编译器会报错，因为它不知道你调用的是哪个实例的变量。

###### 静态的方法也不能调用非静态的方法

> 非静态的方法通常以实例变量的状态来影响方法的行为，同理静态方法也不能调用非静态方法，就算你的非静态方法没有调用实例变量，编译器也无法保证你之后会不会修改或者覆盖这个方法，所以编译器不会通过。

###### 引用变量可以代替类名调用静态方法，但是最好不要那么做

> 就算你这么写了，这个方法执行起来也和引用的对象没有半毛钱关系。

## 静态变量

###### 静态变量的值对所有的实例来说都相同

> 假设你要在执行过程中计算有多少个Duck被创建，你会考虑在构造函数递增某个实例变量的值？

```
class Duck{
    int duckCount = 0;//每个Duck都有自己的duckCount
    public Duck(){
        duckCount++;//每当构造函数执行时递增
    }
}
```

> 这么做可以吗？不行，因为你的每个Duck的duckCount的初始值都是0。也许你可以调用别的类来计算，但是这样写不够优雅。所以我们需要有一个所有实例都通用的变量。

```
public class Duck{
    private int size;//Duck自己的变量
    private static int duckCount = 0;//Duck们通用的变量，只会在类的第一次载入的时候被初始化
    public Duck(){
        duckCount++;//每当构造函数执行时递增
    }
    public void setSize(int s){
        size = s;
    }
    public int getSize(){
        return size;
    }
}
```

###### 静态变量的是共享的，同一类所有的实例分享一份静态变量。

- 实例变量：每个实例一个。
- 静态变量：每个类一个。

###### 静态变量会在该类的任何静态方法执行之前就初始化

> 静态变量会在载入类的时候被初始化，如果没有赋值就按默认值。

###### 静态的final变量是常数

```
//Math.PI
public static final double PI = 3.141592653589793;//常数变量的名称应该都要是大写字母！
//public代表可供各方读取
//static代表你不需要Math的实例
//final代表圆周率是不变的
```

> 此外没有别的方法可以识别变量为不变的常数（constant），但有命名惯例（naming convention）可以帮你认出来

---

> 静态初始化程序（static initializer）是一段在加载类时会执行的程序代码，它会在其他程序可以使用该类之前就执行，所以很适合放静态final变量的起始程序。

```
public class Foo{
    public final static int FOO_X = 25;//注意这个命名惯例——应该都是大写的，并以下划线分隔
}
//声明的时候初始化
public class Bar{
    public static final int BAR_SiGN;
    static{//这段程序会在类被加载时执行
        BAR_SIGN = (double)Math.random();
    }
}
//在静态初始化程序中初始化
```

> 如果你没有以这两种方式之一来赋值的话，编译器会报错。

###### final不止用在静态变量上

1. 非静态final变量（不管是实例变量还是局部变量还是参数）：此变量将不能改变
2. final的method：此方法将不能被覆盖
3. final的class：此类将不能被继承

[关于static、final和static final](https://www.cnblogs.com/EasonJim/p/7841990.html)

## primitive主数据类型的包装

- Boolean
- Character
- Byte
- Short
- Integer
- Long
- Float
- Double

###### 包装值

```
int i = 114;
Interger iWrap = new Integer(i);
```

###### 解开包装

```
int unWrapped = iWrap.intValue();//所有的包装类都有类似的方法
```

> Java5.0(1.5)版本之前不能自动封装拆封，要手动拆装封装。

#### autoboxing

> 从5.0(1.5)开始加入的autoboxing能够自动地将primitive主数据类型转换成包装过的对象！不必把primtive主数据类型与对象分得那么清楚。

###### autoboxing的用处

1. 方法的参数
2. 返回值
3. boolean表达式
4. 数值运算
5. 赋值

> 这些地方autoboxing都会帮你自动的完成primitive主数据类型和类型对象之间的转换。

#### 将String转换成primtive主数据类型

```
String s = "2";
int x = Integer.parseInt(s);//很实用的静态方法
double d = Double.parseDouble("420.24");
boolean b = new Boolean("true").booleanValue();//这个没有上面那种方法，但是可以用构造函数。
```

#### 反过来讲primitive主数据类型转换成string

```
double d = 42.5;
String doubleString = "" + d;//最简单的方法就是将现有的数字接上现有的String。“+”这个操作数是Java中唯一有重载过的运算符
String doubleString2 = Double.toString(d);//Double这个类的静态方法。
```

### 数字格式化

```
String s = String.format("%,d"),1000000000);//"%,d"代表数字要以“,”隔开。1000000000是要格式化的数字
//s的值会变成1,000,000,000
```

> format()的第一个参数被成为“格式化串”，它可以带有实际上就是要这么输出而不用转译的字符。当你看到%符号时，要把它想做是会被方法其余参数替换掉的位置。格式化语句有自己的一套语法，%的语法有非常特殊的规则。

- %,d 代表以十进制整数带有逗号的方式来表示
- %.2f 保留两位小数的浮点数来表示
- %,.2f 整数部分以有逗号的形式来表示，小数部分保留两位

> 格式化说明最多会有5个部分，只有%和type是必要的。下面[]里是可选的

```
%[argument number][flags][width][.precision]type
```

- argument number 如果要格式化的参数超过一个以上，可以在这里指定是哪一个
- flags 特点类型的特定选项，例如数字要加逗号或正负号
- width 最小的字符数，注意：这不是总数；输出可以超过此宽度，若不足则会主动补零
- precision 精确度，注意前面有个圆点符号
- type 要指定的类型标识

> 在格式化指令中一定要给的类型，如果还要指定其他项目的话，要把类型摆在最后，如果需要多个参数时，按照顺序应用到%上。

#### 日期格式化

> Java中用Date类型表示时间,格式化串使用t开头的两个字符来表示

```
String.format("%tc",new Date());//%tc完整的时间
//Sun Nov 28 14:52:41 MST 2004
String.format("%tr",new Date());//%tr只有时间
//03:01:47 PM
Date today = new Date();
String.format("%tA,%tB %td",today,today,today);//%tA周 %tB月 %td日，里面有个逗号是直接输出的
//Sunday，November 28
String.format("%tA,%<tB %<td",today);//<符号告诉程序重复利用之前的参数
//输出结果跟上面一致
```

### Calendar

> 要取得当前的日期时间就用Date，其余功能可以从Calendar上面找。Date就图个方便，实际上没什么屌用了。具体功能很多，不抄书了。

```
Calendar cal = Calendar.getInstance();//Calendar是抽象类，要用这个静态方法获得实例，一般的版本都默认返回java.util.Gregorian-Calendar的实例。
```

- 字段会保存状态
- 日期和时间可以运算
- 日期与时间可以用millisecond来表示

> 可以import静态类，不过会让程序不好阅读，容易产生名称冲突，看起来不太好用的亚子，忽略了。

---

##### 静态初始化块会在类加载的时候就执行，并且要先调用父类的静态初始化块，构造函数只会在new出来的时候执行。

**李晓晗**

**更新于2019-8-22 下午**

