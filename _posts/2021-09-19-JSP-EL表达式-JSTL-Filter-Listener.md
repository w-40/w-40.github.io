---
layout: post
#标题配置
title: JSP-EL表达式-JSTL-Filter-Listener
#时间配置
date:   2021-09-19 07:56:00 +0800
#目录配置
categories: JavaWeb
#标签配置
tag: 学习笔记
---

* content
{:toc}




# 一.JSP基础

## 1.JSP介绍

+ JSP（Java Servlet Pages）：是一种动态网页技术标准
+ JSP 部署在服务器上，可以处理客户端发送的请求，并根据请求内容动态的生成HTML、XML或其他格式文档的Web网页，然后再响应给客户端。
+ JSP是基于Java语言的，它的本质就是Servlet。

| 类别       | 使用场景                               |
| ---------- | -------------------------------------- |
| HTML       | 开发静态资源，无法加载动态资源         |
| CSS        | 美化页面                               |
| JavaScript | 给网页添加一些动态效果                 |
| Servlet    | 编写Java代码，实现后台功能处理         |
| JSP        | 包含了显示页面技术，也具备Java代码功能 |

## 2.JSP文件内容介绍

+ JSP本质就是一个Servlet

## 3.JSP语法
+ JSP注释
```jsp
<%--注释的内容--%>
```
+ Java代码块
```jsp
<%Java代码%>
```
```
+ JSP表达式
```jsp
<%=表达式%>
```
+ JSP声明
```jsp
<%!声明变量或方法%>
如果加！ 代表的是声明的是成员变量
如果不加！ 代表的是声明的是局部变量
声明方法必须加！
```

## 4.JSP指令
+ page指令
```jsp
<%@page 属性名=属性值 属性名=属性值...%>
```

| 属性名       | 作用                                                         |
| ------------ | ------------------------------------------------------------ |
| ContextType  | 响应正文支持的类型和设置编码格式                             |
| language     | 使用的语言，默认是java                                       |
| errorPage    | 当前页面出现异常后跳转的页面                                 |
| isErrorPage  | 是否抓住异常，如果为true则页面中就可以使用异常对象，默认是false |
| import       | 导包import=“java.util.ArrayList”                             |
| session      | 是否创建HttpSession对象，默认是true                          |
| buffer       | 设定JspWriter输出jso内容缓存的大小，默认8kb                  |
| pageEncoding | 翻译jsp时所用的编码格式                                      |
| isElgnored   | 是否忽略EL表达式，默认是false                                |

+ include指令：可以包含其他页面
```jsp
<%@include file=包含的页面%>
```
+ taglib指令：
可以引入外部标签库
```jsp
<%@taglib url=标签库的地址 prefix=前缀名称%>
```

## 5.JSP细节
+ 九大隐式对象

| 隐式对象名称 | 代表实际对象                           |
| ------------ | -------------------------------------- |
| request      | javax.servlet.http.HttpServletRequest  |
| response     | javax.servlet.http.HttpServletResponse |
| session      | javax.servlet.http.HttpSession         |
| application  | javax.servlet.ServletContext           |
| page         | java.lang.Object                       |
| config       | javax.servlet.ServletConfig            |
| exception    | java.lang.Throwable                    |
| out          | javax.servlet.jsp.JspWriter            |
| pageContext  | javax.servlet.jsp.PageContext          |

+ pageContext对象

  ​	是JSP独有的，Servlet中没有。

  ​	是四大域对象之一的页面域对象，还可以操作其他三个域对象中的属性。

  ​	还可以获取其他八个隐式对象。

  ​	生命周期是随着JSP的创建而存在，随着JSP的结束而消失。每个JSP页面都有一个PageContext	对象。

+ 四大域对象

| 域对象名称     | 范围     | 级别                     | 备注                                     |
| -------------- | -------- | ------------------------ | ---------------------------------------- |
| PageContext    | 页面范围 | 最小，只能在当前页面用   | 因范围太小，开发中用的很少               |
| ServletRequest | 请求范围 | 一次请求或当前请求转发用 | 请求转发之后，再次转发时请求域丢失       |
| HttpSession    | 会话范围 | 多次请求数据共享时使用   | 多次请求共享数据，但不同的客户端不能共享 |
| ServletContext | 应用范围 | 最大，整个应用都可以使用 | 尽量少用，如果对数据有修改需要做同步处理 |

## 6.MVC模型
+ M(Model):模型。用于封装数据，封装的是数据模型!
+ v(View):视图。用于显示数据，动态资源用JSP页面，静态资源用HTML页面!
+ C(Controller):控制器。用于处理请求和响应，例如Servlet !

# 二.高级
## 1.EL表达式介绍
+ EL（Expression Language）：表达式语言
+ 在JSP 2.0规范中加入的内容，也是Servlet规范的一部分
+ 作用：在JSP页面中获取数据。让我们的JSP脱离java代码块和JSP表达式
+ 语法：$(表达式内容)
## 2.EL表达式获取数据
```jsp
<%--1.获取基本数据类型--%>
    <% pageContext.setAttribute("num",10);%>
    基本数据类型：${num}<br>
    <%--2.获取自定义对象类型--%>
    <%
        Student stu = new Student("张三",23);
        pageContext.setAttribute("stu",stu);
    %>
    自定义对象：${stu}<br>
    <%--stu.name 实现原理 getName()--%>
    学生姓名：${stu.name}<br>
    学生年龄：${stu.age}<br>
    <%--3.获取数组类型--%>
    <%
        String[] arr = {"hello","world"};
        pageContext.setAttribute("arr",arr);
    %>
    数组：${arr}<br>
    0索引元素：${arr[0]} <br>
    1索引元素：${arr[1]} <br>
    <%--4.获取List集合--%>
    <%
        ArrayList<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbb");
        pageContext.setAttribute("list",list);
    %>
    List集合：${list}<br>
    0索引元素：${list[0]} <br>
    <%--5.获取Map集合--%>
    <%
        HashMap<String,Student> map = new HashMap<>();
        map.put("hm01",new Student("张三",23));
        map.put("hm02",new Student("李四",24));
        pageContext.setAttribute("map",map);
    %>
    Map集合：${map}<br>
    第一个学生对象：${map.hm01}<br>
    第一个学生对象的姓名：${map.hm01.name}
```
## 3.EL表达式注意事项
+ EL表达式没有空指针异常
+ EL表达式没有索引越界异常
+ EL表达式没有字符串的拼接

## 4.EL表达式运算符

+ 关系运算符

| 运算符 | 作用     | 实例                 | 结果  |
| ------ | -------- | -------------------- | ----- |
| ==或eq | 等于     | ${5 == 5}或${5 eq 5} | true  |
| !=或ne | 不等于   | ${5 != 5}或${5 ne 5} | false |
| <或lt  | 小于     | ${3 < 5}或${3 lt 5}  | true  |
| >或gt  | 大于     | ${3 > 5}或${3 gt 5}  | false |
| <=或le | 小于等于 | ${3 <= 5}或${3 le 5} | true  |
| >=或ge | 大于等于 | ${3 >= 5}或${3 ge 5} | false |

+ 逻辑运算符

| 运算符   | 作用 | 示例                 | 结果       |
| -------- | ---- | -------------------- | ---------- |
| &&或and  | 并且 | ${A&&B}或${A and B}  | true/false |
| \|\|或or | 或者 | ${A\|\|B}或${A or B} | true/false |
| !或not   | 取反 | ${!A}或${not A}      | true/false |

+ 其他运算符

- | 运算符                  | 作用                                                         |
  | ----------------------- | ------------------------------------------------------------ |
  | empty                   | 1.判断对象是否为null                                                                                                    2.判断字符串是否为空字符串                                                                                             3.判断容器元素是否为0 |
  | 条件？ 表达式1：表达式2 | 三元运算符                                                   |

## 5.EL表达式使用细节
+ EL表达式能够获取四大域对象的数据，根据名称从小到大在域对象中查找
+ 还可以获取JSP其他八个隐式对象，并调用对象中的方法

## 6.EL表达式隐式对象

| 隐式对象名称     | 对应JSP隐式对象 | 说明                     |
| ---------------- | --------------- | ------------------------ |
| pageContext      | pageContext     | 功能完全相同             |
| applicationScope | 没有            | 操作应用域对象数据       |
| sessionScope     | 没有            | 操作会话域对象数据       |
| requestScope     | 没有            | 操作请求域对象数据       |
| pageScope        | 没有            | 操作页面域对象数据       |
| header           | 没有            | 获取请求头数据           |
| headerValues     | 没有            | 获取请求头数据(多个值)   |
| param            | 没有            | 获取请求参数数据         |
| paramValues      | 没有            | 获取请求参数数据(多个值) |
| initParam        | 没有            | 获取请求参数数据         |
| cookie           | 没有            | 获取cookie对象           |

# 三.JSTL
## 1.JSTL介绍
+ JSTL（Java Server Pages Standarded Tag Libraty）：JSP标准标签库
+ 主要提供给开发人员一个标准通用的标签库
+ 开发人员可以利用这些标签取代JSP页面上的Java代码，从而提高程序的可读性，降低程序的维护难度

| 组成      | 作用       | 说明                   |
| --------- | ---------- | ---------------------- |
| core      | 核心标签库 | 通用的逻辑处理         |
| fmt       | 国际化     | 不同地域显示不同语言   |
| functions | EL函数     | EL表达式可以使用的方法 |
| sql       | 操作数据库 | 了解                   |
| xml       | 操作XML    | 了解                   |

## 2.JSTL核心标签库

| 标签名称                                                     | 功能分类 | 属性       | 作用           |
| ------------------------------------------------------------ | -------- | ---------- | -------------- |
| <标签名：if>                                                 | 流程控制 | 核心标签库 | 用于条件判断   |
| <标签名：choose>         <br> <标签名：when>  <br/><标签名：otherwise> | 流程控制 | 核心标签库 | 用于多条件判断 |
| <标签名：forEach>                                            | 迭代遍历 | 核心标签库 | 用于循环遍历   |

# 四.Filter
## 1.过滤器介绍
+ 在程序中访问服务器资源时，当一个请求到来，服务器首先判断是否有过滤器与请求资源相关联，如果有，过滤器可以将请求拦截下来，完成一些特定的功能，再由过滤器决定是否交给请求资源。如果没有则像之前那样直接请求资源了。响应也是类似的!
+ 过滤器一般用于完成通用的操作，例如∶登录验证、统一编码处理、敏感字符过滤等等~~
## 2.Filter介绍
+ Filter是一个接口。如果想要实现过滤器的功能，必须实现该接口
+ 核心方法

| 返回值 | 方法名                                                       | 作用                     |
| ------ | ------------------------------------------------------------ | ------------------------ |
| void   | init(FilterConfig config)                                    | 初始化方法               |
| void   | doFilter(ServletRequest request,ServletResponse response,FilterChain chain) | 对请求资源和响应资源过滤 |
| void   | destroy                                                      | 销毁方法                 |

## 3.FilterChain介绍
+ FilterChain是一个接口，代表过滤器链对象。由Servlet容器提供实现类对象。直接使用即可。

+ 过滤器可以定义多个，就会组成过滤器链。

+ 核心方法

  | 返回值 | 方法名                                           | 作用     |
  | ------ | ------------------------------------------------ | -------- |
  | void   | doFilter(ServletRequest,ServletResponse reponse) | 放行方法 |

  

  **如果有多个过滤器，在第一个过滤器中调用下一个过滤器，以此类推。直到到达最终访问资源。**
  
  **如果只有一个过滤器，放行时，就会直接到达最终访问资源**

## 4.过滤器使用细节
+ 配置方式
	注解方式   @WebFilter(拦截路径
	配置文件方式
	```xml
	<filter>
        <filter-name>filterDemo01</filter-name>
        <filter-class>com.wzk.filter.FilterDemo01</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>filterDemo01</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
	```
	
+ 多个过滤器使用顺序

     	如果有多个过滤器，取决于过滤器映射的顺序

## 5.FilterConfig介绍
+ FilterConfig是一个接口。代表过滤器的配置对象，可以加载一些初始化参数
+ 核心方法

| 返回值         | 方法名                       | 作用               |
| -------------- | ---------------------------- | ------------------ |
| String         | getFilterName()              | 获取过滤器对象名称 |
| String         | getInitParameter(String key) | 根据key获取value   |
| Enumeration    | getInitParameterNames()      | 获取所有参数的key  |
| ServletContext | getServletContext()          | 获取应用上下文对象 |

## 6.过滤器五种拦截行为
+ Filter过旎器默认拦截的是请求，但是在实际开发中，我们还有请求转发和请求包含，以及由服务器触发调用的全局错误页面。默认情况下过滤器是不参与过滤的，要想使用，就需要我们配置。
+ 配置过滤器
```xml
<filter>
        <filter-name>filterDemo05</filter-name>
        <filter-class>com.wzk.filter.FilterDemo05</filter-class>
        <!--配置开启异步支持，当dispatcher配置ASYNC时，需要配置此行-->
        <async-supported>true</async-supported>
    </filter>
    <filter-mapping>
        <filter-name>filterDemo05</filter-name>
        <url-pattern>/error.jsp</url-pattern>
        <!--过滤请求：默认值。-->
        <dispatcher>REQUEST</dispatcher>
        <!--过滤全局错误页面：当由服务器调用全局错误页面时，过滤器工作-->
        <dispatcher>ERROR</dispatcher>
        <!--过滤请求转发：当请求转发时，过滤器工作。-->
        <dispatcher>FORWARD</dispatcher>
        <!--过滤请求包含：当请求包含时，过滤器工作。它只能过滤动态包含，jsp的include指令是静态包含-->
        <dispatcher>INCLUDE</dispatcher>
        <!--过滤异步类型，它要求我们在filter标签中配置开启异步支持-->
        <dispatcher>ASYNC</dispatcher>
    </filter-mapping>
```

# 5.Listener

## 1.监听器介绍
+ 观察者设计模式，所有的监听器都是基于观察者设计模式的！
+ 三个组成部分
	事件源：触发事件的对象
	事件：触发的动作，封装了事件源
	监听器：当事件源触发事件后，可以完成功能
+ 在程序当中，我们可以对:对象的创建销毁．域对象中属性的变化、会话相关内容进行监听。
+ Servlet规范中共计8个监听器，监听器都是以接口形式提供，具体功能需要我们自己来完成。
## 2.监听对象的监听器
+ ServletContextListener:用于监听ServletContext对象的创建和销毁
+ 核心方法

| 返回值 | 方法名                                      | 作用                 |
| ------ | ------------------------------------------- | -------------------- |
| void   | contextInitialized(ServletContextEvent sce) | 对象创建时执行该方法 |
| void   | contextDestroyed(ServletContextEvent sce)   | 对象销毁时执行该方法 |

**参数:ServletContextEvent代表事件对象**

**事件对象中封装了事件源，也就是ServletContext**

**真正的事件指的是创建或销毁ServletContext对象的操作**

+ HttpSessionListener:用于监听HttpSession对象的创建和销毁
+ 核心方法

| 返回值 | 方法名                                | 作用                 |
| ------ | ------------------------------------- | -------------------- |
| void   | sessionCreated(HttpSessionEvent se)   | 对象创建时执行该方法 |
| void   | sessionDestroyed(HttpSessionEvent se) | 对象销毁时执行该方法 |

**参数:HttpSessionEvent代表事件对象**

**事件对象中封装了事件源，也就是HttpSession**

**真正的事件指的是创建或销毁HttpSession对象的操作**

+ ServletRequestListener:用于监听ServletRequest对象的创建和销毁
+ 核心方法
| 返回值 | 方法名                                      | 作用                 |
| ------ | ------------------------------------------- | -------------------- |
| void   | requestInitialized(ServletRequestEvent sre) | 对象创建时执行该方法 |
| void   | requestDestroyed(ServletRequestEvent sre)   | 对象销毁时执行该方法 |

**参数:ServletRequestEvent代表事件对象**

**事件对象中封装了事件源，也就是ServletRequest**

**真正的事件指的是创建或销毁ServletRequest对象的操作**

## 3.监听域对象属性变化的监听器

+ ServletContextAttributeListener:用于监听ServletContext应用域中属性的变化
+ 核心方法

| 返回值 | 方法名                                                 | 作用                     |
| ------ | ------------------------------------------------------ | ------------------------ |
| void   | attributeAdded(ServletContextAttributeEvent scae)      | 域中添加属性时执行该方法 |
| void   | attributeRemoved(ServletContextAttributeEvent scae)    | 域中移除属性时执行该方法 |
| void   | attributeReplace\ed(ServletContextAttributeEvent scae) | 域中替换属性时执行该方法 |

**参数:ServletContextAttributeEvent代表事件对象**

**事件对象中封装了事件源，也就是ServletContext**

**真正的事件指的是添加、移除、替换应用域中属性的操作**

+ HttpSessionAttributeListener:用于监听HttpSession会话域中属性的变化
+ 核心方法

| 返回值 | 方法名                                          | 作用                     |
| ------ | ----------------------------------------------- | ------------------------ |
| void   | attributeAdded(HttpSessionBindingEvent se)      | 域中添加属性时执行该方法 |
| void   | attributeRemoved(HttpSessionBindingEvent se)    | 域中移除属性时执行该方法 |
| void   | attributeReplace\ed(HttpSessionBindingEvent se) | 域中替换属性时执行该方法 |

**参数:HttpSessionBindingEvent代表事件对象**

**事件对象中封装了事件源，也就是HttpSession**

**真正的事件指的是添加、移除、替换会话域中属性的操作**

+ ServletReauestAttributeListener:用于监听ServletRequest请求域中属性的变化
+ 核心方法
| 返回值 | 方法名                                                 | 作用                     |
| ------ | ------------------------------------------------------ | ------------------------ |
| void   | attributeAdded(ServletRequestAttributeEvent srae)      | 域中添加属性时执行该方法 |
| void   | attributeRemoved(ServletRequestAttributeEvent srae)    | 域中移除属性时执行该方法 |
| void   | attributeReplace\ed(ServletRequestAttributeEvent srae) | 域中替换属性时执行该方法 |

**参数: ServletRequestAttributeEvent代表事件对象**

**事件对象中封装了事件源，也就是ServletRequest**

**真正的事件指的是添加、移除、替换请求域中属性的操作**

## 4.监听会话相关的感知型监听器

+ HttpSessionBindingListener:用于感知对象和会话域绑定的监听器
+ 核心方法

| 返回值 | 方法名                                       | 作用                                 |
| ------ | -------------------------------------------- | ------------------------------------ |
| void   | valueBound(HttpSessionBangdingEvent event)   | 数据添加到会话域中(绑定)时执行该方法 |
| void   | valueUnbound(HttpSessionBangdingEvent event) | 数据添加到会话域中(移除)时执行该方法 |

**参数:HttpSessionBindingEvent代表事件对象**

**事件对象中封装了事件源，也就是HttpSession**

**真正的事件指的是添加、移除会话域中数据的操作**

+ HttpSessionActivationListener:用于感知会话域中对象钝化和活化的监听器
+ 核心方法

| 返回值 | 方法名                                    | 作用                         |
| ------ | ----------------------------------------- | ---------------------------- |
| void   | sessionWillPassivate(HttpSessionEvent se) | 会话域中数据钝化时执行该方法 |
| void   | sessionDidActivate(HttpSessionEvent se)   | 会话域中数据活化时执行该方法 |

**参数:HttpSessionEvent代表事件对象**

**事件对象中封装了事件源，也就是HttpSession**

**事件指的是会话域中数据钝化、活化的操作**

## 5.监听器的使用

+ 监听对象的

  ​	ServletContextListener

  ​	HttpSessionListener

  ​	ServletRequestListener

+ 监听属性变化的
	ServletContextAttributelistener
	HttpSessionAttributeListener
	ServletRequestAttributeListener

+ 会话相关的感知型

  HttpSessionBindingListener

  HttpSessionActivationlListener

