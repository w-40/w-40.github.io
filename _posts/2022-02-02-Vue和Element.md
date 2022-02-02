---
layout: post
#标题配置
title: Vue和Element
#时间配置
date:   2022-02-02 19:44:00 +0800
#目录配置
categories: JavaWeb
#标签配置
tag: 学习笔记
---

* content
{:toc}





# Vue和Element

## 一.Vue 快速入门

### 1.Vue的介绍

- Vue是一套构建用户界面的渐进式前端框架。
- 只关注视图层，并且非常容易学习，还可以很方便的与其它库或已有项目整合。
- 通过尽可能简单的API来实现响应数据的绑定和组合的视图组件。
- 特点
  易用：在有HTML、CSS、JavaScript的基础上，快速上手。
  灵活：简单小巧的核心，渐进式技术栈，足以应付任何规模的应用。
  性能：20kbmin+gzip运行大小、超快虚拟DOM、最省心的优化。

### 2.Vue的快速入门

- **开发步骤**

1. 下载和引入vue.js文件。
2. 编写入门程序。
   视图：负责页面渲染，主要由HTML+CSS构成。
   脚本：负责业务数据模型（Model）以及数据的处理逻辑。

- **代码实现**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>快速入门</title>
  </head>
  <body>
      <!-- 视图 -->
      <div id="div">
          {{msg}}
      </div>
  </body>
  <script src="js/vue.js"></script>
  <script>
      // 脚本
      new Vue({
          el:"#div",
          data:{
              msg:"Hello Vue"
          }
      });
  </script>
  </html>
  ```

### 3.Vue快速入门详解

- Vue 核心对象：每一个 Vue 程序都是从一个 Vue 核心对象开始的。

  ```tex
  let vm = new Vue({
   选项列表;
  });
  ```

- 选项列表
  el选项：用于接收获取到页面中的元素。(根据常用选择器获取)。
  data选项：用于保存当前Vue对象中的数据。在视图中声明的变量需要在此处赋值。
  methods选项：用于定义方法。方法可以直接通过对象名调用，this代表当前Vue对象。

- 数据绑定
  在视图部分获取脚本部分的数据。
  {{变量名}}

### 4.Vue快速入门的升级

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>快速入门升级</title>
</head>
<body>
    <!-- 视图 -->
    <div id="div">
        <div>姓名：{{name}}</div>
        <div>班级：{{classRoom}}</div>
        <button onclick="hi()">打招呼</button>
        <button onclick="update()">修改班级</button>
    </div>
</body>
<script src="js/vue.js"></script>
<script>
    // 脚本
    let vm = new Vue({
        el:"#div",
        data:{
            name:"张三",
            classRoom:"黑马程序员"
        },
        methods:{
            study(){
                alert(this.name + "正在" + this.classRoom + "好好学习!");
            }
        }
    });

    //定义打招呼方法
    function hi(){
        vm.study();
    }

    //定义修改班级
    function update(){
        vm.classRoom = "传智播客";
    }
</script>
</html>
```

### 5.Vue小结

- Vue是一套构建用户界面的渐进式前端框架。
- Vue的程序包含视图和脚本两个核心部分。
- 脚本部分
  - Vue核心对象。
  - 选项列表
    - el：接收获取的元素。
    - data：保存数据。
    - methods：定义方法。
- 视图部分
  - 数据绑定：{{变量名}}

## 二.Vue 常用指令

### 1.指令介绍

- 指令：是带有 v- 前缀的特殊属性，不同指令具有不同含义。例如 v-html，v-if，v-for。

- 使用指令时，通常编写在标签的属性上，值可以使用 JS 的表达式。

- 常用指令

  | 指令      | 作用                                                |
  | --------- | --------------------------------------------------- |
  | v-html    | 把文本解析为HTML代码                                |
  | v-bind    | 为HTML标签绑定属性值                                |
  | v-if      | 条件性的渲染某元素，判定为true时渲染,否则不渲染     |
  | v-else    | 条件性的渲染某元素，判定为true时渲染,否则不渲染     |
  | v-else-if | 条件性的渲染某元素，判定为true时渲染,否则不渲染     |
  | v-show    | 根据条件展示某元素，区别在于切换的是display属性的值 |
  | v-for     | 列表渲染，遍历容器的元素或者对象的属性              |
  | v-on      | 为HTML标签绑定事件                                  |
  | v-model   | 在表单元素上创建双向数据绑定                        |

### 2.文本插值

- v-html：把文本解析为 HTML 代码。

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>文本插值</title>
  </head>
  <body>
      <div id="div">
          <div>{{msg}}</div>
          <div v-html="msg"></div>
      </div>
  </body>
  <script src="js/vue.js"></script>
  <script>
      new Vue({
          el:"#div",
          data:{
              msg:"<b>Hello Vue</b>"
          }
      });
  </script>
  </html>
  ```

### 3.绑定属性

- v-bind：为 HTML 标签绑定属性值。

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>绑定属性</title>
      <style>
          .my{
              border: 1px solid red;
          }
      </style>
  </head>
  <body>
      <div id="div">
          <a v-bind:href="url">百度一下</a>
          <br>
          <a :href="url">百度一下</a>
          <br>
          <div :class="cls">我是div</div>
      </div>
  </body>
  <script src="js/vue.js"></script>
  <script>
      new Vue({
          el:"#div",
          data:{
              url:"https://www.baidu.com",
              cls:"my"
          }
      });
  </script>
  </html>
  ```

### 4.条件渲染

- v-if：条件性的渲染某元素，判定为真时渲染,否则不渲染。

- v-else：条件性的渲染。

- v-else-if：条件性的渲染。

- v-show：根据条件展示某元素，区别在于切换的是display属性的值。

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>条件渲染</title>
  </head>
  <body>
      <div id="div">
          <!-- 判断num的值，对3取余  余数为0显示div1  余数为1显示div2  余数为2显示div3 -->
          <div v-if="num % 3 == 0">div1</div>
          <div v-else-if="num % 3 == 1">div2</div>
          <div v-else="num % 3 == 2">div3</div>
  
          <div v-show="flag">div4</div>
      </div>
  </body>
  <script src="js/vue.js"></script>
  <script>
      new Vue({
          el:"#div",
          data:{
              num:1,
              flag:false
          }
      });
  </script>
  </html>
  ```

### 5.列表渲染

- v-for：列表渲染，遍历容器的元素或者对象的属性。

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>列表渲染</title>
  </head>
  <body>
      <div id="div">
          <ul>
              <li v-for="name in names">
                  {{name}}
              </li>
              <li v-for="value in student">
                  {{value}}
              </li>
          </ul>
      </div>
  </body>
  <script src="js/vue.js"></script>
  <script>
      new Vue({
          el:"#div",
          data:{
              names:["张三","李四","王五"],
              student:{
                  name:"张三",
                  age:23
              }
          }
      });
  </script>
  </html>
  ```

### 6.事件绑定

- v-on：为 HTML 标签绑定事件。

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>事件绑定</title>
  </head>
  <body>
      <div id="div">
          <div>{{name}}</div>
          <button v-on:click="change()">改变div的内容</button>
          <button v-on:dblclick="change()">改变div的内容</button>
  
          <button @click="change()">改变div的内容</button>
      </div>
  </body>
  <script src="js/vue.js"></script>
  <script>
      new Vue({
          el:"#div",
          data:{
              name:"黑马程序员"
          },
          methods:{
              change(){
                  this.name = "传智播客"
              }
          }
      });
  </script>
  </html>
  ```

### 7.表单绑定

- **表单绑定**
  v-model：在表单元素上创建双向数据绑定。

- **双向数据绑定**
  更新data数据，页面中的数据也会更新。
  更新页面数据，data数据也会更新。

- **MVVM模型(ModelViewViewModel)：是MVC模式的改进版**
  在前端页面中，JS对象表示Model，页面表示View，两者做到了最大限度的分离。
  将Model和View关联起来的就是ViewModel，它是桥梁。
  ViewModel负责把Model的数据同步到View显示出来，还负责把View修改的数据同步回Model。

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>表单绑定</title>
  </head>
  <body>
      <div id="div">
          <form autocomplete="off">
              姓名：<input type="text" name="username" v-model="username">
              <br>
              年龄：<input type="number" name="age" v-model="age">
          </form>
      </div>
  </body>
  <script src="js/vue.js"></script>
  <script>
      new Vue({
          el:"#div",
          data:{
              username:"张三",
              age:23
          }
      });
  </script>
  </html>
  ```

### 8.小结

- **指令：是带有v-前缀的特殊属性，不同指令具有不同含义。**
- **文本插值**
  v-html：把文本解析为HTML代码。
- **绑定属性**
  v-bind：为HTML标签绑定属性值。
- **条件渲染**
  v-if：条件性的渲染某元素，判定为真时渲染,否则不渲染。
  v-else：条件性的渲染。
  v-else-if：条件性的渲染。
  v-show：根据条件展示某元素，区别在于切换的是display属性的值。
- **列表渲染**
  v-for：列表渲染，遍历容器的元素或者对象的属性。
- **事件绑定**
  v-on：为HTML标签绑定事件。
- **表单绑定**
  v-model：在表单元素上创建双向数据绑定。

## 三.Element 基本使用

### 1.Element介绍

- Element：网站快速成型工具。是饿了么公司前端开发团队提供的一套基于Vue的网站组件库。
- 使用Element前提必须要有Vue。
- 组件：组成网页的部件，例如超链接、按钮、图片、表格等等~
- Element官网：https://element.eleme.cn/#/zh-CN
- Element 提供的按钮

  - 默认按钮、朴素按钮、圆角按钮
  - 主要按钮、成功按钮、信息按钮
  - 警告按钮、危险按钮

### 2.Element快速入门

- **开发步骤**

  1. 下载 Element 核心库。
  2. 引入 Element 样式文件。
  3. 引入 Vue 核心 js 文件。
  4. 引入 Element 核心 js 文件。
  5. 编写按钮标签。
  6. 通过 Vue 核心对象加载元素。

- **代码实现**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>快速入门</title>
      <link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
      <script src="js/vue.js"></script>
      <script src="element-ui/lib/index.js"></script>
  </head>
  <body>
      <button>我是按钮</button>
      <br>
      <div id="div">
          <el-row>
              <el-button>默认按钮</el-button>
              <el-button type="primary">主要按钮</el-button>
              <el-button type="success">成功按钮</el-button>
              <el-button type="info">信息按钮</el-button>
              <el-button type="warning">警告按钮</el-button>
              <el-button type="danger">危险按钮</el-button>
            </el-row>
            <br>
            <el-row>
              <el-button plain>朴素按钮</el-button>
              <el-button type="primary" plain>主要按钮</el-button>
              <el-button type="success" plain>成功按钮</el-button>
              <el-button type="info" plain>信息按钮</el-button>
              <el-button type="warning" plain>警告按钮</el-button>
              <el-button type="danger" plain>危险按钮</el-button>
            </el-row>
            <br>
            <el-row>
              <el-button round>圆角按钮</el-button>
              <el-button type="primary" round>主要按钮</el-button>
              <el-button type="success" round>成功按钮</el-button>
              <el-button type="info" round>信息按钮</el-button>
              <el-button type="warning" round>警告按钮</el-button>
              <el-button type="danger" round>危险按钮</el-button>
            </el-row>
            <br>
            <el-row>
              <el-button icon="el-icon-search" circle></el-button>
              <el-button type="primary" icon="el-icon-edit" circle></el-button>
              <el-button type="success" icon="el-icon-check" circle></el-button>
              <el-button type="info" icon="el-icon-message" circle></el-button>
              <el-button type="warning" icon="el-icon-star-off" circle></el-button>
              <el-button type="danger" icon="el-icon-delete" circle></el-button>
            </el-row>
      </div>
  </body>
  <script>
      new Vue({
          el:"#div"
      });
  </script>
  </html>
  ```

### 3.基础布局

将页面分成最多 24 个部分，自由切分。

+ 24
+ 12、12
+ 8、8、8

- **代码实现**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>基础布局</title>
      <link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
      <script src="js/vue.js"></script>
      <script src="element-ui/lib/index.js"></script>
      <style>
          .el-row {
              /* 行距为20px */
              margin-bottom: 20px;
          }
          .bg-purple-dark {
              background: red;
          }
          .bg-purple {
              background: blue;
          }
          .bg-purple-light {
              background: green;
          }
          .grid-content {
              /* 边框圆润度 */
              border-radius: 4px;
              /* 行高为36px */
              min-height: 36px;
          }
        </style>
  </head>
  <body>
      <div id="div">
          <el-row>
              <el-col :span="24"><div class="grid-content bg-purple-dark"></div></el-col>
            </el-row>
            <el-row>
              <el-col :span="12"><div class="grid-content bg-purple"></div></el-col>
              <el-col :span="12"><div class="grid-content bg-purple-light"></div></el-col>
            </el-row>
            <el-row>
              <el-col :span="8"><div class="grid-content bg-purple"></div></el-col>
              <el-col :span="8"><div class="grid-content bg-purple-light"></div></el-col>
              <el-col :span="8"><div class="grid-content bg-purple"></div></el-col>
            </el-row>
            <el-row>
              <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
              <el-col :span="6"><div class="grid-content bg-purple-light"></div></el-col>
              <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
              <el-col :span="6"><div class="grid-content bg-purple-light"></div></el-col>
            </el-row>
            <el-row>
              <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
              <el-col :span="4"><div class="grid-content bg-purple-light"></div></el-col>
              <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
              <el-col :span="4"><div class="grid-content bg-purple-light"></div></el-col>
              <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
              <el-col :span="4"><div class="grid-content bg-purple-light"></div></el-col>
            </el-row>
      </div>
  </body>
  <script>
      new Vue({
          el:"#div"
      });
  </script>
  </html>
  ```

### 4.容器布局

将页面分成头部区域、侧边栏区域、主区域、底部区域。

- **代码实现**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>容器布局</title>
      <link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
      <script src="js/vue.js"></script>
      <script src="element-ui/lib/index.js"></script>
      <style>
          .el-header, .el-footer {
              background-color: #d18e66;
              color: #333;
              text-align: center;
              height: 100px;
          }
          .el-aside {
              background-color: #55e658;
              color: #333;
              text-align: center;
              height: 580px;
          }
          .el-main {
              background-color: #5fb1f3;
              color: #333;
              text-align: center;
              height: 520px;
          }
      </style>
  </head>
  <body>
      <div id="div">
          <el-container>
              <el-header>头部区域</el-header>
              <el-container>
                <el-aside width="200px">侧边栏区域</el-aside>
                <el-container>
                  <el-main>主区域</el-main>
                  <el-footer>底部区域</el-footer>
                </el-container>
              </el-container>
            </el-container>
      </div>
  </body>
  <script>
      new Vue({
          el:"#div"
      });
  </script>
  </html>
  ```

### 5.表单组件

由输入框、下拉列表、单选框、多选框等控件组成，用以收集、校验、提交数据。

- **代码实现**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>表单组件</title>
      <link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
      <script src="js/vue.js"></script>
      <script src="element-ui/lib/index.js"></script>
  </head>
  <body>
      <div id="div">
          <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
              <el-form-item label="活动名称" prop="name">
                <el-input v-model="ruleForm.name"></el-input>
              </el-form-item>
              <el-form-item label="活动区域" prop="region">
                <el-select v-model="ruleForm.region" placeholder="请选择活动区域">
                  <el-option label="区域一" value="shanghai"></el-option>
                  <el-option label="区域二" value="beijing"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="活动时间" required>
                <el-col :span="11">
                  <el-form-item prop="date1">
                    <el-date-picker type="date" placeholder="选择日期" v-model="ruleForm.date1" style="width: 100%;"></el-date-picker>
                  </el-form-item>
                </el-col>
                <el-col class="line" :span="2">-</el-col>
                <el-col :span="11">
                  <el-form-item prop="date2">
                    <el-time-picker placeholder="选择时间" v-model="ruleForm.date2" style="width: 100%;"></el-time-picker>
                  </el-form-item>
                </el-col>
              </el-form-item>
              <el-form-item label="即时配送" prop="delivery">
                <el-switch v-model="ruleForm.delivery"></el-switch>
              </el-form-item>
              <el-form-item label="活动性质" prop="type">
                <el-checkbox-group v-model="ruleForm.type">
                  <el-checkbox label="美食/餐厅线上活动" name="type"></el-checkbox>
                  <el-checkbox label="地推活动" name="type"></el-checkbox>
                  <el-checkbox label="线下主题活动" name="type"></el-checkbox>
                  <el-checkbox label="单纯品牌曝光" name="type"></el-checkbox>
                </el-checkbox-group>
              </el-form-item>
              <el-form-item label="特殊资源" prop="resource">
                <el-radio-group v-model="ruleForm.resource">
                  <el-radio label="线上品牌商赞助"></el-radio>
                  <el-radio label="线下场地免费"></el-radio>
                </el-radio-group>
              </el-form-item>
              <el-form-item label="活动形式" prop="desc">
                <el-input type="textarea" v-model="ruleForm.desc"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
                <el-button @click="resetForm('ruleForm')">重置</el-button>
              </el-form-item>
            </el-form>
      </div>
  </body>
  <script>
      new Vue({
          el:"#div",
          data:{
              ruleForm: {
                  name: '',
                  region: '',
                  date1: '',
                  date2: '',
                  delivery: false,
                  type: [],
                  resource: '',
                  desc: ''
                  },
          rules: {
            name: [
              { required: true, message: '请输入活动名称', trigger: 'blur' },
              { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
            ],
            region: [
              { required: true, message: '请选择活动区域', trigger: 'change' }
            ],
            date1: [
              { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
            ],
            date2: [
              { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
            ],
            type: [
              { type: 'array', required: true, message: '请至少选择一个活动性质', trigger: 'change' }
            ],
            resource: [
              { required: true, message: '请选择活动资源', trigger: 'change' }
            ],
            desc: [
              { required: true, message: '请填写活动形式', trigger: 'blur' }
            ]
          }
          },
          methods:{
              submitForm(formName) {
                  this.$refs[formName].validate((valid) => {
                  if (valid) {
                      alert('submit!');
                  } else {
                      console.log('error submit!!');
                      return false;
                  }
                  });
              },
              resetForm(formName) {
                  this.$refs[formName].resetFields();
              }
          }
      });
  </script>
  </html>
  ```

### 6.表格组件

用于展示多条结构类似的数据，可对数据进行编辑、删除或其他自定义操作。

- **代码实现**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>表格组件</title>
      <link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
      <script src="js/vue.js"></script>
      <script src="element-ui/lib/index.js"></script>
  </head>
  <body>
      <div id="div">
          <template>
              <el-table
                :data="tableData"
                style="width: 100%">
                <el-table-column
                  prop="date"
                  label="日期"
                  width="180">
                </el-table-column>
                <el-table-column
                  prop="name"
                  label="姓名"
                  width="180">
                </el-table-column>
                <el-table-column
                  prop="address"
                  label="地址">
                </el-table-column>
  
                <el-table-column
                  label="操作"
                  width="180">
                  <el-button type="warning">编辑</el-button>
                  <el-button type="danger">删除</el-button>
                </el-table-column>
              </el-table>
            </template>
      </div>
  </body>
  <script>
      new Vue({
          el:"#div",
          data:{
              tableData: [{
                  date: '2016-05-02',
                  name: '王小虎',
                  address: '上海市普陀区金沙江路 1518 弄'
              }, {
                  date: '2016-05-04',
                  name: '王小虎',
                  address: '上海市普陀区金沙江路 1517 弄'
              }, {
                  date: '2016-05-01',
                  name: '王小虎',
                  address: '上海市普陀区金沙江路 1519 弄'
              }, {
                  date: '2016-05-03',
                  name: '王小虎',
                  address: '上海市普陀区金沙江路 1516 弄'
              }]
          }
      });
  </script>
  </html>
  ```

### 7.顶部导航栏组件

- **代码实现**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>顶部导航栏</title>
      <link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
      <script src="js/vue.js"></script>
      <script src="element-ui/lib/index.js"></script>
  </head>
  <body>
      <div id="div">
        <el-menu
        :default-active="activeIndex2"
        class="el-menu-demo"
        mode="horizontal"
        @select="handleSelect"
        background-color="#545c64"
        text-color="#fff"
        active-text-color="#ffd04b">
        <el-menu-item index="1">处理中心</el-menu-item>
        <el-submenu index="2">
          <template slot="title">我的工作台</template>
          <el-menu-item index="2-1">选项1</el-menu-item>
          <el-menu-item index="2-2">选项2</el-menu-item>
          <el-menu-item index="2-3">选项3</el-menu-item>
          <el-submenu index="2-4">
            <template slot="title">选项4</template>
            <el-menu-item index="2-4-1">选项1</el-menu-item>
            <el-menu-item index="2-4-2">选项2</el-menu-item>
            <el-menu-item index="2-4-3">选项3</el-menu-item>
          </el-submenu>
        </el-submenu>
        <el-menu-item index="3" disabled>消息中心</el-menu-item>
        <el-menu-item index="4"><a href="https://www.ele.me" target="_blank">订单管理</a></el-menu-item>
      </el-menu>
      </div>
  </body>
  <script>
      new Vue({
          el:"#div"
      });
  </script>
  </html>
  ```

### 8.侧边导航栏组件

- **代码实现**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>侧边导航栏</title>
      <link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
      <script src="js/vue.js"></script>
      <script src="element-ui/lib/index.js"></script>
  </head>
  <body>
      <div id="div">
        <el-col :span="6">
          <el-menu
            default-active="2"
            class="el-menu-vertical-demo"
            @open="handleOpen"
            @close="handleClose"
            background-color="#545c64"
            text-color="#fff"
            active-text-color="#ffd04b">
            <el-submenu index="1">
              <template slot="title">
                <i class="el-icon-location"></i>
                <span>导航一</span>
              </template>
              <el-menu-item-group>
                <template slot="title">分组一</template>
                <el-menu-item index="1-1">选项1</el-menu-item>
                <el-menu-item index="1-2">选项2</el-menu-item>
              </el-menu-item-group>
              <el-menu-item-group title="分组2">
                <el-menu-item index="1-3">选项3</el-menu-item>
              </el-menu-item-group>
              <el-submenu index="1-4">
                <template slot="title">选项4</template>
                <el-menu-item index="1-4-1">选项1</el-menu-item>
              </el-submenu>
            </el-submenu>
            <el-menu-item index="2">
              <i class="el-icon-menu"></i>
              <span slot="title">导航二</span>
            </el-menu-item>
            <el-menu-item index="3" disabled>
              <i class="el-icon-document"></i>
              <span slot="title">导航三</span>
            </el-menu-item>
            <el-menu-item index="4">
              <i class="el-icon-setting"></i>
              <span slot="title">导航四</span>
            </el-menu-item>
          </el-menu>
        </el-col>
      </div>
  </body>
  <script>
      new Vue({
          el:"#div"
      });
  </script>
  </html>
  ```

### 9.小结

- Element：网站快速成型工具。是一套为开发者、设计师、产品经理准备的基于Vue的桌面端组件库。
- 使用Element前提必须要有Vue。
- 使用步骤
  1.下载Element核心库。
  2.引入Element样式文件。
  3.引入Vue核心js文件。
  4.引入Element核心js文件。
  5.借助常用组件编写网页。
- 常用组件
  网页基本组成部分，布局、按钮、表格、表单等等~~~
  常用组件不需要记住，只需要在Element官网中复制使用即可。

## 四.综合案例 学生列表

### 1.案例效果和分析

​		图略

### 2.头部区域的实现

- **实现思路**

  - 头部效果实现。
  - 侧边栏和主区域效果实现。

- **代码实现**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>学生列表</title>
      <link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
      <script src="js/vue.js"></script>
      <script src="element-ui/lib/index.js"></script>
      <style>
        .el-header{
          background-color: #545c64;
        }
        .header-img{
          width: 100px;
          margin-top: 20px;
        }
      </style>
  </head>
  <body>
    <div id="div">
      <el-container>
        <!-- 头部 -->
        <el-header class="el-header">
          <el-container>
            <div>
              <el-image src="img/export.png" class="header-img"></el-image>
            </div>
            <el-menu
              :default-active="activeIndex2"
              mode="horizontal"
              @select="handleSelect"
              background-color="#545c64"
              text-color="white"
              active-text-color="#ffd04b"
              style="margin-left: auto;">
              <el-menu-item index="1">处理中心</el-menu-item>
              <el-submenu index="2">
                <template slot="title">我的工作台</template>
                <el-menu-item index="2-1">选项1</el-menu-item>
                <el-menu-item index="2-2">选项2</el-menu-item>
                <el-menu-item index="2-3">选项3</el-menu-item>
              </el-submenu>
              <el-menu-item index="3"><a href="学生列表.html" target="_self">首页</a></el-menu-item>
            </el-menu>
          </el-container>
        </el-header>
      </el-container>
    </div>
  </body>
  <script>
      new Vue({
          el:"#div"
      });
  </script>
  </html>
  ```

### 3.侧边栏区域的实现

```html
<!-- 侧边栏区域 -->
<el-container style="height: 580px; border: 1px solid #eee">
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <el-menu :default-openeds="['1']">
            <el-submenu index="1">
                <template slot="title"><i class="el-icon-menu"></i>学工部</template>
                <el-menu-item-group>
                    <el-menu-item index="1-1"><i class="el-icon-help"></i>在校学生管理</el-menu-item>
                    <el-menu-item index="1-2"><i class="el-icon-help"></i>学生升级/留级</el-menu-item>
                    <el-menu-item index="1-3"><i class="el-icon-help"></i>学生就业情况</el-menu-item>
                </el-menu-item-group>
            </el-submenu>
            <el-submenu index="2">
                <template slot="title"><i class="el-icon-menu"></i>咨询部</template>
                <el-menu-item-group>
                    <el-menu-item index="2-1"><i class="el-icon-help"></i>意向学生管理</el-menu-item>
                    <el-menu-item index="2-2"><i class="el-icon-help"></i>未报名学生管理</el-menu-item>
                    <el-menu-item index="2-3"><i class="el-icon-help"></i>已报名学生管理</el-menu-item>
                </el-menu-item-group>
            </el-submenu>
            <el-submenu index="3">
                <template slot="title"><i class="el-icon-menu"></i>教研部</template>
                <el-menu-item-group>
                    <el-menu-item index="3-1"><i class="el-icon-help"></i>已有课程管理</el-menu-item>
                    <el-menu-item index="3-2"><i class="el-icon-help"></i>正在研发课程管理</el-menu-item>
                    <el-menu-item index="3-3"><i class="el-icon-help"></i>新技术课程管理</el-menu-item>
                </el-menu-item-group>
            </el-submenu>
        </el-menu>
    </el-aside>

</el-container>
```

### 4.主区域的实现

**主区域和侧边栏区域放在一起**

```html
<!-- 主区域 -->
<el-container>
    <el-main>
        <b style="color: red;font-size: 20px;">学生列表</b>
        <div style="float:right">
            <el-button type="primary">添加学生</el-button>
        </div>
        <el-table :data="tableData">
            <el-table-column prop="date" label="日期" width="140">
            </el-table-column>
            <el-table-column prop="name" label="姓名" width="120">
            </el-table-column>
            <el-table-column prop="address" label="地址">
            </el-table-column>
            <el-table-column
                             label="操作"
                             width="180">
                <el-button type="warning">编辑</el-button>
                <el-button type="danger">删除</el-button>
            </el-table-column>
        </el-table>
    </el-main>
</el-container>
```

**在vue中定义data**

```js
<script>
    new Vue({
        el:"#div",
        data:{
          tableData:[
            {
              date:"2088-08-08",
              name:"张三",
              address:"北京市昌平区"
            },{
              date:"2088-08-08",
              name:"李四",
              address:"北京市昌平区"
            },{
              date:"2088-08-08",
              name:"王五",
              address:"北京市昌平区"
            },
          ]
        }
    });
</script>
```

## 五.Vue 高级使用

### 1.自定义组件

- 学完了 Element 组件后，我们会发现组件其实就是自定义的标签。例如 就是对的封装。 

- 本质上，组件是带有一个名字且可复用的 Vue 实例，我们完全可以自己定义。

- 定义格式

  ```tex
  Vue.component(组件名称, {
   props:组件的属性,
   data: 组件的数据函数,
   template: 组件解析的标签模板
  })
  ```

- **代码实现**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>自定义组件</title>
      <script src="vue/vue.js"></script>
  </head>
  <body>
      <div id="div">
          <my-button>我的按钮</my-button>
      </div>
  </body>
  <script>
      Vue.component("my-button",{
          // 属性
          props:["style"],
          // 数据函数
          data: function(){
              return{
                  msg:"我的按钮"
              }
          },
          //解析标签模板
          template:"<button style='color:red'>{{msg}}</button>"
      });
  
      new Vue({
          el:"#div"
      });
  </script>
  </html>
  ```

### 2.Vue的生命周期

- **生命周期**

​		此处图略（可去官网查看）



- **生命周期的八个阶段**

  | 状态          | 阶段周期 |
  | ------------- | -------- |
  | beforeCreate  | 创建前   |
  | created       | 创建后   |
  | beforeMount   | 载入前   |
  | mounted       | 载入后   |
  | beforeUpdate  | 更新前   |
  | updated       | 更新后   |
  | beforeDestroy | 销毁前   |
  | destroyed     | 销毁后   |

- **代码实现**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>生命周期</title>
      <script src="vue/vue.js"></script>
  </head>
  <body>
      <div id="app">
          {{message}}
      </div>
  </body>
  <script>
      let vm = new Vue({
  				el: '#app',
  				data: {
  					message: 'Vue的生命周期'
  				},
  				beforeCreate: function() {
  					console.group('------beforeCreate创建前状态------');
  					console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
  					console.log("%c%s", "color:red", "data   : " + this.$data); //undefined 
  					console.log("%c%s", "color:red", "message: " + this.message);//undefined
  				},
  				created: function() {
  					console.group('------created创建完毕状态------');
  					console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
  					console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化 
  					console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
  				},
  				beforeMount: function() {
  					console.group('------beforeMount挂载前状态------');
  					console.log("%c%s", "color:red", "el     : " + (this.$el)); //已被初始化
  					console.log(this.$el);
  					console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化  
  					console.log("%c%s", "color:red", "message: " + this.message); //已被初始化  
  				},
  				mounted: function() {
  					console.group('------mounted 挂载结束状态------');
  					console.log("%c%s", "color:red", "el     : " + this.$el); //已被初始化
  					console.log(this.$el);
  					console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
  					console.log("%c%s", "color:red", "message: " + this.message); //已被初始化 
  				},
  				beforeUpdate: function() {
  					console.group('beforeUpdate 更新前状态===============》');
  					let dom = document.getElementById("app").innerHTML;
  					console.log(dom);
  					console.log("%c%s", "color:red", "el     : " + this.$el);
  					console.log(this.$el);
  					console.log("%c%s", "color:red", "data   : " + this.$data);
  					console.log("%c%s", "color:red", "message: " + this.message);
  				},
  				updated: function() {
  					console.group('updated 更新完成状态===============》');
  					let dom = document.getElementById("app").innerHTML;
  					console.log(dom);
  					console.log("%c%s", "color:red", "el     : " + this.$el);
  					console.log(this.$el);
  					console.log("%c%s", "color:red", "data   : " + this.$data);
  					console.log("%c%s", "color:red", "message: " + this.message);
  				},
  				beforeDestroy: function() {
  					console.group('beforeDestroy 销毁前状态===============》');
  					console.log("%c%s", "color:red", "el     : " + this.$el);
  					console.log(this.$el);
  					console.log("%c%s", "color:red", "data   : " + this.$data);
  					console.log("%c%s", "color:red", "message: " + this.message);
  				},
  				destroyed: function() {
  					console.group('destroyed 销毁完成状态===============》');
  					console.log("%c%s", "color:red", "el     : " + this.$el);
  					console.log(this.$el);
  					console.log("%c%s", "color:red", "data   : " + this.$data);
  					console.log("%c%s", "color:red", "message: " + this.message);
  				}
  			});
  
  		
  			// 销毁Vue对象
  			//vm.$destroy();
  			//vm.message = "hehe";	// 销毁后 Vue 实例会解绑所有内容
  
  			// 设置data中message数据值
  			vm.message = "good...";
  </script>
  </html>
  ```

### 3.Vue异步操作

- **在Vue中发送异步请求，本质上还是AJAX。我们可以使用axios这个插件来简化操作！**

- **使用步骤**
  1.引入axios核心js文件。
  2.调用axios对象的方法来发起异步请求。
  3.调用axios对象的方法来处理响应的数据。

- **axios常用方法**

  | 方法名                          | 作用                                             |
  | ------------------------------- | ------------------------------------------------ |
  | get(请求的资源路径与请求的参数) | 发起GET方式请求                                  |
  | post(请求的资源路径,请求的参数) | 发起POST方式请求                                 |
  | then(response)                  | 请求成功后的回调函数，通过response获取响应的数据 |
  | catch(error)                    | 请求失败后的回调函数，通过error获取错误信息      |

- **代码实现**

  - **html代码**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>异步操作</title>
      <script src="js/vue.js"></script>
      <script src="js/axios-0.18.0.js"></script>
  </head>
  <body>
      <div id="div">
          {{name}}
          <button @click="send()">发起请求</button>
      </div>
  </body>
  <script>
      new Vue({
          el:"#div",
          data:{
              name:"张三"
          },
          methods:{
              send(){
                  // GET方式请求
                  // axios.get("testServlet?name=" + this.name)
                  //     .then(resp => {
                  //         alert(resp.data);
                  //     })
                  //     .catch(error => {
                  //         alert(error);
                  //     })
  
                  // POST方式请求
                  axios.post("testServlet","name="+this.name)
                      .then(resp => {
                          alert(resp.data);
                      })
                      .catch(error => {
                          alert(error);
                      })
              }
          }
      });
  </script>
  </html>
  ```

  - **java代码**

  ```java
  package com.itheima;
  
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  @WebServlet("/testServlet")
  public class TestServlet extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          //设置请求和响应的编码
          req.setCharacterEncoding("UTF-8");
          resp.setContentType("text/html;charset=UTF-8");
  
          //获取请求参数
          String name = req.getParameter("name");
          System.out.println(name);
  
          //响应客户端
          resp.getWriter().write("请求成功");
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          this.doGet(req,resp);
      }
  }
  ```

### 4.小结

- 自定义组件：本质上，组件是带有一个名字且可复用的 Vue 实例，我们可以自己来定义。

  ```tex
  Vue.component(组件名称, {
   props:组件的属性,
   data: 组件的数据函数,
   template: 组件解析的标签模板
  })
  ```

- 生命周期：核心八个阶段
  beforeCreate：创建前
  created：创建后
  beforeMount：载入前
  mounted：载入后
  beforeUpdate：更新前
  updated：更新后
  beforeDestroy：销毁前
  destroyed：销毁后

- 异步操作：通过 axios 插件来实现。

  | 方法名                          | 作用                                             |
  | ------------------------------- | ------------------------------------------------ |
  | get(请求的资源路径与请求的参数) | 发起GET方式请求                                  |
  | post(请求的资源路径,请求的参数) | 发起POST方式请求                                 |
  | then(response)                  | 请求成功后的回调函数，通过response获取响应的数据 |
  | catch(error)                    | 请求失败后的回调函数，通过error获取错误信息      |

## 六.综合案例 学生管理系统

### 1.效果环境的介绍

​	图略

### 2.登录功能的实现

- **环境搭建**

  - 从当天的资料中解压《学生管理系统原始项目》，并导入。

- **代码实现**

  - **html代码**

    ```js
    onSubmit(formName) {
                    // 为表单绑定验证功能
                    this.$refs[formName].validate((valid) => {
                        if (valid) {
                            //请求服务器完成登录功能
                            axios.post("userServlet","username=" + this.form.username + "&password=" + this.form.password)
                                .then(resp => {
                                    if(resp.data == true) {
                                        //登录成功，跳转到首页
                                        location.href = "index.html";
                                    }else {
                                        //登录失败，跳转到登录页面
                                        alert("登录失败，请检查用户名和密码");
                                        location.href = "login.html";
                                    }
                                })
                        } else {
                            return false;
                        }
                    });
                }
    ```

  - **java代码**

    - **UserServlet.java**

    ```java
    package com.itheima.controller;
    
    import com.itheima.bean.User;
    import com.itheima.service.UserService;
    import com.itheima.service.impl.UserServiceImpl;
    
    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;
    import java.util.List;
    
    @WebServlet("/userServlet")
    public class UserServlet extends HttpServlet {
        private UserService service = new UserServiceImpl();
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            //设置请求和响应编码
            req.setCharacterEncoding("UTF-8");
            resp.setContentType("text/html;charset=UTF-8");
    
            //1.获取请求参数
            String username = req.getParameter("username");
            String password = req.getParameter("password");
    
            //2.封装User对象
            User user = new User(username,password);
    
            //3.调用业务层的登录方法
            List<User> list = service.login(user);
    
            //4.判断是否查询出结果
            if(list.size() != 0) {
                //将用户名存入会话域当中
                req.getSession().setAttribute("username",username);
                //响应给客户端true
                resp.getWriter().write("true");
            }else {
                //响应给客户端false
                resp.getWriter().write("false");
            }
        }
    
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            doGet(req,resp);
        }
    }
    ```

    - **UserService.java**

    ```java
    package com.itheima.service;
    
    import com.itheima.bean.User;
    
    import java.util.List;
    /*
        业务层约束接口
     */
    public interface UserService {
        /*
            登录方法
         */
        public abstract List<User> login(User user);
    }
    ```

    - **UserServiceImpl.java**

    ```java
    package com.itheima.service.impl;
    
    import com.itheima.bean.User;
    import com.itheima.mapper.UserMapper;
    import com.itheima.service.UserService;
    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;
    
    import java.io.IOException;
    import java.io.InputStream;
    import java.util.List;
    
    public class UserServiceImpl implements UserService {
        @Override
        public List<User> login(User user) {
            InputStream is = null;
            SqlSession sqlSession = null;
            List<User> list = null;
            try{
                //1.加载核心配置文件
                is = Resources.getResourceAsStream("MyBatisConfig.xml");
    
                //2.获取SqlSession工厂对象
                SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    
                //3.通过SqlSession工厂对象获取SqlSession对象
                sqlSession = sqlSessionFactory.openSession(true);
    
                //4.获取UserMapper接口的实现类对象
                UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    
                //5.调用实现类对象的登录方法
                list = mapper.login(user);
    
            }catch (Exception e) {
                e.printStackTrace();
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
            //7.返回结果到控制层
            return list;
        }
    }
    ```

    - **UserMapper.java**

    ```java
    package com.itheima.mapper;
    
    import com.itheima.bean.User;
    import org.apache.ibatis.annotations.Select;
    
    import java.util.List;
    
    public interface UserMapper {
        /*
            登录方法
         */
        @Select("SELECT * FROM user WHERE username=#{username} AND password=#{password}")
        public abstract List<User> login(User user);
    }
    ```

### 3.分页查询功能的实现

- **代码实现**

  - **html代码**

    ```js
    <script>
        new Vue({
            el:"#div",
            data:{
                dialogTableVisible4add: false,  //添加窗口显示状态
                dialogTableVisible4edit: false, //编辑窗口显示状态
                formData:{},//添加表单的数据
                editFormData: {},//编辑表单的数据
                tableData:[],//表格数据
                pagination: {
                    currentPage: 1, //当前页
                    pageSize: 5,    //每页显示条数
                    total: 0        //总条数
                },
                rules: {
                    number: [
                        {required: true, message: '请输入学号', trigger: 'blur'},
                        {min: 2, max: 10, message: '长度在 2 到 10 个字符', trigger: 'blur'}
                    ],
                    name: [
                        {required: true, message: '请输入姓名', trigger: 'blur'},
                        {min: 2, max: 10, message: '长度在 2 到 10 个字符', trigger: 'blur'}
                    ],
                    birthday: [
                        {required: true, message: '请选择日期', trigger: 'change'}
                    ],
                    address: [
                        {required: true, message: '请输入地址', trigger: 'blur'},
                        {min: 2, max: 200, message: '长度在 2 到 200 个字符', trigger: 'blur'}
                    ],
                }
            },
            methods:{
                //分页查询功能
                selectByPage(){
                    axios.post("studentServlet","method=selectByPage&currentPage=" + this.pagination.currentPage + "&pageSize=" + this.pagination.pageSize)
                        .then(resp => {
                            //将查询出的数据赋值tableData
                            this.tableData = resp.data.list;
                            //设置分页参数
                            //当前页
                            this.pagination.currentPage = resp.data.pageNum;
                            //总条数
                            this.pagination.total = resp.data.total;
                        })
                },
                //改变每页条数时执行的函数
                handleSizeChange(pageSize) {
                    //修改分页查询的参数
                    this.pagination.pageSize = pageSize;
                    //重新执行查询
                    this.selectByPage();
                },
                //改变页码时执行的函数
                handleCurrentChange(pageNum) {
                    //修改分页查询的参数
                    this.pagination.currentPage = pageNum;
                    //重新执行查询
                    this.selectByPage();
                },
                showAddStu() {
                    //弹出窗口
                    this.dialogTableVisible4add = true;
                },
                resetForm(addForm) {
                    //双向绑定，输入的数据都赋值给了formData， 清空formData数据
                    this.formData = {};
                    //清除表单的校验数据
                    this.$refs[addForm].resetFields();
                },
                showEditStu(row) {
                    //1. 弹出窗口
                    this.dialogTableVisible4edit = true;
    
                    //2. 显示表单数据
                    this.editFormData = {
                        number:row.number,
                        name:row.name,
                        birthday:row.birthday,
                        address:row.address,
                    }
                }   
            },
            mounted(){
                //调用分页查询功能
                this.selectByPage();
            }
        });
    </script>
    ```

  - **java代码**

    - **1、创建StudentServlet.java**

    ```java
    package com.itheima.controller;
    
    import com.fasterxml.jackson.databind.ObjectMapper;
    import com.github.pagehelper.Page;
    import com.github.pagehelper.PageInfo;
    import com.itheima.bean.Student;
    import com.itheima.service.StudentService;
    import com.itheima.service.impl.StudentServiceImpl;
    import org.apache.commons.beanutils.BeanUtils;
    import org.apache.commons.beanutils.ConvertUtils;
    import org.apache.commons.beanutils.Converter;
    
    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;
    import java.text.ParseException;
    import java.text.SimpleDateFormat;
    import java.util.Date;
    import java.util.Map;
    
    @WebServlet("/studentServlet")
    public class StudentServlet extends HttpServlet {
        private StudentService service = new StudentServiceImpl();
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            //设置请求和响应编码
            req.setCharacterEncoding("UTF-8");
            resp.setContentType("text/html;charset=UTF-8");
    
            //1.获取方法名
            String method = req.getParameter("method");
            if(method.equals("selectByPage")) {
                //分页查询功能
                selectByPage(req,resp);
            }
        }
    
        /*
            分页查询功能
         */
        private void selectByPage(HttpServletRequest req, HttpServletResponse resp) {
            //获取请求参数
            String currentPage = req.getParameter("currentPage");
            String pageSize = req.getParameter("pageSize");
    
            //调用业务层的查询方法
            Page page = service.selectByPage(Integer.parseInt(currentPage), Integer.parseInt(pageSize));
    
            //封装PageInfo
            PageInfo info = new PageInfo(page);
    
            //将info转成json，响应给客户端
            try {
                String json = new ObjectMapper().writeValueAsString(info);
                resp.getWriter().write(json);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            doGet(req,resp);
        }
    }
    ```

    - **2、创建StudentService.java**

    ```java
    package com.itheima.service;
    
    import com.github.pagehelper.Page;
    import com.itheima.bean.Student;
    
    /*
        学生业务层接口
     */
    public interface StudentService {
        /*
            分页查询方法
         */
        public abstract Page selectByPage(Integer currentPage, Integer pageSize);
    }
    ```

    - **3、创建StudentServiceImpl.java**

    ```java
    package com.itheima.service.impl;
    
    import com.github.pagehelper.Page;
    import com.github.pagehelper.PageHelper;
    import com.itheima.bean.Student;
    import com.itheima.mapper.StudentMapper;
    import com.itheima.service.StudentService;
    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;
    
    import java.io.IOException;
    import java.io.InputStream;
    
    /*
        学生业务层实现类
     */
    public class StudentServiceImpl implements StudentService {
    
        /*
            分页查询功能
         */
        @Override
        public Page selectByPage(Integer currentPage, Integer pageSize) {
            InputStream is = null;
            SqlSession sqlSession = null;
            Page page = null;
            try{
                //1.加载核心配置文件
                is = Resources.getResourceAsStream("MyBatisConfig.xml");
                //2.获取SqlSession工厂对象
                SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
                //3.通过SqlSession工厂对象获取SqlSession对象
                sqlSession = sqlSessionFactory.openSession(true);
                //4.获取StudentMapper接口实现类对象
                StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
                //5.设置分页参数
                page = PageHelper.startPage(currentPage,pageSize);
                //6.调用实现类对象查询全部方法
                mapper.selectAll();
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                //7.释放资源
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
    
            //8.返回结果到控制层
            return page;
        }
    }
    ```

    - **4、创建StudentMapper.java**

    ```java
    package com.itheima.mapper;
    
    import com.itheima.bean.Student;
    import org.apache.ibatis.annotations.Delete;
    import org.apache.ibatis.annotations.Insert;
    import org.apache.ibatis.annotations.Select;
    import org.apache.ibatis.annotations.Update;
    
    import java.util.List;
    
    /*
        学生持久层接口
     */
    public interface StudentMapper {
        /*
            查询全部方法
         */
        @Select("SELECT * FROM student")
        public abstract List<Student> selectAll();
    }
    
    ```

### 4.添加功能的实现

- **代码实现**

  - **html代码**

    在stuList.html中增加“添加功能”代码

    ```js
    //添加学生功能
                addStu(){
                    let param = "method=addStu&number=" + this.formData.number + "&name=" + this.formData.name +
                            "&birthday=" + this.formData.birthday + "&address=" + this.formData.address +
                            "&currentPage=" + this.pagination.currentPage + "&pageSize=" + this.pagination.pageSize;
                    axios.post("studentServlet",param)
                        .then(resp => {
                            //将查询出的数据赋值tableData
                            this.tableData = resp.data.list;
                            //设置分页参数
                            //当前页
                            this.pagination.currentPage = resp.data.pageNum;
                            //总条数
                            this.pagination.total = resp.data.total;
                        })
                    //关闭添加窗口
                    this.dialogTableVisible4add = false;
                }
    ```

  - **java代码**

    - 1、在StudentServlet.java中增加“添加功能”代码-addStu

    ```java
    	/*
    	*1、直接复制会报错
    	*2、需要将此行代码需要添加到“doGet”方法中
    	*3、增加“addStu”方法名的判断	
        */
    	else if(method.equals("addStu")) {
                //添加数据功能
                addStu(req,resp);
         }	
    ==================================================================================
    
    	/*
            添加数据功能
         */
        private void addStu(HttpServletRequest req, HttpServletResponse resp) {
            //获取请求参数
            Map<String, String[]> map = req.getParameterMap();
            String currentPage = req.getParameter("currentPage");
            String pageSize = req.getParameter("pageSize");
    
            //封装Student对象
            Student stu = new Student();
    
            //注册日期转换器方法
            dateConvert();
    
            try {
                BeanUtils.populate(stu,map);
            } catch (Exception e) {
                e.printStackTrace();
            }
    
            //调用业务层的添加方法
            service.addStu(stu);
    
            //重定向到分页查询功能
            try {
                resp.sendRedirect(req.getContextPath() + "/studentServlet?method=selectByPage&currentPage=" + currentPage + "&pageSize=" + pageSize);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    
        /*
            日期转换
         */
        private void dateConvert() {
            ConvertUtils.register(new Converter() {
                public Object convert(Class type, Object value) {
                    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
                    try {
                        return simpleDateFormat.parse(value.toString());
                    } catch (ParseException e) {
                        e.printStackTrace();
                    }
                    return null;
                }
            }, Date.class);
        }
    ```

    - 2、在StudentService.java中增加“添加功能”-addStu

    ```java
        /*
            添加数据方法
         */
        public abstract void addStu(Student stu);
    ```

    - 3、StudentServiceImpl.java中增加“添加功能”-addStu

    ```java
    /*
            添加数据方法
         */
        @Override
        public void addStu(Student stu) {
            InputStream is = null;
            SqlSession sqlSession = null;
            try{
                //1.加载核心配置文件
                is = Resources.getResourceAsStream("MyBatisConfig.xml");
                //2.获取SqlSession工厂对象
                SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
                //3.通过SqlSession工厂对象获取SqlSession对象
                sqlSession = sqlSessionFactory.openSession(true);
                //4.获取StudentMapper接口实现类对象
                StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
                //5.调用实现类对象添加方法
                mapper.addStu(stu);
            } catch (Exception e) {
                e.printStackTrace();
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
        }
    ```

    - 4、StudentMapper.java中增加“添加功能”-addStu

    ```java
        /*
            添加数据方法
         */
        @Insert("INSERT INTO student VALUES (#{number},#{name},#{birthday},#{address})")
        public abstract void addStu(Student stu);
    ```

### 5.修改功能的实现

- **代码实现**

  - **html代码**

    在stuList.html中增加“修改功能”代码

    ```js
    //修改数据功能
                updateStu() {
                    let param = "method=updateStu&number=" + this.editFormData.number + "&name=" + this.editFormData.name +
                        "&birthday=" + this.editFormData.birthday + "&address=" + this.editFormData.address +
                        "&currentPage=" + this.pagination.currentPage + "&pageSize=" + this.pagination.pageSize;
                    axios.post("studentServlet",param)
                        .then(resp => {
                            //将查询出的数据赋值tableData
                            this.tableData = resp.data.list;
                            //设置分页参数
                            //当前页
                            this.pagination.currentPage = resp.data.pageNum;
                            //总条数
                            this.pagination.total = resp.data.total;
                        })
                    //关闭编辑窗口
                    this.dialogTableVisible4edit = false;
                }
    ```

  - **java代码**

    - 1、在StudentServlet.java中增加“修改功能”-updateStu

    ```java
        /*
            修改数据功能
         */
        private void updateStu(HttpServletRequest req, HttpServletResponse resp) {
            //获取请求参数
            Map<String, String[]> map = req.getParameterMap();
            String currentPage = req.getParameter("currentPage");
            String pageSize = req.getParameter("pageSize");
    
            //封装Student对象
            Student stu = new Student();
    
            //注册日期转换器方法
            dateConvert();
    
            try {
                BeanUtils.populate(stu,map);
            } catch (Exception e) {
                e.printStackTrace();
            }
    
            //调用业务层的修改方法
            service.updateStu(stu);
    
            //重定向到分页查询功能
            try {
                resp.sendRedirect(req.getContextPath() + "/studentServlet?method=selectByPage&currentPage=" + currentPage + "&pageSize=" + pageSize);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    ```

    - 2、在StudentService.java中增加“修改功能”-updateStu

    ```java
    /*
            修改数据方法
         */
        public abstract void updateStu(Student stu);
    ```

    - 3、StudentServiceImpl.java中增加“修改功能”-updateStu

    ```java
    	/*
    	*1、直接复制会报错
    	*2、需要将此行代码需要添加到“doGet”方法中
    	*3、增加“updateStu”方法名的判断	
        */
    	else if(method.equals("updateStu")) {
                //添加数据功能
                updateStu(req,resp);
         }	
    ==================================================================================
    
    /*
            修改数据方法
         */
        @Override
        public void updateStu(Student stu) {
            InputStream is = null;
            SqlSession sqlSession = null;
            try{
                //1.加载核心配置文件
                is = Resources.getResourceAsStream("MyBatisConfig.xml");
                //2.获取SqlSession工厂对象
                SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
                //3.通过SqlSession工厂对象获取SqlSession对象
                sqlSession = sqlSessionFactory.openSession(true);
                //4.获取StudentMapper接口实现类对象
                StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
                //5.调用实现类对象修改方法
                mapper.updateStu(stu);
            } catch (Exception e) {
                e.printStackTrace();
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
        }
    ```

    - 4、StudentMapper.java中增加“修改功能”-updateStu

    ```java
    /*
            修改数据方法
         */
        @Update("UPDATE student SET number=#{number},name=#{name},birthday=#{birthday},address=#{address} WHERE number=#{number}")
        public abstract void updateStu(Student stu);
    ```

### 6.删除功能的实现

- **代码实现**

  - **html代码**

    在stuList.html中增加“删除功能”代码

    ```js
    //删除数据功能
                deleteStu(row) {
                    if(confirm("确定要删除" + row.number + "数据?")) {
                        let param = "method=deleteStu&number=" + row.number +
                            "&currentPage=" + this.pagination.currentPage + "&pageSize=" + this.pagination.pageSize;
                        axios.post("studentServlet",param)
                            .then(resp => {
                                //将查询出的数据赋值tableData
                                this.tableData = resp.data.list;
                                //设置分页参数
                                //当前页
                                this.pagination.currentPage = resp.data.pageNum;
                                //总条数
                                this.pagination.total = resp.data.total;
                            })
                    }
                }
    ```

  - **java代码**

    - 1、在StudentServlet.java中增加“删除功能”-

    ```java
    	/*
    	*1、直接复制会报错
    	*2、需要将此行代码需要添加到“doGet”方法中
    	*3、增加“deleteStu”方法名的判断	
        */
    	else if(method.equals("deleteStu")) {
                //添加数据功能
                deleteStu(req,resp);
         }	
    ==================================================================================
    
    
    /*
            删除数据功能
         */
        private void deleteStu(HttpServletRequest req, HttpServletResponse resp) {
            //获取请求参数
            String number = req.getParameter("number");
            String currentPage = req.getParameter("currentPage");
            String pageSize = req.getParameter("pageSize");
    
            //调用业务层的删除方法
            service.deleteStu(number);
    
            //重定向到分页查询功能
            try {
                resp.sendRedirect(req.getContextPath() + "/studentServlet?method=selectByPage&currentPage=" + currentPage + "&pageSize=" + pageSize);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    ```

    - 2、在StudentService.java中增加“删除功能”-

    ```java
        /*
            删除数据方法
         */
        public abstract void deleteStu(String number);
    ```

    - 3、StudentServiceImpl.java中增加“删除功能”-

    ```java
    /*
            删除数据方法
         */
        @Override
        public void deleteStu(String number) {
            InputStream is = null;
            SqlSession sqlSession = null;
            try{
                //1.加载核心配置文件
                is = Resources.getResourceAsStream("MyBatisConfig.xml");
                //2.获取SqlSession工厂对象
                SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
                //3.通过SqlSession工厂对象获取SqlSession对象
                sqlSession = sqlSessionFactory.openSession(true);
                //4.获取StudentMapper接口实现类对象
                StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
                //5.调用实现类对象删除方法
                mapper.deleteStu(number);
            } catch (Exception e) {
                e.printStackTrace();
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
        }
    ```

    - 4、StudentMapper.java中增加“删除功能”-

    ```java
        /*
            删除数据方法
         */
        @Delete("DELETE FROM student WHERE number=#{number}")
        public abstract void deleteStu(String number);
    ```