#### 问题描述

> ValidationError: webpack Dev Server Invalid Options options should NOT have

[webpack devServer配置](https://webpack.docschina.org/configuration/dev-server/#devserverproxy)

```js
 devServer: {
    // host: 'localhost',
    port: 8089,
    open: true,
    https: false,
    proxy: {
      '/v1':{
        target:'https://tvapidev.needleos.com', // 目标地址
        changeOrigin:true,
        secure: false,
        pathRewrite:{
          '^/v1': ''
        }
      }
    } //配置跨域支持
  },
```

