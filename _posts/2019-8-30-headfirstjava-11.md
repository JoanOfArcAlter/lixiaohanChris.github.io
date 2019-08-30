---
layout: post
title:  "headfirstjava第十一章笔记"
date:   2019-08-30 17:53:00 +0800
categories: HeadFirstJava
tags: Java 
author: lxh
mathjax: true
---

* content
{:toc}
head first Java第十一章



## 异常处理 

> Java的异常处理是在你已经知道调用的方法有风险的情况下，编写出处理此可能性的方法。如何得知某个方法会抛出异常，看到该方法的声明有throws语句就知道了。编译器要确定你了解所调用的方法是有风险的，它会关注你有没有注意到表示你注意到了异常，你注意到了，它就放心了。

###### 异常是一种Exception类型的对象

###### 在编写可能会抛出异常的方法时，它们都必须声明有异常

> 方法可以抓住其他方法所抛出的异常。异常总是会丢回给调用方。会抛出异常的方法要声明它有可能会这么做。

> 有风险、会抛出异常的程序代码：

```
public void takeRisk() throws BadException{//必须要声明它会抛出BadException
    if (abandonAllHope){
        throw new BadException();//创建Exception对象并抛出
    }
}
```

> 调用该方法的程序代码

```
public void crossFingers(){
    try{
        anObject.takeRisk();
    }catch(BadException ex){
        System.out.println("Aaargh!");
        ex.printStackTrace();//如果无法从异常中恢复，至少也要使用printStackTrace()来列出有用的信息。
    }
}
```

###### 编译器会核对每件事，除了RuntimeExceptions之外。编译器保证：

1. 如果你有抛出异常，则你一定要使用throw来声明这件事
2. 如果你调用会抛出异常的方法，你必须得确认你知道异常的可能性。将调用包在try/catch块中是一种满足编译器的方法。

> RuntimeException大部分是因为程序逻辑的问题，测试的时候就该把它改正掉，并非无法规避解决的问题，没必要用try/catch块来抓一开始就不该出现的问题（但是你想抓也可以抓）。try/catch是用来处理真正的异常，是你无法预测的执行期失败状况，该块做的是恢复与尝试，至少会优雅地列出错误信息。

###### try/catch的控制流程

1. 如果try块失败了，抛出异常，流程会马上转移到catch块。当catch块完成时，会执行finally块。当finally完成时，就会继续执行其余的部分。
2. 如果try块成功，流程会跳过catch块并移动到finally块，当finally完成时，就会继续执行其余的部分。
3. 如果try或catch块有return指令，finally还是会执行！流程会跳到finally然后再回到return指令。

> 可能有异常的方法抛出异常后，方法剩下的代码不会执行。

---

#### 处理多重异常

> 可能有多种异常的方法就一起throws在后面就行，用“，”隔开；捕捉异常时可以多写几个catch块来处理不同的异常。如果有相同的父类，也可以只声明父类就行，异常也是对象，可以利用多态来处理。

1. 以异常的父型来声明会抛出的异常，代表你可以抛出任何该类型的异常或其子类的异常。
2. 以所抛出的异常父型来catch异常，代表你可以catch任何该类型或者其类型的异常

> 但是可以用父类不代表就应该这么做，catch的父型太大比如直接catch Exception类型的异常，你会捕获到所有类型的异常，因此你会搞不清楚哪里出错，所以你应该为每个需要单独处理的异常编写不同的catch块

> 有多个catch块时要从小排到大，catch不会像重载的方法会被调出最符合的项目，Java虚拟机只会从头开始往下找到第一个符合范围的异常处理块，就算你写了某个异常的特定处理catch块，如果Java虚拟机找到了在这个catch块之前它的父类的catch块，就会直接执行父类的catch块来进行处理这个异常，然后忽略掉后面的特定catch处理块。所以要把子类的异常catch放父类前面。如果不是父子爷孙之类的关系就无所谓了，你的异常不会它的叔叔异常catch走。

#### 当你不想处理异常时...

> 当你调用一个有风险的方法时，你又不想处理，你可选择把异常duck掉，就是你的方法也抛出这个异常，让调用你的方法的方法来处理，同样调用你的方法的方法也可以抛出这个异常，让你调用调用你的方法的方法的方法来处理……到最后甚至回到main方法来处理，main方法也可以duck，抛出这个异常，这时Java虚拟机只好死给你看。duck在这里是逃避的意思，不是鸭子。

###### 调用有异常的方法你要么自己catch掉，要么抛给你的调用方。

#### 异常处理规则

1. catch与finally不能没有try
2. try与catch之间不能有程序
3. try一定要有catch或finally
4. 只带有finally的try必须要声明异常

**李晓晗**

**更新于2019-8-30 下午**

