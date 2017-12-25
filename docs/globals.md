# 全局成员

> 参考文档：https://nodejs.org/dist/latest-v9.x/docs/api/globals.html

- Class: Buffer
- __dirname
- __filename
- clearImmediate(immediateObject)
- clearInterval(intervalObject)
- clearTimeout(timeoutObject)
- console
- exports
- global
- module
- process
- require()
- setImmediate(callback[, ...args])
- setInterval(callback, delay[, ...args])
- setTimeout(callback, delay[, ...args])

## `__dirname` 和 `filename`

在每个模块中，除了 `require`、`exports` 等模块相关 API 之外，还有两个特殊的成员：

- `__dirname` **动态获取** 可以用来获取当前文件模块所属目录的绝对路径
- `__filename` **动态获取** 可以用来获取当前文件的绝对路径
- `__dirname` 和 `__filename` 是不受执行 node 命令所属路径影响的

在文件操作中，使用相对路径是不可靠的，因为在 Node 中文件操作的路径被设计为相对于执行 node 命令所处的路径（不是bug，人家这样设计是有使用场景）。

所了为了解决这个问题，很简单，只需要把相对路径变为绝对路径就可以了。

那这里我们就可以使用 `__dirname` 或者 `__filename` 来帮我们解决这个问题了。

在拼接路径的过程中，为了避免手动拼接带来的一些低级错误，所以推荐多使用：`path.join()` 来辅助拼接。

所以为了尽量避免刚才所描述这个问题，大家以后在文件操作中使用的相对路径都统一转换为 **动态的绝对路径**。

> 补充：模块中的路径标识和这里的路径没关系，不受影响（相对于文件模块）

---

