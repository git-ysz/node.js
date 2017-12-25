# Node.js 简介

- [Node 官网](https://nodejs.org/en/)
- [Node 中文社区](https://cnodejs.org/)


- [维基百科 - Node.js](https://zh.wikipedia.org/wiki/Node.js)
- [深入浅出Node.js（一）：什么是Node.js](http://www.infoq.com/cn/articles/what-is-nodejs)
- [Node.js发展历程与版本演化](http://www.jianshu.com/p/5b9b245fcefa)

## Node 是什么

> Node 是一个服务端环境，使用 JavaScript 进行编程
>
> 解决了 JavaScript 只能运行在浏览器的局限性
>
> - PHP、Java、Python、Ruby、Rust、.Net（C#）、能做的，Node 都能做
>   - Node 是一个再 09 年推出的一个服务端技术，采用 JavaScript 进行编程
>   - 这些服务端技术的编程模型（思想、方式）都是一样的，API、某些特性
>
> Node 采用 JavaScript 进行编程，为 JavaScript 提供了系统级别的操作能力
>
> - 操作文件
> - 网络操作
>   - 充当客户端发请求
>   - 充当服务器接收处理请求



- Node.js® is a JavaScript runtime built on [Chrome's V8 JavaScript engine](https://developers.google.com/v8/).
  - Node 不是语言
  - Node 不是 JavaScript 库
  - Node.js 是一个 JavaScript 运行环境（运行时）
    - Node 可以解析执行 JavaScript
    - JavaScript 脱离了浏览器运行了在了 Node 中
  - Chrome V8 JavaScript 引擎
    - 解析执行 JavaScript 的
  - Node 把 Chrome 浏览器的 V8 引擎（开源的）移植了出来，基于它的基础之上为 JavaScript 提供了系统级别的操作能力（文件操作、网络操作）
  - 在 Node 中，我们不处理页面的中的 BOM、DOM（也没有这个能力）
- Node.js uses an event-driven, non-blocking I/O（input，output） model that makes it lightweight and efficient.
  - `event-driven` 事件驱动
  - `non-blocking I/O`（input，output） model 非阻塞 IO 模型（异步操作，文件）
    - 简单理解：异步方式更高效


- Node.js' package ecosystem, [npm](https://www.npmjs.com/), is the largest ecosystem of open source libraries in the world.
  - Node.js 包生态系统 `npm`  是世界上最大的开源库生态系统。
    - 一般都是 Node、前端 开发者使用npm 工具管理第三方包
    - 不同的技术平台有不同的技术解决方案
  - npm 全称 `Node Package Manager`
  - npm 是一个网站（生态系统）
    - 你们把你们开发好的包放到我这里，开发人人员就可以从我这里下载使用
    - 以前 npm 只是存放 Node 相关的包
    - 由于 npm 发展的越来越好，而且都是 JavaScript 相关工具
    - JavaScript Package Manager
      - 浏览器中的 JavaScript
      - Node 中的 JavaScript
  - npm 是一个基于 Node 开发的一个命令行工具（CLI CommandLine Interface 软件）
    - npm 通过命令的方式安装管理包
    - `npm install 包名 包名 包名。。。`

## 为什么要学习 Node

- 企业需求
  + 具有服务端开发经验更好（加分项）
  + front-end
  + back-end
  + WEb 全栈开发工程师
    * 全干
  + 基本的网站开发能力
    * 服务端
    * 前端
    * 运维部署
  + 多人社区

- 帮助前端开发（极大的帮助我们学习前端所谓的框架知识，都是工具）

  - Vue.js

    Angular

    React

  - 构建工具、工作流工具

## Node 能做什么

- Web 服务器后台
- 命令行工具
  - npm(node)
  - git(c 语言)
  - hexo(node) 博客生成器
  - 。。。
- 对于前端开发工程师来讲，接触 node 最多的是它的命令行工具
  - 自己写的很少，主要是使用别人第三发的
  - webpack（构建工具）
  - gulp（构建工具）
  - browser-sync（浏览器同步测试工具）
  - npm

## Node 发展历史

![Node 之父](http://hacktivist.in/introduction-to-nodejs-mongodb-meteor/img/ryan.jpg)

- 作者：Ryan Dahl ，数学博士
- Web.js
  - 作者的初衷是打造一个高性能的 Web 服务器
  - Apache
  - Nginx
  - Tomcat
- Node.js
  - JavaScript
- 后来作者出于某种原因离开了Node开发团队
  - 据说是玩儿 go 去了
  - Google 搞了一个语言：Go 号称高性能
- 目前 Node 由 Joyent 公司团队开发维护

2009年2月，Ryan Dahl在博客上宣布准备基于V8创建一个轻量级的Web服务器并提供一套库。

2009年5月，Ryan Dahl在GitHub上发布了最初版本的部分Node.js包，随后几个月里，有人开始使用Node.js开发应用。

2009年11月和2010年4月，两届JSConf大会都安排了Node.js的讲座。

2010年年底，Node.js获得云计算服务商Joyent资助，创始人Ryan Dahl加入Joyent全职负责Node.js的发展。

2011年7月，Node.js在微软的支持下发布Windows版本。

- 8.9.1 LTS（Lone Term Support）稳定版，适用于生产环境
- 9.2.0 Latest Features 最新特性版，适用于学习测试
- 学习测试最起码：6.x 以上

## 一些资源

- 《深入浅出Node.js》
  - 阿里巴巴：朴灵
  - 偏理论，几乎没有任何实战行内容
  - 理解原理底层有帮助
  - 结合课程的学习去看
- 《Node.js 权威指南》
  - API 讲解
  - 也没有业务，没有实战
- JavaScript 标准参考教程（alpha）：http://javascript.ruanyifeng.com/
- Node 入门：http://www.nodebeginner.org/index-zh-cn.html
- 官方API文档：https://nodejs.org/dist/latest-v6.x/docs/api/
- 中文文档（版本比较旧，凑合看）：http://www.nodeclass.com/api/node.html
- CNODE社区：http://cnodejs.org
- CNODE-新手入门：http://cnodejs.org/getstart

## 这门课程你能学到啥？

- B/S 编程模型
  - Browser - Server
  - back-end
  - 任何服务端技术这种 BS 编程模型都是一样，和语言无关
  - Node 只是作为我们学习 BS 编程模型的一个工具而已
- 模块化编程
  - RequireJS
  - SeaJS
  - `@import('文件路径')`
  - 以前认知的 JavaScript 只能通过 `script` 标签来加载
  - 在 Node 中可以像 `@import()` 一样来引用加载 JavaScript 脚本文件
- Node常用API
- 异步编程
  - 回调函数
  - Promise
  - async
  - generator
- Express Web 开发框架
- Ecmascript 6
  - 在课程中穿插讲解
  - 它只是一个新的语法而已
- ...
- 学习 Node 不仅会帮助大家打开服务端黑盒子，同时会帮助你学习以后的前端高级内容
  - Vue.js
  - React
  - angular
