# 留言本

## 案例编写步骤

0. 拿到 `留言本案例文件`
1. 创建自己的案例目录（feedback），把你拿到的案例文件以 `目录结构的` 的方式组织起来
2. 创建一个响应 `hello world` 的服务器
3. 当请求 `/` 的时候，响应 `views/index.html` ，当请求 `/fabiao` 的时候响应 `views/fabiao.html`
   1. 获取 url
   2. fs 读取文件
4. 处理公共资源（把某些资源开放为 Apache 服务器的方式）
   1. `node_modules`
   2. `public`
   3. 判断路径是否是是以 `/node_modules/` 开头的，如果是则把把url路径当作文件路径（别忘了路径前面加点儿）去读取该文件响应
   4. 如果读取失败就是 404
5. 使用 `mime` 第三方包动态处理资源的 `Content-Type`
   1. `npm install mime`
   2. `const mime = require('mime')`
   3. `mime.getType('带有后缀名的路径')`
6. 使用 `art-template` 第三方包为 Web 服务器加入模板引擎的能力
   1. `npm install art-template`
   2. `const template = require('art-template')`
   3. `template.render('模板字符串(从文件中读取出来的)', {解析替换对象})`
7. 处理 `fabiao.html` 页面中表单的提交路径为：`post` 请求方法，请求：`/fabiao`
8. 把服务端中原来的路径判断加上请求方法判断
   1. `req.method`
   2. 确保能通过 post 方法和 /fabiao 路径收到客户端的表单提交
9. 处理表单 POST 提交
   1. 具体参考 `处理表单请求`
   2. 确保能正确的拿到表单 post 提交的结果

## 目录结构

```
.
├── node_modules npm安装的第三方包目录，使用 npm 装包会自动创建
│   ├── bootstrap html页面需要使用的样式库
│   └── mime 在 Node 中需要使用它处理 Content-Type
├── public 页面需要使用的公共资源，例如 css、img、js（客户端js） 等
│   ├── css
│   ├── fonts
│   ├── img
│   └── js
├── views 所有视图页面（只存储 html 文件）
│   ├── fabiao.html
│   └── index.html
├── app.js 服务端程序入口文件，执行该文件会启动我们的 Web 服务器
└── package-lock.json npm安装包会自动生成该文件，先不用关心（模块与包）
```



## 响应两个页面

```javascript
const http = require('http')
const fs = require('fs')

http
  .createServer((req, res) => {
    // / index.html
    // /fabiao fabiao.html
    const url = req.url

    console.log(url)

    if (url === '/') {
      // 异步的
      fs.readFile('./views/index.html', (err, data) => {
        if (err) {
          return res.end('404 Not Found.')
        }

        // 没有请求就没有响应
        // Error: Can't set headers after they are sent.
        res.setHeader('Content-Type', 'text/html; charset=utf-8')
        res.end(data)
      })
    } else if (url === '/fabiao') {
      // 异步的
      fs.readFile('./views/fabiao.html', (err, data) => {
        if (err) {
          return res.end('404 Not Found.')
        }
        res.setHeader('Content-Type', 'text/html; charset=utf-8')
        res.end(data)
      })
    } else if (url === '/node_modules/bootstrap/dist/css/bootstrap.css') {
      fs.readFile('./node_modules/bootstrap/dist/css/bootstrap.css', (err, data) => {
        if (err) {
          return res.end('404 Not Found.')
        }
        res.setHeader('Content-Type', 'text/css; charset=utf-8')
        res.end(data)
      })
    }

    // 发送响应数据，结束响应
    // res.end('hello nodejs')
  })
  .listen(3000, () => {
    console.log('running...')
  })
```

## 统一处理公开资源

```javascript
/**
 * Node.js 脚本，服务端脚本
 */

const http = require('http')
const fs = require('fs')
const mime = require('mime')

http
  .createServer((req, res) => {
    // / index.html
    // /fabiao fabiao.html
    const url = req.url

    // 我们约定，只要是你以 /public/ 开头的，我就去程序中自动找这个文件，如果有就发送给客户端，如果没有就发送 404
    // /public/css/main.css
    // /public/js/main.js
    // /public/img/ab2.jpg
    console.log(url)

    // /public/css/main.css
    // /public/js/main.js
    // /public/img/ab2.jpg

    if (url === '/') {
      // 异步的
      fs.readFile('./views/index.html', (err, data) => {
        if (err) {
          return res.end('404 Not Found.')
        }

        // 没有请求就没有响应
        // Error: Can't set headers after they are sent.
        res.setHeader('Content-Type', 'text/html; charset=utf-8')
        res.end(data)
      })
    } else if (url === '/fabiao') {
      // 异步的
      fs.readFile('./views/fabiao.html', (err, data) => {
        if (err) {
          return res.end('404 Not Found.')
        }
        res.setHeader('Content-Type', 'text/html; charset=utf-8')
        res.end(data)
      })
    } else if (url.startsWith('/public/') || url.startsWith('/node_modules/')) {
      // 我们在服务端把 public 目录给公开出来了（支持像你的 Apache 服务器一样来请求里面的资源）
      // 在文件操作中，路径开头的 / 表示的磁盘根目录
      // 在 / 前面加上一个点儿就是相对路径
      const filePath = `.${url}`
      // ./public/css/main.css
      // ./public/dsmakdnkslad.sa.dsa
      fs.readFile(filePath, (err, data) => {
        if (err) {
          return res.end('404 Not Found.')
        }

        // 使用第三方包 mime 处理 Content-Type 问题（根据文件后缀名动态获取 Content-Type）
        res.setHeader('Content-Type', mime.getType(filePath))
        res.end(data)
      })
    }

    // 发送响应数据，结束响应
    // res.end('hello nodejs')
  })
  .listen(3000, () => {
    console.log('running...')
  })

```

注意：我们这里可以使用一个第三方包：[mime](https://www.npmjs.com/package/mime)

安装：

```shell
# 在你的项目目录执行
npm install mime
```

加载使用：

```shell
const mime = require('mime')
mime.getType('带有后缀名的路径') // => 返回值就是 Content-Type 类型
```

## 使用 art-template 模板引擎

> 参考文档：http://aui.github.io/art-template/zh-cn/

安装：

```shell
$ npm install art-template
```

使用：

```javascript
var template = require('art-template')

var ret = template.render('hello {{ name }}', {
  name: 'Jack'
})

console.log(ret) // => hello Jack
```

例如，这是高亮的内容：`hello world`

## 处理表单 post 提交

```javascript
// 1. 定义一个变量
let rawData = ''

let count = 0

// 2. 箭头 data 事件，在 data 事件处理函数中把 chunk 拼接到 rawData 中
// data 事件可能执行一次，也可以能执行 n 次，取决于客户端提交的数据量大小
req.on('data', chunk => {
  console.log(++count)
  // 每当 data 事件被触发， 回调函数就会收到一个数据块
  // 为了得到完整的数据，所以每当接收到一个数据块，我就把它拼接到 data 中
  rawData += chunk
})

// 3. 监听 end 事件，在 end 事件使用 rawData 数据
// 当 end 事件触发执行，则意味着你的 data 接收完毕了
req.on('end', () => {
  console.log('数据接收完毕了')
  console.log(data)
})
```

## 处理发表留言功能

```javascript
// 1. 接收表单 POST 请求体数据
// 2. 使用核心模块 querystring 的 parse 方法将查询字符串转换为对象
// 3. 校验客户端提交的表单数据
// 4. 校验通过，将数据添加到数组中存储起来
//    永远不要相信客户端的输入
// 5. 数据保存完毕，发送重定向，让客户端跳转到 / 首页

// 1. 接收表单 POST 请求体数据
// 1.1 定义数据存储变量
let rawData = ''

// 1.2 监听 data 事件，把 chunk 拼接到 rawData 中
// data 事件可能执行一次，也可以能执行 n 次，取决于客户端提交的数据量大小
req.on('data', chunk => {
  // 每当 data 事件被触发， 回调函数就会收到一个数据块
  // 为了得到完整的数据，所以每当接收到一个数据块，我就把它拼接到 data 中
  rawData += chunk
})

// 1.3 在 end 事件，我们就可以使用接收完毕的 rawData 数据了
// 当 end 事件触发执行，则意味着你的 data 接收完毕了
req.on('end', () => {
  // 2. 使用核心模块 querystring 的 parse 方法将查询字符串转换为对象
  const bodyData = queryString.parse(rawData)

  // 3. 校验客户端提交的表单数据
  //  永远不要相信客户端的输入（因为客户端表单校验是不稳定的）

  // 3.1 校验 name 是否可用
  if (!bodyData.name || !bodyData.name.length) {
    res.setHeader('Content-Type', 'text/plain; charset=utf-8')
    return res.end('name 不能为空')
  }

  // 3.2 校验 content 是否可用
  if (!bodyData.content || !bodyData.content.length) {
    res.setHeader('Content-Type', 'text/plain; charset=utf-8')
    return res.end('contet 不能为空')
  }

  // 4. 校验通过，将数据添加到数组中存储起来
  comments.push({
    id: comments[comments.length - 1].id + 1,
    name: bodyData.name,
    content: bodyData.content
  })

  // 5. 数据保存完毕，发送重定向，让客户端跳转到 / 首页
  res.statusCode = 302
  res.setHeader('Location', '/')
  res.end() // 没有响应数据也要结束响应
})
```

## 将内存存储修改为文件存储

## 案例总结
