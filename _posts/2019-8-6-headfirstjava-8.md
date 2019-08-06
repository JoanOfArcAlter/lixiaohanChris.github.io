---
layout: post
title:  "headfirstjava第八章笔记"
date:   2019-08-06 10:24:00 +0800
categories: HeadFirstJava
tags: Java 
author: lxh
mathjax: true
---

* content
{:toc}
head first Java第八章



## 抽象类

#### 作用

```
Wolf aWolf = new Wolf();//没毛病
Animal aHippo = new Hippo();//也没毛病
Animal anim = new Animal();//Animal对象是什么东西？？Animal这个动物有几条腿？什么亚子？
```

> 我们一定要有`Animal`这个类来继承和产生多态，但是要限制只有它的子类才能够被初始化。我们要的是`Wolf`、`Hippo`对象，而不是`Animal`对象 

##### 有些类不该被实例化！

> 通过标记类为抽象类，编译器就知道不管在哪里，这个类就是不能被new出来。你还是可以用这种抽象的类型作为引用类型。

###### 抽象类就是在类的声明前加上`abstract`

```
abstract public class Canine extends Animal{//犬科继承动物，声明为抽象类
    public void roam(){}//覆盖
}
public class MakeCanine{
    public void go(){
        Canine c;
        c = new Dog();//没毛病，你可以赋值子类对象给父类的引用，即使父类是抽象的
        c = new Canine();//有毛病！编译器会报错，这个类已经标记了abstract，不能被实例化
        c.roam()
    }
}
```

###### 抽象的类代表此类必须要被`extend`过，抽象的方法代表此方法一定要被覆盖过

##### 抽象的方法没有实体！

```
public abstract void eat();//没有方法体！直接以分号结束
```

###### 如果你声明出一个抽象的方法，就必须将类也标记为抽象的。你不能在非抽象类中拥有抽象方法

##### 你必须实现所有抽象的方法

###### 实现抽象的方法就如同覆盖过方法一样

> 抽象的方法没有内容，它只是为了标记出多态的存在。这表示在继承树结构下的第一个具体类必须要实现出所有的抽象方法。然而你还是可以通过抽象机制将实现的负担转给下层，抽象类的子抽象类可以实现父类的抽象方法，但不用必须实现父类的抽象方法，但是具体类一定要实现所有抽象方法。实现抽象的方法的意思是以相同的方法鉴名（名称与参数）和相同的返回类型创建出非抽象的方法。Java很注重你的具体子类有没有实现这些方法。

---
## Object
#### 多态的使用

##### 在Java中的所有类都是从`Object`这个类继承出来的。

###### 可以利用这一点创建出处理自定义类型的类。

> 许多`ArrayList`的方法都用到了`Object`这个终极类型。因为每个类都是对象的子类，所以`ArrayList`可以处理任何类！

###### 关于`Objcet`的几个常用方法

- `equals(Object o)` 判断这两个对象是否“相等”
- `getClass` 告诉你此对象是从哪里被初始化
- `hashCode()` 列出此对象的哈希代码，你可以把它想成是一个唯一的ID
- `toString()` 列出类的名称和一个我们不关心的数字

###### `Object`这个类是抽象的吗？

> 不是，至少不是正式的Java抽象类。因为它可以被所有类继承下来的方法都实现了程序代码，所以没有必须被覆盖过得方法。

###### 那是否可以覆盖过`Object`的方法？

> 部分可以。但是有些被标记为`final`，这代表你不能覆盖掉它们。强烈建议你用自己写的类去覆盖掉`hashCode()`、`equals()`以及`toString()`。

###### `Object`类是具体的。怎么会允许有人去创建`Objcet`的对象呢？这不就跟`Animal`对象一样不合理吗？

> 好问题！为何要允许创建出`Objcet`的实例呢？因为有时候你就是会需要一个通用的对象，一个轻量化的对象。它最常见的用途是用在线程的同步化上面。

###### `Object`类有两个主要的目的：作为多态的机制，以及提供Java在执行期对任何对象都有需要的方法的实现程序代码（让所有的类都会继承到）。另外还有一部分方法是与线程相关。

###### 既然多态类型那么有用，为什么不把所有的参数和返回类型都设定成`Object`类型呢？

##### 因为使用`Objcet`类型的多态引用是会付出代价的……

> 当你把对象装进`ArrayList<Dog>`时，它会被当做`Dog`来输入与输出:

```
ArrayList<Dog> myDogArrayList = new ArrayList<Dog>();//保存Dog的ArrayList
Dog aDog = new Dog();//新建一个Dog
myDogArrayList.add(aDog);//装到ArrayList中
Dog d = myDogArrayList.get(0);//将Dog赋值给新的Dog引用变量
```

> 但若你把它声明成`ArrayList<Objcet>`时会怎样？如果你打算创建出一个可以保存任何一种对象的`ArrayList`时，你会如此的声明：

```
ArrayList<Objcet> myDogArrayList = new ArrayList<Object>();//保存Objcet的ArrayList
Dog aDog = new Dog();//新建一个Dog
myDogArrayList.add(aDog);//装到ArrayList中
```

> 如果是这样，当你尝试要把`Dog`对象取出并赋值给`Dog`的引用时会发生什么事？

```
Dog d = myDogArrayList.get(0);//无法通过编译！！对ArrayList<Object>调用get()方法会返回Object类型，编译器无法确认它是Dog！
```

> 从`ArrayList<Object>`取出的Object都会被当做是`Object`这个类的实例。编译器无法将此对象识别为`Object`以外的事物

> 问题在于把所有东西都以多态来当作是`Object`会让对象看起来失去了真正的本质（但不是永久性的）。

```
public void go(){
    Dog aDog = new Dog();
    Dog sameDog = getObjcet(aDog);//无法编译通过！！虽然这个方法会返回一个Dog，但编译器认为这只能赋值给Objcet类型的变量
    //编译器无法得知方法返回的其实是Dog，因此不会同意这项赋值
    //Objcet sameDog = getObjcet(aDog);这样会过关，因为你可以赋值任何东西给Objcet类型的引用，并且每个东西都能对Object通过IS-A测试
}
public Objcet getObjcet(Object o){
    return o;//返回同一各引用，但是类型已经转换成Objcet了
}
```

> 当一个对象被声明为`Objcet`类型的对象所引用时，它无法再赋值给原来的类型的变量。

```
ArrayList<Objcet> myDogArrayList = new ArrayList<Object>();//保存Objcet的ArrayList
Dog aDog = new Dog();//新建一个Dog
myDogArrayList.add(aDog);//装到ArrayList中
Object o = myDogArraylist.get(0);//取得对Dog实例的Object引用
int i = o.hashCode();//调用Object的方法：没问题！Objcet本来就有这个方法
o.bark();//调用Dog的方法：无法通过编译！Object不知道Dog类的方法！就算所有人都知道这其实是个Dog实例！
```

> 编译器是根据引用类型来判断有哪些`method`可以用，而不是根据这个Object真正的类型。就算你知道对象有这个功能，编译器还是会把它当做一般的Object来看待。。编译器只管引用类型，而不是对象类型。

###### 所有对象都会带着Object的内容，这意味着所有对象都可以当做Object来使用，只是使用Objcet类型引用就只能使用Objcet有的方法，而无法使用这个类自己特有的一些方法。（Object引用就像按钮比较少的遥控器一样，只能控制Object的功能）

###### 怎么才能把这个Object恢复成Dog呢？

> 就算成为Objcet的引用，但它本还是个Dog，但如果你想调用Dog特有的方法，就必须要将类型声明为Dog。如果你真的确定它是个Dog，那么你就可以从Objcet中拷贝一个Dog引用，并赋值给Dog引用变量。

```
Object o = myDogArraylist.get(0);
Dog d = (Dog)o;//将类型转换成Dog，如果转换错了，报ClassCastException
d.bark();
```

> 如果不能确定它是Dog，你可以使用instanceof这个运算符来检查。

```
if(o instanceof Dog){
    Dog d = (Dog)o;
}
```

##### Java十分注重引用变量的类型。

###### 你只能在引用变量的类确实有该方法才能调用它

###### 把类的公有方法当作是合约的内容，合约是你对其他程序的承诺协议

###### 这些合约保证你使用的引用类型在这个类的实例上面一定能找到这些方法

---

## 接口

###### 作用

> 例如：猫和老虎继承猫科动物，狗和狼继承犬科动物，猫科动物和犬科动物又继承动物，而猫和狗又能作为宠物，宠物类又该让谁来继承？机器狗连动物都不是，但是不能当宠物吗？

###### Java不支持多重继承这种方式，因为多重继承会有称为“致命方块”的问题

![致命方块](https://github.com/lixiaohanChris/test/blob/master/QQ%E5%9B%BE%E7%89%8720190806103155.jpg)

###### 这个eat()到底要运行哪个版本？？

###### 使用接口来解决这个问题！

> 把全部的方法设为抽象的！如此一来，子类就得要实现此方法，因此Java虚拟机在执行期间就不会搞不清楚要用哪一个版本。

##### Java的接口就好像是100%的纯抽象类

```
public interface Pet{//使用interface取得class
    public abstract void beFriendly();//接口的方法一定是抽象的，所以必须以分号结束。记住它们没有内容！
    public abstract void play();//接口方法带有public和abstract的意义，这两个修饰符是属于选择性的（我们是为了强调才把它们打出来的，实际上不需要）
}
//接口的定义
public class Dog extends Canine implements Pet{//使用implements关键词。注意到实现interface时还是必须在某个类的继承之下
    public void beFriendly(){...}//必须在这里实现出Pet的方法，这是合约的规定
    public void play(){...}//必须在这里实现出Pet的方法，这是合约的规定
    public void roam(){...}//一般的覆盖方法
    public void eat(){...}//一般的覆盖方法
}
//接口的实现
```

###### 这个接口有什么屌用？明明里面啥都没有，每个方法还得子类实现？

> 多态！多态！多态！接口有无比的适用性，若你以接口取代具体的子类或抽象的父类作为参数或返回类型，则你就可以传入任何有实现该接口的东西。很多类继承着不同的父类却又实现了同一个接口，这样就可以为不同的需求组合出不同的层次结构。使用接口代表着“不管你来自哪里，只要你实现这个接口，别人就会知道你一定会履行这个合约”。

> 允许各个继承树下的类来实现共同的接口对于`Java API`来说是非常重要的。如果你想要将对象的状态保存在文件中，只要去实现`Serializable`这个接口就行，打算让对象的方法以单独的线程来执行，就实现`Runnable`就行。

##### 更棒是类可以实现多个接口！！！

---
###### 要如何判断应该是设计类、子类、抽象类或接口呢？

- 如果新的类无法对其它的类通过IS-A测试时，就设计不继承其它的类
- 只有在需要某类的特殊化版本时，以覆盖或增加新的方法来继承现有的类
- 当你需要定义一群子类的模板，又不想让程序员初始化此模板时，设计出抽象的类给它们用
- 如果想要定义出类可以扮演的角色，使用接口

###### 如何在子类设计出保留父类原有功能的方法的同时再加上额外的功能的方法？（不完全覆盖父类的方法）

```
abstract class Report{
    void runReport(){//父类的方法，带有子类可以运用的部分
        //设置报告
    }
    void printReport(){
        //输出
    }
}
class BuzzwordsReport extends Report{
    void runReport(){//子类覆盖的方法
        super.runReport();//使用super关键字调用父类原本的方法
        buzzwordComliance();//自己的方法
        printReport();//继承下来的方法
    }
    void buzzwordComliance(){...}
}
```

**李晓晗**

**更新于2019-8-6 上午**

