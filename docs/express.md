# Express

原生的 http 在某些方面表现不足以应对我们的开发需求，所以我们就需要使用框架来加快我们的开发效率，框架的目的就是提高效率，让我们的代码更高度统一。
在 Node 中，有很多 Web 开发框架，我们这里以学习 `express` 为主。

## Express 介绍

- [Express 官方](http://expressjs.com/)
- [Express 中文文档（非官方）](http://www.expressjs.com.cn/)
- [Express 官方仓库](https://github.com/expressjs/express)

## 起步（Getting Started）

### 安装

> 参考文档：http://expressjs.com/en/starter/installing.html

```shell
# mkdir make directory 创建目录
mkdir myapp
cd myapp
npm init -y
npm i express
```

### hello world

> 参考文档：http://expressjs.com/en/starter/hello-world.html

```javascript
// 0. 加载 Express
const express = require('express')

// 1. 调用 express() 得到一个 app
//    类似于 http.createServer()
const app = express()

// 2. 设置请求对应的处理函数
//    当客户端以 GET 方法请求 / 的时候就会调用第二个参数：请求处理函数
app.get('/', (req, res) => {
  res.send('hello world')
})

app.get('/login', (req, res) => {
  res.send('login page')
})

// 3. 监听端口号，启动 Web 服务
app.listen(3000, () => console.log('app listening on port 3000!'))
```

### 基本路由

> 参考文档：http://expressjs.com/en/starter/basic-routing.html

- 请求方法
- 请求路径
- 请求处理函数

get:

```javascript
// 当你以 GET 方法请求 / 的时候，执行对应的处理函数
app.get('/', function (req, res) {
  res.send('Hello World!')
})
```

post:

```javascript
// 当你以 POST 方法请求 / 的时候，指定对应的处理函数
app.post('/', function (req, res) {
  res.send('Got a POST request')
})
```

## 静态服务

> 参考文档：http://expressjs.com/en/starter/static-files.html

```javascript
// /public资源
app.use(express.static('public'))

// /files资源
app.use(express.static('files'))

// /public/xxx
app.use('/public', express.static('public'))

// /static/xxx
app.use('/static', express.static('public'))

app.use('/static', express.static(path.join(__dirname, 'public')))
```

## 配置使用 `art-template` 模板引擎

Express 没有内置任何模板引擎，但是支持让你配置其它模板引擎和 Express 配合使用。

- [art-template GitHub仓库](https://github.com/aui/art-template)
- [art-template 官方文档](https://aui.github.io/art-template/)

安装：

```shell
npm install --save art-template express-art-template
```

配置：

```javascript
// 第一个参数用来配置视图的后缀名，这里是 art ，则你存储在 views 目录中的模板文件必须是 xxx.art
// app.engine('art', require('express-art-template'))

// 这里我把 art 改为 html
app.engine('html', require('express-art-template'))
```

使用：

```javascript
app.get('/', function (req, res) {
  // express 默认会去项目中的 views 目录找 index.html
  res.render('index.html', {
    title: 'hello world'
  })
})
```

如果希望修改默认的 `views` 视图渲染存储目录，可以：

```javascript
// 注意：第一个参数 views 千万不要写错
app.set('views', 目录路径)
```

## 在 Express 获取表单 POST 请求体数据

> 参考文档：

在 Express 中没有内置获取表单 POST 请求体的 API，这里我们需要使用一个第三方包：`body-parser`。

安装：

```shell
npm install --save body-parser
```

配置：

```javascript
var express = require('express')
// 0. 引包
var bodyParser = require('body-parser')

var app = express()

// 配置 body-parser
// 只要加入这个配置，则在 req 请求对象上会多出来一个属性：body
// 也就是说你就可以直接通过 req.body 来获取表单 POST 请求体数据了
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))
// parse application/json
app.use(bodyParser.json())
```

使用：

```javascript
app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  // 可以通过 req.body 来获取表单 POST 请求体数据
  res.end(JSON.stringify(req.body, null, 2))
})
```

## 在 Express 配置使用 `express-session` 插件

> 参考文档：https://github.com/expressjs/session

安装：

```shell
npm install express-session
```

配置：

```javascript
// 该插件会为 req 请求对象添加一个成员：req.session 默认是一个对象
// 这是最简单的配置方式，暂且先不用关心里面参数的含义
app.use(session({
  // 配置加密字符串，它会在原有加密基础之上和这个字符串拼起来去加密
  // 目的是为了增加安全性，防止客户端恶意伪造
  secret: 'itcast',
  resave: false,
  saveUninitialized: false // 无论你是否使用 Session ，我都默认直接给你分配一把钥匙
}))
```

使用：

```javascript
// 添加 Session 数据
req.session.foo = 'bar'

// 获取 Session 数据
req.session.foo
```

提示：默认 Session 数据是内存存储的，服务器一旦重启就会丢失，真正的生产环境会把 Session 进行持久化存储。

---

## 路由使用

> 参考文档：http://expressjs.com/en/guide/routing.html

一个非常基础的路由：

```javascript
var express = require('express')
var app = express()

// respond with "hello world" when a GET request is made to the homepage
app.get('/', function (req, res) {
  res.send('hello world')
})
```

### 路由方法

```javascript
// GET method route
app.get('/', function (req, res) {
  res.send('GET request to the homepage')
})

// POST method route
app.post('/', function (req, res) {
  res.send('POST request to the homepage')
})
```

### 路由路径

This route path will match requests to the root route, /.

```javascript
app.get('/', function (req, res) {
  res.send('root')
})
```

This route path will match requests to /about.

```javascript
app.get('/about', function (req, res) {
  res.send('about')
})
```

This route path will match requests to /random.text.

```javascript
app.get('/random.text', function (req, res) {
  res.send('random.text')
})
```

Here are some examples of route paths based on string patterns.

This route path will match acd and abcd.

```javascript
app.get('/ab?cd', function (req, res) {
  res.send('ab?cd')
})
```

This route path will match abcd, abbcd, abbbcd, and so on.

```javascript
app.get('/ab+cd', function (req, res) {
  res.send('ab+cd')
})
```

This route path will match abcd, abxcd, abRANDOMcd, ab123cd, and so on.

```javascript
app.get('/ab*cd', function (req, res) {
  res.send('ab*cd')
})
```

This route path will match /abe and /abcde.

```javascript
app.get('/ab(cd)?e', function (req, res) {
  res.send('ab(cd)?e')
})
```

Examples of route paths based on regular expressions:

This route path will match anything with an “a” in the route name.

```javascript
app.get(/a/, function (req, res) {
  res.send('/a/')
})
```

This route path will match butterfly and dragonfly, but not butterflyman, dragonflyman, and so on.

```javascript
app.get(/.*fly$/, function (req, res) {
  res.send('/.*fly$/')
})
```

#### 动态路径

```
Route path: /users/:userId/books/:bookId
Request URL: http://localhost:3000/users/34/books/8989
req.params: { "userId": "34", "bookId": "8989" }
```

定义动态的路由路径：

```javascript
app.get('/users/:userId/books/:bookId', function (req, res) {
  res.send(req.params)
})
```

### 路由处理方法

### app.route()

### express.Router

Create a router file named router.js in the app directory, with the following content:

```javascript
const express = require('express')

const router = express.Router()

router.get('/', function (req, res) {
  res.send('home page')
})

router.get('/about', function (req, res) {
  res.send('About page')
})

module.exports = router
```

Then, load the router module in the app:

```javascript
const router = require('./router')

// ...

app.use(router)
```
