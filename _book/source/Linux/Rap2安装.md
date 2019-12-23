# RAP2安装部署

## 环境准备

### node安装

####  下载node.js

```
#curl --silent --location https://rpm.nodesource.com/setup_9.x | bash -
```

####  yum安装node.js

```
yum install -y nodejs
```

## 后端项目部署

### clone项目至本地

```shell
git clone https://github.com/thx/rap2-delos.git
```

### 修改配置文件rap2-delos\src\config\config.*.ts

```properties
import { IConfigOptions } from "../types"

// 先从环境变量取配置
let config: IConfigOptions =  {
    version: '2.7.0',
    serve: {
        port: (process.env.SERVE_PORT && parseInt(process.env.SERVE_PORT)) || 10010,
        path: '',
    },
    keys: ['some secret hurr'],
    session: {
        key: 'rap2:sess',
    },
    db: {
        dialect: 'mysql',
        host: process.env.MYSQL_URL || '172.20.25.13',
        port: (process.env.MYSQL_PORT && parseInt(process.env.MYSQL_PORT)) || 3306,
        username: process.env.MYSQL_USERNAME || 'root',
        password: process.env.MYSQL_PASSWD || '',
        database: process.env.MYSQL_SCHEMA || 'rap',
        pool: {
            max: 80,
            min: 0,
            idle: 20000,
            acquire: 20000,
        },
        logging: false,
    },
    redis: {
        host: process.env.REDIS_URL || '172.20.25.14',
        port: (process.env.REDIS_PORT && parseInt(process.env.REDIS_PORT)) || 7003
    },
    mail: {
      host: 'smtp-mail.outlook.com',
      port: 587,
      secure: false,
      auth: {
          user: 'rap2_notify@outlook.com',
          pass: ''
      }
    },
    mailSender: 'rap2_notify@outlook.com',
}
```

### 启动后端服务

#### 初始化

```shell
npm install
```

#### 安装 && TypeScript 编译

```bash
npm install -g typescript
npm run build
```

#### 初始化数据库表

```bash
npm run create-db
```

#### 执行 mocha 测试用例和 js 代码规范检查

```bash
npm run check
```

#### 安装 pandoc

我们使用 pandoc 来生成 Rap 的离线文档，安装 Pandoc 最通用的办法是在 pandoc 的 [release 页面](https://github.com/jgm/pandoc/releases/tag/2.7.3)下载对应平台的二进制文件安装即可。

其中 linux 版本最好放在`/usr/local/bin/pandoc` 让终端能直接找到，并执行 `chmod +x /usr/local/bin/pandoc` 给调用权限。

测试在命令行执行命令 `pandoc -h` 有响应即可。

#### 后台执行可以使用 nohup 或 pm2，这里推荐使用 pm2，下面命令会安装 pm2

```sh
npm install -g pm2
```

#### pm2方式介绍

* 查看服务进程 

  ~~~shell
  [root@IDCWX16 rap2-delos]# pm2 list
  
  >>>> In-memory PM2 is out-of-date, do:
  >>>> $ pm2 update
  In memory PM2 version: 2.10.4
  Local PM2 version: 3.4.0
  
  ┌─────────────────────────┬────┬─────────┬──────┬───────┬─────────┬─────────┬────────┬─────┬───────────┬──────┬──────────┐
  │ App name                │ id │ version │ mode │ pid   │ status  │ restart │ uptime │ cpu │ mem       │ user │ watching │
  ├─────────────────────────┼────┼─────────┼──────┼───────┼─────────┼─────────┼────────┼─────┼───────────┼──────┼──────────┤
  │ rap-server-delos        │ 0  │ N/A     │ fork │ 17627 │ online  │ 6       │ 2M     │ 0%  │ 24.7 MB   │ root │ disabled │
  │ rap2-dolores            │ 1  │ N/A     │ fork │ N/A   │ stopped │ 0       │ 0      │ 0%  │ 0 B       │ root │ disabled │
  │ static-page-server-8088 │ 2  │ N/A     │ fork │ 21948 │ online  │ 0       │ 8D     │ 0%  │ 54.2 MB   │ root │ disabled │
  └─────────────────────────┴────┴─────────┴──────┴───────┴─────────┴─────────┴────────┴─────┴───────────┴──────┴──────────┘
   Use `pm2 show <id|name>` to get more details about an app
  
  ~~~

* 停止服务存在服务

  ~~~shell
  pm2 stop xxxxxAppName(rap-server-delos)
  ~~~


#### 启动开发模式的服务器 监视并在发生代码变更时自动重启

```bash
npm run dev
```

#### 生产模式

```sh
# 1. 修改/config/config.prod.js中的服务器配置
# 2. 启动生产模式服务器
npm start
```

## 前端项目部署

### clone项目至本地

```sh
git clone https://github.com/thx/rap2-dolores.git
```

### 修改配置文件rap2-dolores\src\config\config.*.ts

```properties
const config: IConfig = {
  serve: 'http://127.0.0.1:10010',
  keys: ['some secret hurr'],
  session: {
    key: 'koa:sess',
  },
}
```

### development 开发模式

```sh
# initialize 初始化
npm install

# config development mode server API path in /src/config/config.dev.js
# 配置开发模式后端服务器的地址。 /src/config/config.dev.js

# test cases 测试用例
npm run test

# will watch & serve automatically 会自动监视改变后重新编译
npm run dev
```

### production

```sh
# 1. config server API path in /src/config/config.prod.js(production config file)
# 1. 配置后端服务器的地址。 /src/config/config.prod.js(生产模式配置文件)

# 2. produce react production package
# 2. 编译React生产包
npm run build

# 3. use serve or nginx to serve the static build directory
# 3. 用serve命令或nginx服务器路由到编译产出的build文件夹作为静态服务器即可
npm install -g serve
serve -s ./build -p 80
```

## 错误问题

#### xxx权限不够

```sh
#赋权限
chmod +x xxx
```

