---
layout: post
title:  "Java拷贝文件夹的实现"
date:   2019-09-18 13:06:00 +0800
categories: Java
tags: Java 设计模式
author: lxh
mathjax: true
---

* content
{:toc}
Java装饰器设计模式实现



## Java装饰器设计模式实现

```
package com.lxh.io;
/**
 * 模拟咖啡，装饰器的设计模式实现
 * @author lxh
 *1.抽象组件：需要装饰的抽象对象（接口或抽象类）
 *2.具体组件：需要装饰的对象
 *3.抽象装饰类：包含了对抽象组件的引用及装饰着共有的方法
 *4.具体装饰类：装饰具体组件用的对象
 */
public class TestDecorator {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Coffee coffee = new Coffee();//普通咖啡
		Sugar sugarCoffee = new Sugar(coffee);//砂糖咖啡
		Milk milkSugarCoffee = new Milk(sugarCoffee);//牛奶砂糖咖啡
		System.out.println(coffee.info()+"价格是"+coffee.cost());
		System.out.println(sugarCoffee.info()+"价格是"+sugarCoffee.cost());
		System.out.println(milkSugarCoffee.info()+"价格是"+milkSugarCoffee.cost());
	}

}
//抽象组件：喝的
interface Drink{
	double cost();//费用
	String info();//说明
}
//具体组件：咖啡
class Coffee implements Drink{
	private String name = "咖啡";
	private double cost = 10;
	@Override
	public double cost() {
		// TODO Auto-generated method stub
		return cost;//咖啡的价格
	}

	@Override
	public String info() {
		// TODO Auto-generated method stub
		return name;
	}
	
}
//抽象装饰器:佐料
abstract class Decorator implements Drink{
	private Drink drink;//要装饰的对象
	//构造时传入要装饰的对象
	public Decorator(Drink drink) {
		super();
		this.drink = drink;
	}
	@Override
	public double cost() {
		// TODO Auto-generated method stub
		return drink.cost();
	}
	@Override
	public String info() {
		// TODO Auto-generated method stub
		return drink.info();
	}
}
//具体装饰器：砂糖
class Sugar extends Decorator{
	private String name = "砂糖";
	private double cost = 2;
	public Sugar(Drink drink) {
		super(drink);
		// TODO Auto-generated constructor stub
	}
	//加了砂糖的价格
	@Override
	public double cost() {
		// TODO Auto-generated method stub
		return super.cost()+cost;
	}
	//加了砂糖的价格
	@Override
	public String info() {
		// TODO Auto-generated method stub
		return super.info()+"加"+name;
	}
}
//具体装饰器：牛奶
class Milk extends Decorator{
	private String name = "牛奶";
	private double cost = 1;
	public Milk(Drink drink) {
		super(drink);
		// TODO Auto-generated constructor stub
	}
	//加了牛奶的价格
	@Override
	public double cost() {
		// TODO Auto-generated method stub
		return super.cost()+cost;
	}
	//加了牛奶的说明
	@Override
	public String info() {
		// TODO Auto-generated method stub
		return super.info()+"加"+name;
	}
}


```






**李晓晗**

**更新于2019-9-18 中午**

