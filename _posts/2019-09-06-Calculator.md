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

打开资源视图（Ctrl+Shift+E）,在空白处**右键->添加资源->Menu->新建资源**。

![](/assets/post_img/2019-09-06/14.png)

创建之后，找到`工程名.rc\ Menu\ IDR_MENU1`，双击打开，即可进行菜单的编辑，编辑菜单名称的操作这里不多赘述。

菜单编辑完成后，按Ctrl+F5进行调试时会发现调试的主窗口并没有菜单。此时，我们需要回到我们的`工程名.rc\ Dialog\ IDD_工程名_DIGLOG`界面（即编辑主窗口的界面），选择**属性->杂项->MENU**，在下拉菜单中选择我们新建的菜单的ID，再次进行调试，会发现菜单成功显示出来了。

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

#### （1）CString类

在MFC代码中，数值类型与C++相同，但输入输出的字符类型为`TCHAR`，字符串类型为`CString`，可以利用宏定义`_T("字符串常量")`将C字符串转换为`CString`，利用宏定义`_T('字符常量')`将C字符转换为`TCHAR`。

```cpp
CString cs=_T("this is cstring"); //例子
```

`CString`类有如下成员函数可以供我们使用：(以下内容来自百度百科)

>1） CString类的构造函数<br>
`CString(const CString& stringSrc);`<br>
将一个已经存在的CString对象stringSrc的内容拷贝到该CString对象。<br>
`CString(TCHAR ch,int nLength = 1）;`<br>
使用此函数构造的CString对象中将含有nLength个重复的ch字符。<br>
例如：

```cpp
CString str1(_T(jizhuomi)); // 将常量字符串拷贝到str1
CString str2(str1); // 将str1的内容拷贝到str2
CString(TCHAR ch,int nLength = 1);
CString str(_T('w'),3); // str为"www"
```

>2）CString类的大小写转换及顺序转换函数<br>
`CString& MakeLower();` 将字符串中的所有大写字符转换为小写字符。<br>
`CString& MakeUpper(); `将字符串中的所有小写字符转换为大写字符。<br>
`CString& MakeReverse();` 将字符串中所有字符的顺序颠倒。<br>
例如：

```cpp
CString str(_T("JiZhuoMi"));
str.MakeLower(); // str为"jizhuomi"
str.MakeUpper(); // str为"JIZHUOMI"
str.MakeReverse(); // str为"IMOUHZIJ"
```

>3）CString对象的连接<br>
多个CString对象的连接可以通过重载运算符+、+=实现。<br>
例如：

```cpp
CString str(_T("jizhuomi")); // str内容为"jizhuomi"
str = _T("www") + str + _T("-"); // str为"wwwjizhuomi-"
str += _T("com"); // str为wwwjizhuomi-com
```

>4）CString对象的比较<br>
CString对象的比较可以通过==、！=、<；、>；、<=、>=等重载运算符实现，也可以使用`Compare`和`CompareNoCase`成员函数实现。<br>
`int Compare(PCXSTR psz) const;`<br>
将该CString对象与psz字符串比较，如果相等则返回0，如果小于psz则返回值小于0，如果大于psz则返回值大于0。

>5）CString对象字符串的查找操作<br>
`int Find(PCXSTR pszSub,int iStart=0) const throw();`<br>
`int Find(XCHAR ch,int iStart=0) const throw();`<br>
在CString对象字符串的iStart索引位置开始，查找子字符串pszSub或字符ch第一次出现的位置，如果没有找到则返回-1。<br>
`int FindOneOf(PCXSTR pszCharSet) const throw();`<br>
查找pszCharSet字符串中的任意字符，返回第一次出现的位置，找不到则返回-1。
`int ReverseFind(XCHAR ch) const throw();`<br>
从字符串末尾开始查找指定的字符ch，返回其位置，找不到则返回-1。这里要注意，尽管是从后向前查找，但是位置的索引还是要从开始算起。<br>
例如：

```cpp
CString str = _T("jizhuomi");
int nIndex1 = str.Find(_T("zh")); // nIndex1的值为2
int nIndex2 = str.FindOneOf(_T("mui")); // nIndex2的值为1
int nIndex3 = str.ReverseFind(_T('i')); // nIndex3的值为7
```

>6）CString类对象字符串的替换与删除<br>
`int Replace(PCXSTR pszOld,PCXSTR pszNew);`<br>
用字符串pszNew替换CString对象中的子字符串pszOld，返回替换的字符个数。<br>
`int Replace(XCHAR chOld,XCHAR chNew);`<br>
用字符chNew替换CString对象中的字符chOld，返回替换的字符个数。<br>
`int Delete(int iIndex,int nCount = 1）;`<br>
从字符串中删除iIndex位置开始的nCount个字符，返回删除操作后的字符串的长度。<br>
`int Remove(XCHAR chRemove);`<br>
删除字符串中的所有由chRemove指定的字符，返回删除的字符个数。<br>
例如：

```cpp
CString str = _T("jizhuomi");
int n1 = str.Replace(_T('i'),_T('j')); // str为"jjzhuomj"，n1为2
int n2 = str.Delete(1,2); // str为"jhuomj"，n2为6
int n3 = str.Remove(_T('j')); // str为"ihuom"，n3为1
```

>7）CString类的格式化字符串方法<br>
使用CString类的Format成员函数可以将int、short、long、float、double等数据类型格式化为字符串对象。<br>
`void __cdecl Format(PCXSTR pszFormat,[,argument]...);`<br>
参数`pszFormat`为格式控制字符串；参数`argument`可选，为要格式化的数据，一般每个a`rgument`在`pszFormat`中都有对应的表示其类型的子字符串，`int`型的`argument`对应的应该是`"%d"`，`float`型的应对应`"%f"`，等等。<br>
例如：

```cpp
CString str;
int a = 1;
float b = 2.3f;
str.Format(_T("a=%d,b=%f"),a,b); // str为"a=1,b=2.300000"
```

>8）CString类的长度、字符操作<br>
使用`GetLength()`可以得到CString对象的字符个数，使用`GetAt(int)`函数通过下标访问字符串中字符，可访问范围为`0~GetLength()-1`。<br>
例如：

```cpp
CString str=_T("Hello");
int n=str.GetLength(); //n=5
TCHAR CH=str.GetAt(str.GetLength()-1); //CH='o'
```

#### （2）文本框输入输出函数

为了实现计算器功能，我们自然需要往文本框输入文本、向文本框读取文本，这些可以用函数`GetDlgItemText`和`SetDlgltemText`实现。

`int GetDlgItemText( int nID, CString& rString ) const`<br>
调用`GetDlgItemText`可以获得与文本框中的标题或文本，参数`nID`指定了要获取其标题的控件的整数标识符（即文本框的ID），` rString`是对一个`CString`对象的引用。<br>
如果函数调用成功，返回值为拷贝到缓冲区中的`TCHAR`字符个数（不包括结束空字符）；如果函数调用失败，返回值为 0 。

`BOOL SetDlgltemText(int nlDDlgltem,LPCTSTR IpString);`<br>
调用`SetDlgItemText`可设置对话框中控件的文本和标题，参数`nlDDlgltem`标识带有将被设置的标题和文本的控件（即文本框的ID），`IpString`指向一个以`NULL`结尾的字符串指针，该字符串指针包含了将被复制到控件的文本。<br>
返回值：如果函数调用成功，则返回值为非零值。如果函数调用失败，则返回值为零。

#### （3）函数功能设计

回到我们设计的计算器，我们需要实现的功能分为以下几种：
1. 数字0~9输入。
2. 加减乘除符号输入。
3. 括号的输入.
4. 删除符号。
5. 等号完成计算。

数字按钮0~9的实现方式都是一样的，以按钮1为例，我们想要实现的是：按下按钮1，文本框中的字符串在最后一位增加一个1，那么我们可以这么实现：

```cpp
/*
按钮1的ID为Button1，文本框的ID为IDC_EDIT1
*/
void CcalculatorDlg::OnBnClickedButton1()
{
	// TODO: 在此添加控件通知处理程序代码
	CString cs;
	GetDlgItemText(IDC_EDIT1, cs); //读取当前文本框中的内容
	SetDlgItemText(IDC_EDIT1, cs + _T("1")); //将内容最后加上"1"并写入文本框
}
```

加减乘除符号的输入，我们不能像数字那样点击即可输入，因为算式`1+++++2`显然是不成立的。运算符号需要在数字或者右括号后才能输入：`(2+4)-2`；除此之外，减号`-`还能当作负号使用，那么负号还能作为第一个字符、在左括号后输入：`-2+(-1)`。根据我们的需求，我们可以写出以下代码，其中`+×÷`实现方式相同，`-`为另一种实现方式：

```cpp
/*
按钮+的ID为b，按钮-的ID为c
*/
void CcalculatorDlg::OnBnClickedButtonb() //加号操作
{
	CString cs;
	GetDlgItemText(IDC_EDIT1, cs);
	if (cs.GetLength()!=0 
	&& (cs.GetAt(cs.GetLength() - 1) >= _T('0') && cs.GetAt(cs.GetLength() - 1) <= _T('9') 
	|| cs.GetAt(cs.GetLength() - 1) == _T(')'))) 
	{
		SetDlgItemText(IDC_EDIT1, cs + _T("+")); 
	}
	return;
}

void CcalculatorDlg::OnBnClickedButtonc() //减号操作
{
	CString cs;
	GetDlgItemText(IDC_EDIT1, cs);
	if (cs.GetLength() == 0
	|| (cs.GetAt(cs.GetLength() - 1) >= _T('0') && cs.GetAt(cs.GetLength() - 1) <= _T('9'))
	|| cs.GetAt(cs.GetLength() - 1) == _T('(') || cs.GetAt(cs.GetLength() - 1) == _T(')')) 
	{
		SetDlgItemText(IDC_EDIT1, cs + _T("-"));
	}
	return;
}
```

左右括号的输入要求不同，左括号不能在数字之后输入、也能作为第一个字符输入；右括号只能在数字、右括号之后输入，代码如下：

```cpp
void CcalculatorDlg::OnBnClickedButtonf() //左括号
{
	CString cs;
	GetDlgItemText(IDC_EDIT1, cs);
	if(cs.GetLength() == 0 
	|| cs.GetAt(cs.GetLength() - 1) == _T('+') || cs.GetAt(cs.GetLength() - 1) == _T('-')
	|| cs.GetAt(cs.GetLength() - 1) == _T('×') || cs.GetAt(cs.GetLength() - 1) == _T('÷') 
	|| cs.GetAt(cs.GetLength() - 1) == _T('('))
		SetDlgItemText(IDC_EDIT1, cs + _T("("));
}

void CcalculatorDlg::OnBnClickedButtong() //右括号
{
	CString cs;
	GetDlgItemText(IDC_EDIT1, cs);
	if (cs.GetLength() != 0 
	&& ((cs.GetAt(cs.GetLength() - 1) >= _T('0') && cs.GetAt(cs.GetLength() - 1) <= _T('9') )
	|| cs.GetAt(cs.GetLength() - 1) == _T(')')))
		SetDlgItemText(IDC_EDIT1, cs + _T(")"));
}
```

删除符号的功能很简单，如果文本框中的字符串不为空，则删除最后一个字符，代码如下：

```cpp
/*
按钮←的ID为Button18
*/
void CcalculatorDlg::OnBnClickedButton18()
{
	CString cs;
	GetDlgItemText(IDC_EDIT1, cs);
	if (cs.GetLength() != 0) {
		cs.Delete(cs.GetLength() - 1);
		SetDlgItemText(IDC_EDIT1, cs);
	}
	return;
}
```

等号按钮是计算器的核心，需要对文本框中的中缀表达式进行运算并得出结果，这里我使用双栈进行计算，规则如下：

1. 运算时使用两个栈，一个数字栈，一个操作符栈。
2. 从左到右依次读取表达式，如果遇到数字，则把数字压入数字栈。
3. 如果是操作符，比较栈顶操作符和新操作符优先级：如果栈空、新的操作符是左括号(或优先级高于栈顶元素时，新的操作符入栈；如果新的操作符优先级不高于栈顶元素 ，就先出栈一个操作符进行运算，直到优先级高于栈顶元素，将新操作符入栈。
4. 若操作符为右括号，依次将栈顶元素弹出，直到遇到左括号，并将左括号弹出。
5. 一个操作符弹出后，将数字栈栈顶的两个元素弹出，进行该操作符的运算，再将运算结果压栈。
6. 当读取表达式完成后，判断操作符栈是否为空，若不为空，则依次出栈直到栈空。
7. 当操作符栈空，则数字栈栈顶元素为计算结果。

但是，这种方式没法处理以负数开头的表达式和在括号中以负数开头的表达式，所以最后在实现时添加了一条规则：

8. 如果操作符是负号，当负号为第一个字符或者负号的前一个字符是左括号时，该负号不入栈，并将下一个入栈的数字取反。

为了方便，我直接调用了STL的`<stack>`来实现栈，最终得到的代码如下，其中函数`OnBnClickedButtona`为点击按钮，函数`NumOperate`将数字字符串转为整型，函数`Operate`为操作符入栈操作，函数`PopOne`将一个操作符出栈并完成对应运算：

```cpp
#include <stack>

void CcalculatorDlg::OnBnClickedButtona()
{
	CString cs;
	GetDlgItemText(IDC_EDIT1, cs);
	std::stack<int> num; //数字栈
	std::stack<TCHAR> ope; //操作符栈
	int count = cs.GetLength();
	int neg = 1; //判断是否取反的操作
	int n = 0;
	while (n != count) {
		if (cs.GetAt(n) >= _T('0') && cs.GetAt(n) <= _T('9')) {
			num.push(neg * NumOperate(cs,n)); //数字入栈
			if (neg == -1)neg = 1;
		}
		else {
			Operate(num ,ope ,cs ,n ,neg ); //操作符入栈
			n++;
		}
	}
	while (PopOne(num, ope));
	cs.Format(_T("%d"), num.top());
	SetDlgItemText(IDC_EDIT1, cs);

}

int NumOperate(CString cs, int &n) {
	int num = 0;
	while (cs.GetAt(n) >= _T('0') && cs.GetAt(n) <= _T('9')) {
		num *= 10;
		num += cs.GetAt(n) - _T('0');
		n++;
	}
	return num;
}

bool PopOne(std::stack<int> &num, std::stack<TCHAR>& ope) {
	if (ope.empty() || ope.top() == _T('('))return 0; //
	int a = num.top();
	num.pop();
	int b = num.top();
	num.pop();
	switch(ope.top()){
		case _T('+'): {
			num.push(a + b);
			break;
		}
		case _T('-'): {
			num.push(b - a);
			break;
		}
		case _T('×'): {
			num.push(a * b);
			break;
		}
		case _T('÷'): {
			num.push(b / a);
			break;
		}
	}
	ope.pop();
	return 1;
}

void Operate(std::stack<int>& num,std::stack<TCHAR>& ope, CString &cs,int n,int &neg) {
	TCHAR cx = cs.GetAt(n);
	switch (cx) {
		case _T('+'): {
			if(ope.empty())
				ope.push(cx);
			else {
				while (PopOne(num, ope));
				ope.push(cx);
			}
			break;
		}
		case _T('-'): {
			if (n == 0 || cs.GetAt(n - 1) == _T('(')) {
				neg = -1; //当减号作为负号使用时，将neg赋值为-1且不入栈	
				break;
			}
			if (ope.empty())
				ope.push(cx);
			else {
				while (PopOne(num, ope));
				ope.push(cx);
			}
			break;
		}
		case _T('×'): {
			if (ope.empty()||ope.top()== _T('-')||ope.top()== _T('+'))
				ope.push(cx);
			else {
				while (ope.top() != _T('-') && ope.top() != _T('+') && PopOne(num, ope));
				ope.push(cx);
			}
			break;
		}
		case _T('÷'): {
			if (ope.empty() || ope.top() == _T('-') || ope.top() == _T('+'))
				ope.push(cx);
			else {
				while (ope.top() != _T('-') && ope.top() != _T('+') && PopOne(num, ope));
				ope.push(cx);
			}
			break;
		}
		case _T('('): {
			ope.push(cx);
			break;
		}
		case _T(')'): {
			while (PopOne(num, ope));
			ope.pop(); //将左括号弹出
			break;
		}
	}
}
```

自此，我们就完成了整个计算器功能的实现。

# Todo

初版的计算器功能并不完善，需要进一步的改善，现在准备在以下几个方面进行改进：

- [ ] 引入小数点的输入，将计算改为浮点运算。
- [ ] 在算式出现逻辑错误时（如3+2/0）在文本框显示`Error`字样。
- [ ] 引入平方/开方/百分号等操作符。

***
