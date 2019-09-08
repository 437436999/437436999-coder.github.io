---
layout: post
title: Calculator
subtitle:   "利用MFC设计一个计算器"
date: 2019-09-06
author: Max.C
header-img: 'assets/img/pro3.jpg'
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

首先我们需要创建一个MFC项目，在VS2019主界面选择`创建新项目`->`平台：Windows`->`MFC应用`->`下一步`。

![](/assets/post_img/2019-09-06/3.png)

在应用程序类型选项，我们需要选择`应用程序类型-应用程序类型：基于对话框`、`用户界面功能-主框架样式：最小化框`，其他选项默认即可，点击完成进行创建。

![](/assets/post_img/2019-09-06/4.png)

创建完成后，在主界面打开资源视图（Ctrl+Shift+E）,找到`工程名.rc\ Dialog\ IDD_工程名_DIGLOG`，双击打开。

![](/assets/post_img/2019-09-06/5.png)

接下来，我们就可以对创建的MFC窗口进行编辑操作了。

### 3、MFC组件的编辑

在我们打开的窗口里，我们可以调节对话框大小，鼠标选择窗口中的组件后用`Delete`键删除不必要的组件，通过`工具箱`为对话框添加组件（工具箱可在视图菜单打开），接下来介绍我们需要用到的几个基本组件的操作。

#### （1）按钮

双击`工具箱-Button`可在窗口中创建一个按钮，单击选择创建出来的按钮，在菜单的`属性`中可以看到这个按钮的各项属性，选择各个属性，在属性栏可看到属性的相关介绍，我们需要修改的属性有：
- Caption：该按钮显示的文本。
- ID：该按钮的ID，可以理解为该按钮的变量名，在后续编程操作中需要使用。

![](/assets/post_img/2019-09-06/6.png)
![](/assets/post_img/2019-09-06/7.png)
![](/assets/post_img/2019-09-06/8.png)

双击按钮，会自动跳转到该按钮对应的代码区，我已经将按钮ID改为`B1`，则按钮对应的代码如图所示，`OnBnClickedB1()`函数对应按下该按钮时产生的操作。

![](/assets/post_img/2019-09-06/9.png)

#### （2）文本框

双击`工具箱-Static Text`可在窗口中创建一个常量文本框，单击选择常量文本框后可以输入字符、调整大小位置。

![](/assets/post_img/2019-09-06/10.png)

双击`工具箱-Edit Control`可在窗口中创建一个文本框，同样单击选择文本框后可以调整大小位置。打开`属性`菜单，我们同样需要记住这个文本框的ID；双击文本框，也会跳转到该文本框对应的代码区。

![](/assets/post_img/2019-09-06/11.png)
![](/assets/post_img/2019-09-06/12.png)

#### （3）菜单

为设计一个计算器，我们先把所需的组件创建出来并排列好位置，接下来就可以通过编辑代码慢慢实现计算器的功能。

![](/assets/post_img/2019-09-06/1.png)

### 3、计算器的代码实现



***
