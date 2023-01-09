[TOC]

# Darukjs博客项目教程1

上篇文章我们设计了功能，然后启动了一个简单实例。

到这的兄弟们应该大概了解了装饰器的用法，现在我们就要开始实际开发了。

## 丰富目录结构

之前的目录只是一个demo，现在我们要创建更多的目录用来放不同的功能代码，当然如果你认为你更喜欢放在一起也可以。

```
├── config
│   └── config.ts    //数据库等配置信息
├── controllers			 //
│   ├── admin.ts
├── entity  				 // typeorm 实体
│   ├── admin.ts
├── glues            // 胶水层
│   ├── auth.ts
│   ├── connection.ts  // typeorm 数据库连接
│   ├── http-exception.ts
│   ├── resTypes.ts
│   └── validator.ts
├── index.ts    // 入口文件
├── middlewares
		├── catch-error.ts
│   └── koa-favicon.ts  // koa中间件
├── public
│   └── favicon.png
├── services
│   ├── admin
│   │   └── index.ts
├── typings
│   └── global.d.ts
├── util
│   └── index.ts
├── validators
│   ├── admin.ts      //参数校验
```





## 入口文件index.ts

```typescript
import { DarukServer } from "daruk";

// 因为部分实例方法是异步的，所以我们一般都在启动服务器时，用 async 匿名函数来配合 await 简化我们的代码。
(async () => {
  let app = DarukServer({
    rootPath: __dirname,
    middlewareOrder: ["koa-cors", "catch-error"], //中间件，写进数组按顺序加载
    validateOptions: {
      error: true, // 验证器配置，error表示是否抛出错误
    },
  });

  let port = process.env.NODE_ENV !== "production" ? 3000 : 3001;
	
  
  // 加载路径下的ts文件
  await app.loadFile("./middlewares");
  await app.loadFile("./glues");
  await app.loadFile("./controllers");
  await app.loadFile("./services");
  
  await Db.createConnection(); //创建数据库连接，下面会有提到
  
  await app.binding();
  
  

  app.listen(port);
  app.logger.info(`app listen port ${port}`);
})();

```



可以看到在index.ts中调用了多次`loadfile`, 简单的说loadfile作用就是目录下的代码，Daruk 会帮助你遍历递归执行这些目录中的 ts 和 js 文件。

他们没有前后顺序一说，因为我们的`源文件`中定义的应该都是不可执行的类文件，这些文件会因为你选择的不同功能的装饰器，一起 bind 到 Daruk 的 IoC 容器中，并不会实际执行，当然除非你在这些目录的文件里写了一些自执行代码，那么它们可能将会立即执行。

比如` await app.loadFile("./middlewares");` 就是加载了`src/middlewares`目录下的全部ts文件，即全部的中间件。

这里用到了两个中间件，`koa-cors`是koa的，可以直接npm安装，但是需要在daruk里包装一下才能使用。`catch-error`是我需要自己写的，两个文件都放在`middlewares`文件夹



## 中间件

```typescript
// src/middlewares/koa-cors.ts

const cors = require("@koa/cors");

import { join } from "path";
import { Daruk, defineMiddleware, injectable, MiddlewareClass } from "daruk";

// 定义一个中间件，defineMiddleware的参数就是中间件的名字，写在index.ts	的middlewareOrder数组内就可以加载
@defineMiddleware("koa-cors")   
class koaCors implements MiddlewareClass {
  public initMiddleware(daruk: Daruk) {
    daruk.app.use(cors());
  }
}

```



```typescript
// src/middlewares/catch-error.ts


import { errors } from "../glues/http-exception"; //这里定义了一些错误类型，方便快速抛出错误

import { config } from "../config/config";

import {
  Daruk,
  defineMiddleware,
  injectable,
  MiddlewareClass,
  Next,
  DarukContext,
} from "daruk";

@defineMiddleware("catch-error")
class CatchError implements MiddlewareClass {
  public initMiddleware(daruk: Daruk) {
    return async (ctx: DarukContext, next: Next) => {
      try {
        await next();  //不处理，直接调用next，在下面的catch里捕获错误
      } catch (error) {
        // 开发环境
        const isHttpException = error instanceof errors.HttpException;
        //   const isDev = global.config.environment === "dev";
        const isDev = config.environment === "dev";
        // console.log('error1', error)
        if (isDev && !isHttpException) {
          throw error;
        }

        // 生产环境
        if (isHttpException) {
          ctx.body = {
            msg: error.msg,
            error_code: error.errorCode,
            request: `${ctx.method} ${ctx.path}`,
          };
          ctx.response.status = error.code;
        } else {
          ctx.body = {
            msg: "未知错误！",
            error_code: 9999,
            request: `${ctx.method} ${ctx.path}`,
          };
          ctx.response.status = 500;
        }
      }
    };
  }
}

```



## 数据库连接

我把创建数据库连接的方法放在`gules`目录下，然后在入口文件index.ts中执行一次` await Db.createConnection();`就可以了。

```typescript
// src/gules/connection.ts

const path = require("path");
import { Connection, createConnection, EntityTarget } from "typeorm";
import { fluentProvide } from "daruk";

import { config } from "../config/config";

const { dbName, host, port, user, password, charset } = config.database;

import { Admin } from "../entity/admin";
import { Article } from "../entity/article";
import { Category } from "../entity/category";
import { Comment } from "../entity/comment";
import { Reply } from "../entity/reply";
import { User } from "../entity/user";

@(fluentProvide("Db").inSingletonScope().done())
export default class Db {
  static connection: Connection;
	
  
  //	获取数据库连接
  static async getConnection() {
    if (!this.connection) {
      this.connection = await createConnection({
        type: "mysql",
        port: port,
        host: host,
        username: user,
        password: password,
        database: dbName,
        timezone: "local",
        entities: [Admin, Article, Category, Comment, Reply, User], //加载实体
        synchronize: true,
        charset: charset,
        logging: process.env.NODE_ENV === "dev",
      });
    }

    return this.connection;
  }

  static async getRepository<entity>(target: EntityTarget<entity>) {
    let c = await this.getConnection();
    return c.getRepository(target);
  }
}

```

连接数据库，我们使用的是typeorm来简化操作，可以让我们操作实体对象来达到操作数据库的目的，可以避免手写sql语句

请自行学习typeorm的使用方法，可以看看[官方文档](https://typeorm.biunav.com/zh/) ，里面有使用案例写的非常详细



## 第一个API
要开发一个api，我们需要走个流程
首先是  controller 控制器， 比如我们先创建一个 管理员控制器 , 在 controllers 文件夹中

```typescript
// src/controllers/admin.ts
import {
  controller,
  DarukContext,
  get,
  inject,
  injectable,
  middleware,
  Next,
  post,
  prefix,
  type,
  validate,
} from "daruk";

import { AdminLoginValidator, RegisterValidator } from "../validators/admin";
import { auth } from "../glues/auth";
import { AdminSer } from "../services/admin";
import { ResTypes } from "../glues/resTypes";
import { generateToken } from "../util";
import { errors } from "../glues/http-exception";
import { Admin } from "../entity/admin";

@controller()    // 定义一个控制器
@prefix("/api/v1/admin")  // 给路由地址添加一个前缀
class AdminController {
  
  @inject("AdminSer")   
  public adminSer: AdminSer;

  //   管理员注册
  @post("/register")
  @validate(RegisterValidator)
  public async register(ctx: DarukContext, next: Next) {
    const body = ctx.request.body as typeof RegisterValidator;

    const { email, password1, nickname } = body;
    const [error, data] = await this.adminSer.create({
      email,
      password: password1,
      nickname,
    });

    if (!error) {
      // 返回结果
      ctx.body = ResTypes.json(data);
    } else {
      ctx.body = ResTypes.fail(error);
    }
  }

 	......
}


```



这里用到了几个新的装饰器

@controller()    定义一个装饰器

@prefix("/api/v1/admin")   给路由地址添加一个前缀

@post("/register") 定义一个路由，method为post，  因为上面有prefix，所以完整的访问地址就是 /api/v1/admin/register



代码非常好理解，我们要实现一个创建管理员的功能，当用户post请求 /api/v1/admin/register 的时候，首先要验证一下post传过来的参数，验证通过之后，**我们在数据库里查询管理员是否存在，如果不存在，就在数据库里创建一个管理员**，并返回给用户

这里我们把上面文字加粗的部分写在另一个地方，并不在controller里



### 参数验证

参数验证是用了daruk的一个装饰器 @validate(RegisterValidator) ，相关用法可以点击查看[daruk文档](https://doc.darukjs.com/index.html#validate)，而大乳鸽的参数验证是基于parameter实现的的，所以你完全可以去看parameter的[文档](https://www.npmjs.com/package/parameter)

```typescript
// 简单的使用如下
@validate({
  name:{
    type:'string',
    required: true,
  }
  passward:{
  	type:'string',
  	required: true
	}
}) 
 public async register(){}
```

我们这里把参数验证放在另一个文件 `src/validators/admin.ts` ，然后引入进来 `@validate('这里就是引入的') ` 

之后我们所有的参数验证代码都放在这个文件夹中

```typescript
// src/validators/admin.ts
import { RulesType } from "daruk-validate";

export const RegisterValidator: RulesType = {
  nickname: {
    type: "string",
    min: 2,
    max: 16,
    required: true,
  },
  email: {
    type: "email",
    required: true,
  },
  password1: {
    type: "password",
    min: 6,
    max: 22,
  },
  password2: {
    compare: "password1",
    type: "password",
    min: 6,
    max: 22,
  },
};

export const AdminLoginValidator: RulesType = {
  email: {
    type: "email",
    required: true,
  },
  password: {
    type: "password",
    min: 6,
    max: 22,
  },
};

```





### 数据库操作

好，参数验证通过了，现在我们开始下一步操作

因为我们不想把数据库的相关操作放在controller里面，所以我又创建了一个新的目录 `src/services` ,  在里面创建一个`adminSer.ts`

```typescript
//src/services/adminSer.ts
import { service, DarukContext, inject, provide } from "daruk";
import { Admin } from "../../entity/admin";
import Db from "../../glues/connection";
import { errors } from "../../glues/http-exception";
import * as crypto from "crypto-js";
import { generateToken } from "../../util";

@service()
export class AdminSer {
  public ctx!: DarukContext;
  @inject("Db") public Db!: Db;    // 这里的!:  是typescript的一个特殊写法，非空断言

  /**创建管理员 */
  public async create(params) {
    const { email, password, nickname } = params;

    const isExist = await (
      await Db.getRepository(Admin)
    ).findOne({ email: email, deleted_at: null });

    if (Boolean(isExist)) {
      throw new errors.Existing("管理员已存在");
    }

    console.log("isExist", isExist);

    const admin = new Admin();
    admin.nickname = nickname;
    admin.password = crypto.MD5(password).toString();   // md5加个密
    admin.email = email;

    try {
      const res = await Db.connection.manager.save(admin);

      const data = {
        email: res.email,
        nickname: res.nickname,
      };

      return [null, data];
    } catch (error) {
      return [error, null];
    }
  }

  …………
}

```

这里我们inject了Db，`@inject("Db") public Db!: Db; `  

就是把之前创建的glues/connection.ts 注入到了 Db属性里，用的时候比较方便

 `public Db!: Db;` 这里前一个Db是属性名，后一个Db是类型。我们是直接 `import Db from "../../glues/connection";` 把Db引入了进来当作类型使用。（typescript里面可以把class当作类型来用）

虽然是当作类型使用，但是它还是一个class，所以我们还是可以直接用引入进来的Db

所以这里可以省略掉 @inject 装饰器定义的Db属性，把Db这个class引入进来也能直接用

（这里我认为就非常愚蠢，@inject装饰的属性为了能有类型定义，还要重复引入一次，有点脱了裤子放屁的意思）



回到代码

```typescript
 return [null, data];
```

返回的是一个长度为2的数组，第一位是error错误，第二位是数据，如果有错误就返回错误error，一切正常就返回数据data

然后我们在controller中拿到这个结果

```
const [error, data] = await this.adminSer.create({
      email,
      password: password1,
      nickname,
    });

    if (!error) {   //如果没有error就回复数据
      // 返回结果
      ctx.body = ResTypes.json(data);
    } else {
    	// 有错误就回复错误信息
      ctx.body = ResTypes.fail(error);
    }
```

这里的 ResTypes是我提前封装好的几个响应格式，要用的时候直接调用就行

```typescript
//src/gules/resTypes.ts

export class ResTypes {
  static fail(err = {}, msg = "操作失败", errorCode = 10001) {
    return {
      msg,
      err,
      errorCode,
    };
  }

  static success(msg = "success", errorCode = 0, code = 200) {
    return {
      msg,
      code,
      errorCode,
    };
  }

  static json(data, msg = "success", errorCode = 0, code = 200) {
    return {
      code,
      msg,
      errorCode,
      data,
    };
  }
}

```



### api完成

非常好，我们现在完成了一个 createAdmin的api，从定义访问路由开始，到返回结果结束，中间对数据库进行了一番操作。

其他的api也是类似的流程，都是重复的增删改查，我就不多赘述了，大家看看代码更快。
