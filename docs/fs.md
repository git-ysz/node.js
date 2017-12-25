# 文件操作 fs

> 参考文档：https://nodejs.org/dist/latest-v9.x/docs/api/fs.html

## 同步和异步

对于文件操作，Node 几乎为所有的文件操作 API 提供了同步操作和异步操作两种方式。

- 同步会阻塞程序的执行，效率低（知道就行）
- 异步相当于多找了一个人帮你干活，效率高
- 所以建议：尽量使用异步

## 常用 API

| API                                      | 作用        | 备注      |
| ---------------------------------------- | --------- | ------- |
| fs.access(path, callback)                | 判断路径是否存在  |         |
| fs.appendFile(file, data, callback)      | 向文件中追加内容  |         |
| fs.copyFile(src, callback)               | 复制文件      |         |
| fs.mkdir(path, callback)                 | 创建目录      |         |
| fs.readDir(path, callback)               | 读取目录列表    |         |
| fs.rename(oldPath, newPath, callback)    | 重命名文件/目录  |         |
| fs.rmdir(path, callback)                 | 删除目录      | 只能删除空目录 |
| fs.stat(path, callback)                  | 获取文件/目录信息 |         |
| fs.unlink(path, callback)                | 删除文件      |         |
| fs.watch(filename[, options]\[, listener]) | 监视文件/目录   |         |
| fs.watchFile(filename[, options], listener) | 监视文件      |         |

## 案例：Markdown 文件转换器

> 需求：用户编写 md 格式的文件，实时的编译成 html 文件
>
> 使用 Node 提供的 API 结合 EcmaScript 6 语法

## 文件操作的路径

```javascript
// 相对于当前路径
fs.readFile('./README.md')

// 相对当前路径，可以省略 ./
// 注意：加载模块中的标识路径不能省略 ./
fs.readFile('README.md')

// 绝对路径
fs.readFile('c:/README.md')

// 绝对路径，当前 js 脚本所处磁盘根目录
fs.readFile('/README.md')
```

## 文件操作的相对路径问题

> 建议：以后操作文件使用相对路径都使用 `path.join()` 方法结合 `__dirname` 来避免问题。
