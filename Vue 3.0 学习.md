# Vue 3.0 学习



## webpack 前端工程化



### 1. 实际的前端开发

- **模块化**（js的模块化、css的模块化、资源的模块化）
- **组件化**（服用现有的ui结构、样式、行为）
- **规范化**（目录结构的划分、编码的规范化、接口的规范化、文档规范化、git分支管理）
- **自动化**（自动化构建、自动部署、自动化测试）



### 2. 什么是前端工程化

> 在企业级的前端项目开发中，把前端开发所需的工具、技术、流程、经验等进行规范化、标准化。



### 3. 好处

让前端开发自成体系

调高开发效率、降低技术选型、前后端联调带来的协调沟通成本



### 4. 解决方案



早期：

- grunt

- gulp



目前：

- webpack

- parcel



## webpack基本使用

### 1. 什么是webpack

> 前端项目共功能成化的具体解决方案



**主要功能：**

前端模块化开发支持、代码压缩混淆、处理浏览器js兼容性、性能优化。



**好处：**

提高开发效率和可维护性



### 2. 简单使用

创建一个 隔行变色 的项目

1. 新建项目空白目录，并运行npm init -y命令，初始化包管理配置文件package.json
2. 新建src源代码目录
3. 新建src -> index.html首页和src -> index.js 脚本文件
4. 初始化首页基本的结构
5. 运行npm install jquery -S命令，安装jQuery
6. 通过ES6模块化的方式导入jQuery，实现列表隔行变色效果



> 列表的一种写法： `ul>li{这是第$个li}*9`



index.js

```js
// 1．使用ES6模块化的语法导入 jquery
import $ from 'jquery'
// 2．实现隔行变色的效果
$(function () {
	$( 'li:odd' ).css( 'backgroundColor', 'red ')
    $( 'li:even ').css('backgroundColor', 'cyan ')
})

```



### 3. 在项目中安装webpack

在终端运行如下的命令，安装webpack相关的两个包:
`npm install webpack@5.5.1 webpack-cli@4.2.0 -D`

