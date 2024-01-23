### 问题

> 使用`eggjs`脚手架安装项目，需要完成简单的数据库操作，使用插件`egg-mysql`，正常按照官方方式进行操作，结果却不尽人意，找不到解决方案，一头雾水，捣鼓了好几天，一步步的排错查询，终于解决，以下是踩坑中的解决过程和错误信息汇总

```ini
"egg": "^3.17.5",
"egg-mysql": "^4.0.0",
```

### 经验证可行性处理方式

> 首先将两个配置文件附上

- `config.default.js`

```js

/**
 * @param {Egg.EggAppInfo} appInfo app info
 */
module.exports = appInfo => {
  /**
   * built-in config
   * @type {Egg.EggAppConfig}
   **/
  const config = exports = {};

  // use for cookie sign key, should change to your own and keep security
  config.keys = appInfo.name + '_1705563141102_525';

  // add your middleware config here
  config.middleware = [];

  // add your user config here
  const userConfig = {
    // myAppName: 'egg',
  };

  config.security = {
    csrf: {
      enable: false,
      ignoreJSON: true
    },
    domainWhiteList: ['*']
  }

  config.view = {
    mapping: {'.html': 'ejs'}, // 左边写成.html，渲染时候会自动渲染成该格式文件
  }

  // 推荐 - 配置方式
  config.mysql = {
    // 单数据库信息配置
    client: {
      host: "localhost",  // 主机
      port: '3306', // 端口号
      user: 'root', // 用户
      password: 'password', // 密码
      database: 'test', // 数据库名
    },
    app: true,  // 是否挂载到app上，默认开启
    agent: false, // 是否挂载到agent上，默认关闭
  }

  return {
    ...config,
    ...userConfig,
  };
};

// 不推荐 - 配置方式
// exports.mysql = {
//   // 单数据库信息配置
//   client: {
//     host: "localhost",  // 主机
//     port: 3306, // 端口号
//     user: 'root', // 用户
//     password: 'password', // 密码
//     database: 'test', // 数据库名
//   },
//   app: true,  // 是否挂载到app上，默认开启
//   agent: false, // 是否挂载到agent上，默认关闭
// }


```

- `plugin.js`

```js
/** @type Egg.EggPlugin */
module.exports = {
  ejs: {
    enable: true,
    package: 'egg-view-ejs'
  },
  // 推荐 - 配置方式  
  mysql: {
    enable: true,
    package: 'egg-mysql'
  }
};

// 不推荐 - 配置方式
// exports.mysql = {
//   enable: true,
//   package: 'egg-mysql'
// }
```

- 设置加密方式

> 处理`mysql v8.x`版本默认加密方式变化导致的问题（**8.0 之前的 `mysql` 版本，加密规则是 `mysql_native_password`，而在 8.0 之后，加密规则变为 `caching_sha2_password`**），将以下脚本信息在数据库中执行，完成后便更改完成

> 为此，特意使用 `node + express + mysql` 创建一个简易的demo来验证并辅助完成问题排查和解决

```js
const mysql = require('mysql')
const express = require('express')
const app = express()

const conn = mysql.createConnection({
    user: 'root',
    password: 'password',
    host: 'localhost',
    database: 'test'
})

conn.connect(err=> {
    if (!err) {
        console.log('连接成功');
    } else {
        console.log('连接失败', err);
    }
})

app.listen(3000, () => {
    console.log('服务启动成功。。。');
})

// 获取用户信息
app.get('/userInfo', (req, res) => {
    let sql = 'select id, name from list'
    conn.query(sql, (err, data) => {
        if (!err) {
            console.log('获取数据成功', data);
            res.send(data)
        } else {
            console.log('获取数据失败');
            res.send('获取信息失败')   
        }
    })
})

// 添加用户信息
app.post('/addInfo', (req, res) => {
    console.log(req.query);
    const {name} = req.query
    console.log(name);
    let sql = `insert into list(id, name) values(0, ?)`
    conn.query(sql, name, async (err, data) => {
        console.log(2);
        if(!err) {
            const id = data.insertId
            console.log('插入成功', data);
            const result = await getInfoById(id)
            res.send(result)
        } else {
            console.log('插入失败', err);
        }
    })
})

// 用户列表
app.get('/user-list', (req, res) => {
    const sql = 'select * from list'
    conn.query(sql, (err, data) => {
        if (!err) {
            const result = {
                list: data
            }
            res.send(result)
        } else {
            res.send([])
        }
    })
})

// 根据id, 获取用户信息
function getInfoById(id) {
    if (!id) return
    console.log('开始查询', id);
    return new Promise((resolve, reject)=> {
        const sql = `select id, name from list where id=${id}`
        console.log(12);
        conn.query(sql, (err, data) => {
            console.log(err);
            if (!err) {
                console.log('返回结果', data);
                resolve(data)
            } else {
                reject(null)
            }
        })
    })
}

```

> 设置方式如下，这里推荐使用数据库连接工具来执行，通过试错之后表示这样更可靠（也可能是个人错觉，可供参考），如使用`DBeaver`按此步骤执行，找到菜单栏【SQL编辑器】-> 【SQL编辑器】或【新建SQL编辑器】-> 将下面语句填入 -> 点击右键 ->【执行】-> 【执行SQL语句】或【执行SQL脚本】-> OVER

```sql
ALTER user 'root'@'localhost' identified with 'mysql_native_password' by 'password';

flush privileges;
```

- 检查数据库连接配置信息，如账户密码、端口号、数据库等

> `debug` **配置**和**连接**状态，可以将操作对象信息打印出来看一下

- 检测编码语法是否规范




### 错误展示

- 错误零

> 首先，检查mysql服务是否开启，这里的错误日志大概包含如下信息

```sh
...
Error: connect ECONNREFUSED ::1:3306
...
```

- 错误一

> egg 的app对象上没有mysql，mysql未挂载到app上，为什么首先要附上这个，因为这个在整个操作流程中占比真的蛮重要的

```sh
umac@umacdexuniji egg-example % npm run dev

> example@1.0.0 dev
> egg-bin dev

[egg-ts-helper] create typings/app/controller/index.d.ts (1ms)
[egg-ts-helper] create typings/app/middleware/index.d.ts (1ms)
[egg-ts-helper] create typings/config/index.d.ts (8ms)
[egg-ts-helper] create typings/config/plugin.d.ts (0ms)
[egg-ts-helper] create typings/app/service/index.d.ts (1ms)
[egg-ts-helper] create typings/app/index.d.ts (0ms)
2024-01-22 15:28:26,359 INFO 24809 [master] node version v19.9.0
2024-01-22 15:28:26,360 INFO 24809 [master] egg version 3.17.7
2024-01-22 15:28:26,632 INFO 24809 [master] agent_worker#1:24812 started (271ms)
2024-01-22 15:28:26,904 INFO 24809 [master] egg started on http://127.0.0.1:7001 (544ms)
app:  {
  env: 'local',
  name: 'example',
  baseDir: '/Users/umac/Desktop/Dev/Backend/Node/egg-example',
  subdomainOffset: 2,
  config: '<egg config>',
  controller: '<egg controller>',
  httpclient: '<egg httpclient>',
  loggers: '<egg loggers>',
  middlewares: '<egg middlewares>',
  router: '<egg router>',
  serviceClasses: '<egg serviceClasses>'
}
sql select id, name from list;
app.mysql undefined
出错啦！！！ TypeError: Cannot read properties of undefined (reading 'query')
    at HomeService.userInfo (/Users/umac/Desktop/Dev/Backend/Node/egg-example/app/service/home.js:21:44)
    at HomeController.userInfo (/Users/umac/Desktop/Dev/Backend/Node/egg-example/app/controller/home.js:30:40)
    at Object.callFn (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/egg-core/lib/utils/index.js:44:21)
    at Object.classControllerMiddleware (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/egg-core/lib/loader/mixin/controller.js:87:20)
    at Object.callFn (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/lib/utils.js:12:21)
    at wrappedController (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/lib/egg_router.js:322:18)
    at dispatch (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/node_modules/koa-compose/index.js:44:32)
    at next (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/node_modules/koa-compose/index.js:45:18)
    at /Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/lib/router.js:186:18
    at dispatch (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/node_modules/koa-compose/index.js:44:32)
获取结果 null
```

- 错误二

> `TypeError: app.mysql.query is not a function`，读取mysql方法出错

```sh
umac@umacdexuniji egg-example % npm run dev

> example@1.0.0 dev
> egg-bin dev

[egg-ts-helper] create typings/app/controller/index.d.ts (2ms)
[egg-ts-helper] create typings/app/middleware/index.d.ts (1ms)
[egg-ts-helper] create typings/config/index.d.ts (9ms)
[egg-ts-helper] create typings/config/plugin.d.ts (0ms)
[egg-ts-helper] create typings/app/service/index.d.ts (1ms)
[egg-ts-helper] create typings/app/index.d.ts (0ms)
2024-01-22 15:34:05,179 INFO 25265 [master] node version v19.9.0
2024-01-22 15:34:05,180 INFO 25265 [master] egg version 3.17.7
2024-01-22 15:34:05,477 INFO 25265 [master] agent_worker#1:25266 started (295ms)
2024-01-22 15:34:05,785 INFO 25265 [master] egg started on http://127.0.0.1:7001 (605ms)
app:  {
  env: 'local',
  name: 'example',
  baseDir: '/Users/umac/Desktop/Dev/Backend/Node/egg-example',
  subdomainOffset: 2,
  config: '<egg config>',
  controller: '<egg controller>',
  httpclient: '<egg httpclient>',
  loggers: '<egg loggers>',
  middlewares: '<egg middlewares>',
  router: '<egg router>',
  serviceClasses: '<egg serviceClasses>'
}
sql select id, name from list;
app.mysql Singleton {
  clients: Map(0) {},
  app: {
    env: 'local',
    name: 'example',
    baseDir: '/Users/umac/Desktop/Dev/Backend/Node/egg-example',
    subdomainOffset: 2,
    config: '<egg config>',
    controller: '<egg controller>',
    httpclient: '<egg httpclient>',
    loggers: '<egg loggers>',
    middlewares: '<egg middlewares>',
    router: '<egg router>',
    serviceClasses: '<egg serviceClasses>'
  },
  name: 'mysql',
  create: [Function: createOneClient],
  options: {
    default: { database: null, connectionLimit: 5 },
    app: true,
    agent: false
  }
}
出错啦！！！ TypeError: app.mysql.query is not a function
    at HomeService.userInfo (/Users/umac/Desktop/Dev/Backend/Node/egg-example/app/service/home.js:21:44)
    at HomeController.userInfo (/Users/umac/Desktop/Dev/Backend/Node/egg-example/app/controller/home.js:30:40)
    at Object.callFn (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/egg-core/lib/utils/index.js:44:21)
    at Object.classControllerMiddleware (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/egg-core/lib/loader/mixin/controller.js:87:20)
    at Object.callFn (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/lib/utils.js:12:21)
    at wrappedController (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/lib/egg_router.js:322:18)
    at dispatch (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/node_modules/koa-compose/index.js:44:32)
    at next (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/node_modules/koa-compose/index.js:45:18)
    at /Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/lib/router.js:186:18
    at dispatch (/Users/umac/Desktop/Dev/Backend/Node/egg-example/node_modules/@eggjs/router/node_modules/koa-compose/index.js:44:32)
获取结果 null

```

- 错误三

> `ER_NOT_SUPPORTED_AUTH_MODEError` mysql权限控制问题，`v8.x`之后默认的加密方式导致的问题

```sh
umac@umacdexuniji egg-example % npm run dev

> example@1.0.0 dev
> egg-bin dev

(node:3602) [DEP0148] DeprecationWarning: Use of deprecated folder mapping "./" in the "exports" field module resolution of the package at /Users/umac/Downloads/egg-example/node_modules/koa/package.json.
Update this package.json to use a subpath pattern like "./*".
(Use `node --trace-deprecation ...` to show where the warning was created)
[egg-ts-helper] create typings/app/controller/index.d.ts (1ms)
[egg-ts-helper] create typings/config/index.d.ts (8ms)
[egg-ts-helper] create typings/config/plugin.d.ts (1ms)
[egg-ts-helper] create typings/app/service/index.d.ts (1ms)
[egg-ts-helper] create typings/app/index.d.ts (0ms)
2024-01-22 10:48:56,115 INFO 3601 [master] node version v16.20.0
2024-01-22 10:48:56,115 INFO 3601 [master] egg version 2.29.4
(node:3601) [DEP0148] DeprecationWarning: Use of deprecated folder mapping "./" in the "exports" field module resolution of the package at /Users/umac/Downloads/egg-example/node_modules/koa/package.json.
Update this package.json to use a subpath pattern like "./*".
(Use `node --trace-deprecation ...` to show where the warning was created)
(node:3605) [DEP0148] DeprecationWarning: Use of deprecated folder mapping "./" in the "exports" field module resolution of the package at /Users/umac/Downloads/egg-example/node_modules/koa/package.json.
Update this package.json to use a subpath pattern like "./*".
(Use `node --trace-deprecation ...` to show where the warning was created)
2024-01-22 10:48:56,488 INFO 3601 [master] agent_worker#1:3605 started (371ms)
(node:3607) [DEP0148] DeprecationWarning: Use of deprecated folder mapping "./" in the "exports" field module resolution of the package at /Users/umac/Downloads/egg-example/node_modules/koa/package.json.
Update this package.json to use a subpath pattern like "./*".
(Use `node --trace-deprecation ...` to show where the warning was created)
2024-01-22 10:48:56,962 ERROR 3607 [-/127.0.0.1/-/0ms GET /] nodejs.ER_NOT_SUPPORTED_AUTH_MODEError: ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL client
    at Handshake.Sequence._packetToError (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/sequences/Sequence.js:47:14)
    at Handshake.ErrorPacket (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/sequences/Handshake.js:123:18)
    at Protocol._parsePacket (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Protocol.js:291:23)
    at Parser._parsePacket (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Parser.js:433:10)
    at Parser.write (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Parser.js:43:10)
    at Protocol.write (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Protocol.js:38:16)
    at Socket.<anonymous> (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/Connection.js:88:28)
    at Socket.<anonymous> (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/Connection.js:526:10)
    at Socket.emit (node:events:513:28)
    at addChunk (node:internal/streams/readable:315:12)
    --------------------
    at Protocol._enqueue (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Protocol.js:144:48)
    at Protocol.handshake (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Protocol.js:51:23)
    at PoolConnection.connect (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/Connection.js:116:18)
    at Pool.getConnection (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/Pool.js:48:16)
    at /Users/umac/Downloads/egg-example/node_modules/ali-rds/node_modules/pify/index.js:29:7
    at new Promise (<anonymous>)
    at Pool.<anonymous> (/Users/umac/Downloads/egg-example/node_modules/ali-rds/node_modules/pify/index.js:12:10)
    at Pool.ret [as getConnection] (/Users/umac/Downloads/egg-example/node_modules/ali-rds/node_modules/pify/index.js:56:34)
    at Pool.query (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/Pool.js:202:8)
    at /Users/umac/Downloads/egg-example/node_modules/ali-rds/node_modules/pify/index.js:29:7
    sql: select now() as currentTime;
code: "ER_NOT_SUPPORTED_AUTH_MODE"
errno: 1251
sqlMessage: "Client does not support authentication protocol requested by server; consider upgrading MySQL client"
sqlState: "08004"
fatal: true
name: "ER_NOT_SUPPORTED_AUTH_MODEError"
pid: 3607
hostname: umacdexuniji.local

2024-01-22 10:48:56,963 ERROR 3607 nodejs.ER_NOT_SUPPORTED_AUTH_MODEError: ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL client
    at Handshake.Sequence._packetToError (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/sequences/Sequence.js:47:14)
    at Handshake.ErrorPacket (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/sequences/Handshake.js:123:18)
    at Protocol._parsePacket (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Protocol.js:291:23)
    at Parser._parsePacket (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Parser.js:433:10)
    at Parser.write (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Parser.js:43:10)
    at Protocol.write (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Protocol.js:38:16)
    at Socket.<anonymous> (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/Connection.js:88:28)
    at Socket.<anonymous> (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/Connection.js:526:10)
    at Socket.emit (node:events:513:28)
    at addChunk (node:internal/streams/readable:315:12)
    --------------------
    at Protocol._enqueue (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Protocol.js:144:48)
    at Protocol.handshake (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/protocol/Protocol.js:51:23)
    at PoolConnection.connect (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/Connection.js:116:18)
    at Pool.getConnection (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/Pool.js:48:16)
    at /Users/umac/Downloads/egg-example/node_modules/ali-rds/node_modules/pify/index.js:29:7
    at new Promise (<anonymous>)
    at Pool.<anonymous> (/Users/umac/Downloads/egg-example/node_modules/ali-rds/node_modules/pify/index.js:12:10)
    at Pool.ret [as getConnection] (/Users/umac/Downloads/egg-example/node_modules/ali-rds/node_modules/pify/index.js:56:34)
    at Pool.query (/Users/umac/Downloads/egg-example/node_modules/mysql/lib/Pool.js:202:8)
    at /Users/umac/Downloads/egg-example/node_modules/ali-rds/node_modules/pify/index.js:29:7
    sql: select now() as currentTime;
code: "ER_NOT_SUPPORTED_AUTH_MODE"
errno: 1251
sqlMessage: "Client does not support authentication protocol requested by server; consider upgrading MySQL client"
sqlState: "08004"
fatal: true
name: "ER_NOT_SUPPORTED_AUTH_MODEError"
pid: 3607
hostname: umacdexuniji.local

2024-01-22 10:48:56,963 ERROR 3607 [app_worker] start error, exiting with code:1
[2024-01-22 10:48:56.971] [cfork:master:3601] worker:3607 disconnect (exitedAfterDisconnect: false, state: disconnected, isDead: false, worker.disableRefork: true)
[2024-01-22 10:48:56.972] [cfork:master:3601] don't fork, because worker:3607 will be kill soon
2024-01-22 10:48:56,972 INFO 3601 [master] app_worker#1:3607 disconnect, suicide: false, state: disconnected, current workers: ["1"]
[2024-01-22 10:48:56.972] [cfork:master:3601] worker:3607 exit (code: 0, exitedAfterDisconnect: false, state: dead, isDead: true, isExpected: false, worker.disableRefork: true)
2024-01-22 10:48:56,973 ERROR 3601 nodejs.AppWorkerDiedError: [master] app_worker#1:3607 died (code: 0, signal: null, suicide: false, state: dead), current workers: []
    at Master.onAppExit (/Users/umac/Downloads/egg-example/node_modules/egg-cluster/lib/master.js:510:21)
    at Master.emit (node:events:513:28)
    at Messenger.sendToMaster (/Users/umac/Downloads/egg-example/node_modules/egg-cluster/lib/utils/messenger.js:137:17)
    at Messenger.send (/Users/umac/Downloads/egg-example/node_modules/egg-cluster/lib/utils/messenger.js:102:12)
    at EventEmitter.<anonymous> (/Users/umac/Downloads/egg-example/node_modules/egg-cluster/lib/master.js:353:22)
    at EventEmitter.emit (node:events:525:35)
    at ChildProcess.<anonymous> (node:internal/cluster/primary:188:13)
    at Object.onceWrapper (node:events:628:26)
    at ChildProcess.emit (node:events:513:28)
    at Process.ChildProcess._handle.onexit (node:internal/child_process:293:12)
name: "AppWorkerDiedError"
pid: 3601
hostname: umacdexuniji.local

2024-01-22 10:48:56,973 ERROR 3601 [master] app_worker#1:3607 start fail, exiting with code:1
2024-01-22 10:48:56,973 ERROR 3601 [master] exit with code:1
2024-01-22 10:48:56,976 ERROR 3605 [agent_worker] receive disconnect event on child_process fork mode, exiting with code:110
2024-01-22 10:48:56,977 ERROR 3605 [agent_worker] exit with code:110
Error: /Users/umac/Downloads/egg-example/node_modules/egg-bin/lib/start-cluster {"declarations":true,"tscompiler":"ts-node/register","workers":1,"baseDir":"/Users/umac/Downloads/egg-example","framework":"/Users/umac/Downloads/egg-example/node_modules/egg"} exit with code 1
    at ChildProcess.<anonymous> (/Users/umac/Downloads/egg-example/node_modules/common-bin/lib/helper.js:56:21)
    at Object.onceWrapper (node:events:628:26)
    at ChildProcess.emit (node:events:513:28)
    at Process.ChildProcess._handle.onexit (node:internal/child_process:293:12) {
  code: 1
}
```

- 错误四

> mysql 服务关不掉

```sh
umac@umacdexuniji egg-example % mysql.server start
Starting MySQL
 SUCCESS! 
umac@umacdexuniji egg-example % 2024-01-22T02:51:04.6NZ mysqld_safe A mysqld process already exists

umac@umacdexuniji egg-example % 
umac@umacdexuniji egg-example % mysql.server stop 
Shutting down MySQL
. SUCCESS! 
umac@umacdexuniji egg-example % mysql.server start
Starting MySQL
 SUCCESS! 
umac@umacdexuniji egg-example % 2024-01-22T02:51:19.6NZ mysqld_safe A mysqld process already exists

umac@umacdexuniji egg-example % 
umac@umacdexuniji egg-example % 
umac@umacdexuniji egg-example % ps -aux |grep mysql
ps: No user named 'x'
umac@umacdexuniji egg-example % ps aux |grep mysql 
umac              4501   0.1  1.0 409786384  79552   ??  S    10:51上午   0:00.56 /opt/homebrew/opt/mysql/bin/mysqld --basedir=/opt/homebrew/opt/mysql --datadir=/opt/homebrew/var/mysql --plugin-dir=/opt/homebrew/opt/mysql/lib/plugin --log-error=umacdexuniji.local.err --pid-file=umacdexuniji.local.pid
umac              4689   0.0  0.0 408102576   1120 s007  S+   10:52上午   0:00.00 grep mysql
umac              4402   0.0  0.0 408507632   1696   ??  S    10:51上午   0:00.01 /bin/sh /opt/homebrew/opt/mysql/bin/mysqld_safe --datadir=/opt/homebrew/var/mysql
umac@umacdexuniji egg-example % killall -9 4501
No matching processes belonging to you were found
umac@umacdexuniji egg-example % kill -9 4501   
umac@umacdexuniji egg-example % killall -9 4501
No matching processes belonging to you were found
umac@umacdexuniji egg-example % kill -9 4501   
kill: kill 4501 failed: no such process
umac@umacdexuniji egg-example % ps aux |grep mysql
umac              4402   0.0  0.0 408507632   2512   ??  S    10:51上午   0:00.02 /bin/sh /opt/homebrew/opt/mysql/bin/mysqld_safe --datadir=/opt/homebrew/var/mysql
umac              4770   0.0  0.0 407963312    656 s007  R+   10:53上午   0:00.00 grep mysql
umac              4737   0.0  4.5 409918496 374304   ??  S    10:53上午   0:00.39 /opt/homebrew/opt/mysql/bin/mysqld --basedir=/opt/homebrew/opt/mysql --datadir=/opt/homebrew/var/mysql --plugin-dir=/opt/homebrew/opt/mysql/lib/plugin --log-error=umacdexuniji.local.err --pid-file=umacdexuniji.local.pid
umac@umacdexuniji egg-example % kill -9 4402      
umac@umacdexuniji egg-example % ps aux |grep mysql
umac              4878   0.0  0.0 408102576   1120 s007  S+   10:53上午   0:00.00 grep mysql
umac@umacdexuniji egg-example % kill -9 4878      
kill: kill 4878 failed: no such process
umac@umacdexuniji egg-example % ps aux |grep mysql
umac              5028   0.0  0.0 407971504    912 s007  R+   10:54上午   0:00.00 grep mysql
umac              5000   0.0  5.2 409787424 435104   ??  S    10:53上午   0:00.33 /opt/homebrew/opt/mysql/bin/mysqld --basedir=/opt/homebrew/opt/mysql --datadir=/opt/homebrew/var/mysql --plugin-dir=/opt/homebrew/opt/mysql/lib/plugin --log-error=umacdexuniji.local.err --pid-file=umacdexuniji.local.pid
umac              4901   0.0  0.0 408506608   2592   ??  S    10:53上午   0:00.02 /bin/sh /opt/homebrew/opt/mysql/bin/mysqld_safe --datadir=/opt/homebrew/var/mysql
umac@umacdexuniji egg-example % kill -9 5028      
kill: kill 5028 failed: no such process
umac@umacdexuniji egg-example % ps aux |grep mysql
umac              5063   0.0  0.0 408111792   1184 s007  S+   10:54上午   0:00.00 grep mysql
umac              5000   0.0  5.2 409787424 435104   ??  S    10:53上午   0:00.39 /opt/homebrew/opt/mysql/bin/mysqld --basedir=/opt/homebrew/opt/mysql --datadir=/opt/homebrew/var/mysql --plugin-dir=/opt/homebrew/opt/mysql/lib/plugin --log-error=umacdexuniji.local.err --pid-file=umacdexuniji.local.pid
umac              4901   0.0  0.0 408506608   2592   ??  S    10:53上午   0:00.02 /bin/sh /opt/homebrew/opt/mysql/bin/mysqld_safe --datadir=/opt/homebrew/var/mysql
umac@umacdexuniji egg-example % 
umac@umacdexuniji egg-example % 
umac@umacdexuniji egg-example % 
umac@umacdexuniji egg-example % 
umac@umacdexuniji egg-example % 
umac@umacdexuniji egg-example % 
umac@umacdexuniji egg-example % ps aux |grep mysql
umac              5085   0.0  0.0 408103600   1152 s007  S+   10:54上午   0:00.00 grep mysql
umac              5000   0.0  5.2 409787424 435104   ??  S    10:53上午   0:00.39 /opt/homebrew/opt/mysql/bin/mysqld --basedir=/opt/homebrew/opt/mysql --datadir=/opt/homebrew/var/mysql --plugin-dir=/opt/homebrew/opt/mysql/lib/plugin --log-error=umacdexuniji.local.err --pid-file=umacdexuniji.local.pid
umac              4901   0.0  0.0 408506608   2592   ??  S    10:53上午   0:00.02 /bin/sh /opt/homebrew/opt/mysql/bin/mysqld_safe --datadir=/opt/homebrew/var/mysql
umac@umacdexuniji egg-example % kill -9 5085      
kill: kill 5085 failed: no such process
umac@umacdexuniji egg-example % ps aux |grep mysql
umac              4901   0.0  0.0 408506608   2592   ??  S    10:53上午   0:00.02 /bin/sh /opt/homebrew/opt/mysql/bin/mysqld_safe --datadir=/opt/homebrew/var/mysql
umac              5114   0.0  0.0 408121008   1248 s007  S+   10:54上午   0:00.00 grep mysql
umac              5000   0.0  5.2 409787424 435120   ??  S    10:53上午   0:00.43 /opt/homebrew/opt/mysql/bin/mysqld --basedir=/opt/homebrew/opt/mysql --datadir=/opt/homebrew/var/mysql --plugin-dir=/opt/homebrew/opt/mysql/lib/plugin --log-error=umacdexuniji.local.err --pid-file=umacdexuniji.local.pid
```


- 错误五

> `Your password does not satisfy the current policy requirements'`，设置低安全性密码（简单密码设置）

> `2024-01-19 16:20:27,050 ERROR 2580 [-/127.0.0.1/-/0ms GET /] nodejs.ECONNREFUSEDError: connect ECONNREFUSED ::1:3306`

```sql
umac@umacdexuniji egg-example % mysql -u root -p         
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.2.0 Homebrew

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> select version()
    -> ;
+-----------+
| version() |
+-----------+
| 8.2.0     |
+-----------+
1 row in set (0.00 sec)

mysql> alter user 'root'@'localhost' identified by 'password' password expire never;
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
mysql> show variables like 'validate_password%';
+-------------------------------------------------+--------+
| Variable_name                                   | Value  |
+-------------------------------------------------+--------+
| validate_password.changed_characters_percentage | 0      |
| validate_password.check_user_name               | ON     |
| validate_password.dictionary_file               |        |
| validate_password.length                        | 8      |
| validate_password.mixed_case_count              | 1      |
| validate_password.number_count                  | 1      |
| validate_password.policy                        | MEDIUM |
| validate_password.special_char_count            | 1      |
+-------------------------------------------------+--------+
8 rows in set (0.01 sec)

mysql> set global validate_password.policy=0;
Query OK, 0 rows affected (0.00 sec)

mysql> set global validate_password.length=1;
Query OK, 0 rows affected (0.00 sec)

mysql> show variables like 'validate_password%';
+-------------------------------------------------+-------+
| Variable_name                                   | Value |
+-------------------------------------------------+-------+
| validate_password.changed_characters_percentage | 0     |
| validate_password.check_user_name               | ON    |
| validate_password.dictionary_file               |       |
| validate_password.length                        | 4     |
| validate_password.mixed_case_count              | 1     |
| validate_password.number_count                  | 1     |
| validate_password.policy                        | LOW   |
| validate_password.special_char_count            | 1     |
+-------------------------------------------------+-------+
8 rows in set (0.00 sec)

mysql> alter user 'root'@'localhost' identified by 'password';
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> exit
Bye
umac@umacdexuniji egg-example % mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.2.0 Homebrew

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show variables like 'validate_password%';
+-------------------------------------------------+-------+
| Variable_name                                   | Value |
+-------------------------------------------------+-------+
| validate_password.changed_characters_percentage | 0     |
| validate_password.check_user_name               | ON    |
| validate_password.dictionary_file               |       |
| validate_password.length                        | 4     |
| validate_password.mixed_case_count              | 1     |
| validate_password.number_count                  | 1     |
| validate_password.policy                        | LOW   |
| validate_password.special_char_count            | 1     |
+-------------------------------------------------+-------+
8 rows in set (0.00 sec)

mysql> exit
Bye
```


#### 参考

- [Node.js MySQL - Error: connect ECONNREFUSED](https://stackoverflow.com/questions/30266221/node-js-mysql-error-connect-econnrefused)

- [解决MySQL8.0报错Client does not support authentication protocol requested by server...问题](https://blog.csdn.net/qq_42372031/article/details/133639714)

- [MySQL修改root用户密码](https://blog.csdn.net/qq_40757240/article/details/118068317)

- [mysql报错 Your password does not satisfy the current policy requirements](https://blog.csdn.net/ayychiguoguo/article/details/120370686)

- [Node express 连接 MySQL报错：TypeError: Cannot read properties of undefined (reading ‘query‘) 解决方案](https://blog.csdn.net/Ninja_Kiro/article/details/125952054)

- [mysql报错：mysqld_safe A mysqld process already exists](https://blog.csdn.net/enthan809882/article/details/103221459)

- [解决Navicat连接不上MySql服务器报错：Client does not support authentication protocol requested by server； conside](https://blog.csdn.net/weixin_43111077/article/details/108811949)

- 🌟[Mysql8.0默认加密连接方式修改](https://cloud.tencent.com/developer/article/1561810)

- [node.js连接mysql出现错误ER_NOT_SUPPORTED_AUTH_MODE Client does not support authentication protocol...](https://blog.csdn.net/leilei__66/article/details/110674462)

- [MySQL连接错误：”connect ECONNREFUSED 127.0.0.1:3306″](https://deepinout.com/mysql/mysql-questions/1340_mysql_getting_error_connect_econnrefused_1270013306_while_connecting_to_mysql.html)

- [egg.js连接mysql数据库遇到的问题](https://segmentfault.com/a/1190000016432073)