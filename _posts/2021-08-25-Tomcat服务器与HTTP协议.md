---
layout: post
#标题配置
title: Tomat服务器与HTTP协议
#时间配置
date:   2021-08-25 22:30:00 +0800
#目录配置
categories: JavaWeb
#标签配置
tag: 学习笔记
---

* content
{:toc}






# Tomcat服务器与HTTP协议
## 一.服务器的介绍
+ 常用的应用服务器
  服务器名称                                        说明
  weblogic                        实现了javaEE规范，重量级服务器，又称为javaEE容器
  websphereAS              实现了javaEE规范，重量级服务器
  JBOSSAS                        实现了javaEE规范，重量级服务器，免费的
  Tomcat                         实现了jsp/servlet规范，是一个轻量级服务器，开源免费

## 二.Tomcat的基本使用
### 1.启动
```
startup.bat            Windows下启动执行文件
startup.sh             Linux下启动执行文件
```
### 2.停止
```
shutdown.bat         Windows下关闭执行文件
shutdown.sh          Linux下关闭执行文件
```
### 3.启动问题
启动窗口一闪而过：没有配置jdk环境变量
java.net.BindException：端口8080被占用
### 4.部署自己的项目
①在webapps目录下创建一个文件夹
②将资源放到该文件夹里
③启动tomcat，输入正确路径
### 5.tomcat控制台乱码解决
```
打开conf目录下的logging.properties文件，找到java.util.logging.ConsoleHandler.encoding = UTF-8
修改为java.util.logging.ConsoleHandler.encoding = gbk
```
### 6.IDEA集成Tomcat
①点击Run -> Edit Configurations
②弹出界面点Defaults -> Tomcat Server -> Local
③点击Configure -> Tomcat Home -> 选择tomcat所在路径

### 7.Linux安装Tomcat
```
1.上传压缩包到/home路径：
put D:/apache-tomcat-9.0.29.tar.gz
移动到home路径下
mv apache-tomcat-9.0.29.tar.gz /home/
2.解压压缩包：
tar -zxvf apache-tomcat-9.0.28.tar.gz
3.进入bin目录下：
cd apache-tomcat-9.0.29/bin
4.启动tomcat服务：
./startup.sh
5.使用浏览器测试：http://xxx.xxx.xxx.xxx:8080/
```
### 8. JavaWeb项目的发布
#### 1.通过IDEA直接发布
①点击Run -> Edit Configurations
②点击Tomcat Server -> Deployment
**Application Context 是项目访问路径，/代表默认路径，多个项目中只能有一个默认路径**
③点击Tomcat Server -> Server
配置关联浏览器，是否能加载资源两个都改成update sources，jdk，端口号
④启动tomcat服务
⑤验证结果（控制台出现Connect to server）
#### 2.通过war包发布（了解）
①在项目的web路径下打war包：jar -cvf 项目名.war（进入目录后右键点击在此处打开命令窗口）
②将打好的war包剪切到tomcat的webapps路径下
③启动tomcat服务，自动解压war包
### 9.Tomcat配置文件
+ 主配置文件server.xml（tomcat的conf目录下）
Connect 后的port是默认端口号
```xml
<Connector port="8080" protocol="HTTP/1.1"
```
**8080端口：tomcat服务默认端口号，访问url地址后必须手动写:8080
80端口：HTTP协议采用的端口号，访问url地址后不用写:80**
### 10.Tomcat配置虚拟目录
虚拟目录的作用：可以发布任意目录下的项目
①编辑server.xml配置文件，找到<Host>标签
②在</host>前加入
```xml
<Context path="/地址栏访问的路径" docBase="项目路径"> 
```
path属性：访问资源的虚拟目录名称
docBase属性：项目真实存在的路径
### 11.Tomcat配置虚拟主机
虚拟主机作用：可以指定访问路径的名称
①编辑server.xml配置文件，找到<Engine>标签
②加入以下内容

```xml
<Engine name="Catalina" defaultHost="localhost">
	<Host name="www.webdemo.com" appBase="webapps"
		unpackWARs="true" autoDeploy="true">
	  <Context path="" docBase="项目名"/>
	</Host>
</Engine>
```

name属性：访问虚拟主机的名称
appBase属性：项目存放的路径
unpackWARs属性：是否自动解压war包
autoDepioy属性：是否自动发布
③修改hosts文件（C:\Windows\System32\drivers\etc\hosts）
将ip地址与域名绑定

## 三.HTTP协议
### 1.HTTP协议的介绍
+ HTTP（Hyper Text Transfer Protocl）：超文本传输协议。
+ HTTP协议是基于TCP/IP协议的
+ 超文本：比普通文本更强大
+ 传输协议：客户端和服务器端的通信规则（握手规则）
客户端向服务器发起请求，服务器向客户端响应
**注意：JavaScript、CSS、图片资源会自动发起请求**

### 2.HTTP协议的请求
(1))请求的组成部分
①请求行
②请求头
③请求空行             //就是一个空行
④请求体
（2）请求的方式
+ GET

+ POST
  **注意：只有POST请求方式才有请求体**
  1.请求行
  请求方式提交路径(提交参数)HTTP/版本号
  2.请求头

  | 名称              | 说明                                                     |
  | ----------------- | -------------------------------------------------------- |
  | Accept            | 客户端浏览器所支持的MIME类型                             |
  | Accept-Encoding   | 客户端浏览器所支持的压缩编码格式。最常用的就是gzip压缩。 |
  | Accept-Language   | 客户端浏览器所支持的语言。一般都是zh_CN或en_US等。       |
  | Referer           | 告知服务器，当前请求的来源                               |
  | Content-Type      | 请求正文所支持的MIME类型,                                |
  | Content-Length    | 请求正文的长度                                           |
  | User-Agent        | 浏览器相关信息                                           |
  | Connection        | 连接的状态。Keep-Alive保持连接。                         |
  | lf-Modified-Since | 客户端浏览器缓存文件的最后修改时间。                     |
  | Cookie            | 会话管理相关，非常的重要。                               |

  
  3.请求空行
   普通换行，用于区分请求头和请求体
  4.请求体
   只有POST提交方式才有请求体，用于显示提交参数。
### 3.HTTP协议的响应
（1）响应的组成部分
①响应行
②响应头
③响应空行             //就是一个空行
④响应体

+ 常见状态码

| 状态码  | 说明                                 |
| ------- | ------------------------------------ |
| 200     | 一切OK                               |
| 302/307 | 请求重定向，两次请求，地址栏发生变化 |
| 304     | 请求资源未发生变化，使用缓存         |
| 404     | 请求资源未找到                       |
| 500     | 服务器错误                           |

（2）响应头

| 名称                   | 说明                                                    |
| ---------------------- | ------------------------------------------------------- |
| Location               | 请求重定向地址，请求重定向的地址，常与302,307配合使用。 |
| Server                 | 服务器相关信息。                                        |
| Content-Type           | 响应正文的MIME类型。                                    |
| Content-Length         | 响应正文的长度。                                        |
| Content-Disposition    | 告知客户端浏览器，以下载的方式打开响应正文。            |
| Refresh                | 定时刷新。                                              |
| Last-Modified          | 服务器资源的最后修改时间。                              |
| Set-Cookie             | 会话管理相关，非常的重要。                              |
| Expires:-1             | 服务器资源到客户端浏览器后的缓存时间。                  |
| Catch-Control:no-catch | 不要缓存                                                |

（3）响应空行

普通换行，用于区分响应头和响应头

（4）响应体

将资源文件发送给客户端浏览器进行解析