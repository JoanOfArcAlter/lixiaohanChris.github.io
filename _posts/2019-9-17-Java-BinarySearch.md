---
layout: post
title:  "二分法查找算法的Java实现"
date:   2019-09-17 12:03:00 +0800
categories: Java
tags: Java 算法
author: lxh
mathjax: true
---

* content
{:toc}
二分法查找算法的Java实现



## 二分法查找算法的Java实现

```
/**
 * 二分法查找
 * @author lxh
 *
 */
package com.lxh.algorithm;

import java.util.Arrays;

public class TestBinarySearch {

	public static void main(String[] args) {
		int arr[] = {114,514,1919,810,542,7,41,13,47,666};//准备数组
		Arrays.sort(arr);//二分法查找必须先排序
		System.out.println("排好序的数组："+Arrays.toString(arr));
		int search = 14;//要查询的数字
		TestBinarySearch tbs = new TestBinarySearch();
		tbs.BinarySearch(arr, search);

	}
	public int BinarySearch(int[] a,int b){
		int low = 0;//最小值的索引
		int high = a.length-1;//最大值的索引
		while(low <= high){
			int middle = (low + high)/2;
			if(b == a[middle]){
				System.out.println("找到了，在排好序的数组索引值为"+middle+"的位置");
				return middle;//如果中间值正好等于要查询的值，则直接返回索引
			}
			if(b < a[middle]){
				high = middle - 1;//如果比中间值小，中间值及以上的全部舍去，再来一遍
			}
			if(b > a[middle]){
				low = middle + 1;//如果比中间值大，中间值及以下的全部舍去，再来一遍
			}
		}
		System.out.println("没找到");
		return -1;//还是没有返回就返回-1，代表没有找到
	}

}

```





**李晓晗**

**更新于2019-9-17 中午**

