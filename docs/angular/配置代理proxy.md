### Angular 配置代理 `proxy` 实践

### :: 解决方案

#### :: 方式一 ::

- 第一步，在根目录或`/src` 下新建一个 `proxy.conf.json` 文件

  > 备注：这里不用纠结文件名称即`xxx.xxx.json`，只要使用时能找到，且正确配置文件内容格式即可

  ```json
  {
    "/dev-list": {
      "target": "https://a.examples.com/dev",	// 代理地址
      "secure": true,	// 接口是否开启 https
      "changeOrigin": true	// 支持跨域
    },
    "/dev-info": {
      "target": "https://a.examples.com/dev",
      "secure": true,
      "changeOrigin": true
    },
    "/test-list": {
      "target": "http://b.examples.com/test",
      "secure": false,
      "changeOrigin": true,
      "logLevel": "debug"	// 代理日志
    },
    "/test-info": {
      "target": "http://b.examples.com/test",
      "secure": false,
      "changeOrigin": true,
      "logLevel": "debug"
    }
  }
  ```

- 第二步，在 `angular.json` 文件中找到 `architect` 下的 `serve` ，并在 `serve` 内的 `options` 选项内配置 `proxyConfig`，没有就依次创建对应选项参数，事例如下：

  ```json
  ...
  {
    "architect": {
      ...
      "serve": {
        ...
        "options": {
          ...
          // 当前配置文件所做目录，相对于`angular.json`文件的位置，具体以实际配置位置为准
          "proxyConfig": "src/proxy.conf.json"	
        }
      }
    }
  }
  ...
  ```

- 第三步，重启项目，验证成果

  > 说明：这里的 `npm run start` 实际指向的也是 `ng serve` 启动方式，故不用配置 `--proxy-config`

  ```js
  npm run start 
  ===OR===
  ng serve
  ```

#### :: 方式二 ::

- 第一步，在根目录或`/src` 下新建一个 `proxy.conf.js` 文件

  > 备注：这里不用纠结文件名称即`xxx.xxx.js`，只要使用时能找到，且正确配置文件内容格式即可

  ```js
  const PROXY_CONFIG = [
  	{
      context: [
        "/dev-list",
        "/dev-info"
      ],
      "target": "https://a.examples.com/dev",
      "secure": true,
      "changeOrigin": true
    },
    {
    	context: [
    		"/test-list",
        "/test-info"
  		],
      "target": "http://b.examples.com/test",
      "secure": false,
      "changeOrigin": true,
      "logLevel": "debug"
    }
  ]
  
  module.exports = PROXY_CONFIG
  ```

- 第二步，在 `angular.json` 文件中找到 `architect` 下的 `serve` ，并在 `serve` 内的 `options` 选项内配置 `proxyConfig`，没有就依次创建对应选项参数，事例如下：

  ```json
  ...
  {
    "architect": {
      ...
      "serve": {
        ...
        "options": {
          ...
          // 当前配置文件所做目录，相对于`angular.json`文件的位置，具体以实际配置位置为准
          "proxyConfig": "src/proxy.conf.js"	
        }
      }
    }
  }
  ...
  ```

- 第三步，重启项目，验证成果

  > 说明：这里的 `npm run start` 实际指向的也是 `ng serve` 启动方式，故不用配置 `--proxy-config`

  ```js
  npm run start 
  ===OR===
  ng serve
  ```

### :: 总结

相对来讲，两种方式都可实现，具体差别也不大，但若是需要配置多个面向 `target` 的代理地址，且同时需要多个匹配规则时，推荐使用 **`方式二`** `proxy.conf.js` 的方式



### :: 衍伸阅读

[代理到后端服务器](https://angular.cn/guide/build#proxying-to-a-backend-server)
