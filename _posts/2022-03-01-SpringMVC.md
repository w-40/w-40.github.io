---
layout: post
#标题配置
title: SpringMVC框架
#时间配置
date:   2022-03-01 11:23:00 +0800
#目录配置
categories: 框架
#标签配置
tag: 学习笔记
---

* content
{:toc}



# SpringMVC

## 1.SpringMVC 概述

**三层架构**    

- 表现层：负责数据展示

- 业务层：负责业务处理

- 数据层：负责数据操作

![1](https://img-blog.csdnimg.cn/65a6348ada7c4ab2bdf18a348a081aec.png)

MVC（Model View Controller），**一种用于设计创建Web应用程序表现层的模式**

- Model**（模型）：数据模型，用于封装数据**

- View**（视图）：页面视图，用于展示数据**

* jsp  
* html

Controller（控制器）：处理用户交互的调度器，用于根据用户需求处理程序逻辑

* Servlet
* SpringMVC

![2](https://img-blog.csdnimg.cn/d4ee71dddd1546aba39b7edaa129a34e.png)  

**SpringMVC简介**

+ SpringMVC**是一种基于**Java**实现**MVC**模型的轻量级**Web**框架**

**SpringMVC优点**

+ 使用简单
+ ***性能突出（相比现有的框架基础）***
+ 灵活性强

## 2.入门案例

### 2.1 入门案例制作

①导入SpringMVC相关坐标

```xml
<!-- servlet3.1规范的坐标 -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
<!--jsp坐标-->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.1</version>
    <scope>provided</scope>
</dependency>
<!--spring的坐标-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.1.9.RELEASE</version>
</dependency>
<!--spring web的坐标-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.1.9.RELEASE</version>
</dependency>
<!--springmvc的坐标-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.1.9.RELEASE</version>
</dependency>
```

②定义表现层业务处理器Controller，并配置成spring的bean（等同于Servlet）

```java
@Controller
public class UserController {

    public void save(){
        System.out.println("user mvc controller is running ...");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
    <!--扫描加载所有的控制类类-->
    <context:component-scan base-package="com.wzk"/>

</beans>
```

③web.xml中配置SpringMVC核心控制器，用于将请求转发到对应的具体业务处理器Controller中（等同于Servlet配置）

```xml
<servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath*:spring-mvc.xml</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

④设定具体Controller的访问路径（等同于Servlet在web.xml中的配置）

```java
//设定当前方法的访问映射地址
@RequestMapping("/save")
public void save(){
    System.out.println("user mvc controller is running ...");
}
```

⑤设置返回页面  

```java
//设定当前方法的访问映射地址
@RequestMapping("/save")
//设置当前方法返回值类型为String，用于指定请求完成后跳转的页面
public String save(){
    System.out.println("user mvc controller is running ...");
    //设定具体跳转的页面
    return "success.jsp";
}
```

### 2.2 入门案例工作流程分析  

* 服务器启动
  1. 加载web.xml中DispatcherServlet
  2. 读取spring-mvc.xml中的配置，加载所有com.wzk包中所有标记为bean的类
  3. 读取bean中方法上方标注@RequestMapping的内容
* 处理请求
  1. DispatcherServlet配置拦截所有请求 /
  2. 使用请求路径与所有加载的@RequestMapping的内容进行比对
  3. 执行对应的方法
  4. 根据方法的返回值在webapp目录中查找对应的页面并展示  

### 2.3 SpringMVC 技术架构图

![4](https://img-blog.csdnimg.cn/5372227691af4f93a410a6c0c16e2825.png)



![5](https://img-blog.csdnimg.cn/08c0b05a6bf74d48b8851bfce18bd4dd.png)



*  DispatcherServlet：前端控制器， 是整体流程控制的中心，由其调用其它组件处理用户的请求， 有
   效的降低了组件间的耦合性
*  HandlerMapping：处理器映射器， 负责根据用户请求找到对应具体的Handler处理器
*  Handler：处理器，业务处理的核心类，通常由开发者编写，描述具体的业务
*  HandlAdapter：处理器适配器，通过它对处理器进行执行
*  View Resolver：视图解析器， 将处理结果生成View视图
*  View：视图，最终产出结果， 常用视图如jsp、 html  

![6](https://img-blog.csdnimg.cn/12a62e15a5b94758ace11cfa65b3a62c.png)

## 3.基本配置

### 3.1常规配置（Controller加载控制）

* SpringMVC的处理器对应的bean必须按照规范格式开发，未避免加入无效的bean可通过bean加载过滤器进
  行包含设定或排除设定，表现层bean标注通常设定为@Controller  

**xml方式**

```xml
<context:component-scan base-package="com.wzk">
    <context:include-filter
                            type="annotation"
                            expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

#### 3.1.1 静态资源加载

```xml
<!--放行指定类型静态资源配置方式-->
<mvc:resources mapping="/img/**" location="/img/"/>
<mvc:resources mapping="/js/**" location="/js/"/>
<mvc:resources mapping="/css/**" location="/css/"/>

<!--SpringMVC提供的通用资源放行方式-->
<mvc:default-servlet-handler/>
```

#### 3.1.2 中文乱码处理 

  SpringMVC提供专用的中文字符过滤器，用于处理乱码问题  

  配置在 **web.xml** 里面

```XML
<!--乱码处理过滤器，与Servlet中使用的完全相同，差异之处在于处理器的类由Spring提供-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 3.2 注解驱动

*   使用注解形式转化SpringMVC核心配置文件为配置类  


```java
@Configuration
@ComponentScan(value = "com.wzk",includeFilters =
    @ComponentScan.Filter(type=FilterType.ANNOTATION,classes = {Controller.class})
    )
public class SpringMVCConfiguration implements WebMvcConfigurer{
    //注解配置放行指定资源格式
//    @Override
//    public void addResourceHandlers(ResourceHandlerRegistry registry) {
//        registry.addResourceHandler("/img/**").addResourceLocations("/img/");
//        registry.addResourceHandler("/js/**").addResourceLocations("/js/");
//        registry.addResourceHandler("/css/**").addResourceLocations("/css/");
//    }

    //注解配置通用放行资源的格式
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();;
    }
}
```

*   基于servlet3.0规范，自定义Servlet容器初始化配置类，加载SpringMVC核心配置类  


```java
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
    //创建Servlet容器时，使用注解的方式加载SPRINGMVC配置类中的信息，并加载成WEB专用的			           //ApplicationContext对象
    //该对象放入了ServletContext范围，后期在整个WEB容器中可以随时获取调用
    @Override
    protected WebApplicationContext createServletApplicationContext() {
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMVCConfiguration.class);
        return ctx;
    }

    //注解配置映射地址方式，服务于SpringMVC的核心控制器DispatcherServlet
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    @Override
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }

    //乱码处理作为过滤器，在servlet容器启动时进行配置，相关内容参看Servlet零配置相关课程
    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        super.onStartup(servletContext);
        CharacterEncodingFilter cef = new CharacterEncodingFilter();
        cef.setEncoding("UTF-8");
        FilterRegistration.Dynamic registration = servletContext.addFilter("characterEncodingFilter", cef);
        registration.addMappingForUrlPatterns(EnumSet.of(DispatcherType.REQUEST,DispatcherType.FORWARD,DispatcherType.INCLUDE),false,"/*");
    }
}
```

  删除web.xml
  删除spring-mvc.xml  

 **小节**

+ 基于servlet3.0规范，配置Servlet容器初始化配置类，初始化时加载SpringMVC配置类
+ 转化SpringMVC核心配置文件
+ 转化为注解（例如： spring处理器加载过滤）
+ 转化为bean进行加载
+ 按照标准接口进行开发并加载（例如：中文乱码处理、静态资源加载过滤）  

## 4.请求

### 4.1 普通类型参数传参

  参数名与处理器方法形参名保持一致  

  访问URL： http://localhost/requestParam1?name=wzk&age=14  

```java
@RequestMapping("/requestParam1")
public String requestParam1(String name ,String age){
    System.out.println("name="+name+",age="+age);
    return "page.jsp";
}
```

 **@RequestParam** 的使用

+ 类型： 形参注解
+ 位置：处理器类中的方法形参前方
+ 作用：绑定请求参数与对应处理方法形参间的关系  

```java
@RequestMapping("/requestParam2")
public String requestParam2(@RequestParam(
                            name = "userName",
                            required = true,
                            defaultValue = "wzk") String name){
    
    System.out.println("name="+name);
    return "page.jsp";
}
```

### 4.2 POJO类型参数传参

当POJO中使用简单类型属性时， 参数名称与POJO类属性名保持一致  

访问URL： http://localhost/requestParam3?name=wzk&age=14  

**Controller**

```java
@RequestMapping("/requestParam3")
public String requestParam3(User user){
    System.out.println("name="+user.getName());
    return "page.jsp";
}
```

**POJO类**

```java
public class User {
    private String name;
    private Integer age;
    
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

}
```



**参数冲突**

+ 当POJO类型属性与其他形参出现同名问题时，将被同时赋值
+ 建议使用@RequestParam注解进行区分
+ 访问URL： http://localhost/requestParam4?name=wzk&**age**=14  

```java
@RequestMapping("/requestParam4")
public String requestParam4(User user,String age){
    System.out.println("user.age="+user.getAge()+",age="+age);
    return "page.jsp";
}
```



**复杂POJO类型参数**

+ 当POJO中出现对象属性时，参数名称与对象层次结构名称保持一致  

+ 访问URL： http://localhost/requestParam5?address.province=beijing  

```java
public class User {
    private String name;
    private Integer age;

    private Address address;
    
    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

}
```

```java
@RequestMapping("/requestParam5")
public String requestParam5(User user){
    System.out.println("user.address="+user.getAddress().getProvince());
    return "page.jsp";
}
```

![7](https://img-blog.csdnimg.cn/f3288d7357ff4745a699664da7a2af79.png)

+ **当POJO中出现List，保存对象数据，参数名称与对象层次结构名称保持一致，使用数组格式描述集合中对象的位置**  

+ 访问URL： http://localhost/requestParam7?addresses[0].province=bj&addresses[1].province=tj  

```java
public class User {
    private String name;
    private Integer age;
    private List<Address> addresses;
}

public class Address {
    private String province;
    private String city;
    private String address;
}
```

```java
@RequestMapping("/requestParam7")
public String requestParam7(User user){
    System.out.println("user.addresses="+user.getAddress());
    return "page.jsp";
}
```

 **当POJO中出现Map，保存对象数据，参数名称与对象层次结构名称保持一致，使用映射格式描述集合中对象的位置**  

+  访问URL： http://localhost/requestParam8?addressMap[’home’].province=bj&addressMap[’job’].province=tj  

```java
public class User {
    private String name;
    private Integer age;
    private Map<String,Address> addressMap;
}
public class Address {
    private String province;
    private String city;
    private String address;
}
```

```java
@RequestMapping("/requestParam8")
public String requestParam8(User user){
    System.out.println("user.addressMap="+user.getAddressMap());
    return "page.jsp";
}
```

### 4.3 数组与集合类型参数传参

**数组类型参数**

+ 请求参数名与处理器方法形参名保持一致，且请求参数数量＞ 1个  

+ 访问URL： http://localhost/requestParam9?nick=Jockme&nick=zahc  

```java
@RequestMapping("/requestParam9")
public String requestParam9(String[] nick){
    System.out.println(nick[0]+","+nick[1]);
    return "page.jsp";
}
```

**集合类型参数**

+ 保存简单类型数据，请求参数名与处理器方法形参名保持一致，且请求参数数量＞ 1个
+ 访问URL： http://localhost/requestParam10?nick=Jockme&nick=zahc

```java
@RequestMapping("/requestParam10")
public String requestParam10(@RequestParam("nick") List<String> nick){
    System.out.println(nick);
    return "page.jsp";
}
```

+ 注意： SpringMVC默认将List作为对象处理，赋值前先创建对象，然后将nick作为对象的属性进行处理。由于List是接口，无法创建对象，报无法找到构造方法异常；修复类型为可创建对象的ArrayList类型后，对象可以创建，但没有nick属性，因此数据为空。此时需要告知SpringMVC的处理器nick是一组数据，而不是一个单一数据。通过@RequestParam注解，将数量大于1个names参数打包成参数数组后， SpringMVC才能识别该数据格式，并判定形参类型是否为数组或集合，并按数组或集合对象的形式操作数据。  

 **小节**

+ 请求POJO类型参数获取
+ POJO的简单属性
+ POJO的对象属性
+ POJO的集合属性（存储简单数据）
+ POJO的集合属性（存储对象数据）
+ 名称冲突问题  

### 4.4 类型转换器

SpringMVC对接收的数据进行自动类型转换，该工作通过Converter接口实现  

![8](https://img-blog.csdnimg.cn/9f0055718cd14db5b6717deb1d729e79.png)

* **标量转换器**

  + StringToBooleanConverter String→Boolean
  + ObjectToStringConverter Object→String
  + StringToNumberConverterFactory String→Number（ Integer、 Long等）
  + NumberToNumberConverterFactory Number子类型之间(Integer、 Long、 Double等)
  + StringToCharacterConverter String→java.lang.Character
  + NumberToCharacterConverter Number子类型(Integer、 Long、 Double等)→java.lang.Character
  + CharacterToNumberFactory java.lang.Character→Number子类型(Integer、 Long、 Double等)
  + StringToEnumConverterFactory String→enum类型
  + EnumToStringConverter enum类型→String
  + StringToLocaleConverter String→java.util.Local
  + PropertiesToStringConverter java.util.Properties→String
  + StringToPropertiesConverter String→java.util.Properties  

* **集合、数组相关转换器**
  
  + ArrayToCollectionConverter 数组→集合（ List、 Set）
  + CollectionToArrayConverter 集合（ List、 Set） →数组
  + ArrayToArrayConverter 数组间
  + CollectionToCollectionConverter 集合间（ List、 Set）
  + MapToMapConverter Map间
  + ArrayToStringConverter 数组→String类型
  + StringToArrayConverter String→数组， trim后使用“,”split
  + ArrayToObjectConverter 数组→Object
  + ObjectToArrayConverter Object→单元素数组
  + CollectionToStringConverter 集合（ List、 Set） →String
  + StringToCollectionConverter String→集合（ List、 Set）， trim后使用“,”split
  + CollectionToObjectConverter 集合→Object
  + ObjectToCollectionConverter Object→单元素集合  
  
* **默认转换器**
  
  + ObjectToObjectConverter Object间
  + IdToEntityConverter Id→Entity
  + FallbackObjectToStringConverter Object→String  
  
* **SpringMVC对接收的数据进行自动类型转换，该工作通过Converter接口实现**  

  ![9](https://img-blog.csdnimg.cn/e5b6bf47453b4166bc62239e78e787a2.png)

### 4.5 日期类型格式转换  

* **声明自定义的转换格式并覆盖系统转换格式**  

  ```xml
  <!--5.启用自定义Converter-->
  <mvc:annotation-driven conversion-service="conversionService"/>
  <!--1.设定格式类型Converter，注册为Bean，受SpringMVC管理-->
  <bean id="conversionService"
        class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
      <!--2.自定义Converter格式类型设定，该设定使用的是同类型覆盖的思想-->
      <property name="formatters">
          <!--3.使用set保障相同类型的转换器仅保留一个，避免冲突-->
          <set>
              <!--4.设置具体的格式类型-->
              <bean class="org.springframework.format.datetime.DateFormatter">
                  <!--5.类型规则-->
                  <property name="pattern" value="yyyy-MM-dd"/>
              </bean>
          </set>
      </property>
  </bean>
  ```

* **日期类型格式转换（简化版）**
  
  + 名称： @DateTimeFormat
  + 类型： 形参注解、成员变量注解
  + 位置：形参前面 或 成员变量上方
  + 作用：为当前参数或变量指定类型转换规则
  + 范例：  
  
  ```java
  public String requestParam12(@DateTimeFormat(pattern = "yyyy-MM-dd") Date date){
      System.out.println("date="+date);
      return "page.jsp";
  }
  ```
  
  ```java
  @DateTimeFormat(pattern = "yyyy-MM-dd")
  private Date birthday;
  ```
  
* 注意：依赖注解驱动支持  

  <mvc:annotation-driven />  

### 4.6 自定义类型转换器

* 自定义类型转换器，实现Converter接口，并制定转换前与转换后的类型

  ```xml
  <!--1.将自定义Converter注册为Bean，受SpringMVC管理-->
  <bean id="myDateConverter" class="com.wzk.converter.MyDateConverter"/>
  <!--2.设定自定义Converter服务bean-->
  <bean id="conversionService"
        class="org.springframework.context.support.ConversionServiceFactoryBean">
      <!--3.注入所有的自定义Converter，该设定使用的是同类型覆盖的思想-->
      <property name="converters">
          <!--4.set保障同类型转换器仅保留一个，去重规则以Converter<S,T>的泛型为准-->
          <set>
              <!--5.具体的类型转换器-->
              <ref bean="myDateConverter"/>
          </set>
      </property>
  </bean>
  ```

  ```java
  //自定义类型转换器，实现Converter接口，接口中指定的泛型即为最终作用的条件
  //本例中的泛型填写的是String，Date，最终出现字符串转日期时，该类型转换器生效
  public class MyDateConverter implements Converter<String, Date> {
      //重写接口的抽象方法，参数由泛型决定
      public Date convert(String source) {
          DateFormat df = new SimpleDateFormat("yyyy-MM-dd");
          Date date = null;
          //类型转换器无法预计使用过程中出现的异常，因此必须在类型转换器内部捕获，不允许抛出，框架无法预计此类异常如何处理
          try {
              date = df.parse(source);
          } catch (ParseException e) {
              e.printStackTrace();
          }
          return date;
      }
  }
  ```

  

* **通过注册自定义转换器，将该功能加入到SpringMVC的转换服务ConverterService中**  

  ```xml
  <!--开启注解驱动，加载自定义格式化转换器对应的类型转换服务-->
  <mvc:annotation-driven conversion-service="conversionService"/>
  ```

  

### 4.7 请求映射 @RequestMapping

#### 4.7.1 方法注解

* 名称： @RequestMapping
  + 类型： 方法注解 
  + 位置：处理器类中的方法定义上方
  + 作用：绑定请求地址与对应处理方法间的关系
  + 范例：
  + 访问路径： /requestURL1  

```java
@Controller
@RequestMapping("/user")
public class UserController {
    @RequestMapping("/requestURL2")
    public String requestURL2() {
        return "page.jsp";
    }
}
```

#### 4.7.2 类注解

**名称： @RequestMapping**

+ 类型： 类注解
+ 位置：处理器类定义上方
+ 作用：为当前处理器中所有方法设定公共的访问路径前缀
+ 范例：
+ 访问路径： /user/requestURL1

```java
@Controller
@RequestMapping("/user")
public class UserController {
    @RequestMapping("/requestURL2")
    public String requestURL2() {
        return "page.jsp";
    }
}  
```

* 常用属性

  ```java
  @RequestMapping(
      value="/requestURL3", //设定请求路径，与path属性、 value属性相同
      method = RequestMethod.GET, //设定请求方式
      params = "name", //设定请求参数条件
      headers = "content-type=text/*", //设定请求消息头条件
      consumes = "text/*", //用于指定可以接收的请求正文类型（MIME类型）
      produces = "text/*" //用于指定可以生成的响应正文类型（MIME类型）
  )
  public String requestURL3() {
      return "/page.jsp";
  }
  ```

  

## 5.响应

### 5.1 页面跳转

* 转发（默认）

```java
@RequestMapping("/showPage1")
public String showPage1() {
    System.out.println("user mvc controller is running ...");
    return "forward:page.jsp";
}
```

*  重定向

```
@RequestMapping("/showPage2")
public String showPage2() {
System.out.println("user mvc controller is running ...");
return "redirect:page.jsp";
}
```

+ 注意：页面访问地址中所携带的   **/**  

### 5.2 页面访问快捷设定 (InternalResourceViewResolver)

  展示页面的保存位置通常固定，且结构相似，可以设定通用的访问路径，简化页面配置格式  

```java
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/pages/"/>
    <property name="suffix" value=".jsp"/>
/bean>
```

```java
public String showPage3() {
    return "page";
}
```

  **如果未设定了返回值，使用void类型，则默认使用访问路径作页面地址的前缀后缀**  

```java
//最简页面配置方式，使用访问路径作为页面名称，省略返回值
@RequestMapping("/showPage5")
public void showPage5() {
    System.out.println("user mvc controller is running ...");
}
```

### 5.3 带数据页面跳转

* 方式一：使用HttpServletRequest类型形参进行数据传递  

  ```java
  @RequestMapping("/showPageAndData1")
  public String showPageAndData1(HttpServletRequest request) {
      request.setAttribute("name","wzk");
      return "page";
  }
  ```

  

* 方式二：使用Model类型形参进行数据传递  

  ```java
  @RequestMapping("/showPageAndData2")
  public String showPageAndData2(Model model) {
      model.addAttribute("name","wzk");
      Book book = new Book();
      book.setName("SpringMVC入门实战");
      book.setPrice(66.6d);
      model.addAttribute("book",book);
      return "page";
  }
  ```

  

* 方式三：使用ModelAndView类型形参进行数据传递，将该对象作为返回值传递给调用者  

  ```java
  //使用ModelAndView形参传递参数，该对象还封装了页面信息
  @RequestMapping("/showPageAndData3")
  public ModelAndView showPageAndData3(ModelAndView modelAndView) {
      //ModelAndView mav = new ModelAndView();    替换形参中的参数
      Book book  = new Book();
      book.setName("SpringMVC入门案例");
      book.setPrice(66.66d);
      //添加数据的方式，key对value
      modelAndView.addObject("book",book);
      //添加数据的方式，key对value
      modelAndView.addObject("name","Jockme");
      //设置页面的方式，该方法最后一次执行的结果生效
      modelAndView.setViewName("page");
      //返回值设定成ModelAndView对象
      return modelAndView;
  }
  ```

### 5.4 返回json数据

* 方式一：基于response返回数据的简化格式，返回JSON数据

  ```java
  //使用jackson进行json数据格式转化
  @RequestMapping("/showData3")
  @ResponseBody
  public String showData3() throws JsonProcessingException {
      Book book  = new Book();
      book.setName("SpringMVC入门案例");
      book.setPrice(66.66d);
  
      ObjectMapper om = new ObjectMapper();
      return om.writeValueAsString(book);
  }
  ```

* 使用SpringMVC提供的消息类型转换器将对象与集合数据自动转换为JSON数据    

  ```java
  //使用SpringMVC注解驱动，对标注@ResponseBody注解的控制器方法进行结果转换，由于返回值为引用类型，自动调用jackson提供的类型转换器进行格式转换
  @RequestMapping("/showData4")
  @ResponseBody
  public Book showData4() {
      Book book  = new Book();
      book.setName("SpringMVC入门案例");
      book.setPrice(66.66d);
      return book;
  }
  ```

    需要手工添加信息类型转换器  

  ```xml
  <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
      <property name="messageConverters">
          <list>
              <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
          </list>
      </property>
  </bean
  ```

* 方式三：使用SpringMVC注解驱动简化配置  

  ```xml
  <!--开启springmvc注解驱动，对@ResponseBody的注解进行格式增强，追加其类型转换的功能，具体实现由MappingJackson2HttpMessageConverter进行-->
  <mvc:annotation-driven/>
  ```

  

### 6.Servlet相关接口-Servlet相关接口替换方案

  **HttpServletRequest / HttpServletResponse / HttpSession**  

* **SpringMVC提供访问原始Servlet接口API的功能，通过形参声明即可**  

  ```java
  @RequestMapping("/servletApi")
  public String servletApi(HttpServletRequest request,
                           HttpServletResponse response, HttpSession session){
      System.out.println(request);
      System.out.println(response);
      System.out.println(session);
      request.setAttribute("name","wzk");
      System.out.println(request.getAttribute("name"));
      return "page.jsp";
  }
  ```

* **Head数据获取**  

  + 名称： @RequestHeader
  + 类型： 形参注解
  + 位置：处理器类中的方法形参前方
  + 作用：绑定请求头数据与对应处理方法形参间的关系
  + 范例：

  ```java
  @RequestMapping("/headApi")
  public String headApi(@RequestHeader("Accept-Language") String head){
      System.out.println(head);
      return "page.jsp";
  }  
  ```

* **Cookie数据获取**  

  *   名称： @CookieValue
  *   类型： 形参注解
  *   位置：处理器类中的方法形参前方
  *   作用：绑定请求Cookie数据与对应处理方法形参间的关系
  *   范例：

  ```java
  @RequestMapping("/cookieApi")
  public String cookieApi(@CookieValue("JSESSIONID") String jsessionid){
      System.out.println(jsessionid);
      return "page.jsp";
  }  
  ```

* **Session数据获取**  

  *   名称： @SessionAttribute
  *   类型： 形参注解
  *   位置：处理器类中的方法形参前方
  *   作用：绑定请求Session数据与对应处理方法形参间的关系
  *   范例：

  ```java
  @RequestMapping("/sessionApi")
  public String sessionApi(@SessionAttribute("name") String name){
      System.out.println(name);
      return "page.jsp";
  }  
  ```

* **Session数据设置（了解）**  

  *   名称： @SessionAttributes
  *   类型： 类注解
  *   位置：处理器类上方
  *   作用：声明放入session范围的变量名称，适用于Model类型数据传参
  *   范例：

  ```java
  @Controller
  @SessionAttributes(names={"name"})
  public class ServletController {
      @RequestMapping("/setSessionData2")
      public String setSessionDate2(Model model) {
          model.addAttribute("name", "Jock2");
          return "page.jsp";
      }
  }  
  ```

* **注解式参数数据封装底层原理**  

  *   数据的来源不同，对应的处理策略要进行区分
  *   Head
  *   Cookie
  *   Session
  *   SpringMVC使用策略模式进行处理分发
  *   顶层接口： HandlerMethodArgumentResolver
  *   实现类： ……  

## 7.异步调用

### 7.1 发送异步请求（回顾）  

```js
<a href="javascript:void(0);" id="testAjax">访问controller</a>
<script type="text/javascript" src="/js/jquery-3.3.1.min.js"></script>
<script type="text/javascript">
    $(function(){
    $("#testAjax").click(function(){ //为id="testAjax"的组件绑定点击事件
        $.ajax({ //发送异步调用
            type:"POST", //请求方式： POST请求
            url:"ajaxController", //请求参数（也就是请求内容）
            data:'ajax message', //请求参数（也就是请求内容）
            dataType:"text", //响应正文类型
            contentType:"application/text", //请求正文的MIME类型
        });
    });
});
</script>
```

### 7.2 接受异步请求参数

+ 名称： @RequestBody
+ 类型： 形参注解
+ 位置：处理器类中的方法形参前方
+ 作用：将异步提交数据组织成标准请求参数格式，并赋值给形参
+ 范例：

```java
@RequestMapping("/ajaxController")
public String ajaxController(@RequestBody String message){
    System.out.println(message);
    return "page.jsp";
}  
```

* 注解添加到Pojo参数前方时，封装的异步提交数据按照Pojo的属性格式进行关系映射
* 注解添加到集合参数前方时，封装的异步提交数据按照集合的存储结构进行关系映射 

```java
@RequestMapping("/ajaxPojoToController")
//如果处理参数是POJO，且页面发送的请求数据格式与POJO中的属性对应，@RequestBody注解可以自动映射对应请求数据到POJO中
//注意：POJO中的属性如果请求数据中没有，属性值为null，POJO中没有的属性如果请求数据中有，不进行映射
public String  ajaxPojoToController(@RequestBody User user){
    System.out.println("controller pojo :"+user);
    return "page.jsp";
}

@RequestMapping("/ajaxListToController")
//如果处理参数是List集合且封装了POJO，且页面发送的数据是JSON格式的对象数组，数据将自动映射到集合参数中
public String  ajaxListToController(@RequestBody List<User> userList){
    System.out.println("controller list :"+userList);
    return "page.jsp";
}
```

### 7.3 异步请求接受响应数据

* 方法返回值为Pojo时，自动封装数据成json对象数据

```java
@RequestMapping("/ajaxReturnJson")
@ResponseBody
public User ajaxReturnJson(){
    System.out.println("controller return json pojo...");
    User user = new User();
    user.setName("Jockme");
    user.setAge(40);
    return user;
}  
```

* 方法返回值为List时，自动封装数据成json对象数组数据  

```java
@RequestMapping("/ajaxReturnJsonList")
@ResponseBody
//基于jackon技术，使用@ResponseBody注解可以将返回的保存POJO对象的集合转成json数组格式数据
public List ajaxReturnJsonList(){
    System.out.println("controller return json list...");
    User user1 = new User();
    user1.setName("Tom");
    user1.setAge(3);

    User user2 = new User();
    user2.setName("Jerry");
    user2.setAge(5);

    ArrayList al = new ArrayList();
    al.add(user1);
    al.add(user2);

    return al;
}
```

## 8.异步请求-跨域访问

### 8.1 跨域访问介绍

* 当通过域名A下的操作访问域名B下的资源时，称为跨域访问
* 跨域访问时，会出现无法访问的现象   

![10](https://img-blog.csdnimg.cn/4114bbb21cc44406888f646a8b151673.png)

### 8.2 跨域环境搭建

* 为当前主机添加备用域名
  * 修改windows安装目录中的host文件
  * 格式： ip 域名
* 动态刷新DNS
  *  命令： ipconfig /displaydns
  *  命令： ipconfig /flushdns   

### 8.3 跨域访问支持  

+ 名称： @CrossOrigin
+ 类型： 方法注解 、 类注解
+ 位置：处理器类中的方法上方 或 类上方
+ 作用：设置当前处理器方法/处理器类中所有方法支持跨域访问
+ 范例：  

```java
@RequestMapping("/cross")
@ResponseBody
//使用@CrossOrigin开启跨域访问
//标注在处理器方法上方表示该方法支持跨域访问
//标注在处理器类上方表示该处理器类中的所有处理器方法均支持跨域访问
@CrossOrigin
public User cross(HttpServletRequest request){
    System.out.println("controller cross..."+request.getRequestURL());
    User user = new User();
    user.setName("Jockme");
    user.setAge(39);
    return user;
}
```

## 9.拦截器

### 9.1 拦截器概念

* 请求处理过程解析  

![11](https://img-blog.csdnimg.cn/7585c55bb7604baeaca01474d3780dfb.png)

+ 拦截器（ Interceptor）是一种动态拦截方法调用的机制

+ 作用：

  1.在指定的方法调用前后执行预先设定后的的代码

  2.阻止原始方法的执行

+ 核心原理： AOP思想
+ 拦截器链：多个拦截器按照一定的顺序，对原始被调用功能进行增强  



* **拦截器VS过滤器**
  
  + 归属不同： Filter属于Servlet技术， Interceptor属于SpringMVC技术
  + 拦截内容不同： Filter对所有访问进行增强， Interceptor仅针对SpringMVC的访问进行增强  
  
  ![12](https://img-blog.csdnimg.cn/ae17c61e80424535962810287bde1822.png)

### 9.2 自定义拦截器开发过程

* 实现HandlerInterceptor接口  

  ```java
  //自定义拦截器需要实现HandleInterceptor接口
  public class MyInterceptor implements HandlerInterceptor {
      //处理器运行之前执行
      @Override
      public boolean preHandle(HttpServletRequest request,
                               HttpServletResponse response,
                               Object handler) throws Exception {
          System.out.println("前置运行----a1");
          //返回值为false将拦截原始处理器的运行
          //如果配置多拦截器，返回值为false将终止当前拦截器后面配置的拦截器的运行
          return true;
      }
  
      //处理器运行之后执行
      @Override
      public void postHandle(HttpServletRequest request,
                             HttpServletResponse response,
                             Object handler,
                             ModelAndView modelAndView) throws Exception {
          System.out.println("后置运行----b1");
      }
  
      //所有拦截器的后置执行全部结束后，执行该操作
      @Override
      public void afterCompletion(HttpServletRequest request,
                                  HttpServletResponse response,
                                  Object handler,
                                  Exception ex) throws Exception {
          System.out.println("完成运行----c1");
      }
  
      //三个方法的运行顺序为    preHandle -> postHandle -> afterCompletion
      //如果preHandle返回值为false，三个方法仅运行preHandle
  }
  ```

  

* 配置拦截器  

  配置拦截器  

```xml
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/showPage"/>
        <bean class="com.wzk.interceptor.MyInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```

  <u>注意：配置顺序为先配置执行位置，后配置执行类</u>  

### 9.3 拦截器执行流程

![13](https://img-blog.csdnimg.cn/b2c445b8cc9d4fd89702fb862cd063bc.png)

### 9.4 拦截器配置与方法参数

#### 9.4.1 前置处理方法

原始方法之前运行

```java
public boolean preHandle(HttpServletRequest request,
                         HttpServletResponse response,
                         Object handler) throws Exception {
    System.out.println("preHandle");
    return true;
}
```

* 参数
  + request:请求对象
  + response:响应对象
  + handler:被调用的处理器对象，本质上是一个方法对象，对反射中的Method对象进行了再包装
* 返回值
  * 返回值为false，被拦截的处理器将不执行  


#### 9.4.2   后置处理方法

原始方法运行后运行，如果原始方法被拦截，则不执行  

```java
public void postHandle(HttpServletRequest request,
                       HttpServletResponse response,
                       Object handler,
                       ModelAndView modelAndView) throws Exception {
    System.out.println("postHandle");
}
```

+ 参数
+ modelAndView:如果处理器执行完成具有返回结果，可以读取到对应数据与页面信息，并进行调整  

#### 9.4.3 完成处理方法

  拦截器最后执行的方法，无论原始方法是否执行  

```java
public void afterCompletion(HttpServletRequest request,
                            HttpServletResponse response,
                            Object handler,
                            Exception ex) throws Exception {
    System.out.println("afterCompletion");
}
```

+ 参数
+ ex:如果处理器执行过程中出现异常对象，可以针对异常情况进行单独处理  

### 9.5 拦截器配置项  

```xml
<mvc:interceptors>
    <!--开启具体的拦截器的使用，可以配置多个-->
    <mvc:interceptor>
        <!--设置拦截器的拦截路径，支持*通配-->
        <!--/**         表示拦截所有映射-->
        <!--/*          表示拦截所有/开头的映射-->
        <!--/user/*     表示拦截所有/user/开头的映射-->
        <!--/user/add*  表示拦截所有/user/开头，且具体映射名称以add开头的映射-->
        <!--/user/*All  表示拦截所有/user/开头，且具体映射名称以All结尾的映射-->
        <mvc:mapping path="/*"/>
        <mvc:mapping path="/**"/>
        <mvc:mapping path="/handleRun*"/>
        <!--设置拦截排除的路径，配置/**或/*，达到快速配置的目的-->
        <mvc:exclude-mapping path="/b*"/>
        <!--指定具体的拦截器类-->
        <bean class="MyInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```

### 9.6 多拦截器配置

![14](https://img-blog.csdnimg.cn/6282633164c146b2839d1391cbb8bb38.png)

**责任链模式**
 责任链模式是一种行为模式
 特征：
沿着一条预先设定的任务链顺序执行，每个节点具有独立的工作任务
 优势：
独立性：只关注当前节点的任务，对其他任务直接放行到下一节点
隔离性：具备链式传递特征，无需知晓整体链路结构，只需等待请求到达后进行处理即可
灵活性：可以任意修改链路结构动态新增或删减整体链路责任
解耦：将动态任务与原始任务解耦
 弊端：
链路过长时，处理效率低下
可能存在节点上的循环引用现象，造成死循环，导致系统崩溃  

## 10.异常处理

### 10.1 异常处理器

  **HandlerExceptionResolver**接口（异常处理器）  

```java
@Component
public class ExceptionResolver implements HandlerExceptionResolver {
    public ModelAndView resolveException(HttpServletRequest request,
                                         HttpServletResponse response,
                                         Object handler,
                                         Exception ex) {
        System.out.println("异常处理器正在执行中");
        ModelAndView modelAndView = new ModelAndView();
        //定义异常现象出现后，反馈给用户查看的信息
        modelAndView.addObject("msg","出错啦！ ");
        //定义异常现象出现后，反馈给用户查看的页面
        modelAndView.setViewName("error.jsp");
        return modelAndView;
    }
}
```

  根据异常的种类不同，进行分门别类的管理，返回不同的信息  

```java
public class ExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request,
                                         HttpServletResponse response,
                                         Object handler,
                                         Exception ex) {
        System.out.println("my exception is running ...."+ex);
        ModelAndView modelAndView = new ModelAndView();
        if( ex instanceof NullPointerException){
            modelAndView.addObject("msg","空指针异常");
        }else if ( ex instanceof  ArithmeticException){
            modelAndView.addObject("msg","算数运算异常");
        }else{
            modelAndView.addObject("msg","未知的异常");
        }
        modelAndView.setViewName("error.jsp");
        return modelAndView;
    }
}
```

### 10.2 注解开发异常处理器

* 使用注解实现异常分类管理
* 名称： @ControllerAdvice
* 类型： 类注解
* 位置：异常处理器类上方
* 作用：设置当前类为异常处理器类
* 范例：

```java
@Component
@ControllerAdvice
public class ExceptionAdvice {
}  
```

* 使用注解实现异常分类管理
* 名称： @ExceptionHandler
* 类型： 方法注解
* 位置：异常处理器类中针对指定异常进行处理的方法上方
* 作用：设置指定异常的处理方式
* 范例：
* 说明：处理器方法可以设定多个

 ```java
@ExceptionHandler(Exception.class)
@ResponseBody
public String doOtherException(Exception ex){
    return "出错啦，请联系管理员！ ";
}  
 ```

### 10.3 异常处理解决方案

* 异常处理方案
  * 业务异常：
    + 发送对应消息传递给用户，提醒规范操作
  * 系统异常：
    + 发送固定消息传递给用户，安抚用户
    + 发送特定消息给运维人员，提醒维护
    + 记录日志
  * 其他异常：
    * 发送固定消息传递给用户，安抚用户
    * 发送特定消息给编程人员，提醒维护
    * 纳入预期范围内
    * 记录日志  

### 10.4 自定义异常

* 异常定义格式

  ```java
  //自定义异常继承RuntimeException，覆盖父类所有的构造方法
  public class BusinessException extends RuntimeException {
      public BusinessException() {
      }
  
      public BusinessException(String message) {
          super(message);
      }
  
      public BusinessException(String message, Throwable cause) {
          super(message, cause);
      }
  
      public BusinessException(Throwable cause) {
          super(cause);
      }
  
      public BusinessException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace) {
          super(message, cause, enableSuppression, writableStackTrace);
      }
  }
  ```

* 异常触发方式

  ```java
  if(user.getName().trim().length()<4) {
      throw new BusinessException("用户名长度必须在2-4位之间，请重新输入！ ");
  }
  ```

* 通过自定义异常将所有的异常现象进行分类管理，以统一的格式对外呈现异常消息  

## 11.实用技术

### 11.1 文件上传下载

* 上传文件过程分析  

  ![15](https://img-blog.csdnimg.cn/e7eaf77b0cab434ea8a265bab38021db.png)

* MultipartResolver接口  

  *  MultipartResolver接口定义了文件上传过程中的相关操作，并对通用性操作进行了封装
  *  MultipartResolver接口底层实现类CommonsMultipartResovler
  *  CommonsMultipartResovler并未自主实现文件上传下载对应的功能，而是调用了apache的文件上传下载组件  

  ```xml
  <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.4</version>
  </dependency>
  ```

* 文件上传下载实现

  * 页面表单  

    ```html
    <form action="/fileupload" method="post" enctype="multipart/form-data">
        上传LOGO： <input type="file" name="file"/><br/>
        <input type="submit" value="上传"/>
    </form>
    ```

  * SpringMVC配置  

    ```xml
    <bean id="multipartResolver"
          class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    </bean>
    ```

  * 控制器  

    ```java
    @RequestMapping(value = "/fileupload")
    public void fileupload(MultipartFile file){
        file.transferTo(new File("file.png"));
    }
    ```

### 11.2 文件上传注意事项

1.文件命名问题， 获取上传文件名，并解析文件名与扩展名

```java
file.getOriginalFilename();
```

 2.文件名过长问题

3.文件保存路径

```java
ServletContext context = request.getServletContext();
String basePath = context.getRealPath("/uploads");
File file = new File(basePath + "/");
if(!file.exists()) file.mxdirs();
```

4.重名问题

```java
String uuid = UUID.randomUUID().toString().replace("-","").toUpperCase();
```



```java
@RequestMapping(value = "/fileupload")
//参数中定义MultipartFile参数，用于接收页面提交的type=file类型的表单，要求表单名称与参数名相同
public String fileupload(MultipartFile file,MultipartFile file1,MultipartFile file2, HttpServletRequest request) throws IOException {
    System.out.println("file upload is running ..."+file);
    //        MultipartFile参数中封装了上传的文件的相关信息
    //        System.out.println(file.getSize());
    //        System.out.println(file.getBytes().length);
    //        System.out.println(file.getContentType());
    //        System.out.println(file.getName());
    //        System.out.println(file.getOriginalFilename());
    //        System.out.println(file.isEmpty());
    //首先判断是否是空文件，也就是存储空间占用为0的文件
    if(!file.isEmpty()){
        //如果大小在范围要求内正常处理，否则抛出自定义异常告知用户（未实现）
        //获取原始上传的文件名，可以作为当前文件的真实名称保存到数据库中备用
        String fileName = file.getOriginalFilename();
        //设置保存的路径
        String realPath = request.getServletContext().getRealPath("/images");
        //保存文件的方法，指定保存的位置和文件名即可，通常文件名使用随机生成策略产生，避免文件名冲突问题
        file.transferTo(new File(realPath,file.getOriginalFilename()));
    }
    //测试一次性上传多个文件
    if(!file1.isEmpty()){
        String fileName = file1.getOriginalFilename();
        //可以根据需要，对不同种类的文件做不同的存储路径的区分，修改对应的保存位置即可
        String realPath = request.getServletContext().getRealPath("/images");
        file1.transferTo(new File(realPath,file1.getOriginalFilename()));
    }
    if(!file2.isEmpty()){
        String fileName = file2.getOriginalFilename();
        String realPath = request.getServletContext().getRealPath("/images");
        file2.transferTo(new File(realPath,file2.getOriginalFilename()));
    }
    return "page.jsp";
}
```

### 11.3 Restful风格配置

#### 11.3.1 Rest

* Rest（ REpresentational State Transfer） 一种网络资源的访问风格，定义了网络资源的访问方式
  * 传统风格访问路径
    + http://localhost/user/get?id=1
    + http://localhost/deleteUser?id=1
  * Rest风格访问路径
    + http://localhost/user/1
* Restful是按照Rest风格访问网络资源
* 优点
  +  隐藏资源的访问行为，通过地址无法得知做的是何种操作
  + 书写简化

#### 11.3.2 Rest行为约定方式  

+ GET（查询） http://localhost/user/1 GET
+ POST（保存） http://localhost/user POST
+ PUT（更新） http://localhost/user PUT
+ DELETE（删除） http://localhost/user DELETE
  **注意：**上述行为是约定方式，约定不是规范，可以打破，所以称Rest风格，而不是Rest规范  

#### 11.3.3 Restful开发入门  

```java
//设置rest风格的控制器
@RestController
//设置公共访问路径，配合下方访问路径使用
@RequestMapping("/user/")
public class UserController {

    //rest风格访问路径完整书写方式
    @RequestMapping("/user/{id}")
    //使用@PathVariable注解获取路径上配置的具名变量，该配置可以使用多次
    public String restLocation(@PathVariable Integer id){
        System.out.println("restful is running ....");
        return "success.jsp";
    }

    //rest风格访问路径简化书写方式，配合类注解@RequestMapping使用
    @RequestMapping("{id}")
    public String restLocation2(@PathVariable Integer id){
        System.out.println("restful is running ....get:"+id);
        return "success.jsp";
    }

    //接收GET请求配置方式
    @RequestMapping(value = "{id}",method = RequestMethod.GET)
    //接收GET请求简化配置方式
    @GetMapping("{id}")
    public String get(@PathVariable Integer id){
        System.out.println("restful is running ....get:"+id);
        return "success.jsp";
    }

    //接收POST请求配置方式
    @RequestMapping(value = "{id}",method = RequestMethod.POST)
    //接收POST请求简化配置方式
    @PostMapping("{id}")
    public String post(@PathVariable Integer id){
        System.out.println("restful is running ....post:"+id);
        return "success.jsp";
    }

    //接收PUT请求简化配置方式
    @RequestMapping(value = "{id}",method = RequestMethod.PUT)
    //接收PUT请求简化配置方式
    @PutMapping("{id}")
    public String put(@PathVariable Integer id){
        System.out.println("restful is running ....put:"+id);
        return "success.jsp";
    }

    //接收DELETE请求简化配置方式
    @RequestMapping(value = "{id}",method = RequestMethod.DELETE)
    //接收DELETE请求简化配置方式
    @DeleteMapping("{id}")
    public String delete(@PathVariable Integer id){
        System.out.println("restful is running ....delete:"+id);
        return "success.jsp";
    }
}
```

```xml
<!--配置拦截器，解析请求中的参数_method，否则无法发起PUT请求与DELETE请求，配合页面表单使用-->
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <servlet-name>DispatcherServlet</servlet-name>
</filter-mapping>
```

+ 开启SpringMVC对Restful风格的访问支持过滤器，即可通过页面表单提交PUT与DELETE请求
+ 页面表单使用隐藏域提交请求类型，参数名称固定为_method，必须配合提交类型method=post使用

```xml
<form action="/user/1" method="post">
    <input type="hidden" name="_method" value="PUT"/>
    <input type="submit"/>
</form>  
```

* Restful请求路径简化配置方式  

  ```java
  @RestController
  public class UserController {
      @RequestMapping(value = "/user/{id}",method = RequestMethod.DELETE)
      public String restDelete(@PathVariable String id){
          System.out.println("restful is running ....delete:"+id);
          return "success.jsp";
      }
  }  
  ```

### 11.4 postman工具安装与使用

**postman** 是  一款可以发送Restful风格请求的工具，方便开发调试。首次运行需要联网注册  

![16](https://img-blog.csdnimg.cn/642afa73aa424078a83e7d484692ef1b.png)

## 12.校验框架

### 12.1 校验框架入门

#### 12.1.1 表单校验的重要性

* 表单校验保障了数据有效性、安全性  

![17](https://img-blog.csdnimg.cn/f8be0e82e2d447e5a0dc3297b4003501.png)

数据可以随意输入，导致错误的结果。后端表单校验的重要性。

#### 12.1.2 表单校验分类  

* 校验位置：
  * 客户端校验
  * 服务端校验
* 校验内容与对应方式：
  * 格式校验
    * 客户端：使用Js技术，利用正则表达式校验
    * 服务端：使用校验框架 
  * 逻辑校验
    * 客户端：使用ajax发送要校验的数据，在服务端完成逻辑校验，返回校验结果
    * 服务端：接收到完整的请求后，在执行业务操作前，完成逻辑校验

#### 12.1.3 表单校验规则

* 长度：例如用户名长度，评论字符数量
* 非法字符：例如用户名组成
* 数据格式：例如Email格式、 IP地址格式
* 边界值：例如转账金额上限，年龄上下限
* 重复性：例如用户名是否重复

#### 12.1.4 表单校验框架

* JSR（Java Specification Requests）：Java 规范提案  

  303：提供bean属性相关校验规则  

* JSR规范列表

  * 企业应用技术
    * Contexts and Dependency Injection for Java (Web Beans 1.0) (JSR 299)
    * Dependency Injection for Java 1.0 (JSR 330)@postConstruct, @PreDestroy
    * **Bean Validation 1.0 (JSR 303)**
    * Enterprise JavaBeans 3.1 (includes Interceptors 1.1) (JSR 318)
    * Java EE Connector Architecture 1.6 (JSR 322)
    * Java Persistence 2.0 (JSR 317)
    * Common Annotations for the Java Platform 1.1 (JSR 250)
    * Java Message Service API 1.1 (JSR 914)
    * Java Transaction API (JTA) 1.1 (JSR 907)
    * JavaMail 1.4 (JSR 919)
  * Web应用技术
    + Java Servlet 3.0 (JSR 315)
    + JavaServer Faces 2.0 (JSR 314)
    + JavaServer Pages 2.2/Expression Language 2.2 (JSR 245)
    + Standard Tag Library for JavaServer Pages (JSTL) 1.2 (JSR 52)
    + Debugging Support for Other Languages 1.0 (JSR 45)
    + 模块化 (JSR 294)
    + Swing应用框架 (JSR 296)
    + avaBeans Activation Framework (JAF) 1.1 (JSR 925)
    + Streaming API for XML (StAX) 1.0 (JSR 173)
  * 管理与安全技术
    + Java Authentication Service Provider Interface for Containers (JSR 196)
    + Java Authorization Contract for Containers 1.3 (JSR 115)
    + Java EE Application Deployment 1.2 (JSR 88)
    + J2EE Management 1.1 (JSR 77)
    + Java SE中与Java EE有关的规范
    + JCache API (JSR 107)
    + Java Memory Model (JSR 133)
    + Concurrency Utilitie (JSR 166)
    + Java API for XML Processing (JAXP) 1.3 (JSR 206)
    + Java Database Connectivity 4.0 (JSR 221)
    + Java Management Extensions (JMX) 2.0 (JSR 255)
    + Java Portlet API (JSR 286)

* Web Service技术
  
  + Java Date与Time API (JSR 310)
  + Java API for RESTful Web Services (JAX-RS) 1.1 (JSR 311)
  + Implementing Enterprise Web Services 1.3 (JSR 109)
  + Java API for XML-Based Web Services (JAX-WS) 2.2 (JSR 224)
  + Java Architecture for XML Binding (JAXB) 2.2 (JSR 222)
  + Web Services Metadata for the Java Platform (JSR 181)
  + Java API for XML-Based RPC (JAX-RPC) 1.1 (JSR 101)
  + Java APIs for XML Messaging 1.3 (JSR 67)
  + Java API for XML Registries (JAXR) 1.0 (JSR 93)
  
* JCP（Java Community Process）：Java社区

* Hibernate框架中包含一套独立的校验框架hibernate-validator  

  导入坐标

  ```xml
  <dependency>
      <groupId>org.hibernate</groupId>
      <artifactId>hibernate-validator</artifactId>
      <version>6.1.0.Final</version>
  </dependency>
  ```

  **注意：**
  tomcat7 ：搭配hibernate-validator版本5.*.*.Final
  tomcat8.5↑ ：搭配hibernate-validator版本6.*.*.Final  

### 12.2 快速使用

**1. 开启校验**

+ 名称：@Valid 、 @Validated
+ 类型：形参注解
+ 位置：处理器类中的实体类类型的方法形参前方
+ 作用：设定对当前实体类类型参数进行校验
+ 范例：  

```java
@RequestMapping(value = "/addemployee")
public String addEmployee(@Valid Employee employee) {
    System.out.println(employee);
}
```

**2.设置校验规则**

+ 名称：@NotNull
+ 类型：属性注解 等
+ 位置：实体类属性上方
+ 作用：设定当前属性校验规则
+ 范例：
      每个校验规则所携带的参数不同，根据校验规则进行相应的调整
      具体的校验规则查看对应的校验框架进行获取

```java
public class Employee{
    @NotNull(message = "姓名不能为空")
    private String name;//员工姓名
}  
```

**3.获取错误信息**

```java
@RequestMapping(value = "/addemployee")
public String addEmployee(@Valid Employee employee, Errors errors, Model model){
    System.out.println(employee);
    if(errors.hasErrors()){
        for(FieldError error : errors.getFieldErrors()){
            model.addAttribute(error.getField(),error.getDefaultMessage());
        }
        return "addemployee.jsp";
    }
    return "success.jsp";
}  
```

  通过形参Errors获取校验结果数据，通过Model接口将数据封装后传递到页面显示  

```html
<form action="/addemployee" method="post">
    员工姓名：<input type="text" name="name"><span style="color:red">${name}</span><br/>
    员工年龄：<input type="text" name="age"><span style="color:red">${age}</span><br/>
    <input type="submit" value="提交">
</form>
```

通过形参Errors获取校验结果数据，通过Model接口将数据封装后传递到页面显示
页面获取后台封装的校验结果信息  

### 12.3 多规则校验

* 同一个属性可以添加多个校验器  

```java
@NotNull(message = "请输入您的年龄")
@Max(value = 60,message = "年龄最大值不允许超过60岁")
@Min(value = 18,message = "年龄最小值不允许低于18岁")
private Integer age;//员工年龄
```

* 3种判定空校验器的区别  

![22](https://img-blog.csdnimg.cn/8aba45ac0bb94072b203ff66b30b8e09.png)

### 12.4 嵌套校验

+ 名称：@Valid
+ 类型：属性注解
+ 位置：实体类中的引用类型属性上方
+ 作用：设定当前应用类型属性中的属性开启校验
+ 范例：

```java
public class Employee {
    //实体类中的引用类型通过标注@Valid注解，设定开启当前引用类型字段中的属性参与校验
    @Valid
    private Address address;
}
```

+ 注意：开启嵌套校验后，被校验对象内部需要添加对应的校验规则  

### 12.5 分组校验

* 同一个模块，根据执行的业务不同，需要校验的属性会有不同
  * 新增用户
  * 修改用户
* 对不同种类的属性进行分组，在校验时可以指定参与校验的字段所属的组类别
  * 定义组（通用）
  * 为属性设置所属组，可以设置多个
  * 开启组校验

```java
public interface GroupOne {
}
```

```java
public String addEmployee(@Validated({GroupOne.class}) Employee employee){
}  
```




```java
@NotEmpty(message = "姓名不能为空",groups = {GroupOne.class})
private String name;//员工姓名
```

## 13.SSM整合

### 13.1 整合流程简介

整合步骤分析

SSM（Spring+SpringMVC+MyBatis）

* Spring
  * 框架基础

* MyBatis
  * mysql+druid+pagehelper

* Spring整合MyBatis

* junit测试业务层接口

* SpringMVC
  * rest风格（postman测试请求结果）
  * 数据封装json（jackson）

* Spring整合SpringMVC

  * Controller调用Service

* 其他

  * 表现层数据封装

  * 自定义异常

**表结构**

![18](https://img-blog.csdnimg.cn/33aef7667e6c400b823e4f325d3ff7f1.png)

​    最重要的5个步骤

1. Spring

2. MyBatis

3. Spring整合MyBatis

4. SpringMVC

5. Spring整合SpringMVC

### 13.2 项目结构搭建

**Part0：**   项目基础结构搭建

* 创建项目，组织项目结构，创建包

* 创建表与实体类

* 创建三层架构对应的模块、接口与实体类，建立关联关系

* 数据层接口（代理自动创建实现类）
  * 业务层接口+业务层实现类
  * 表现层类



![19](https://img-blog.csdnimg.cn/0369c8a0ae934128b2a0db051b5a18ea.png)



```java
public interface UserDao {
    public boolean save(User user);  public boolean update(User user);  
    public boolean delete(Integer uuid);  public User get(Integer uuid);
    public List<User> getAll(int page,int size);

    public interface UserService {  
        public boolean save(User user);  public boolean update(User user);
        public boolean delete(Integer uuid);
        public User get(Integer uuid);
        public List<User> getAll(int page, int size);
        /**
        用户登录
        @param userName 用户名
        @param password 密码信息
        @return
        */
        public User login(String userName,String password);
    }
```

### 13.3 Spring整合Mybatis（复习）

**Part1 :**  Spring环境配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"  xmlns:context="http://www.springframework.org/schema/context"  xmlns:tx="http://www.springframework.org/schema/tx"  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xsi:schemaLocation="http://www.springframework.org/schema/beans  http://www.springframework.org/schema/beans/spring-beans.xsd  http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd  http://www.springframework.org/schema/tx  http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--开启bean注解扫描-->
    <context:component-scan base-package="com.wzk"/>

</beans>

```

**Part1 :**  Mybatis配置事务

* MyBatis映射

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="com.wzk.dao.UserDao">
  
      <!--添加-->
      <insert id="save" parameterType="user">
          insert into user(userName,password,realName,gender,birthday)values(#{userName},#{password},#{realName},#{gender},#{birthday})
      </insert>
  
      <!--删除-->
      <delete id="delete" parameterType="int">
          delete from user where uuid = #{uuid}
      </delete>
  
      <!--修改-->
      <update id="update" parameterType="user">
          update user set userName=#{userName},password=#{password},realName=#{realName},gender=#{gender},birthday=#{birthday} where uuid=#{uuid}
      </update>
  
      <!--查询单个-->
      <select id="get" resultType="user" parameterType="int">
          select * from user where uuid = #{uuid}
      </select>
  
      <!--分页查询-->
      <select id="getAll" resultType="user">
          select * from user
      </select>
  
      <!--登录-->
      <select id="getByUserNameAndPassword" resultType="user" >
          select * from user where userName=#{userName} and password=#{password}
      </select>
  
  </mapper>
  ```

  

* Mybatis核心配置

  ```xml
  <!--开启注解式事务-->
  <tx:annotation-driven transaction-manager="txManager"/>
  
  <!--加载properties文件-->
  <context:property-placeholder location="classpath*:jdbc.properties"/>
  
  <!--数据源-->
  <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
      <property name="driverClassName" value="${jdbc.driver}"/>
      <property name="url" value="${jdbc.url}"/>
      <property name="username" value="${jdbc.username}"/>
      <property name="password" value="${jdbc.password}"/>
  </bean>
  
  <!--整合mybatis到spring中-->
  <bean class="org.mybatis.spring.SqlSessionFactoryBean">
      <property name="dataSource" ref="dataSource"/>
      <property name="typeAliasesPackage" value="com.wzk.domain"/>
      <!--分页插件-->
      <property name="plugins">
          <array>
              <bean class="com.github.pagehelper.PageInterceptor">
                  <property name="properties">
                      <props>
                          <prop key="helperDialect">mysql</prop>
                          <prop key="reasonable">true</prop>
                      </props>
                  </property>
              </bean>
          </array>
      </property>
  </bean>
  
  <!--映射扫描-->
  <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
      <property name="basePackage" value="com.wzk.dao"/>
  </bean>
  
  <!--事务管理器-->
  <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
      <property name="dataSource" ref="dataSource"/>
  </bean>
  ```

  

### 13.4 整合junit

**Part2：**单元测试整合junit

```java
@RunWith(SpringJUnit4ClassRunner.class)  
@ContextConfiguration(locations = "classpath:applicationContext.xml")  
public class UserServiceTest {

    @Autowired
    private UserService userService;

    @Test
    public void testDelete(){  
        User user = new User();  userService.delete(3);
    }
}

```

### 13.5 Spring整合SpringMVC

**Part3：**SpringMVC

* web.xml配置

  ```xml
  <servlet>
      <servlet-name>DispatcherServlet</servlet-name>
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath*:spring-mvc.xml</param-value>
      </init-param>
  </servlet>
  <servlet-mapping>
      <servlet-name>DispatcherServlet</servlet-name>
      <url-pattern>/</url-pattern>
  </servlet-mapping>
  ```

* spring-mvc.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:mvc="http://www.springframework.org/schema/mvc"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
  
      <mvc:annotation-driven/>
  
      <context:component-scan base-package="com.wzk.controller"/>
  
  </beans>
  ```

* controller层

  ```java
  @RestController  
  @RequestMapping("/user")  public class UserController {
      @PostMapping
      public boolean save(User user) {  
          System.out.println("save ..." + user);  return true;
      }
      @PostMapping("/login")
      public User login(String userName,String password){  
          System.out.println("login ..." + userName + " ," +password);
          return null;
      }
  }
  ```



**Part4：**Spring整合SpringMVC

* web.xml加载Spring环境

  ```xml
  <context-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath*:applicationContext.xml</param-value>
  </context-param>
  
  <!--启动服务器时，通过监听器加载spring运行环境-->
  <listener>
      <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  ```

* Controller调用Service

  ```java
  @RestController
  @RequestMapping("/user")
  public class UserController {
      @Autowired
      private UserService userService;
  
  
      @PostMapping
      public boolean save(User user){
          return userService.save(user);
      }
  }
  ```

### 13.6 表现层数据封装

**Part5-1：**表现层数据封装

* 前端接收表现层返回的数据种类

| u操作是否成功 | true/false | 格式A |
| ------------- | ---------- | ----- |
| u单个数据     | 1,100,true | 格式B |
| u对象数据     | json对象   | 格式C |
| u集合数据     | json数组   | 格式D |

![20](https://img-blog.csdnimg.cn/4413e69338a749e09c828f235e2f70eb.png)



* 返回数据格式设计

![21](https://img-blog.csdnimg.cn/16c42cbcaac843ff9948ecc3483b9db2.png)

* 代码

  ```java
  public class Result {
      // 操作结果编码
      private Integer code;
      // 操作数据结果
      private Object data;
      // 消息
      private String message;
      public Result(Integer code) {
          this.code = code;
      }
      public Result(Integer code, Object data) {
          this.code = code;
          this.data = data;
      }
  }
  ```

  状态码常量可以根据自己的业务需求设定

  ```java
  public class Code {
      public static final Integer SAVE_OK = 20011;
      public static final Integer SAVE_ERROR = 20010;
      //其他编码
  }
  ```

  controller 调用

  ```java
  @RestController
  public class UserController {
      @Autowired
      private UserService userService;
      @PostMapping
      public Result save(User user){
          boolean flag = userService.save(user);
          return new Result(flag ? Code.SAVE_OK:Code.SAVE_ERROR);
      }
      @GetMapping("/{uuid}")
      public Result get(@PathVariable Integer uuid){
          User user = userService.get(uuid);
          return new Result(null != user ?Code.GET_OK: Code.GET_ERROR,user);
      }
  }
  ```

  

### 13.7 自定义异常

**Part5-2：**自定义异常

* 设定自定义异常，封装程序执行过程中出现的问题，便于表现层进行统一的异常拦截并进行处理
  * BusinessException
  * SystemException

* 自定义异常消息返回时需要与业务正常执行的消息按照统一的格式进行处理



**定义BusinessException**

```java
public class BusinessException extends RuntimeException {
    //自定义异常中封装对应的错误编码，用于异常处理时获取对应的操作编码
    private Integer code;

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public BusinessException(Integer code) {
        this.code = code;
    }

    public BusinessException(String message, Integer code) {
        super(message);
        this.code = code;
    }

    public BusinessException(String message, Throwable cause,Integer code) {
        super(message, cause);
        this.code = code;
    }

    public BusinessException(Throwable cause,Integer code) {
        super(cause);
        this.code = code;
    }

    public BusinessException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace,Integer code) {
        super(message, cause, enableSuppression, writableStackTrace);
        this.code = code;
    }
}

```

```java
@GetMapping("/{uuid}")
public Result get(@PathVariable Integer uuid){
    User user = userService.get(uuid);
    //模拟出现异常，使用条件控制，便于测试结果
    if (uuid == 10 ) throw new BusinessException("查询出错啦，请重试！",Code.GET_ERROR);
    return new Result(null != user ?Code.GET_OK: Code.GET_ERROR,user);
}
```

### **13.8 返回消息兼容异常信息**

```java
@Component
@ControllerAdvice
public class ProjectExceptionAdivce {
    @ExceptionHandler(BusinessException.class)
    @ResponseBody
    //对出现异常的情况进行拦截，并将其处理成统一的页面数据结果格式
    public Result doBusinessException(BusinessException e){
        return new Result(e.getCode(),e.getMessage());
    }
}
```

## 14.纯注解开发SSM

### 14.1 用注解替代applicationContext.xml

同前期设置，添加事务注解驱动
@Configuration

```java
//扫描组件，排除SpringMVC对应的bean，等同于<context:component-scan />
@ComponentScan(value = "com.wzk",excludeFilters = {
    @ComponentScan.Filter(type= FilterType.ANNOTATION,classes = {Controller.class})})
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MyBatisConfig.class})
//等同于<tx:annotation-driven transaction-manager="txManager"/>，导入的默认名称为transactionManager
@EnableTransactionManagement
public class SpringConfig {
    //等同于<bean   class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    @Bean("transactionManager")
    public DataSourceTransactionManager getDataSourceTxManager(@Autowired DataSource dataSource){
        DataSourceTransactionManager dtm = new DataSourceTransactionManager();
        //等同于<property name="dataSource" ref="dataSource"/>
        dtm.setDataSource(dataSource);
        return dtm;
    }
}  
```

### 14.2 用注解替代spring-mvc.xml  

* 同前期设置，添加@EnableWebMvc注解  

  ```java
  @Configuration
  @ComponentScan("com.wzk.controller")
  @EnableWebMvc
  public class SpringMvcConfig implements WebMvcConfigurer {
  }
  ```

* EnableWebMvc  


1. 支持ConversionService的配置，可以方便配置自定义类型转换器
2. 支持@NumberFormat注解格式化数字类型
3. 支持@DateTimeFormat注解格式化日期数据，日期包括Date,Calendar,JodaTime（JodaTime要导包）
4. 支持@Valid的参数校验(需要导入JSR-303规范)
5. 配合第三方jar包和SpringMVC提供的注解读写XML和JSON格式数据  