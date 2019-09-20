---
layout: post
title:  "Java静态代理模式的简单实现"
date:   2019-09-19 18:08:00 +0800
categories: Java
tags: Java 设计模式
author: lxh
mathjax: true
---

* content
{:toc}
Java静态代理模式的简单实现



## 静态代理模式的简单实现

```
package com.lxh.thread;
/**
 * 静态代理的简单实现
 * @author lxh
 *
 */
public class TestStaticProxy {
	public static void main(String[] args) {
		Person person = new Person("李晓晗","夏色祭");
		//让婚庆公司来代理我的婚礼
		new WeddingCompany(person).marry();
	}
}
interface Marry{
	void marry();
}
//要结婚的人
class Person implements Marry{
	private String name;//名字
	private String lover;///对象
	@Override
	public void marry() {
		// TODO Auto-generated method stub
		System.out.println(this.getName()+"和"+this.getLover()+"结婚了");//结婚
	}
	public String getName() {
		return name;
	}
	public String getLover() {
		return lover;
	}
	public Person(String name, String lover) {
		super();
		this.name = name;
		this.lover = lover;
	}
}
//婚庆公司
class WeddingCompany implements Marry{
	private Person customer;
	@Override
	public void marry() {
		// TODO Auto-generated method stub
		start();//婚庆的工作
		this.getCustomer().marry();//结婚
		end();//婚庆的工作
	}
	private void end() {
		// TODO Auto-generated method stub
		System.out.println("婚礼结束");
	}
	private void start() {
		// TODO Auto-generated method stub
		System.out.println("婚礼开始");
	}
	public Person getCustomer() {
		return customer;
	}
	public void setCustomer(Person customer) {
		this.customer = customer;
	}
	public WeddingCompany(Person customer) {
		super();
		this.customer = customer;
	}
	
}

```








**李晓晗**

**更新于2019-9-19 下午**

