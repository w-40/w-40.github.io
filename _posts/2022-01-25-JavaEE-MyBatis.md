---
layout: post
#标题配置
title: JavaEE-MyBatis框架
#时间配置
date:   2022-01-25 25:46:00 +0800
#目录配置
categories: JavaEE框架
#标签配置
tag: 学习笔记
---

* content
{:toc}


# MyBatis

## 一.框架介绍

+ 框架是一款半成品软件，我们可以基于这个半成品软件继续开发，来完成我们个性化的需求!

## 二.ORM介绍

+ ORM(Object Relational Mapping):对象关系映射
+ 指的是持久化数据和实体对象的映射模式，为了解决面向对象与关系型数据库存在的互不匹配的现象的技术。

+ 映射规则
  + 数据表->类
  + 表字段->类属性
  + 表数据->对象

## 三.MyBatis介绍

+ 原始JDBC的操作问题分析

  + 1.频繁创建和销毁数据库的连接会造成系统资源浪费从而影响系统性能。

  + 2.sql语句在代码中硬编码，如果要修改sql语句，就需要修改java代码，造成代码不易维护。
  + 3.查询操作时，需要手动将结果集中的数据封装到实体对象中。
  + 4.增删改查操作需要参数时，需要手动将实体对象的数据设置到sql语句的占位符。

+ 原始JDBC的操作问题解决方案
  + 1.使用数据库连接池初始化连接资源。
  + 2.将sql语句抽取到配置文件中。
  + 3.使用反射、内省等底层技术，将实体与表进行属性与字段的自动映射。

+ MyBatis介绍
  + MyBatis是一个优秀的基于Java的持久层框架，它内部封装了JDBC，使开发者只需要关注SQL语句本身，而不需要花费精力去处理加载驱动、创建连接、创建执行者等复杂的操作。
  + MyBatis通过xml或注解的方式将要执行的各种Statement配置起来，并通过Java对象和Statement中SQL的动态参数进行映射生成最终要执行的SQL语句。
  + 最后MyBatis框架执行完SQL并将结果映射为Java对象并返回。采用ORM思想解决了实体和数据库映射的问题，对JDBC进行了封装，屏蔽了JDBC API底层访问细节，使我们不用与JDBC API打交道，就可以完成对数据库的持久化操作。

## 四.MyBatis入门程序

+ 1.数据准备。
+ 2.导入jar包。
+ 3.在src下创建映射配置文件。
+ 4.在src下创建核心配置文件。
+ 5.编写测试类完成相关API的使用。

## 五.相关API

### 1.Resources介绍

+ org.apache.ibatis.io.Resources :加载资源的工具类。
+ 核心方法

| 返回值      | 方法名                               | 说明                                 |
| ----------- | ------------------------------------ | ------------------------------------ |
| lnputStream | getResourceAsStream(String fileName) | 通过类加载器返回指定资源的字节输入流 |

### 2.SqlSessionFactoryBuilder

+ org.apache.ibatis.session.SqlSessionFactoryBuilder:获取SqlSessionFactory工厂对象的功能类。

+ 核心方法

| 返回值            | 方法名                | 说明                                         |
| ----------------- | --------------------- | -------------------------------------------- |
| SqlSessionFactory | build(InputStream is) | 通过指定资源字节输入流获取SqlSession工厂对象 |

### 3.SqlSessionFactory

+ org.apache.ibatis.session.SqlSessionFactory:获取SqlSession构建者对象的工厂接口。
+ 核心方法

| 返回值     | 方法名                          | 说明                                                     |
| ---------- | ------------------------------- | -------------------------------------------------------- |
| SqlSession | openSession()                   | 获取SqlSession构建者对象，并开启手动提交事务获取         |
| SqlSession | openSession(boolean autoCommit) | SqlSession构建者对象，如果参数为true，则开启自动提交事务 |

### 4.SqlSession

+ org.apache.ibatis.session.SqlSession：构建者对象接口。用于执行 SQL、管理事务、接口代理。+
+ 核心方法

| 返回值  | 方法名                                       | 说明                           |
| ------- | -------------------------------------------- | ------------------------------ |
| List<E> | selectList(String statement,Object paramter) | 执行查询语句，返回List集合     |
| T       | selectOne(String statement,Object paramter)  | 执行查询语句，返回一个结果对象 |
| int     | insert(String statement,Object paramter)     | 执行新增语句，返回影响行数     |
| int     | update(String statement,Object paramter)     | 执行修改语句，返回影响行数     |
| int     | delete(String statement,Object paramter)     | 执行删除语句，返回影响行数     |
| void    | commit()                                     | 提交事务                       |
| void    | rollback()                                   | 回滚事务                       |
| T       | getMapper(Class<T> cls)                      | 获取指定接口的代理实现类对象   |
| void    | close()                                      | 释放资源                       |

## 六.映射配置文件

### 1.映射配置文件介绍

+ 映射配置文件包含了数据和对象之间的映射关系以及要执行的 SQL 语句

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis的DTD约束-->
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--
    mapper：核心根标签
    namespace属性：名称空间
-->
<mapper namespace="StudentMapper">
    <!--
       select：查询功能的标签
       id属性：代表唯一标识
       resultType属性：指定结果映射对象类型
       parameterType属性：指定参数映射对象类型（SQL语句有参数才需要）
     -->
    <select id="selectAll" resultType="com.wzk.bean.Student">
        SELECT * FROM student
    </select>
</mapper>
```

### 2.查询功能

+ <select>：查询功能标签。

+ 属性        

  id：唯一标识， 配合名称空间使用。     

  parameterType：指定参数映射的对象类型。       

  resultType：指定结果映射的对象类型。

+ SQL 获取参数:        #{属性名}

+ 例子

```xml
<select id="selectById" resultType="com.wzk.bean.Student" parameterType="java.lang.Integer">
    SELECT * FROM student WHERE id = #{id}
</select>
```

### 3.新增功能

- <insert>：新增功能标签。

- 属性        

  id：唯一标识， 配合名称空间使用。     

  parameterType：指定参数映射的对象类型。       

  resultType：指定结果映射的对象类型。

- SQL 获取参数:        #{属性名}

+ 例子

```xml
<insert id="insert" parameterType="com.wzk.bean.Student">
    INSERT INTO student VALUES (#{id},#{name},#{age})
</insert>
```

### 4.修改功能

- <update>：修改功能标签。

- 属性        

  id：唯一标识， 配合名称空间使用。     

  parameterType：指定参数映射的对象类型。       

  resultType：指定结果映射的对象类型。

- SQL 获取参数:        #{属性名}

+ 例子

```xml
<update id="update" parameterType="com.wzk.bean.Student">
    UPDATE student SET name = #{name},age = #{age} WHERE id = #{id}
</update>
```

### 5.删除功能

- <delete>：查询功能标签。

- 属性        

  id：唯一标识， 配合名称空间使用。     

  parameterType：指定参数映射的对象类型。       

  resultType：指定结果映射的对象类型。

- SQL 获取参数:        #{属性名}

- 例子

```xml
<delete id="delete" parameterType="java.lang.Integer">
    DELETE FROM student WHERE id = #{id}
</delete>
```

## 七.核心配置文件

### 1.核心配置文件介绍

核心配置文件包含了 MyBatis 最核心的设置和属性信息。如数据库的连接、事务、连接池信息等。

如下图:

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis的DTD约束-->
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--configuration 核心根标签-->
<configuration>

    <!--environments配置数据库环境，环境可以有多个。default属性指定使用的是哪个-->
    <environments default="mysql">
        <!--environment配置数据库环境  id属性唯一标识-->
        <environment id="mysql">
            <!-- transactionManager事务管理。  type属性，采用JDBC默认的事务-->
            <transactionManager type="JDBC"></transactionManager>
            <!-- dataSource数据源信息   type属性 连接池-->
            <dataSource type="POOLED">
                <!-- property获取数据库连接的配置信息 -->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>

    <!-- mappers引入映射配置文件 -->
    <mappers>
        <!-- mapper 引入指定的映射配置文件   resource属性指定映射配置文件的名称 -->
        <mapper resource="StudentMapper.xml"/>
    </mappers>
</configuration>
~~~

### 2.数据库连接配置文件引入

* properties标签引入外部文件

  ~~~xml
      <!--引入数据库连接的配置文件-->
      <properties resource="jdbc.properties"/>
  ~~~

* 具体使用，如下配置

  ~~~xml
    <!-- property获取数据库连接的配置信息 -->
      <property name="driver" value="${driver}" />
      <property name="url" value="${url}" />
      <property name="username" value="${username}" />
      <property name="password" value="${password}" />
  ~~~

  

### 3.起别名

* <typeAliases>：为全类名起别名的父标签。

* <typeAlias>：为全类名起别名的子标签。

* 属性      

  type：指定全类名      

  alias：指定别名

* <package>：为指定包下所有类起别名的子标签。(别名就是类名)

* 如下图

| 别名    | 数据类型          |
| ------- | ----------------- |
| string  | java.lang.String  |
| long    | java.lang.Long    |
| int     | java.lang.Integer |
| double  | java.lang.Double  |
| boolean | java.lang.Boolean |
| ...     | ...               |



* 具体如下配置

  ~~~xml
      <!--起别名-->
      <typeAliases>
          <typeAlias type="com.itheima.bean.Student" alias="student"/>
          <!--<package name="com.itheima.bean"/>-->
      </typeAliase
  ~~~

### 4.核心配置文件小结

+ 核心配置文件包含了MyBatis最核心的设置和属性信息。如数据库的连接、事务
  连接池信息等。
+ <configuration>︰核心根标签。
+ <properties> :引入数据库连接信息配置文件标签。
+ <typeAliases>:起别名标签。
+ <environments>:配置数据库环境标签。
+ <environment>:配置数据库信息标签。
+ <transactionManager>:事务管理标签。
+ <dataSource>∶数据源标签。
+ <property>:数据库连接信息标签。
+ <mappers>:引入映射配置文件标签。

## 八.Mybatis传统方式开发

### 1.Dao 层传统实现方式

+ 分层思想：控制层(controller)、业务层(service)、持久层(dao)。
+ 调用流程

​		控制层--->业务层--->持久层--->DB

### 2.LOG4J的配置和使用

* 在日常开发过程中，排查问题时难免需要输出 MyBatis 真正执行的 SQL 语句、参数、结果等信息，我们就可以借助 LOG4J 的功能来实现执行信息的输出。

+ 使用步骤：
  + 1.导入jar包
  + 2.修改核心配置文件
  
  ```xml
  <!--配置LOG4J-->
      <settings>
          <setting name="logImpl" value="log4j"/>
      </settings>
  ```
  
  + 3.在src下编写LOG4J配置文件
  
  ```properties
  # Global logging configuration
  # ERROR WARN INFO DEBUG
  log4j.rootLogger=DEBUG, stdout
  # Console output...
  log4j.appender.stdout=org.apache.log4j.ConsoleAppender
  log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
  log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
  ```
  
  

## 九.MyBatis接口代理方式实现dao层

### 1.接口代理方式-实现规则

+ 传统方式实现Dao层，我们既要写接口，还要写实现类。而MyBatis框架可以帮助我们省略编写Dao层接口实现类的步骤。程序员只需要编写接口，由MyBatis框架根据接口的定义来创建该接口的动态代理对象。

+ 实现规则
  + 1.映射配置文件中的名称空间必须和Dao层接口的全类名（包括路径）相同。如：com.wzk.mapper.StudentMapper
  + 2.映射配置文件中的增删改查标签的id属性必须和Dao层接口的方法名相同。
  + 3.映射配置文件中的增删改查标签的parameterType属性必须和Dao层接口方法的参数相同。
  + 4.映射配置文件中的增删改查标签的resultType属性必须和Dao层接口方法的返回值相同。

### 2.接口代理方式-代码实现

+ 1.删除 mapper层接口的实现类。
+ 2.修改映射配置文件。
+ 3.修改service层接口的实现类，采用接口代理方式实现功能。

### 3.测试代理方式

```java
 public Student selectById(Integer id) {
        Student stu = null;
        SqlSession sqlSession = null;
        InputStream is = null;
        try{
            //1.加载核心配置文件
            is = Resources.getResourceAsStream("MyBatisConfig.xml");

            //2.获取SqlSession工厂对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

            //3.通过工厂对象获取SqlSession对象
            sqlSession = sqlSessionFactory.openSession(true);

            //4.获取StudentMapper接口的实现类对象
            StudentMapper mapper = sqlSession.getMapper(StudentMapper.class); // StudentMapper mapper = new StudentMapperImpl();

            //5.通过实现类对象调用方法，接收结果
            stu = mapper.selectById(id);

        } catch (Exception e) {

        } finally {
            //6.释放资源
            if(sqlSession != null) {
                sqlSession.close();
            }
            if(is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        //7.返回结果
        return stu;
    }
```

### 4.源码分析

* 分析动态代理对象如何生成的？ 

  通过动态代理开发模式，我们只编写一个接口，不写实现类，我们通过 getMapper() 方法最终获取到 org.apache.ibatis.binding.MapperProxy 代理对象，然后执行功能，而这个代理对象正是 MyBatis 使用了 JDK 的动态代理技术，帮助我们生成了代理实现类对象。从而可以进行相关持久化操作。 

* 分析方法是如何执行的？

  动态代理实现类对象在执行方法的时候最终调用了 mapperMethod.execute() 方法，这个方法中通过 switch 语句根据操作类型来判断是新增、修改、删除、查询操作，最后一步回到了 MyBatis 最原生的 SqlSession 方式来执行增删改查。    

### 5.知识小结

+ 接口代理方式可以让我们只编写接口即可，而实现类对象由 MyBatis 生成。 

+ 实现规则 ：
  + 1.映射配置文件中的名称空间必须和 Dao 层接口的全类名相同。
  + 2.映射配置文件中的增删改查标签的 id 属性必须和 Dao 层接口的方法名相同。 
  + 3.映射配置文件中的增删改查标签的 parameterType 属性必须和 Dao 层接口方法的参数相同。 
  + 4.映射配置文件中的增删改查标签的 resultType 属性必须和 Dao 层接口方法的返回值相同。 
+ 获取动态代理对象
  + SqlSession 功能类中的 getMapper() 方法。   

## 十.MyBatis映射配置文件-动态SQL

### 1.动态SQL介绍

+ Mybatis 的映射文件中，前面我们的 SQL 都是比较简单的，有些时候业务逻辑复杂时，我们的 SQL是动态变化的，此时在前面的学习中我们的 SQL 就不能满足要求了。
+ 多条件查询
+ 动态SQL标签
  + <if>：条件判断标签
  + <foreach>：循环遍历标签

### 2.<if>标签（if）

+ <where>：条件标签。如果有动态条件，则使用该标签代替where关键字
+ <if>：条件判断标签

```xml
<if test="条件判断">
	查询条件拼接
</if>
```

### 3.<foreach>标签（foreach）

+ <foreach>：循环遍历标签。适用于多个参数或者的关系

```xml
<foreach collection="" open="" close="" item="" separator="">
	获取参数
</foreach>
```

+ 属性
  + collection：参数容器类型， (list-集合， array-数组)。
  + open：开始的 SQL 语句。
  + close：结束的 SQL 语句。
  + item：参数变量名。
  + separator：分隔符。

### 4.SQL片段抽取

+ 我们可以将一些重复性的 SQL 语句进行抽取，以达到复用的效果。 

- <sql>：抽取 SQL 语句标签。

```xml
<sql id="片段唯一标识">抽取的 SQL 语句</sql>
```

- <include>：引入 SQL 片段标签。 

```xml
<include refid=“片段唯一标识”/>
```

+ Sql 中可将重复的 sql 提取出来，使用时用 include 引用即可，最终达到 sql 重用的目的

```xml
<!--抽取sql片段简化编写-->
<sql id="select" SELECT * FROM student</sql>
<select id="findById" parameterType="int" resultType="student">
    <include refid="selectStudent"></include> where id=#{id}
</select>
<select id="findByIds" parameterType="list" resultType="student">
    <include refid="select"></include>
    <where>
        <foreach collection="array" open="id in(" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
</select>
```



### 5.知识小结

MyBatis映射文件配置：

~~~xml
<select>：查询

<insert>：插入

<update>：修改

<delete>：删除

<where>：where条件

<if>：if判断

<foreach>：循环

<sql>：sql片段抽取

~~~

## 十一.MyBatis核心配置文件-分页插件

### 1.分页插件介绍

+ 在企业级开发中，分页也是一种常见的技术。而目前使用的 MyBatis 是不带分页功能的，如果想实现分页的 功能，需要我们手动编写 LIMIT 语句。但是不同的数据库实现分页的 SQL 语句也是不同的，所以手写分页 成本较高。这个时候就可以借助分页插件来帮助我们实现分页功能。 
+ PageHelper：第三方分页助手。将复杂的分页操作进行封装，从而让分页功能变得非常简单。  

### 2.分页插件的使用

+ ①导入PageHelper的jar包

+ ②在mybatis核心配置文件中配置PageHelper插件

  ~~~xml
  <!-- 注意：分页助手的插件  配置在通用mapper之前 -->
  <plugin interceptor="com.github.pagehelper.PageHelper">
      <!-- 指定方言 -->
      <property name="dialect" value="mysql"/>
  </plugin>
  ~~~

  ③测试分页数据获取

  ~~~java
  @Test
  public void testPageHelper(){
      //设置分页参数
      PageHelper.startPage(1,2);
  
      List<User> select = userMapper2.select(null);
      for(User user : select){
          System.out.println(user);
      }
  }
  ~~~


### 3.分页插件相关参数

+ Pagelnfo :封装分页相关参数的功能类。
+ 核心方法

| 返回值  | 方法名          | 说明               |
| ------- | --------------- | ------------------ |
| long    | getTotal()      | 获取总条数         |
| int     | getPages()      | 获取总页数         |
| int     | getPageNum()    | 获取当前页         |
| int     | getPageSize()   | 获取每页显示条数   |
| int     | getPrePage()    | 获取上一页         |
| int     | getNextPage()   | 获取下一页         |
| boolean | islsFirstPage() | 获取是否是第一页   |
| boolean | islsLastPage()  | 获取是否是最后一页 |

### 4.分页插件知识小结

​    分页：可以将很多条结果进行分页显示。 

* 分页插件 jar 包： pagehelper-5.1.10.jar jsqlparser-3.1.jar 

* <plugins>：集成插件标签。 

* 分页助手相关 API 

  ​    1.PageHelper：分页助手功能类。

  2. startPage()：设置分页参数 
  3. PageInfo：分页相关参数功能类。 
  4. getTotal()：获取总条数 
  5. getPages()：获取总页数
  6. getPageNum()：获取当前页
  7. getPageSize()：获取每页显示条数
  8. getPrePage()：获取上一页 
  9. getNextPage()：获取下一页 
  10. isIsFirstPage()：获取是否是第一页 
  11. isIsLastPage()：获取是否是最后一页  

## 十二.MyBatis多表操作

### 1.多表模型

+ 我们之前学习的都是基于单表操作的，而实际开发中，随着业务难度的加深，肯定需要多表操作的。 
+ 多表模型分类
  + 一对一：在任意一方建立外键，关联对方的主键。
  + 一对多：在多的一方建立外键，关联一的一方的主键。
  + 多对多：借助中间表，中间表至少两个字段，分别关联两张表的主键。    

### 2.多表模型一对一操作

+ 1.一对一模型： 人和身份证，一个人只有一个身份证

+ 2.代码实现

  * 步骤一: sql语句准备

    ~~~sql
    CREATE TABLE person(
    	id INT PRIMARY KEY AUTO_INCREMENT,
    	NAME VARCHAR(20),
    	age INT
    );
    INSERT INTO person VALUES (NULL,'张三',23);
    INSERT INTO person VALUES (NULL,'李四',24);
    INSERT INTO person VALUES (NULL,'王五',25);
    
    CREATE TABLE card(
    	id INT PRIMARY KEY AUTO_INCREMENT,
    	number VARCHAR(30),
    	pid INT,
    	CONSTRAINT cp_fk FOREIGN KEY (pid) REFERENCES person(id)
    );
    INSERT INTO card VALUES (NULL,'12345',1);
    INSERT INTO card VALUES (NULL,'23456',2);
    INSERT INTO card VALUES (NULL,'34567',3);
    ~~~

  * 步骤二:配置文件

    ~~~xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    
    <mapper namespace="com.wzk.table01.OneToOneMapper">
        <!--配置字段和实体对象属性的映射关系-->
        <resultMap id="oneToOne" type="card">
            <!--id标签是为主键属性配置，其余属性用result标签-->
            <id column="cid" property="id" />
            <result column="number" property="number" />
            <!--
                association：配置被包含对象的映射关系
                property：被包含对象的变量名
                javaType：被包含对象的数据类型
            -->
            <association property="p" javaType="person">
                <id column="pid" property="id" />
                <result column="name" property="name" />
                <result column="age" property="age" />
            </association>
        </resultMap>
    
        <select id="selectAll" resultMap="oneToOne">
            SELECT c.id cid,number,pid,NAME,age FROM card c,person p WHERE c.pid=p.id
        </select>
    </mapper>
    ~~~
  
  * 步骤三：测试类
  
    ~~~java
    @Test
        public void selectAll() throws IOException {
            InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
    
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    
            SqlSession sqlSession = sqlSessionFactory.openSession(true);
    
            OneToOneMapper mapper = sqlSession.getMapper(OneToOneMapper.class);
    
    
            List<Card> list = mapper.selectAll();
    
            for (Card c : list) {
                System.out.println(c);
            }
    
            sqlSession.close();
            is.close();
        }
    ~~~
  
  3.一对一配置总结:
  
  ~~~xml-dtd
  <resultMap>：配置字段和对象属性的映射关系标签。
      id 属性：唯一标识
      type 属性：实体对象类型
  <id>：配置主键映射关系标签。
  <result>：配置非主键映射关系标签。
      column 属性：表中字段名称
      property 属性： 实体对象变量名称
  <association>：配置被包含对象的映射关系标签。
      property 属性：被包含对象的变量名
      javaType 属性：被包含对象的数据类型
  ~~~

### 3.多表模型一对多操作

  1. 一对多模型： 一对多模型：班级和学生，一个班级可以有多个学生。    

  2. 代码实现

     - 步骤一: sql语句准备

       ```sql
       CREATE TABLE classes(
       	id INT PRIMARY KEY AUTO_INCREMENT,
       	NAME VARCHAR(20)
       );
       INSERT INTO classes VALUES (NULL,'一班');
       INSERT INTO classes VALUES (NULL,'二班');
       
       
       CREATE TABLE student(
       	id INT PRIMARY KEY AUTO_INCREMENT,
       	NAME VARCHAR(30),
       	age INT,
       	cid INT,
       	CONSTRAINT cs_fk FOREIGN KEY (cid) REFERENCES classes(id)
       );
       INSERT INTO student VALUES (NULL,'张三',23,1);
       INSERT INTO student VALUES (NULL,'李四',24,1);
       INSERT INTO student VALUES (NULL,'王五',25,2);
       INSERT INTO student VALUES (NULL,'赵六',26,2);
       ```

     - 步骤二:配置文件

       ```xml
       <mapper namespace="com.itheima.table02.OneToManyMapper">
           <resultMap id="oneToMany" type="classes">
               <id column="cid" property="id"/>
               <result column="cname" property="name"/>
       
               <!--
                   collection：配置被包含的集合对象映射关系
                   property：被包含对象的变量名
                   ofType：被包含对象的实际数据类型
               -->
               <collection property="students" ofType="student">
                   <id column="sid" property="id"/>
                   <result column="sname" property="name"/>
                   <result column="sage" property="age"/>
               </collection>
           </resultMap>
           <select id="selectAll" resultMap="oneToMany">
               SELECT c.id cid,c.name cname,s.id sid,s.name sname,s.age sage FROM classes c,student s WHERE c.id=s.cid
           </select>
       </mapper>
       ```

     - 步骤三：测试类

       ```java
           @Test
           public void selectAll() throws Exception{
               //1.加载核心配置文件
               InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
       
               //2.获取SqlSession工厂对象
               SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
       
               //3.通过工厂对象获取SqlSession对象
               SqlSession sqlSession = sqlSessionFactory.openSession(true);
       
               //4.获取OneToManyMapper接口的实现类对象
               OneToManyMapper mapper = sqlSession.getMapper(OneToManyMapper.class);
       
               //5.调用实现类的方法，接收结果
               List<Classes> classes = mapper.selectAll();
       
               //6.处理结果
               for (Classes cls : classes) {
                   System.out.println(cls.getId() + "," + cls.getName());
                   List<Student> students = cls.getStudents();
                   for (Student student : students) {
                       System.out.println("\t" + student);
                   }
               }
       
               //7.释放资源
               sqlSession.close();
               is.close();
           }
       ```

     3.一对多配置文件总结：

     ~~~xml-dtd
     <resultMap>：配置字段和对象属性的映射关系标签。
         id 属性：唯一标识
         type 属性：实体对象类型
     <id>：配置主键映射关系标签。
     <result>：配置非主键映射关系标签。
         column 属性：表中字段名称
         property 属性： 实体对象变量名称
     <collection>：配置被包含集合对象的映射关系标签。
         property 属性：被包含集合对象的变量名
         ofType 属性：集合中保存的对象数据类型
     ~~~


### 4.多表模型多对多操作

  1. 多对多模型：学生和课程，一个学生可以选择多门课程、一个课程也可以被多个学生所选择。       

  2. 代码实现

     - 步骤一: sql语句准备

       ```sql
       CREATE TABLE course(
       	id INT PRIMARY KEY AUTO_INCREMENT,
       	NAME VARCHAR(20)
       );
       INSERT INTO course VALUES (NULL,'语文');
       INSERT INTO course VALUES (NULL,'数学');
       
       
       CREATE TABLE stu_cr(
       	id INT PRIMARY KEY AUTO_INCREMENT,
       	sid INT,
       	cid INT,
       	CONSTRAINT sc_fk1 FOREIGN KEY (sid) REFERENCES student(id),
       	CONSTRAINT sc_fk2 FOREIGN KEY (cid) REFERENCES course(id)
       );
       INSERT INTO stu_cr VALUES (NULL,1,1);
       INSERT INTO stu_cr VALUES (NULL,1,2);
       INSERT INTO stu_cr VALUES (NULL,2,1);
       INSERT INTO stu_cr VALUES (NULL,2,2);
       ```

     - 步骤二:配置文件

       ```xml
       <?xml version="1.0" encoding="UTF-8" ?>
       <!DOCTYPE mapper
               PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
               "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
       
       <mapper namespace="com.itheima.table03.ManyToManyMapper">
           <resultMap id="manyToMany" type="student">
               <id column="sid" property="id"/>
               <result column="sname" property="name"/>
               <result column="sage" property="age"/>
       
               <collection property="courses" ofType="course">
                   <id column="cid" property="id"/>
                   <result column="cname" property="name"/>
               </collection>
           </resultMap>
           <select id="selectAll" resultMap="manyToMany">
               SELECT sc.sid,s.name sname,s.age sage,sc.cid,c.name cname FROM student s,course c,stu_cr sc WHERE sc.sid=s.id AND sc.cid=c.id
           </select>
       </mapper>
       ```

     - 步骤三：测试类

       ```java
        @Test
           public void selectAll() throws Exception{
               //1.加载核心配置文件
               InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
       
               //2.获取SqlSession工厂对象
               SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
       
               //3.通过工厂对象获取SqlSession对象
               SqlSession sqlSession = sqlSessionFactory.openSession(true);
       
               //4.获取ManyToManyMapper接口的实现类对象
               ManyToManyMapper mapper = sqlSession.getMapper(ManyToManyMapper.class);
       
               //5.调用实现类的方法，接收结果
               List<Student> students = mapper.selectAll();
       
               //6.处理结果
               for (Student student : students) {
                   System.out.println(student.getId() + "," + student.getName() + "," + student.getAge());
                   List<Course> courses = student.getCourses();
                   for (Course cours : courses) {
                       System.out.println("\t" + cours);
                   }
               }
       
               //7.释放资源
               sqlSession.close();
               is.close();
           }
           
       ```

     3.多对多配置文件总结：

     ```xml-dtd
     <resultMap>：配置字段和对象属性的映射关系标签。
     	id 属性：唯一标识
     	type 属性：实体对象类型
      <id>：配置主键映射关系标签。
      <result>：配置非主键映射关系标签。
     	column 属性：表中字段名称
     	property 属性： 实体对象变量名称
     <collection>：配置被包含集合对象的映射关系标签。
     	property 属性：被包含集合对象的变量名
     	ofType 属性：集合中保存的对象数据类型
     ```

      

### 5.多表模型操作总结

  ~~~xml-dtd
  <resultMap>：配置字段和对象属性的映射关系标签。
      id 属性：唯一标识
      type 属性：实体对象类型
  <id>：配置主键映射关系标签。
  <result>：配置非主键映射关系标签。
  	column 属性：表中字段名称
  	property 属性： 实体对象变量名称
  <association>：配置被包含对象的映射关系标签。
  	property 属性：被包含对象的变量名
  	javaType 属性：被包含对象的数据类型
  <collection>：配置被包含集合对象的映射关系标签。
  	property 属性：被包含集合对象的变量名
  	ofType 属性：集合中保存的对象数据类型
  ~~~

## 十三.MyBatis注解开发

### 1.常用注解介绍

+ 我们除了可以使用映射配置文件来操作以外，还可以使用注解形式来操作。
+  常用注解 
  + @Select(“查询的 SQL 语句”)：执行查询操作注解 
  + @Insert(“新增的 SQL 语句”)：执行新增操作注解 
  + @Update(“修改的 SQL 语句”)：执行修改操作注解 
  + @Delete(“删除的 SQL 语句”)：执行删除操作注解

### 2.注解实现查询操作

+ 创建接口和查询方法
+ 在核心配置文件中配置映射关系
+ 编写测试类

### 3.注解实现新增操作

+ 创建新增方法
+ 编写测试类

### 4.注解实现修改操作

+ 创建修改方法
+ 编写测试类

### 5.注解实现删除操作

+ 创建删除方法
+ 编写测试类

### 6.注解开发总结

注解可以简化开发操作，省略映射配置文件的编写。 

* 常用注解 

  ```java
  @Select(“查询的 SQL 语句”)：执行查询操作注解
  
  @Insert(“查询的 SQL 语句”)：执行新增操作注解
  
  @Update(“查询的 SQL 语句”)：执行修改操作注解
  
  @Delete(“查询的 SQL 语句”)：执行删除操作注解 
  ```

* 配置映射关系 

  ~~~xml
  <mappers>
      <package name="接口所在包"/>
  </mappers>    
  ~~~
  

## 十四.注解多表操作

### 1.一对一查询

+ 一对一查询的需求：查询一个用户信息，与此同时查询出该用户对应的身份证信息

+ 对应的sql语句：

```sql
SELECT * FROM card；

SELECT * FROM person WHERE id=#{id};
```

#### 1.1创建PersonMapper接口

```java
public interface PersonMapper {
    //根据id查询
    @Select("SELECT * FROM person WHERE id=#{id}")
    public abstract Person selectById(Integer id);
}

```

#### 1.2使用注解配置Mapper

```java
public interface CardMapper {
    //查询全部
    @Select("SELECT * FROM card")
    @Results({
            @Result(column = "id",property = "id"),
            @Result(column = "number",property = "number"),
            @Result(
                    property = "p",             // 被包含对象的变量名
                    javaType = Person.class,    // 被包含对象的实际数据类型
                    column = "pid",             // 根据查询出的card表中的pid字段来查询person表
                    /*
                        one、@One 一对一固定写法
                        select属性：指定调用哪个接口中的哪个方法
                     */
                    one = @One(select = "com.wzk.one_to_one.PersonMapper.selectById")
            )
    })
    public abstract List<Card> selectAll();
}
```

#### 1.3核心配置文件

```xml
	<!--配置映射关系-->
	<mappers>
        <package name="com.wzk.one_to_one"/>
    </mappers>
```



#### 1.4测试类

```java
public class Test01 {
    @Test
    public void selectAll() throws Exception{
        //1.加载核心配置文件
        InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

        //2.获取SqlSession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

        //3.通过工厂对象获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession(true);

        //4.获取CardMapper接口的实现类对象
        CardMapper mapper = sqlSession.getMapper(CardMapper.class);

        //5.调用实现类对象中的方法，接收结果
        List<Card> list = mapper.selectAll();

        //6.处理结果
        for (Card card : list) {
            System.out.println(card);
        }

        //7.释放资源
        sqlSession.close();
        is.close();
    }

}

```

#### 1.5一对一配置总结

~~~xml-dtd
@Results：封装映射关系的父注解。
	Result[] value()：定义了 Result 数组
@Result：封装映射关系的子注解。
	column 属性：查询出的表中字段名称
	property 属性：实体对象中的属性名称
	javaType 属性：被包含对象的数据类型
	one 属性：一对一查询固定属性
@One：一对一查询的注解。
	select 属性：指定调用某个接口中的方法
~~~

### 2.一对多查询

+ 一对多查询的需求：查询一个课程，与此同时查询出该该课程对应的学生信息

+ 对应的sql语句：

```sql
SELECT * FROM classes

SELECT * FROM student WHERE cid=#{cid}
```

#### 2.1创建StudentMapper接口

```java
public interface StudentMapper {
    //根据cid查询student表
    @Select("SELECT * FROM student WHERE cid=#{cid}")
    public abstract List<Student> selectByCid(Integer cid);
}

```

#### 2.2使用注解配置Mapper

```java
public interface ClassesMapper {
    //查询全部
    @Select("SELECT * FROM classes")
    @Results({
            @Result(column = "id",property = "id"),
            @Result(column = "name",property = "name"),
            @Result(
                    property = "students",  // 被包含对象的变量名
                    javaType = List.class,  // 被包含对象的实际数据类型
                    column = "id",          // 根据查询出的classes表的id字段来查询student表
                    /*
                        many、@Many 一对多查询的固定写法
                        select属性：指定调用哪个接口中的哪个查询方法
                     */
                    many = @Many(select = "com.itheima.one_to_many.StudentMapper.selectByCid")
            )
    })
    public abstract List<Classes> selectAll();
}
```

#### 2.3核心配置文件

```xml
	<!--配置映射关系-->
    <mappers>
        <package name="com.wzk"/>
    </mappers>
```

#### 2.4测试类

```java
public class Test01 {
    @Test
    public void selectAll() throws Exception{
        //1.加载核心配置文件
        InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

        //2.获取SqlSession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

        //3.通过工厂对象获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession(true);

        //4.获取ClassesMapper接口的实现类对象
        ClassesMapper mapper = sqlSession.getMapper(ClassesMapper.class);

        //5.调用实现类对象中的方法，接收结果
        List<Classes> list = mapper.selectAll();

        //6.处理结果
        for (Classes cls : list) {
            System.out.println(cls.getId() + "," + cls.getName());
            List<Student> students = cls.getStudents();
            for (Student student : students) {
                System.out.println("\t" + student);
            }
        }

        //7.释放资源
        sqlSession.close();
        is.close();
    }

}

```

#### 2.5一对多配置总结

~~~xml-dtd
@Results：封装映射关系的父注解。
	Result[] value()：定义了 Result 数组
@Result：封装映射关系的子注解。
	column 属性：查询出的表中字段名称
	property 属性：实体对象中的属性名称
	javaType 属性：被包含对象的数据类型
	many 属性：一对多查询固定属性
@Many：一对多查询的注解。
	select 属性：指定调用某个接口中的方法
~~~

### 3.多对多查询

+ 多对多查询的需求：查询学生以及所对应的课程信息

+ 对应的sql语句：

```sql
SELECT DISTINCT s.id,s.name,s.age FROM student s,stu_cr sc WHERE sc.sid=s.id
SELECT c.id,c.name FROM stu_cr sc,course c WHERE sc.cid=c.id AND sc.sid=#{id}
```

#### 3.1添加CourseMapper 接口方法

```java
public interface CourseMapper {
    //根据学生id查询所选课程
    @Select("SELECT c.id,c.name FROM stu_cr sc,course c WHERE sc.cid=c.id AND sc.sid=#{id}")
    public abstract List<Course> selectBySid(Integer id);
}

```

#### 3.2使用注解配置Mapper

```java
public interface StudentMapper {
    //查询全部
    @Select("SELECT DISTINCT s.id,s.name,s.age FROM student s,stu_cr sc WHERE sc.sid=s.id")
    @Results({
            @Result(column = "id",property = "id"),
            @Result(column = "name",property = "name"),
            @Result(column = "age",property = "age"),
            @Result(
                    property = "courses",   // 被包含对象的变量名
                    javaType = List.class,  // 被包含对象的实际数据类型
                    column = "id",          // 根据查询出student表的id来作为关联条件，去查询中间表和课程表
                    /*
                        many、@Many 一对多查询的固定写法
                        select属性：指定调用哪个接口中的哪个查询方法
                     */
                    many = @Many(select = "com.itheima.many_to_many.CourseMapper.selectBySid")
            )
    })
    public abstract List<Student> selectAll();
}

```

#### 3.3核心配置文件

```xml
	<!--配置映射关系-->
    <mappers>
        <package name="com.wzk"/>
    </mappers>
```

#### 3.4测试类

```java
public class Test01 {
    @Test
    public void selectAll() throws Exception{
        //1.加载核心配置文件
        InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

        //2.获取SqlSession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

        //3.通过工厂对象获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession(true);

        //4.获取StudentMapper接口的实现类对象
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

        //5.调用实现类对象中的方法，接收结果
        List<Student> list = mapper.selectAll();

        //6.处理结果
        for (Student student : list) {
            System.out.println(student.getId() + "," + student.getName() + "," + student.getAge());
            List<Course> courses = student.getCourses();
            for (Course cours : courses) {
                System.out.println("\t" + cours);
            }
        }

        //7.释放资源
        sqlSession.close();
        is.close();
    }

}
```

#### 3.5多对多配置总结

~~~xml-dtd
@Results：封装映射关系的父注解。
	Result[] value()：定义了 Result 数组
@Result：封装映射关系的子注解。
	column 属性：查询出的表中字段名称
	property 属性：实体对象中的属性名称
	javaType 属性：被包含对象的数据类型
	many 属性：一对多查询固定属性
@Many：一对多查询的注解。
	select 属性：指定调用某个接口中的方法
~~~

## 十五.构建SQL语句

### 1.SQL构建对象介绍

* 我们之前通过注解开发时，相关 SQL 语句都是自己直接拼写的。一些关键字写起来比较麻烦、而且容易出错。 
* MyBatis 给我们提供了 org.apache.ibatis.jdbc.SQL 功能类，专门用于构建 SQL 语句    

| 方法名                              | 说明                         |
| ----------------------------------- | ---------------------------- |
| SELECT(String...column)             | 根据字段拼接查询语句         |
| FROM(String..table)                 | 根据表名拼接语句             |
| WHERE(String...condition)           | 根据条件拼接语句             |
| INSERT_INTO(String table)           | 根据表名拼接新增语句         |
| VALUES(String column,String values) | 根据字段和值拼接插入数据语句 |
| UPDATE(String table)                | 根据表名拼接修改语句         |
| DELETE_FROM(String table)           | 根据表名拼接删除语句         |
| ...                                 | ...                          |

### 2.查询功能的实现

* 定义功能类并提供获取查询的 SQL 语句的方法。 

* @SelectProvider：生成查询用的 SQL 语句注解。

  type 属性：生成 SQL 语句功能类对象 

  method 属性：指定调用方法    

### 3.新增功能的实现

* 定义功能类并提供获取新增的 SQL 语句的方法。 

* @InsertProvider：生成新增用的 SQL 语句注解。 

  type 属性：生成 SQL 语句功能类对象 

  method 属性：指定调用方法    

### 4.修改功能的实现

* 定义功能类并提供获取修改的 SQL 语句的方法。 

* @UpdateProvider：生成修改用的 SQL 语句注解。 

  type 属性：生成 SQL 语句功能类对象

   method 属性：指定调用方法    

### 5.删除功能的实现

* 定义功能类并提供获取删除的 SQL 语句的方法。 

* @DeleteProvider：生成删除用的 SQL 语句注解。

  type 属性：生成 SQL 语句功能类对象 

  method 属性：指定调用方法   

