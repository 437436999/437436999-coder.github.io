---
layout: post
title: Calculator
subtitle:   "利用MFC设计一个计算器"
date: 2019-09-08
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

在编辑的过程中，可用Ctrl+F5进行调试。

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

由于第一个版本的计算器只创建了一个菜单，没有在菜单上实现什么功能，所以暂时先介绍菜单的创建与编辑。

![](/assets/post_img/2019-09-06/13.png)

打开资源视图（Ctrl+Shift+E）,在空白处右键->添加资源->Menu->新建资源。

![](/assets/post_img/2019-09-06/14.png)

创建之后，找到`工程名.rc\ Menu\ IDR_MENU1`，双击打开，即可进行菜单的编辑，编辑菜单名称的操作这里不多赘述。

菜单编辑完成后，按Ctrl+F5进行调试时会发现调试的主窗口并没有菜单。此时，我们需要回到我们的`工程名.rc\ Dialog\ IDD_工程名_DIGLOG`界面（即编辑主窗口的界面），选择属性->杂项->MENU，在下拉菜单中选择我们新建的菜单的ID，再次进行调试，会发现菜单成功显示出来了。

![](/assets/post_img/2019-09-06/15.png)

我们还可以给我们的菜单设置快捷键，例如“帮助(V)”：选择我们需要添加快捷键的菜单栏，将属性中的`Caption`改为“帮助(&V)”即可。（即括号内&+快捷键）

![](/assets/post_img/2019-09-06/17.png)

![](/assets/post_img/2019-09-06/16.png)

若要为菜单添加点击事件，右键选择需要添加事件的菜单栏，选择`添加事件处理程序`，注意在弹出的对话框选择**消息类型：COMMAND、类列表：C工程名Dlg**，自行修改函数名，就可以在弹出的代码窗口里编辑事件操作了。

![](/assets/post_img/2019-09-06/18.png)

![代码界面](/assets/post_img/2019-09-06/19.png)

为设计一个计算器，我们先把所需的组件创建出来并排列好位置，接下来就可以通过编辑代码慢慢实现计算器的功能。

![](/assets/post_img/2019-09-06/1.png)

### 3、计算器的代码实现

在MFC代码中，数值类型与C++相同，但输入输出的字符类型为`TCHAR`，字符串类型为`CString`，可以利用宏定义`_T("字符串常量")`将C字符串转换为`CString`，利用宏定义`_T('字符常量')`将C字符转换为`TCHAR`。

```cpp
CString cs=_T("this is cstring"); //例子
```

`CString`类有如下成员函数可以供我们使用：

>1） CString类的构造函数
CString(const CString& stringSrc);
将一个已经存在的CString对象stringSrc的内容拷贝到该CString对象。
CString(TCHAR ch,int nLength = 1）;
使用此函数构造的CString对象中将含有nLength个重复的ch字符。
例如：
```cpp
CString str1(_T(jizhuomi)); // 将常量字符串拷贝到str1

CString str2(str1）； // 将str1的内容拷贝到str2

CString(TCHAR ch,int nLength = 1）;
CString str(_T('w'),3）； // str为"www"

```

>2）CString类的大小写转换及顺序转换函数
CString& MakeLower(); 将字符串中的所有大写字符转换为小写字符。
CString& MakeUpper(); 将字符串中的所有小写字符转换为大写字符。
CString& MakeReverse(); 将字符串中所有字符的顺序颠倒。
例如：

```cpp
CString str(_T("JiZhuoMi"));
str.MakeLower(); // str为"jizhuomi"

str.MakeUpper(); // str为"JIZHUOMI"

str.MakeReverse(); // str为"IMOUHZIJ"

```

>3）CString对象的连接
多个CString对象的连接可以通过重载运算符+、+=实现。
例如：
```cpp
CString str(_T("jizhuomi")); // str内容为"jizhuomi"

str = _T("www") + str + _T("-"); // str为"wwwjizhuomi-"

str += _T("com"); // str为wwwjizhuomi-com
```

>4）CString对象的比较
CString对象的比较可以通过==、！=、<；、>；、<=、>=等重载运算符实现，也可以使用Compare和CompareNoCase成员函数实现。
int Compare(PCXSTR psz) const;
将该CString对象与psz字符串比较，如果相等则返回0，如果小于psz则返回值小于0，如果大于psz则返回值大于0。

>5）CString对象字符串的查找操作
int Find(PCXSTR pszSub,int iStart=0) const throw();
int Find(XCHAR ch,int iStart=0) const throw();
在CString对象字符串的iStart索引位置开始，查找子字符串pszSub或字符ch第一次出现的位置，如果没有找到则返回-1。
int FindOneOf(PCXSTR pszCharSet) const throw();
查找pszCharSet字符串中的任意字符，返回第一次出现的位置，找不到则返回-1。
int ReverseFind(XCHAR ch) const throw();
从字符串末尾开始查找指定的字符ch，返回其位置，找不到则返回-1。这里要注意，尽管是从后向前查找，但是位置的索引还是要从开始算起。
例如：
```cpp
CString str = _T("jizhuomi");
int nIndex1 = str.Find(_T("zh")); // nIndex1的值为2

int nIndex2 = str.FindOneOf(_T("mui")); // nIndex2的值为1

int nIndex3 = str.ReverseFind(_T('i')); // nIndex3的值为7
```


>6）CString类对象字符串的替换与删除
int Replace(PCXSTR pszOld,PCXSTR pszNew);
用字符串pszNew替换CString对象中的子字符串pszOld，返回替换的字符个数。
int Replace(XCHAR chOld,XCHAR chNew);
用字符chNew替换CString对象中的字符chOld，返回替换的字符个数。
int Delete(int iIndex,int nCount = 1）；
从字符串中删除iIndex位置开始的nCount个字符，返回删除操作后的字符串的长度。
int Remove(XCHAR chRemove);
删除字符串中的所有由chRemove指定的字符，返回删除的字符个数。
例如：
```cpp
CString str = _T("jizhuomi");
int n1 = str.Replace(_T('i'),_T('j')); // str为"jjzhuomj"，n1为2

int n2 = str.Delete（1,2）； // str为"jhuomj"，n2为6

int n3 = str.Remove(_T('j')); // str为"ihuom"，n3为1
```

>8）CString类的格式化字符串方法
使用CString类的Format成员函数可以将int、short、long、float、double等数据类型格式化为字符串对象。
void __cdecl Format(PCXSTR pszFormat,[,argument]...);
参数pszFormat为格式控制字符串；参数argument可选，为要格式化的数据，一般每个argument在pszFormat中都有对应的表示其类型的子字符串，int型的argument对应的应该是"%d"，float型的应对应"%f"，等等。
例如：
```cpp
CString str;
int a = 1;
float b = 2.3f;
str.Format(_T("a=%d,b=%f"),a,b); // str为"a=1,b=2.300000"
```



***
