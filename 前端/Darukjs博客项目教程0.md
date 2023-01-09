# Darukjs博客项目教程0

话不多说，我们直接进入主题

## 功能设计

要做一个项目，首先我们得想考虑清楚它的基本功能，然后再根据功能需求来考虑技术选型，最后再进行实际开发。

我们的博客项目功能设计比较简单，首先考虑单纯前端部分需要的功能：

1. 文章
   1. 文章列表展示
   1. 文章搜索（搜索条件：根据关键词搜索，根据分类搜索）
   1. 文章分类
   2. 文章详情


2. 用户相关

  1. 登陆，注册
  2. 查看用户信息，查看自己发布的评论与回复
2. 评论相关
1. 登陆用户可以在文章下发布评论，回复
2. 匿名用户可以在文章下发布评论，回复



以上概括了一下我们的博客的前端部分的主要功能，简单整理一下前端部分需求后，可以比较快速的想到后台管理所需要的功能

1. 分类管理
   1. 查看全部
   2. 增删改查
2. 文章管理
   1. 查看全部
   2. 增删改查
2. 用户管理
   1. 查看全部
   2. 增删改查
2. 评论管理
   1. 查看全部
   2. 增删改查（可能不需要对用户的评论进行编辑修改）
2. 回复管理
   1. 查看全部
   2. 增删改查（可能不需要对用户的回复进行编辑修改）



## 技术选型

上面的功能设计可以说是非常的简单，非常人性化，我们做api开发的时候需要对这几个功能分别整个**增删改查** 就完事了。

现在开始技术选型，既然我们是前端选手，那就要用我们自己熟悉的语言，JavaScript——的儿子，**typescript**

#### 前端页面ui：

* vue

  方便好使。

* Nuxt.js

  因为博客是很需要做seo的，所以我们必须得做ssr，单页面是不够的。

#### api开发

* nodejs

  我们前端仔就用node

* Darukjs

  一个node的web框架，能让我们比较快速和方便的开发api接口，使用的是**typescript**

* MySQL

  经典数据库



## 开始开发

我们从api开发开始，首先兄弟们可以先去Darukjs看一下快速使用方法

https://darukjs.com/tutorial/starup.html#%E5%88%9D%E5%A7%8B%E5%8C%96%E6%9C%8D%E5%8A%A1%E6%96%B9%E6%B3%95

不看也行，一会儿直接看我代码



### 创建文件夹

千里之行始于足下

daruk对项目的目录结构是没有什么要求的，甚至它没有一个cli快速创建项目的工具（为了减少理解成本，作者说的）

那我们就随便创建一个文件夹就叫  `daruk-blog`

```shell
mkdir daruk-blog # 创建项目目录
cd daruk-blog # 进入项目目录
npm init # 使用 npm 初始化项目信息
npm install daruk ts-node typescript # 安装 Daruk 框架和 typescript
mkdir src # 创建源码目录
touch src/index.ts # 创建入口文件
touch tsconfig.json # 创建 typescript 的项目配置文件
```

 编写 web 应用 `src/index.ts`

```typescript
import { DarukServer, controller, get, DarukContext } from "daruk";

(async () => {
  const myapp = DarukServer();

  @controller()
  class Index {
    @get("/")
    public async index(ctx: DarukContext) {
      ctx.body = "hello world";
    }
  }

  await myapp.binding();
  myapp.listen(3000);
})();
```

编写 tsconfig.json 文件

```json
{
  "compileOnSave": true,
  "compilerOptions": {
    "target": "es2017",
    "module": "commonjs",
    "sourceMap": true,
    "outDir": "./build",
    "rootDir": "./src",
    "typeRoots": [],
    "types": [],
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  },
  "exclude": ["node_modules"],
  "include": ["./src/**/*.ts"]
}
```

编辑 `package.json` 的启动和编译脚本

```json
{
  "scripts": {
    "dev": "ts-node --project tsconfig.json --files src/index.ts",
    "build": "tsc"
  }
}
```

启动 Daruk 服务

```bash
npm run dev
```

不出意外的话，现在就可以直接访问 `localhost:3000`  看到hello world了



上面的代码中使用到了 typescript的装饰器语法，

简单来说，装饰器本质上是一种特殊的函数，它可以用在： 类，类属性，类方法，类访问器，类方法的参数

```typescript
// 类装饰器
@classDecorator
class Bird {

  // 属性装饰器
  @propertyDecorator
  name: string;
  
  // 方法装饰器
  @methodDecorator
  fly(
    // 参数装饰器
    @parameterDecorator
      meters: number
  ) {}
  
  // 访问器装饰器
  @accessorDecorator
  get egg() {}
}
```





### 总结
文本确定了博客的功能需求，并启动了一个最简单的daruk实例，

其实用起来跟koa2是很相似的，因为daruk就是基于koa2开发的，它里面封装了koa-body，koa-route等功能，如果了解koa2的兄弟应该上手非常简单。



这里涉及到两个需要兄弟去认真看下的文档

因为后续的文章会使用到很多daruk内置的装饰器，所以大家务必要先看一遍 [daruk文档](https://darukjs.com/tutorial/about.html)
如果不太理解装饰器是什么，可以参考这篇[TypeScript装饰器完全指南](http://blog.7hds.com/article/detail?id=10)