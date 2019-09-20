---
layout: post
title:  "Java从内部类到Lambda"
date:   2019-09-20 11:55:00 +0800
categories: Java
tags: Java 内部类 Lambda
author: lxh
mathjax: true
---

* content
{:toc}
Java从内部类到Lambda



## 从内部类到Lambda

```
/**
 * 类到lambda的转化
 * @author lxh
 *
 */
//接口
interface ILove{
	String love(String name,int num);
}
/*
 * 1.外部类
 */
class Love1 implements ILove{

	@Override
	public String love(String name,int year) {
		return"我单推"+name+year+"年了";
	}	
}
public class TestLambda {
	/*
	 * 2.内部类
	 */
	class Love2 implements ILove{

		@Override
		public String love(String name,int year) {
			return"我单推"+name+year+"年了";
		}
	}
	/*
	 * 3.静态内部类
	 */
	static class Love3 implements ILove{

		@Override
		public String love(String name,int year) {
			return"我单推"+name+year+"年了";
		}
	}
	public static void main(String[] args) {
		//1.调用外部类方法
		System.out.println(new Love1().love("夏哥",17));
		//2.调用内部类方法
		System.out.println(new TestLambda().new Love2().love("FBK",18)); //必须先创建外部对象，静态main方法无权访问非静态成员
		//3.调用静态内部类方法
		System.out.println(new Love3().love("阿夸",5));
		/*
		 * 4.局部内部类
		 */
		class Love4 implements ILove{

			@Override
			public String love(String name,int year) {
				return"我单推"+name+year+"年了";
			}
		}
		//4.调用局部内部类方法
		System.out.println(new Love4().love("MEA",39));
		/*
		 * 5.匿名内部类
		 */
		//5.调用匿名内部类方法
		System.out.println(
			new ILove(){
				@Override
				public String love(String name,int year) {
					return"我单推"+name+year+"年了";
				}
			}.love("樱火龙",19)
		);
		/*
		 * 6.Lambda，使用lambda实现的接口只能有一个方法！！！
		 */
		ILove love6 =(String name,int year)->{
			return"我单推"+name+year+"年了";
		};
		//6.调用Lambda方法
		System.out.println(love6.love("狗妈",20));
		/*
		 * 参数的类型可以省略，它可以自动匹配
		 * 如果只有一个参数，参数的括号可以省略
		 * 如果方法只有一句，花括号可以省略
		 * 写成一行的时候return也要省略
		 */
		//简化版Lambda
		ILove love7 = (name,year) -> "我单推"+name+year+"年了";
		//调用简化版Lambda
		System.out.println(love7.love("犬山哥",21));
		;
	}
}



```









**李晓晗**

**更新于2019-9-20 中午**

