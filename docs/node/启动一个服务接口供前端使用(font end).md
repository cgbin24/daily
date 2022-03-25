### 启动一个服务接口供前端使用

> 使用node 启动一个服务，并提供测试接口供前端调用测试

1、创建项目文件

2、初始化

3、安装 `axios` 依赖

```shell
npm i axios -s
```

4、配置 `axios`

```js
import axios from "axios";

const baseUrl = '192.168.0.76:9999'
axios.create({
  baseURL: baseUrl,
  timeout: 5000,
  responseType: 'json',
  headers: {
    'Content-Type': 'application/json;charset=utf-8'
  }
})

// 请求拦截
axios.interceptors.request.use(headConfig => {
  return headConfig
}, (error) => {
  return Promise.reject(error)
})

// 响应拦截
axios.interceptors.response.use(response => {
  if (response) {
    return Promise.resolve(response.data)
  } else {
    return Promise.reject()
  }
})

export default axios
```

5、配置（统一管理）接口文件

```js
import axios from "../utils/api";

const baseUrl = 'http://192.168.0.76:9999'
// 获取列表
function getTestList(params) {
  return axios.get(baseUrl + '/test-list', { params }).then(res => res.data)
}
// 编辑
function edit(params) {
  return axios.post(baseUrl + '/edit', params).then(res => res.data)
}

export {
  getTestList,
  edit
}
```

6、使用

```js
import { getTestList, edit } from '../api/test'

const params = {
  id: 1,
  name: '测试'
}
getTestList(params).then(res => {
  console.log('list', res);
})
edit().then(res => {
  console.log('edit', res);
})
```

> **特别注意：** 在配置 `baseUrl` 时应需要添加协议 `http` ，否则会出现 **请求路径重复拼接问题**
>
> - 正确
>
>   ```js
>   const baseUrl = 'http://path:9999'  // http://path:9999
>   ```
>
> - 错误
>
>   ```js
>   const baseUrl = 'path:9999'  // http://localhost:3000/path:9999
>   ```
>
>   

