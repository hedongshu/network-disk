# Darukjs博客项目-部署流程

当全部的crud接口都开发完成之后，我们就要把项目部署到服务器上了。

首先你需要有一个服务器，我这边服务器是用的腾讯云ubuntu。



ts的web项目一般的部署流程如下：

1. 在本地把代码打包成js。
2. 把打包后的文件上传到服务器。
3. 服务器用 pm2 启动，加上watch参数。后续我们只需要往服务器上传修改后的js文件就行，无需再用pm2启动。
4. 用nginx反向代理，让用户能通过域名或者ip访问。





##  打包

我们使用tsc把ts代码打包成js

```shell
npm run build


// 事实上，它运行的是下面的代码，只是写在package里面，用npm run xxx 来运行比较方便

NODE_ENV=production tsc
```



打包的配置是 tsconfig.json

```json
{
    "compileOnSave": true,
    "compilerOptions": {
        /* 基本选项 */
        "target": "es2017", // 指定 ECMAScript 目标版本: 'ES3' (default), 'ES5', 'ES6'/'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'
        "module": "commonjs", // 指定使用模块: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
        // "lib": [], // 指定要包含在编译中的库文件
        "allowJs": true, // 允许编译 javascript 文件
        "checkJs": true, // 报告 javascript 文件中的错误
        // "jsx": "preserve", // 指定 jsx 代码的生成: 'preserve', 'react-native', or 'react'
        // "declaration": true, // 生成相应的 '.d.ts' 文件
        "sourceMap": true, // 生成相应的 '.map' 文件
        // "outFile": "./", // 将输出文件合并为一个文件
        "outDir": "./build", // 指定输出目录
        "rootDir": "src", // 用来控制输出目录结构 --outDir.
        "removeComments": true, // 删除编译后的所有的注释
        // "noEmit": true, // 不生成输出文件
        // "importHelpers": true, // 从 tslib 导入辅助工具函数
        // "isolatedModules": true, // 将每个文件作为单独的模块 （与 'ts.transpileModule' 类似）.
        /* 严格的类型检查选项 */
        // "strict": true, // 启用所有严格类型检查选项
        // "noImplicitAny": true, // 在表达式和声明上有隐含的 any类型时报错
        // "strictNullChecks": true, // 启用严格的 null 检查
        // "noImplicitThis": true, // 当 this 表达式值为 any 类型的时候，生成一个错误
        // "alwaysStrict": true, // 以严格模式检查每个模块，并在每个文件里加入 'use strict'
        /* 额外的检查 */
        // "noUnusedLocals": true, // 有未使用的变量时，抛出错误
        // "noUnusedParameters": true, // 有未使用的参数时，抛出错误
        // "noImplicitReturns": true, // 并不是所有函数里的代码都有返回值时，抛出错误
        // "noFallthroughCasesInSwitch": true, // 报告 switch 语句的 fallthrough 错误。（即，不允许 switch 的 case 语句贯穿）
        /* 模块解析选项 */
        // "moduleResolution": "node", // 选择模块解析策略： 'node' (Node.js) or 'classic' (TypeScript pre-1.6)
        // "baseUrl": "./", // 用于解析非相对模块名称的基目录
        // "paths": {}, // 模块名到基于 baseUrl 的路径映射的列表
        // "rootDirs": [], // 根文件夹列表，其组合内容表示项目运行时的结构内容
        "typeRoots": [], // 包含类型声明的文件列表
        "types": [], // 需要包含的类型声明文件名列表
        // "allowSyntheticDefaultImports": true, // 允许从没有设置默认导出的模块中默认导入。
        /* Source Map Options */
        // "sourceRoot": "./", // 指定调试器应该找到 TypeScript 文件而不是源文件的位置
        // "mapRoot": "./", // 指定调试器应该找到映射文件而不是生成文件的位置
        // "inlineSourceMap": true, // 生成单个 soucemaps 文件，而不是将 sourcemaps 生成不同的文件
        // "inlineSources": true, // 将代码与 sourcemaps 生成到一个文件中，要求同时设置了 --inlineSourceMap 或 --sourceMap 属性
        // /* 其他选项 */
        "experimentalDecorators": true, // 启用装饰器
        "emitDecoratorMetadata": true, // 为装饰器提供元数据的支持,
        "esModuleInterop": true,
    },
    "exclude": [
        "node_modules"
    ],
    "include": [
        "src/**/*.ts"
    ]
}
```

你可以直接使用我的配置，但是请务必都看一遍。





## 上传到服务器

我使用scp来上传文件。   这需要你的服务器能支持ssh连接，现在一般的云服务器都默认支持这个功能，可以去找找你的服务器的资料，我这里就不细嗦了。



```shell
scp -r ./dist user@1.1.1.1:/user/home/dist

// scp命令用来远程复制，-r 是指文件夹（递归复制子文件），后面的就是本地地址和远程地址
```

点击[这里](https://blog.7hds.com/article/detail?id=21)，查看scp命令的使用方法





## pm2启动

上传到服务器之后，进入到那个目录， 运行下面这个命令

```shell
export NODE_ENV=production && pm2 start build/index.js --watch --name=blog-api
```



这里用到了pm2， 文档[在这](https://pm2.keymetrics.io/docs/usage/quick-start/)

> PM2 是一个守护进程管理器，它将帮助您管理和保持您的应用程序



现在运行 `pm2 list` 就能看到pm2运行的服务啦 

我们用pm2启动，其实跟你在本地 `npm run dev` 区别不大，只是它是在后台跑着，哪怕你关掉了终端，它也还在跑着。

pm2启动后，监听的端口就是你在darukjs里面写的端口，所以现在访问的方式是

你的ip地址: 端口

```
12.12.12.12:3000
```





## nginx代理

上面我们用pm2启动之后，访问需要用ip地址加端口的方式。现在我们想要用域名访问，所以要用上nginx来做反向代理



> Nginx是异步框架的网页服务器，也可以用作反向代理、负载平衡器和HTTP缓存。

这是[入门教程](https://github.com/dunwu/nginx-tutorial )，都给兄弟们准备好了。



一个简单的nginx配置如下

```json
upstream nodenuxt {
    server 127.0.0.1:3000; #nuxt项目 监听端口
    keepalive 64;
}

server {
    listen 80;
    server_name 你的域名写这里;

    location / {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Nginx-Proxy true;
        proxy_cache_bypass $http_upgrade;
        proxy_pass http://nodenuxt; #反向代理
    }

    location /admin {
      alias /home/ubuntu/docker-project/hds-blog/static/admin/;
      index  index.html;
      try_files $uri $uri/ /index.html;
    }
}

```



现在检查一下nginx配置有没有错误 `nginx -t`   ,  然后重新启动nginx `systemctl restart nginx`

不出意外，现在你就可以用域名访问了。





## 自动化

当你更新修改了daruk项目之后，想要部署到服务器，你需要在本地先编译，然后上传。（pm2 用了watch参数启动，当文件有变动它会自动更新的）

那么这两步能不能合并一下呢

我们可以写一个node的脚本来实现自动部署



```js
//  deploy.js
// ora   // 一个进度条插件 让等待不在无聊的插件
// zip-local   // 压缩文件的插件 用来压缩打包好的静态文件
// shelljs   // 用来在代码中执行命令行操作
// chalk   // 用于控制台带颜色的输出 告别纯白字体
// inquirer   // 用于命令行交互 使脚本更灵活
// node-ssh   // 用来连接服务器

const fs = require("fs");
const path = require("path");
const ora = require("ora");
const shell = require("shelljs");
const chalk = require("chalk");
const CONFIG = require(path.resolve(process.cwd(), "./deploy.config.js"));
const inquirer = require("inquirer");
const NodeSSH = require("node-ssh").NodeSSH;
const SSH = new NodeSSH();
const errorLog = (error) => console.log(chalk.red(`---------------> ${error}`));
const defaultLog = (log) => console.log(chalk.blue(`---------------> ${log}`));
const successLog = (log) => console.log(chalk.green(`---------------> ${log}`));
// 文件夹位置

// 打包 npm run build
const compileDist = async () => {
  // 进入本地文件夹
  shell.cd(path.resolve(process.cwd(), "./"));
  shell.exec(CONFIG.SCRIPT || "npm run build");
  successLog("编译完成");
};

// ********* 执行线上文件夹指令 *********
const runCommond = async (commond) => {
  const result = await SSH.exec(commond, [], { cwd: CONFIG.PATH });

  console.log(chalk.yellow(result));
};
const COMMONDS = CONFIG.COMMONDS || [];
// ********* 执行线上文件夹指令 *********
const runAfterCommand = async () => {
  defaultLog("执行命令");
  for (let i = 0; i < COMMONDS.length; i++) {
    await runCommond(COMMONDS[i]);
  }
};

// ********* 通过ssh 上传文件到服务器 *********
const uploadZipBySSH = async () => {
  // 上传文件
  const spinner = ora("准备上传文件").start();
  try {
    for (let i = 0; i < CONFIG.DIST.length; i++) {
      const item = CONFIG.DIST[i];

      const thepath = path.resolve(process.cwd(), item);
      defaultLog(`文件名： ${thepath} : ${CONFIG.PATH + item}`);
      const stat = fs.lstatSync(thepath);
      stat.isDirectory()
        ? await SSH.putDirectory(thepath, CONFIG.PATH + item)
        : await SSH.putFile(thepath, CONFIG.PATH + item);
    }

    successLog("完成上传");
    successLog("部署完成");
    process.exit(0);
  } catch (error) {
    errorLog(error);
    errorLog("上传失败");
  }
  spinner.stop();
};

// ********* 连接ssh *********
const connectSSh = async () => {
  defaultLog(`尝试连接服务： ${CONFIG.SERVER_PATH}`);
  const spinner = ora("正在连接");
  spinner.start();
  try {
    spinner.stop();
    SSH.connect({
      host: CONFIG.SERVER_PATH,
      username: CONFIG.SSH_USER,
      password: CONFIG.SSH_KEY,
      port: CONFIG.PORT || 22,
    }).then(async () => {
      await uploadZipBySSH();
      await runAfterCommand();
    });
    successLog("SSH 连接成功");
  } catch (error) {
    errorLog(error);
    errorLog("SSH 连接失败");
  }
};

// 执行打包上传命令
async function runTask(hasBuild) {
  if (hasBuild) {
    await compileDist();
  }
  await connectSSh();
}

console.log(CONFIG);

defaultLog("服务器配置检查");
inquirer
  .prompt([
    {
      type: "confirm",
      name: "hasBuild",
      message: "是否需要打包",
    },
  ])
  .then((answers) => {
    const hasBuild = answers.hasBuild;

    runTask(hasBuild);
  });

```



这个脚本可以执行打包，和上传的命令，你需要新建一个`deploy.config.js`

```js
// deploy.config.js
module.exports = Object.freeze({
  SERVER_PATH: "xxx", // 服务器ip地址
  SSH_USER: "xxx", // 服务器用户名
  SSH_KEY: "xxx", // 服务器密码
  PORT: 22, // 端口 默认为22
  DIST: ["build", "package.json", "yarn.lock"], // 需要部署的打包过后的文件夹 根据项目不同值不同 一般为 build static 或者dist
  SCRIPT: "npm run build", // 打包命令 可能项目由不同的构建命令 如打包指定环境的代码
  PATH: "/home/ubuntu/xxxx/", // 服务器存放静态文件目录  记得末尾的 /
  //COMMONDS: [`ls`, `rm index.html`, `rm -r css/`, `rm -r js/`] // 上传前执行的命令 本例是指 查看服务器对应/opt/book/admin下的文件 然后删除index.html,css,js这写文件和目录,  之后会执行上传本地文件到服务器操作
});

```

然后在package.json里面添加一条命令

```
"deploy": "node deploy"
```

现在只需要运行一下 ` npm run deploy`   ,   就能完成打包和部署了。
