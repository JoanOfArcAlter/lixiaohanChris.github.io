---
layout: post
title:  "Java双重检查锁定的单例模式"
date:   2019-09-21 16:36:00 +0800
categories: Java
tags: Java 设计模式 多线程 并发
author: lxh
mathjax: true
---

* content
{:toc}
Java双重检查锁定的单例模式



## 双重检查锁定的单例模式

```
package com.lxh.thread;
/**
 * DoubleCheckiedLocking双重检查锁定的单例模式
 * @author lxh
 * 单例模式：懒汉式套路上加入并发控制，保证在多线程环境下,对外存在一个对象
 *1.构造器私有化-->避免外部new构造器
 *2.提供私有的静态属性-->存储对象的地址（不是静态外部无法获取，因为不能new）
 *3.提供共有的静态方法-->获取属性
 */
public class TestDCLSingleton {
	//2.提供私有的静态属性
	/*
	 * 这里如果不加上 volatile，有可能因为指令重排出现错误
	 * 创建对象的线程可能还没创建好就把对象地址返回，其它线程会看到一个为空的地址而出现错误
	 */
	private static volatile TestDCLSingleton instance;//先不创建：懒汉式
	//1.构造器私有化
	private TestDCLSingleton(){
		
	}
	//3.提供共有的静态方法
	public static TestDCLSingleton getInstance(){
		/*
		 * DoubleChecking!双重检查
		 * 如果每一个线程都要等前一个线程结束占有才能进去判断对象是否存在的话效率就太低了
		 * 所以在这里提前判断一下，有就不用等了
		 */
		if(instance != null){//避免不必要的同步，已经存在对象
			return instance;
		}
		/*
		 * 这里主要防止在第一个线程还没new好对象时，
		 * 另一个线也看到instance为null然后自己也new了一个对象导致对象不唯一有不同的地址。
		 * 这里锁定的是用于创建对象
		 * 它是创建TestDCLSingleton的模子，被一个线程占有后，其它线程将无法执行这段代码
		 */
		synchronized(TestDCLSingleton.class){
			//再次检查，没有才能创建
			if(instance == null){
				/* 
				 * 1.开辟空间
				 * 2.初始化对象信息
				 * 3.返回对象的地址给引用（这里有可能发生指令重排HappenBefore）
				 */
				instance = new TestDCLSingleton();
			}
		}
		return instance;
	}
	public static void main(String[] args) {
		Thread t = new Thread(()-> {
			System.out.println(TestDCLSingleton.getInstance());
		});
		t.start();
		System.out.println(TestDCLSingleton.getInstance());
	}
}


```

> 当一个变量被 volatile 修饰时，任何线程对它的写操作都会立即刷新到主内存中，并且会强制让缓存了该变量的线程中的数据清空，必须从主内存重新读取最新数据。

> synchronized表示当前线程，独占对象someObject当前线程独占了对象someObject，如果有其他线程试图占有对象someObject，就会等待，直到当前线程释放对someObject的占用。占有这个对象才能执行同步块里的方法。

> 最后就是双重检查，提高效率









**李晓晗**

**更新于2019-9-21 下午**

