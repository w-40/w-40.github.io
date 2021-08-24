---
layout: post
#标题配置
title:  Java中的String与StringBuilder
#时间配置
date:   2021-07-06 07:00:00 +0800
#目录配置
categories: Java
#标签配置
tag: 学习笔记
---

* content
{:toc}




# String与StringBuilder
## 一.String类常用方法
```java
public boolean equals(Object anObject)                                        //比较字符串的内容，严格区分大小写

public boolean equalsIgnoreCase(String anotherString)            //比较字符串的内容，忽略大小写

public int length()                                                                                //返回此字符串的长度

public char charAt(int Index)                                                             //返回指定索引处的char值

public char[] to Array()           															//将字符串拆分为字符数组后返回

public String substring(int beginIndex,int endIdex)                     //根据开头和结束索引进行截取，得到新                              的字符串（包含头，不包含尾）                       

public String substring(int beginIndex)                                          //从传入的索引处截取，截取到末尾，得到新的字符串

public String replace(CharSequence target,CharSequence replacement)   //使用新值，将字符串中的旧值替换，得到新的字符串

public String[] split(String regex)                                                   //根据传入的规则切割字符串，得到字符串数组      
```

## 二.StringBuilder

StringBuilder是一个可变的字符串类，可以看成是一个容器

作用：提高字符串的操作效率

### 1.构造方法：
```java
public StringBuilder()                  //创建一个空白可变字符串对象，不含有任何内容

public StringBuilder(String str)  //根据字符串的内容，来创建可变字符串对象
```
### 2.常用方法：
```java
public StringBuilder append()                 //添加数据，并返回对象本身

public StringBuilde reverser()                 //返回相反的字符序列

public int length()                                      //返回长度(字符出现的个数)

public String toString()                             //通过toString()就可以实现把StringBuilder转换为String
```

### 3.String与StringBuilder相互转化：

### （1）StringBuilder转换为String
```java
 public String toString()：通过toString()就可以把StringBuilder类型转换为String类型
```
### （2）String转换为StringBuilder
```java
 public StringBuilder(String s)：通过构造方法实现
```