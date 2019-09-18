---
layout: post
title:  "Java拷贝文件夹的实现"
date:   2019-09-17 21:52:00 +0800
categories: Java
tags: Java IO
author: lxh
mathjax: true
---

* content
{:toc}
Java拷贝文件夹的实现



## Java拷贝文件夹的实现

```
package com.lxh.io;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
/**
 * 文件夹拷贝
 * @author lxh
 *
 */
public class TestCopyFolder {

	public static void main(String[] args)throws Exception {
		TestCopyFolder.copyFileFolder("src/com/lxh/io", "e:/测试");
	}
	public static void copyFileFolder(String src/*要拷贝的文件夹*/,String dest/*目标文件夹*/)throws Exception{
		File srcFile = new File(src);//原文件夹
		File destFile = new File(dest);//拷贝的文件夹
		File[] files = srcFile.listFiles();
		//如果是文件夹
		if(srcFile.isDirectory()){  
			//这一步一定要确定是文件夹再操作，如果不是会创建出以文件的名称为名的文件夹
			if(!destFile.exists()){
				destFile.mkdirs();
			}
			for(File fs:files){
				TestCopyFolder.copyFileFolder(fs.getPath()/*原文件的目录*/,dest+"/"+fs.getName()/*根据拷贝的文件夹的名字生成拷贝位置的路径*/);
			}
		}else if(srcFile.isFile()){
			//如果是文件就调用拷贝方法
			TestCopyFolder.copyFile(src,dest);
		}
		
	}
	public static void copyFile(String src,String dest){
		//1.创建源
		File srcFile = new File(src);//被拷贝的文件
		File destFile = new File(dest);//输出的文件
		//2.创建流
		InputStream is = null;
		OutputStream os = null;
		try {
			is = new FileInputStream(srcFile);
			os = new FileOutputStream(destFile);
			//3.操作，分段读取
			byte[] flush = new byte[3];//1k字节数组
			int len = -1;//接收长度
			while((len = is.read(flush))!=-1){
				os.write(flush, 0, len);//按长度写出
			}
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally{
			//4.关闭流，先开的要后关，保证拷贝完毕
			try {
				if(os!=null){
					os.close();				
				}
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			try {
				if(is!=null){
					is.close();
				}
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}
}

```

---

### copyFile方法的改进写法

```
	public static void copyFile(String src,String dest){
		//1.创建源
		File srcFile = new File(src);//被拷贝的文件
		File destFile = new File(dest);//输出的文件
		//2.创建流：JDK1.7之后可以在try()里创建不用手动关闭
		//使用缓冲流装饰字节流可以提高效率
		try (InputStream is = new BufferedInputStream(new FileInputStream(srcFile));
			OutputStream os = new BufferedOutputStream(new FileOutputStream(destFile));){
			//3.操作，分段读取
			byte[] flush = new byte[3];//1k字节数组
			int len = -1;//接收长度
			while((len = is.read(flush))!=-1){
				os.write(flush, 0, len);//按长度写出
			}
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	}
```




**李晓晗**

**更新于2019-9-17 晚上**

