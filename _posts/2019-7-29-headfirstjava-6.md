---
layout: post
title:  "headfirstjava第六章笔记"
date:   2019-07-29 17:31:00 +0800
categories: HeadFirstJava
tags: Java 
author: lxh
mathjax: true
---

* content
{:toc}
head first Java第六章



## 比较`ArrayList`与一般数组
1. 一般数组在创建时就必须确定大小

```
new String[2]//指定大小
new ArrayList<String>()
```
2. 存放对象给一般数组时必须指定位置

```
myList[1] = b;//指定索引值（必须要指定介于0到比length小1之间的数字）
myList.add(b);//不需指定索引值
```

3. 一般数组使用特殊的语法

```
myList[1]//[方括号]只是在数组上的特殊语法
```

4. 在Java5.0中的`ArrayList`是参数化的（`parameterized`）

```
ArrayList<String>//这代表String的集合
```
###### 虽然`ArrayList`只能携带对象而不是`primitive`主数据类型，但编译器能够自动地将`primitive`主数据类型包装成`Object`以存放在`ArrayList`。

###### 除了`java.lang`之外，使用到其他包的类都需要指定全名。也可以在原始程序代码的最开始部分下`import`指令来说明所使用到的包。
> 要记得`java.lang`是个预先被引用的包。因为`java.lang`是个经常会被用到的基础包，所以你可以不必指定名称。`java.lang.String`与`java.lang.System`是独一无二的`class`，Java会知道去哪里找。

###### 运用`import`只是帮你省下每个类前面的包名称而已。程序不会因为用了`import`而变大或变慢。
## 如何查询API
1. 查阅参考书
2. 查阅[HTML API文档](https://docs.oracle.com/javase/1.5.0/docs/api/)
（[中文API](http://tool.oschina.net/apidocs/apidoc?api=jdk-zh)）

**李晓晗**

**更新于2019-7-29 下午**
