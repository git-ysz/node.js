# Node 中的模块系统

## 什么是模块化

![模块化](http://img1.gtimg.com/digi/pics/hv1/241/121/1791/116490871.jpg)

> 现实角度（手机、电脑、活动板房）：
>
>  - 生产效率高
> - 可维护性好
>
> 程序角度（就是把大一个文件中很多的代码拆分到不同的小文件中，每个小文件就称之为一个模块，例如我们看到的 jQuery 真正的源码）
>
> - 开发效率高（不需要在一个文件中翻来翻去，例如 jQuery 不可能在一个文件写 1w+ 代码，按照功能划分到不同文件中）
> - 可维护性好（哪个功能出问题，直接定位该功能模块即可）



模块化的概念有了，那程序中的模块到底该具有哪些特性就满足我们的使用了呢？

- 模块作用域
  - 好处就是模块不需要考虑全局命名空间冲突的问题
- 模块通信规则
  - 所有模块如果都是封闭自然不行，我们需要让组件与组件相互组织联系起来，例如 CPU 需要读取内存中的数据来进行计算，然后把计算结果又交给了我们的操作系统
  - 所以我们的模块也是需要具有通信的能力的
  - 所谓的通信说白了也就是输入与输出



下面我们具体来看一下在 Node.js 中如何在多模块之间进行输入与输出。

## Node 中的模块分类

在开始了解具体的规则之前，我们先来了解一下在 Node 中对不模块的一个具体分类，一共就三种类别：

- 核心模块
  - 由 Node 本身提供，具名的，例如 fs 文件操作模块、http 网络操作模块
- 第三方模块
  - 由第三方提供，使用的时候我们需要通过 npm 进行下载然后才可以加载使用，例如我们使用过的 `mime`、`art-template`、`marked`
  - 注意：不可能有第三方包的名字和核心模块的名字是一样的，否则会造成冲突
- 用户模块（自己写的）
  - 我们在文件中写的代码很多的情况下不好编写和维护，所以我们可以考虑把文件中的代码拆分到多个文件中，那这些我们自己创建的文件就是用户模块

## 模块通信之：输入 `require`

```javascript
// 核心模块
var fs = require('fs')

// 第三方模块
// npm install marked
var marked = require('marked')

// 用户模块（自己写的），正确的，正确的方式
// 注意：加载自己写的模块，相对路径不能省略 ./
var foo = require('./foo.js')

// 用户模块（自己写的），正确的（推荐），可以省略后缀名 .js
var foo = require('./foo')
```

##模块通信：输出 `exports`

导出多个成员：写法一（麻烦，不推荐）：

```javascript
// 导出多个成员：写法一
module.exports.a = 123
module.exports.b = 456
module.exports.c = 789
```

**导出多个成员：写法二（推荐）**

Node 为了降低开发人员的痛苦，所以为 `module.exports` 提供了一个别名 `exports` （下面协大等价于上面的写法）。

```javascript
console.log(exports === module.exports) // => true
exports.a = 123
exports.b = 456
exports.c = 789
exports.fn = function () { 
}
```

导出多个成员：写法三（代码少可以，但是代码一多就不推荐了）：

```javascript
// module.exports = {
//   d: 'hello',
//   e: 'world',
//   fn: function () {
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // fs.readFile(function () {
      
//     // })
//   }
// }
```

**导出单个成员：（唯一的写法）：**

```javascript
// 导出单个成员：错误的写法
// 因为每个模块最终导出是 module.exports 而不是 exports 这个别名
// exports = function (x, y) {
//   return x + y
// }


// 导出单个成员：必须这么写
module.exports = function (x, y) {
  return x + y
}
```

注意：导出单个只能导出一次，下面的情况后者会覆盖前者：

```javascript
module.exports = 'hello'

// 以这个为准，后者会覆盖前者
module.exports = function (x, y) {
  return x + y
}
```

### 为什么 `exports = xxx` 不行

exports 和 `module.exports` 的一个引用：

```javascript
function fn() {
  // 每个模块内部有一个 module 对象
  // module 对象中有一个成员 exports 也是一个对象
  var module = {
    exports: {}
  }

  // 模块中同时还有一个成员 exports 等价于 module.exports
  var exports = module.exports

  console.log(exports === module.exports) // => true

  // 这样是可以的，因为 exports === module.exports
  // module.exports.a = 123
  // exports.b = 456

  // 这里重新赋值不管用，因为模块最后 return 的是 module.exports
  // exports = function () {
  // }

  // 这才是正确的方式
  module.exports = function () {
    console.log(123)
  }

  // 最后导出的是 module.exports
  return module.exports
}

var ret = fn()

console.log(ret)
```

### exports 和 module.exports 的区别

- exports 和 module.exports 的区别
  + 每个模块中都有一个 module 对象
  + module 对象中有一个 exports 对象
  + 我们可以把需要导出的成员都挂载到 module.exports 接口对象中
  + 也就是：`moudle.exports.xxx = xxx` 的方式
  + 但是每次都 `moudle.exports.xxx = xxx` 很麻烦，点儿的太多了
  + 所以 Node 为了你方便，同时在每一个模块中都提供了一个成员叫：`exports`
  + `exports === module.exports` 结果为  `true`
  + 所以对于：`moudle.exports.xxx = xxx` 的方式 完全可以：`expots.xxx = xxx`
  + 当一个模块需要导出单个成员的时候，这个时候必须使用：`module.exports = xxx` 的方式
  + 不要使用 `exports = xxx` 不管用
  + 因为每个模块最终向外 `return` 的是 **module.exports**
  + 而 `exports` 只是 `module.exports` 的一个引用
  + 所以即便你为 `exports = xx` 重新赋值，也不会影响 `module.exports`
  + 但是有一种赋值方式比较特殊：`exports = module.exports` 这个用来重新建立引用关系的
  + 之所以让大家明白这个道理，是希望可以更灵活的去用它

### 特殊的导出方式

```javascript
exports = module.exports = function () {
  console.log('默认函数被调用了')
}

exports.ajax = function () {
  console.log('ajax 方法被调用了')
}

exports.get = function () {
  console.log('get 方法被调用了')
}

```

## 深入模块加载机制

## 探索模块原理

