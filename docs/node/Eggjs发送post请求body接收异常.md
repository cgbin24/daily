### 问题

> 在 `Eggjs` 中使用 `post` 请求方式创建路由信息及接口实现，本地通过 `Apifox` 进行测试，出现 `ctx.request.body` 中无法正常获取参数信息的问题

下面 `body` 对象的信息
```js
console.log('request.body: ', ctx.request.body);
```
```sh
request.body:  {}
```

同时也看一下 `ctx.request` 下的信息

```js
console.log('request: ', ctx.request);
```
```sh
request:  {
  method: 'POST',
  url: '/add-user',
  header: {
    'user-agent': 'Apifox/1.0.0 (https://apifox.com)',
    accept: '*/*',
    host: '127.0.0.1:7001',
    'accept-encoding': 'gzip, deflate, br',
    connection: 'keep-alive',
    'content-type': 'multipart/form-data; boundary=--------------------------774899165972769608320501',
    'content-length': '165'
  }
}

```

### 经验证可行性处理方式

> 在 `Apifox` 中，若为创建的接口文档，找到`修改文档` -> **请求参数**下的 `Body` -> `x-www-form-urlencoded` 格式 -> `保存` -> `运行`，再次检测是否成功获取

现在再看一下控制台输出信息，显然已经成功接收到传递参数

```sh
request:  {
  method: 'POST',
  url: '/add-user',
  header: {
    'user-agent': 'Apifox/1.0.0 (https://apifox.com)',
    'content-type': 'application/x-www-form-urlencoded',
    accept: '*/*',
    host: '127.0.0.1:7001',
    'accept-encoding': 'gzip, deflate, br',
    connection: 'keep-alive',
    'content-length': '9'
  }
}
request.body:  { name: 'test' }
```

`x-www-form-urlencoded` 是一种常见的 `http` 请求体编码方式，通常用于将表单数据发送到服务器


#### 衍伸阅读

[Post请求的3种编码格式：application/x-www-form-urlencoded和multipart/form-data和application/json](https://blog.csdn.net/u013258447/article/details/101107743)