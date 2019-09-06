---
layout: post
title: Calculator
subtitle:   "利用MFC设计一个计算器"
date: 2019-09-06
author: Max.C
header-img: 'assets/img/pro2.jpg'
catalog: true
tags: C++ MFC 
---

# 引言

> 微软基础类库（英语：Microsoft Foundation Classes，简称MFC）是微软公司提供的一个类库（class libraries），以C++类的形式封装了Windows API，并且包含一个应用程序框架，以减少应用程序开发人员的工作量。其中包含大量Windows句柄封装类和很多Windows的内建控件和组件的封装类。（百度百科）

暑假想学习一下Windows API的使用，于是想利用Windows窗口设计一个简单的计算器，虽然之前在图书馆借了一本书但过于硬核，后来在bilibili找到一个MFC的教程才开始上手做这个。

[bilibili MFC教程](https://www.bilibili.com/video/av49780425?from=search&seid=15179969927203435795)

## 一、Calculator V1.0

![](/assets/post_img/2019-09-06/1.png)

当前完成的最初版本的计算器，能够进行整数的四则运算，输入有基本的纠错功能（比如无法连续输入两个加号`++`），但输入错误的括号形式时计算会出错（比如`(6+(4`），小数点暂时没有作用，由于是整数运算，进行除法运算时结果不一定正确。

### 1、开发环境

本次MFC程序设计使用Visual Studio Community 2019进行开发，Community为免费版本，可直接到[官网](https://visualstudio.microsoft.com/zh-hans/vs/)进行下载安装。

![](/assets/post_img/2019-09-06/2.png)

### 2、新建MFC项目

***
