---
layout: post
title:  "Java下载百度首页源代码"
date:   2019-09-18 18:03:00 +0800
categories: Java
tags: Java IO
author: lxh
mathjax: true
---

* content
{:toc}
Java下载百度首页源代码



## 下载百度首页源代码

```
package com.lxh.io;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.MalformedURLException;
import java.net.URL;
/**
 * 下载百度源代码
 * @author lxh
 *
 */
public class TestDownloadBaiduCode {
	public static void main(String[] args) {
		try (BufferedReader reader = 
				new BufferedReader(	//提高字符流效率的装饰流
						new InputStreamReader(	//将字节流转化为字符流
								new URL("https://www.baidu.com/").openStream(),"UTF-8"));//百度首页源代码，指定字符集
			BufferedWriter writer = 
				new BufferedWriter(	//提高字符流效率的装饰流
						new OutputStreamWriter(	//将字节流转化为字符流
								new FileOutputStream("baidu.html"),"UTF-8"))){	//输出到本地的html文件中，指定字符集
			String str;
			while((str = reader.readLine())!=null){//读取
				writer.write(str);//写出
				writer.newLine();//换行
				writer.flush();//强制刷新
			}
		} catch (MalformedURLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}

```







**李晓晗**

**更新于2019-9-18 下午**

