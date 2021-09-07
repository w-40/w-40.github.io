---
layout: post
#标题配置
title: Cookie&&Session
#时间配置
date:   2021-09-07 21:26:00 +0800
#目录配置
categories: JavaWeb
#标签配置
tag: 学习笔记
---

* content
{:toc}



# 一.Cookie
## 1.会话介绍
+ 会话：浏览器和服务器之间的多次请求和响应
+ 为了实现一些功能，浏览器和服务器之间可能会产生多次的请求和响应，从浏览器访问服务器开始,到访问服务器结束(关闭浏览器、到了过期时间)。这期间产生的多次请求和响应加在一起就称之为浏览器和服务器之间的一次会话。
+ 会话过程所产生的一些数据，可以通过会话技术（Cookie和Session）保存

## 2.Cookie介绍
+ Cookie：客户端会话管理技术
     把要共享的数据保存到客户端
     每次请求时，把会话信息带的服务器端，从而实现多次请求的数据共享!
+ 作用：可以保存客户端访问网站的相关内容，从而保证每次访问时先从本地缓存中获取，以此提高效率！

| 属性名  | 作用                   | 是否重要 |
| ------- | ---------------------- | -------- |
| name    | Cookie的名称           | 必须属性 |
| value   | Cookie的值(不支持中文) | 必须属性 |
| path    | Cookie的路径           | 重要     |
| domain  | Cookie的域名           | 重要     |
| maxAge  | Cookie的存活时间       | 重要     |
| version | Cookie的版本号         | 不重要   |
| comment | Cookie的描述           | 不重要   |

## 3.Cookie方法

| 方法名                           | 作用             |
| -------------------------------- | ---------------- |
| Cookie(String name,String value) | 构造方法创建对象 |
| 属性对应的set和get方法           | 赋值和获取值     |

## 4.Cookie添加和获取
+ 添加：HttpServletResponse

| 返回值 | 方法名                   | 说明               |
| ------ | ------------------------ | ------------------ |
| void   | addCookie(Cookie cookie) | 向客户端添加Cookie |

+ 获取：HttpServletRequest

| 返回值   | 方法名      | 说明             |
| -------- | ----------- | ---------------- |
| Cookie[] | getCookie() | 获取所有的Cookie |

## 5.Cookie的使用

## 6.Cookie的细节
+ 数量限制
   每个网站最多只能有20个Cookie，且大小不能超过4KB，所有网站的Cookie总数不能超过300个
+ 名称限制
   Cookie的名称只能包含ASCCI码表的字母、数字字符。不能包含逗号、分号、空格、不能以$开头。Cookie的值不支持中文。
+ 存活时间限制setMaxAge()方法接收数字
   负整数：当前会话有效，浏览器关闭则清楚
   0：立即清楚
   正整数：以秒为单位设置存活时间 
+ 访问路径限制
   默认路径：取自第一次访问的资源路径前缀。只要以这个路径开头就能访问到。
   设置路径：setPath()方法设置指定路径

# 二.Session
## 1.HttpSession介绍
+ HttpSession：服务器端会话管理技术
    本质也是采用客户端会话管理技术
    只不过客户端保存的是一个特殊标识，而共享的数据保存到了服务器端的内存对象中。
    每次请求时，会将特殊标识带到服务器端，根据这个标识来找到对应的内存空间，从而实现数	  据共享。
    是Servlet规范中四大域对象之一的会话域对象。
+ 作用：可以实现数据共享

| 域对象         | 功能   | 作用                                   |
| -------------- | ------ | -------------------------------------- |
| ServletContext | 应用域 | 在整个应用之间实现数据共享             |
| ServletRequest | 请求域 | 在当前的请求或请求转发之间实现数据共享 |
| HttpSession    | 会话域 | 在当前会话范围之间实现数据共享         |

## 2.HttpSession常用方法

| 返回值 | 方法名                                  | 说明             |
| ------ | --------------------------------------- | ---------------- |
| void   | setAttribute(String name, Object value) | 设置共享数据     |
| Object | getAttribute(String name)               | 获取共享数据     |
| void   | removeAttribute(String name)            | 移除共享数据     |
| String | getId()                                 | 获取唯一标识名称 |
| void   | Invalidate()                            | 让Session立即    |

## 3.HttpSession获取
+ HttpSession实现类对象是通过HttpServletRequest对象来获取

| 返回值      | 方法名                     | 说明                              |
| ----------- | -------------------------- | --------------------------------- |
| HttpSession | getSession()               | 获取HttpSession对象               |
| HttpSession | getSession(boolean create) | HttpSession，未获取到是否自动创建 |

## 4.HttpSession的细节
+ 唯一标识的查看（浏览器开发者工具）
+ 浏览器禁用Cookie
  方式一：通过提示信息告知用户，大部分网站采用的解决方式（推荐）
  方式二：访问时拼接jsession标识，通过encodeURL()方法重写地址（了解）
+ 钝化和活化
   + 什么是钝化和活化
      钝化：序列化。把长时间不用，但还不到过期时间的HttpSession进行序列化，写到磁盘上
      活化：相反的状态
   + 何时钝化
      第一种情况︰当访问量很大时，服务器会根据getLastAccessTime来进行排序，
      对长时间不用，但是还没到过期时间的HttpSession进行序列化。
      第二种情况∶当服务器进行重启的时候，为了保持客户HttpSession 中的数据，也要对其		 进行序列化。
+ 注意
   HttpSession的序列化由服务器自动完成，我们无需关心