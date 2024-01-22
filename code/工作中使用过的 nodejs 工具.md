[toc]

# 工作中使用过的 nodejs 工具

# nodejs

> Node.js 是一个基于 Chrome V8 引擎的 Javascript 运行环境

# 工具包

## nvm

nvm 是什么？
nvm（Node Version Manager）是 node 版本管理器。

nvm 可以做些什么？

1. nvm 是命令行安装有专业范，更加方便。
2. nvm 上可以列出所有的 nodejs 版本。
3. nvm 可以安装多个版本并可以在多个版本中来回切换。

**nvm 安装使用：**

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash

nvm --version // 查看版本号

nvm ls-remote // 列出所有node版本

nvm install v12.6.0 // 安装node版本

node --version // 查看nodejs版本号

nvm ls // 查看nvm上安装的所以node版本

nvm alias default v12.6.0 // 切换版本
nvm use v12.6.0 // 切换版本
```

## npm

npm 是什么？
npm 是随同 nodejs 一起下载的包管理工具，世界上最大的可复用代码的仓库。

npm 解决了什么问题：

1. npm 是避免开发者重复造轮子，让大家的劳动成果可以共享。
2. npm 包安装以后会将包名和版本号记录在 package.json 之中。
3. npm 实际下载的包文件会存放在根目录的 mode-modules 之中。
4. 可以使用`require('包名')`引入它。

## nodemon

nodemon 是什么？
nodemon 是一种工具，可以自动检测到目录中的文件更改时通过重新启动应用程序来调试基于 node.js 的应用程序。

**安装**

```
npm install -g nodemon
//或
npm install --save-dev nodemon
```

**使用**

```
nodemon   ./main.js // 启动node服务

nodemon ./main.js localhost 6677 // 在本地6677端口启动node服务

"start": "ts-node -r tsconfig-paths/register nodemon src/main.ts",
```

**延时启用**

```
nodemon -delay10 main.js

nodemon --delay 2.5 server.js

nodemon --delay 2500ms server.js
```

## sequelize

使用 sequelize 的好处是不需要再使用 sql 语句，查数据库的操作和操作对象是相似的，把数据库映射成对象，对对象进行操作

**安装**

```
npm install --save sequelize
```

## bcryptjs

bcryptjs 是 nodejs 中比较好的一款加盐(salt)加密的包.

**安装**

```
npm install bcryptjs
```

## jsonwebtoken(jwt)

JSON Web Token 是一个开放标准协议，它定义了一种紧凑和自包含的方式，它用于各方之间作为 JSON 对象安全地传输信息。

// TODO
