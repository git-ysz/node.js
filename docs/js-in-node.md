# Node 中的 JavaScript

![node中的JavaScript](../media/node中的JavaScript.png)

> 了解 Node 中的 JavaScript 由哪几部分组成

- EcmaScript
  - 目前 EcmaScript 最新的标准规范：EcmaScript 2017（EcmaScript 6）
  - 推荐多使用 EcmaScript 6 进行编程
  - EcmaScript 6 解决了EcmaScript 5 中的一些问题，同时提供了一些非常好用的语法及 API
  - 提高编程效率
- Node 支持 JavaScript 模块化（CommonJS 模块规范）
  - 浏览器中的 JavaScript 只能通过 `script` 标签来加载使用
  - `include`
  - `require`
  - css 中：`@import()`
  - 让你的 JavaScript 文件具有了相互通信的能力了
- 核心模块（由Node提供）
- 第三方模块（由第三方提供）
- 用户模块（自己写的）

## EcmaScript

## Node 中的模块化

![node中的模块化](./media/node中的模块化.png)

- require
- exports
- module



## 核心模块

> 参考文档：https://nodejs.org/dist/latest-v9.x/docs/api/

Node 中都以具名的方式提供了不同功能的模块，例如操作文件就是：`fs`

核心模块（系统模块）由 Node 提供，使用的时候都必须根据特定的核心模块名称来加载使用。例如使用文件操作模块：`fs`

```javascript
var fs = require('fs')

// fs.readFile
// fs.writeFile
// fs.appendFile
```



| 模块名称                                     | 作用           |
| ---------------------------------------- | ------------ |
| [fs](https://nodejs.org/dist/latest-v9.x/docs/api/fs.html) | 文件操作         |
| [http](https://nodejs.org/dist/latest-v9.x/docs/api/http.html) | 网络操作         |
| [path](https://nodejs.org/dist/latest-v9.x/docs/api/path.html) | 路径操作         |
| [url](https://nodejs.org/dist/latest-v9.x/docs/api/url.html) | url 地址操作     |
| [os](https://nodejs.org/dist/latest-v9.x/docs/api/os.html) | 操作系统信息       |
| [net](https://nodejs.org/dist/latest-v9.x/docs/api/net.html) | 一种更底层的网络操作方式 |
| [querystring](https://nodejs.org/dist/latest-v9.x/docs/api/querystring.html) | 解析查询字符串      |
| [util](https://nodejs.org/dist/latest-v9.x/docs/api/util.html) | 工具函数模块       |
| ...                                      | ...          |



## 用户自定义模块

- 用户基于模块规则编写的文件模块代码

## 第三方模块

> 第三方模块就是别人开发的模块，例如我们在前端中使用的 jQuery
>
> npm 社区提供了大量的第三方模块，多到几十万个。
>
> 咱们再这里举个例子，例如我使用一个第三方模块，把 markdown 格式的字符串转为 html 格式的字符串

1. 使用 npm 安装下载

```shell
# 测试
npm install marked
```

2. 加载并使用 `marked`



```javascript
var marked = require('marked')

// 返回 html 格式字符串
marked('markdown 格式字符串')
```
