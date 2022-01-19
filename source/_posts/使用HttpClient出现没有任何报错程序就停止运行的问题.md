---
title: 使用HttpClient出现没有任何报错程序就停止运行的问题
categories:
  - - 编程
date: 2022-01-19 09:08:59
tags:
---


## 背景

昨天在写一个爬虫应用，编写过程中发现一个非常奇怪的问题。程序老是中途停止运行，并且没有任何报错。

```C#
Main()
{
  …………………………
  A();
}

async void A(){
  // 程序运行到这一步就停止运行
  await AsyncTask1()
  // 运行不到这里
  await AsyncTask2()
}
```

## 原理

因为程序最后一个函数是异步函数A，并且没有await或Wait()，因此程序在运行到A中第一个await后就会停止运行