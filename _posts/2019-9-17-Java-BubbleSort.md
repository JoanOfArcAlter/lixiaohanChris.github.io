---
layout: post
title:  "冒泡排序算法的Java实现"
date:   2019-09-17 11:53:00 +0800
categories: Java
tags: Java 算法
author: lxh
mathjax: true
---

* content
{:toc}
冒泡排序算法的Java实现



## 冒泡排序算法的Java实现

```
/**
 * 冒泡排序
 * @author lxh
 *
 */
package com.lxh.algorithm;

import java.util.Arrays;

public class TestBubbleSort {
	public static void main(String[] arg){
		int[] arr = {1,15,19,100,170,114,514,7,542};
		TestBubbleSort tbs = new TestBubbleSort();
		tbs.BubbleSort(arr);
		System.out.println("排序结果："+Arrays.toString(arr));
		
	}
	public int[] BubbleSort(int[] a){
		int temp = 0;
		for(int i = 0;i < a.length - 1;i++){
			boolean stop = true;//用于判断还用不用继续
			for(int j = 0;j < a.length - 1 - i;j++){
				if(a[j] > a[j+1]){
					temp = a[j];
					a[j] = a[j+1];
					a[j+1] = temp;
					stop = false;//这里指只要还有排序的必要，就不要停下来啊
				}
				System.out.println(Arrays.toString(a));
			}
			System.out.println("###################");
			if(stop){
				break;//stop为true代表这次冒泡顺序完全没有变化，所有可以停下来了
			}
		}
		return a;
	}
}
```




**李晓晗**

**更新于2019-9-17 中午**

