# npm 和 包

- node package manager
- 因为都是 JavaScript 所以慢慢的前端的包也放到了 npm 中
- 官方的对 npm 的定义：JavaScript Package Manager

## npm 的两层含义

### npm 网站

> npmjs.com，提供了存放数据包的能力，类似于 Github ，但是不提供版本管理
>
> 开发者可以把一些 JavaScript 相关工具放到这个系统中

### npm 命令行工具

npm 的第二层含义就是一个命令行工具，只要你安装了 node 就已经安装了 npm。

npm 也有版本这个概念。

可以通过在命令行中输入：

```shell
npm --version
```

升级 npm（自己升级自己）：

```shell
npm install --global npm
```

## 常用命令

- `npm init`
  - `npm init -y` 可以跳过向导，快速生成，随后可以自己手动改文件
- `npm install`
  - 一次性把 dependencies 选项中的依赖项全部安装
  - npm i
- `npm install 包名 [包名]...`
  - npm 5 以前只下载，不会保存依赖信息，如果需要保存，则需要加上 `--save` 选项
  - `npm i 包名`
- `npm install 报名@版本号`
- `npm install --save 包名`
  - 下载并且保存依赖项（package.json 文件中的 dependencies 选项）
  - npm i -S 包名
- `npm uninstall 包名`
  - npm 5 以前只删除，如果有依赖项会依然保存
  - npm 5 以后会自动移除依赖信息
  - npm un 包名
- `npm uninstall --save 包名`
  - 删除的同时也会把依赖信息也去除
  - npm un -S 包名
- npm install --global 包名
  - 不是安装到项目中
  - 而是安装到全局（肯定在计算机里面）
- `npm help`
  - 查看使用帮助
- `npm 命令 --help`
  - 查看指定命令的使用帮助
  - 例如我忘记了 uninstall 命令的简写了，这个时候，可以输入 `npm uninstall --help` 来查看使用帮助

## npm 安装全局包

我们除了可以使用 npm 安装项目中适应的依赖包之外，还有一些包比较特殊，这种不是在项目 `require()` 使用的，而是
通过全局安装之后使用它提供的命令来完成某种功能，就像咱们学过的 git 一样。

> 凡是往全局安装的包我们称之为全局命令行工具，这种包安装完毕之后会在命令行中为你提供了一个命令来完成某种功能。
>
> 程序交互方式分为两种：
>
> - GUI（Graphical User Interface）图形交互软件
> - CLI（Command Line Interface） 命令行交互软件，某些方面命令行操作要比图形界面更高效
> - 《浪潮之巅》
> - 施乐 -》 苹果、微软

> 提示：安装全局包必须加 `--global` 参数
> 安装全局包在哪里执行都可以

### `http-server`

### `nodemon`

### `less`

> 参考文档：http://lesscss.org/
> 作用：支持在命令行中打命令完成 less 文件的编译

安装：

```shell
npm i -g less
```

安装完毕之后，我们可以在终端中输入以下命令进行测试：

```shell
# 因为在我们的 Linus 操作系统中，有一个操作系统默认命令：less
lessc --version
```

基本使用：

```shell
# 编译 less 文件，将结束输出到命令行中
lessc styles.less

# 编译 less 文件，将结果输出到指定的路径
lessc styles.less styles.css
```

### browser-sync

> 参考文档：https://browsersync.io/
> 作用：文件改变浏览器自动刷新（同步测试）

安装：

```shell
npm install -g browser-sync
```

基本使用：

```shell
browser-sync start --server --files "css/*.css, js/*.js, *.html"
```

### 全局包到底安装到了哪里

我们可以通过在命令行输入来查看全局包的安装路径：

```shell
npm root -g
```

现在这个东西只需要了解就行了。

因为以前的 node 版本一旦升级就会导致丢失了全局包，新版的 node 已经没有这个问题了。

## 解决 npm 被墙问题

> 参考文档：http://npm.taobao.org/

npm 存储包文件的服务器在国外，有时候会被墙，速度很慢，所以我们需要解决这个问题。

国内淘宝的开发团队把 npm 在国内做了一个备份，网址是：http://npm.taobao.org/。

最简单的方式就是我们在安装包的时候告诉 npm 你去哪个服务器下载。

例如使用淘宝的 npm 镜像源下载 jquery:

```shell
npm install jquery --registry=https://registry.npm.taobao.org
```

但是每次手动往后面加 `--registry=https://registry.npm.taobao.org` 很麻烦，
所以我们可以通过修改配置文件的方式来处理解决。

```shell
# 配置到淘宝服务器
npm config set registry https://registry.npm.taobao.org

# 查看 npm 配置信息
npm config list
```

只要经过了上面命令的配置，则你以后所有的 `npm install` 都会使用你配置的 `registry` 下载。

## package.json

我们的项目会放到云端的仓库中，例如 github ，第三方包没有上传的意义，我们只需要把我们的源码放到云端仓库，`node_modules` 目录中存储的就是第三方包（不用担心丢失问题），如果没有 `package.json` 文件则你就找不回来了。



我们建议每一个项目都要有一个 `package.json` 文件（包描述文件，就像产品的说明书一样），给人踏实的感觉最重要的就是保存这个项目的第三方依赖信息（因为我们不需要提交第三方包到我们的云端仓库，只需要提交我们自己的代码），有了这个文件中的依赖信息结合 `npm install` 命令我们就可以放心了。

这个文件可以通过 `npm init` 的方式来自动初始化出来。

```javascript
C:\Users\lpz\Desktop\npm-demo>npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (npm-demo) 项目的名称
version: (1.0.0) 0.0.1 项目版本
description: 项目简介
entry point: (index.js) main.js 项目入口
test command: 测试命令，暂且不用关系
git repository: 仓库地址
keywords: 关键字，如果是一个开源项目，则可以填一些关键字被别人在 npm 中搜索到
author: lipengzhou 项目的开发者
license: (ISC) 开源协议
About to write to C:\Users\lpz\Desktop\npm-demo\package.json:

{
  "name": "npm-demo",
  "version": "0.0.1",
  "description": "这是一个测试项目",
  "main": "main.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "lipengzhou",
  "license": "ISC"
}


Is this ok? (yes) yes
```

对于咱们目前来讲，最有用的是那个 `dependencies` 选项，可以用来帮我们保存第三方包的依赖信息。

如果你的 `node_modules` 删除了也不用担心，我们只需要：`npm install` 就会自动把 `package.json` 中的 `dependencies` 中所有的依赖项都下载回来。

- **建议每个项目的根目录下都有一个 `package.json` 文件**
  - 不同的项目有不同依赖，各自保存各自的
- 建议执行 `npm install 包名的` 的时候都加上 `--save` 这个选项，目的是用来保存依赖项信息
  - npm 5 以前不会自动保存依赖信息到 package.json 文件中，必须手动加 `--save` 选项才可以
  - npm 5 以后不需要加 `--save` 选项了，因为它会自动保存依赖项

### dependencies

### main

### scripts

## package-lock.json

npm 5 以前是不会有 `package-lock.json` 这个文件的。（被开发者诟病，吐槽的问题）。

以前会自作多情的自动给你升级。

npm 5 以后才加入了这个文件。

当你安装包的时候，npm 都会生成或者更新 `package-lock.json` 这个文件。

- npm 5 以后的版本安装包不需要加 `--save` 参数，它会自动保存依赖信息
- 当你安装包的时候，会自动创建或者是更新 `package-lock.json` 这个文件
- `package-lock.json` 这个文件会保存 `node_modules` 中所有包的信息（版本、下载地址）
  - 这样的话重新 `npm install` 的时候速度就可以提升
- 从文件来看，有一个 `lock` 称之为锁
  - 这个 `lock` 是用来锁定版本的
  - 如果项目依赖了 `1.1.1` 版本
  - 如果你重新 isntall 其实会下载最新版本，而不是 1.1.1
  - 我们的目的就是希望可以锁住 1.1.1 这个版本
  - 所以这个 `package-lock.json` 这个文件的另一个作用就是锁定版本号，防止自动升级新版

