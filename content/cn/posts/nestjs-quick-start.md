
Nest (NestJS)是一个用于构建高效、可伸缩的 Node.js 服务器端框架。

NestJS 默认使用 JavaScript 的超集 `TypeScript` 进行开发。 

### 环境准备

查看node和npm版本: 

```sh
$ node --version
v15.2.1

$  npm --version            
7.0.14
```

### 安装 @nestjs/cli

使用 npm 全局安装 `@nestjs/cli`:

```sh
$ npm i -g @nestjs/cli
/usr/local/bin/nest -> /usr/local/lib/node_modules/@nestjs/cli/bin/nest.js
in/nest.js
+ @nestjs/cli@7.5.3
added 3 packages from 3 contributors and updated 10 packages in 39.209s
```

使用 `nest --version` 命令查看 nest 当前版本: 

```sh
$ nest --version
7.5.3
```


使用 `nest new` 命令创建一个名为 `nest-api` 的项目:

```sh
$ nest new nest-api
⚡  We will scaffold your app in a few seconds..

CREATE nest-api/.eslintrc.js (630 bytes)
CREATE nest-api/.prettierrc (51 bytes)
CREATE nest-api/README.md (3339 bytes)
CREATE nest-api/nest-cli.json (64 bytes)
CREATE nest-api/package.json (1962 bytes)
CREATE nest-api/tsconfig.build.json (97 bytes)
CREATE nest-api/tsconfig.json (339 bytes)
CREATE nest-api/src/app.controller.spec.ts (617 bytes)
CREATE nest-api/src/app.controller.ts (274 bytes)
CREATE nest-api/src/app.module.ts (249 bytes)
CREATE nest-api/src/app.service.ts (142 bytes)
CREATE nest-api/src/main.ts (208 bytes)
CREATE nest-api/test/app.e2e-spec.ts (630 bytes)
CREATE nest-api/test/jest-e2e.json (183 bytes)

? Which package manager would you ❤️  to use? npm
▸▹▹▹▹ Installation in progress... ☕

🚀  Successfully created project nest-api
👉  Get started with the following commands:

$ cd nest-api
$ npm run start

Thanks for installing Nest 🙏
Please consider donating to our open collective
to help us maintain this package.

🍷  Donate: https://opencollective.com/nest
```


### 启动项目

进入项目，并启动项目

```sh
$ cd nest-api 
$ npm run start

> nest-api@0.0.1 start
> nest start

[Nest] 95920   - 2020/12/02 下午4:17:34   [NestFactory] Starting Nest application...
[Nest] 95920   - 2020/12/02 下午4:17:34   [InstanceLoader] AppModule dependencies initialized +18ms
[Nest] 95920   - 2020/12/02 下午4:17:34   [RoutesResolver] AppController {}: +9ms
[Nest] 95920   - 2020/12/02 下午4:17:34   [RouterExplorer] Mapped {, GET} route +4ms
[Nest] 95920   - 2020/12/02 下午4:17:34   [NestApplication] Nest application successfully started +3ms
```

查看启动效果：

（1）打开浏览器，访问 `http://localhost:3000/` 就可以看到页面输出了 `Hello World!` 文字。

（2）在终端使用 `curl` 请求:

```sh
$ curl -i localhost:3000
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 12
ETag: W/"c-Lve95gjOVATpfV8EL5X4nxwjKHE"
Date: Wed, 02 Dec 2020 08:18:21 GMT
Connection: keep-alive
Keep-Alive: timeout=5

Hello World!%
```

可以看到， 输出了 `Hello World!` 字符串。 

使用 `-i` 参数，表示要输出 header 头信息。

在 header 头信息中，我们可以看到，`X-Powered-By` 值为 `Express`， 就是告诉我们这个网站或框架底层是 `Express` 框架。

Express 是一个基于 Node.js 平台，快速、开放、极简的 Web 开发框架。

### 项目结构

查看一下项目结构: 

```sh
$ ll nest-api 
total 1432
-rw-r--r--    1 wangtom  staff   3.3K 12  2 16:14 README.md
drwxr-xr-x   15 wangtom  staff   480B 12  2 16:17 dist/
-rw-r--r--    1 wangtom  staff    64B 12  2 16:14 nest-cli.json
drwxr-xr-x  593 wangtom  staff    19K 12  2 16:15 node_modules/
-rw-r--r--    1 wangtom  staff   695K 12  2 16:15 package-lock.json
-rw-r--r--    1 wangtom  staff   1.9K 12  2 16:15 package.json
drwxr-xr-x    7 wangtom  staff   224B 12  2 16:14 src/
drwxr-xr-x    4 wangtom  staff   128B 12  2 16:14 test/
-rw-r--r--    1 wangtom  staff    97B 12  2 16:14 tsconfig.build.json
-rw-r--r--    1 wangtom  staff   339B 12  2 16:14 tsconfig.json
```

可以使用 tree 命令查看项目的目录结构: 

比如使用 `tree ./nest-api -L 1`, 查看1级目录结构。

```sh
$ tree ./nest-api -L 1
./nest-api
├── README.md
├── dist/
├── nest-cli.json
├── node_modules/
├── package-lock.json
├── package.json
├── src/
├── test/
├── tsconfig.build.json
└── tsconfig.json
4 directories, 6 files
```

项目的结构如下: 

```sh
./nest-api
├── README.md
├── nest-cli.json
├── node_modules/
├── package-lock.json
├── package.json
├── src/  # 源码目录
│   ├── app.controller.spec.ts # 控制器测试文件
│   ├── app.controller.ts # 控制器类
│   ├── app.module.ts  # 模块
│   ├── app.service.ts # 服务类
│   └── main.ts   # 入口文件
├── test/  # 测试代码目录
│   ├── app.e2e-spec.ts
│   └── jest-e2e.json
├── tsconfig.build.json
└── tsconfig.json
```

可以看到，和 Angular 的项目结构很相似。

查看项目的入口文件 `src/main.ts`, 定义了一个异步方法(bootstrap)来启动应用，默认监听端口 `3000`: 

```js
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}

bootstrap();
```

控制器: `app.controller.ts`

```php
// app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```

控制器中定义了一个名为 `getHello()` 的方法，使用 `@Get` 注解，表示可以通过 Get 方法访问。 

控制器构造方法中引入了私有只读的服务类 `AppService`， 在 `getHello()` 方法中调动了服务类(AppService)中的 `getHello()` 方法。

服务类: `app.service.ts`

```java
// app.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```

在服务类AppService中，定义了 `getHello()` 方法，该方法只返回了字符串 `Hello World!`。

### 将代码提交到 gitee

为了方便管理代码，可以将代码提交到 github 或 gitee 代码托管网站中保存。这里我提交到 gitee 网站。

查看当前的 git 版本：

```
$ git --version
git version 2.23.0
```

初始化 git 仓库: 

```sh
$ git init
Reinitialized existing Git repository in /Users/wangtom/Code/nest-api/.git/
```

提示 git repository 已经存在了。说明在使用 nest new 命令创建这个项目时，就已经帮我们初始化了git仓库了。

提交代码，将远程源设置为 gitee.com 的仓库。
需要先在 gitee.com 网站创建好自己的仓库（【新建仓库】）。 

比如，我们已经在 gitee 创建了一个名为 nest-api 的新仓库，直接执行 git remote add origin git@gitee.com:wangyongtao/nest-api.git 即可。

```sh
$ git commit -a "init"
$ git remote add origin git@gitee.com:wangyongtao/nest-api.git 
$ git pull origin 
You asked to pull from the remote 'origin', but did not specify
a branch. Because this is not the default configured remote
for your current branch, you must specify a branch on the command line.
$ git pull origin master
From gitee.com:wangyongtao/nest-api
 * branch            master     -> FETCH_HEAD
fatal: refusing to merge unrelated histories
```

出错了，提示不能合并不相干的历史记录。网上一搜，找到解决办法，增加个 `--allow-unrelated-histories` 参数。

```sh
$ git pull origin master --allow-unrelated-histories
From gitee.com:wangyongtao/nest-api
 * branch            master     -> FETCH_HEAD
CONFLICT (add/add): Merge conflict in README.md
Auto-merging README.md
CONFLICT (add/add): Merge conflict in .gitignore
Auto-merging .gitignore
Automatic merge failed; fix conflicts and then commit the result.
```

有冲突，打开代码文件，解决冲突代码后保存文件。提交代码后推送到远程(master)。

```sh
$ git add .
$ git commit -am "MERGE"   
[master 5347fd5] MERGE
$ git push origin master
```

打开网址 https://gitee.com/wangyongtao/nest-api 就可看到提交的代码了。

### 小试牛刀

熟悉了项目目录结构与基本运行代码后，我们来增加一些自己的方法，感受一个这个框架。

推荐使用 `Visual Studio Code` 编辑器来编辑代码。

(1) 自定义一个返回当前版本的接口，获取当前应用的版本: 

修改控制器文件 `src/app.controller.ts`:  

```javascript
// app.controller.ts
// 导入 Post
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
  // 自定义 getVersion 方法: 
  @Get('/version')
  getVersion(): Object {
    return this.appService.getVersion();
  }
  // 自定义 postIndex 方法: 
  @Post('/api')
  postIndex(): Object {
    return this.appService.getVersion();
  }
}
```

在控制器文件 `src/app.controller.ts` 中:  
新增了一个使用 `@Get` 注解的 `getVersion()` 方法，可以通过Get访问，路由为 “/version”。
新增了一个使用 `@Post` 注解的 `postIndex()` 方法，可以通过Post访问, 路由为 “/api”。

修改服务类文件 `app.service.ts`: 

新增一个名为 `getVersion()` 的方法，返回 Object 格式的json数据。

```javascript
// app.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
  // 自定义：获取版本
  getVersion(): Object {
    return {
      code: 200, 
      msg: "OK",
      data: {
        version: "0.0.1"
      }, 
    }
  }
}
```

代码修改好后，使用 `Control` + `C` 结束终端运行。

这次我们使用 `npm run start:dev` 启动项目，这样项目里的文件有修改会自动重启服务:

(更多的命令可以在项目 package.json 文件的 scripts 部分看到)
 
```sh
$ npm run start:dev  

[下午4:56:14] Starting compilation in watch mode...
[下午4:56:17] Found 0 errors. Watching for file changes.
[Nest] 2491 - 2020/12/02 下午4:56:18 [NestFactory] Starting Nest application...
[Nest] 2491 - 2020/12/02 下午4:56:18 [InstanceLoader] AppModule dependencies initialized +20ms
[Nest] 2491 - 2020/12/02 下午4:56:18 [RoutesResolver] AppController {}: +7ms
[Nest] 2491 - 2020/12/02 下午4:56:18 [RouterExplorer] Mapped {, GET} route +3ms
[Nest] 2491 - 2020/12/02 下午4:56:18 [NestApplication] Nest application successfully started +3ms
 ... 
```

启动成功了。现在我们使用 `curl` 命令分别来请求这几个路由地址:

请求 “localhost:3000/version”:  

```sh
# 项目默认的，首页
$ curl localhost:3000            
Hello World!% 

# 默认GET请求'/version': 存在，返回预期的结果
$ curl localhost:3000/version
{"code":200,"msg":"OK","data":{"version":"0.0.1"}}%                                                   

# 改成POST请求'/version': 不存在，框架自带的错误提示
$ curl -X POST localhost:3000/version    
{"statusCode":404,"message":"Cannot POST /version","error":"Not Found"}%
```

请求 “localhost:3000/api”: 

```sh
# 使用 POST 请求 '/api': 存在，返回预期的结果
$ curl -X POST localhost:3000/api
{"code":200,"msg":"OK","data":{"version":"0.0.1"}}% 

# 改成 GET 请求 '/api': 不存在，框架自带的错误提示
$ curl localhost:3000/api 
{"statusCode":404,"error":"Not Found","message":"Cannot GET /api"}% 
```

### 参考文献 References

https://blog.csdn.net/cnwyt
https://nodejs.org/en/download/  
https://docs.nestjs.com/first-steps

### 更新记录 Change log

2018.12.18 新增本文档，使用 nestjs 5.7.1 版本。
2019.06.13 修改内容，并更新 nestjs 至 6.5 版本。
2020.12.01 完善内容，并更新 nestjs 至 7.5 版本。

> 感谢阅读，如有问题请留言。

[END]
