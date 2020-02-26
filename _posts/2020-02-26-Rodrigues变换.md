---
layout: post
title: Rodrigues变换
date: 2020-02-26
author: Max.C
header-img: 'assets/img/pro25.jpg'
catalog: true
tags: openCV 
---

## 一、旋转矩阵和平移变量



向量在三维坐标的旋转可以通过$\vec{b}=R\vec{a}$实现，其中R为**针对三个坐标轴**的旋转矩阵的乘积：$R=R_zR_yR_x$，即分别绕x、y、z轴旋转α、β、θ的角度。

$R_x=\begin{bmatrix}1 & 0 & 0 \\ 0 &\cos{\alpha} &\sin{\alpha}\\ 0 & -\sin{\alpha} & \cos{\alpha} \end{bmatrix}$

$R_y=\begin{bmatrix}\cos{\beta} & 0 & -\sin{\beta}\\ 0&1&0\\ \sin{\beta} & 0 & \cos{\beta} \end{bmatrix}$

$R_z=\begin{bmatrix} \cos{\theta} &  \sin{\theta} &0\\ -\sin{\theta} & \cos{\theta} &0 \\ 0&0&1 \end{bmatrix}$

将旋转矩阵视为$R = [ X_x,X_y,X_z]$，因为只做了旋转变换，各向量的相对位置不变，即依旧**相互正交**，且均为单位向量，即R为标准正交阵，满足性质：

1. $R^{-1} = R^T$
2. $||X_x|| = ||X_y|| = ||X_z|| = 1$
3. 各列向量相互正交

平移变量用来表示将**一个坐标系的原点**移动到**另一个坐标系的原点**。

## 二、Rodrigues变换

在做双目立体视觉深度图像生成的时候，会出现旋转向量（1x3）与旋转矩阵（3x3），它们之间可以通过罗德里格斯相互转化。

### 1、旋转向量

向量旋转公式最早由 Rodrigues 提出，用一个**三维向量**来表示三维旋转变换，该向量的**方向是旋转轴**，其**模是旋转角度**。

设旋转向量的单位向量为 r，模为 θ。三维点（或者说三维向量）p 在旋转向量 r 的作用下变换至 p′，则：

![img](https://img-blog.csdnimg.cn/20181130105754473.png)

### 2、相互转换

设旋转向量的单位向量 $r=[rx ry rz]^T$，旋转角度为 θ，对应的旋转矩阵为 R，则 r 到 R 的转换是：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018113011014873.png)

其中 I 是三阶单位矩阵。反过来 R 到 r 的转换则可以利用等式：

![img](https://img-blog.csdnimg.cn/20181130110241110.png)

openCV函数实现为`Rodrigues(Vec3, Mat33)`。