# 前端库汇总

## 需求

下面将一些前端相关的库汇总，方便资料查阅

## 描述

前端经过多年发展，趋向稳定，但是也迎来很多问题，比如三大框架 ```react、vue、angular``` 的崛起到稳定。
 ```JQuery``` 老大哥在苦苦坚持。其实是四大 ```JQuery、react、vue、angular```。很多人想把 ```JQuery```
 排除在门外，奈何JQuery永远是你最解决问题的港湾。比如需要seo但是很小的一个项目，再比如需要兼容的项目，
 这个时候可以选择JQuery。技术在于解决问题，没有银弹。

## 事件驱动和数据驱动
### 事件驱动思维
在GUI和Javascript的设计场景下，我们写代码的时候也会代入这样的思维：

用户输入 => 事件响应 => 代码运行 => 刷新页面状态

于是乎，刚开始写应用的思路如下：

```
开发静态页面。
添加事件监听，包括用户输入、http请求、定时器触发等事件。
针对不同事件，编写不同的处理逻辑，包括获取事件状态/输入、计算并更新状态等。
根据计算后的数据状态，重新渲染页面。
```
通俗地说，事件驱动思维是从事件响应出发，来完成应用的设计和编程。


### 数据驱动
数据是什么，官方回答：数据是科学实验、检验、统计等所获得的和用于科学研究、技术设计、查证、决策等的数值。

但其实不管是资料中、生活和工作中，所有的事物我们都可以抽象为数据。像游戏里面的角色、物品、经验值、天气、时间等等，都是数据。游戏其实也算是对真实世界抽象的一种，而抽象之后，最终都可呈现为数据。

我认为，数据是一个抽象的过程。

回到日常写码中，前端写页面，抽象成数据常用的无非是：

```
列表 => array
状态 => number/boolen
一个卡片 => object
等等
```


### 数据驱动 vs 事件驱动

要对事件驱动和数据驱动进行直观的比较，大概是以下这样：

事件驱动
```
构建页面：设计DOM => 生成DOM => 绑定事件
监听事件：操作UI => 触发事件 => 响应处理 => 更新UI
```
数据驱动
```
构建页面：设计数据结构 => 事件绑定逻辑 => 生成DOM
监听事件：操作UI => 触发事件 => 响应处理 => 更新数据 => 更新UI
```
其实最大的转变是，以前会把组件视为DOM，把事件/逻辑处理视为Javascript，把样式视为CSS。而当转换思维方式之后，组件、事件、逻辑处理、样式都是一份数据，我们只需要把数据的状态和转换设计好，剩下的实现则由具现方式（模版引擎、事件机制等）来实现。

## 三大框架介绍

上面介绍了事件驱动和数据驱动，既然我们知道数据驱动的好处，接下来介绍回三大框架。

### react

#### 基础库

react-官网：https://reactjs.org/

react-github：https://github.com/facebook/react

react-router-官网：https://reacttraining.com/react-router/

react-router-github：https://github.com/ReactTraining/react-router

注：react-router有3.x 和 4.x 的区别，目前大多数支持4.x，2个版本是分别维护

redux-官网：https://redux.js.org/

redux-github：https://github.com/reduxjs/redux

redux-saga-官网：https://redux-saga.js.org/

redux-saga-github：https://github.com/redux-saga/redux-saga

#### UI库

阿里出品antd

antd-官网：https://ant.design/index-cn

antd-github：https://github.com/ant-design/ant-design/

antd-pro-官网：https://pro.ant.design/index-cn

antd-pro-github：https://github.com/ant-design/ant-design-pro/


### vue

#### 基础库

vue-官网：https://cn.vuejs.org/index.html

vue-githut：https://github.com/vuejs/vue

router：https://router.vuejs.org/zh/

vuex：https://vuex.vuejs.org/zh/

vue-ssr：https://ssr.vuejs.org/zh/

vue-cli：https://cli.vuejs.org/zh/

#### UI库

element：https://element.eleme.io/#/zh-CN

有赞：https://open.youzan.com/zanui


### angular

#### 基础库

angular-官网：https://angular.io/

## 参考

```
https://godbasin.github.io/2017/09/29/data-driven-or-event-driven/
```
