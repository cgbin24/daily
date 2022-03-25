### 启动一个服务接口供前端使用

> 使用node 启动一个服务，并提供测试接口供前端调用测试

1、创建项目文件

2、初始化

```shell
npm init
```

3、安装 `express` 和 `body-parser` 依赖

```shell
npm i express body-parser -s
```

> 在http请求中，POST、PUT、PATCH三种请求方法中包含着请求体，也就是所谓的request，在Nodejs原生的http模块中，请求体是要基于流的方式来接受和解析;
>
>  **body-parser** 是一个HTTP请求体解析的中间件，使用这个模块可以解析JSON、Raw、文本、URL-encoded格式的请求体

4、coding，提供接口，启动服务

```js
const express = require('express')
const app = express()
const bodyParser = require('body-parser')
// 引用bodyParser 
app.use(bodyParser.json())
app.use(bodyParser.urlencoded({ extended: true}))
// 设置跨域访问
app.all("*", (req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "X-Requested-With");
  res.header("Access-Control-Allow-Methods", "PUT,GET,POST,DELETE,OPTIONS");
  res.header("X-Powered-By", "3.2.1");
  res.header("Content-Type", "application/json;charset=utf-8");
  next()
})

let questions = [
  {
    id: 1,
    data: "测试数据1"
  },
  {
    id: 2,
    data: "测试数据2"
  },
]

// 列表接口
app.get('/test-list', (req, res) => {
  res.status(200)
  res.json({
    code: 200,
    data: questions,
    msg: '获取成功'
  })
})

app.post('/edit', (req, res) => {
  console.log(req.stack);
  console.log(req.body);
  console.log(req.query);
  console.log(req.url);
  res.json({
    code: 200,
    data: [],
    msg: '编辑成功'
  })
})

// 配置服务端口
const server = app.listen(9999, () => {
  const host = server.address().address;
  const port = server.address().port;
  console.log("listening at http://%s:%s", host, port);
})
```





