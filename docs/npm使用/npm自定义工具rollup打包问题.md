### npm自定义工具rollup打包问题

> 使用 `rollup` 工具打包代码，具体配置文件如下

```js

import resolve from 'rollup-plugin-node-resolve'; // 依赖引用插件
import commonjs from 'rollup-plugin-commonjs'; // commonjs模块转换插件
import json from 'rollup-plugin-json'; // json
import babel from "rollup-plugin-babel"; // babel 插件
import { uglify } from 'rollup-plugin-uglify';

const paths = {
  input: {
      root:  'src/index.js',
  },
  output: {
      root:  'dist/',
  },
};
const fileName = `view-win.js`

export default {
  input: `${paths.input.root}`,
  output:{
    name: 'view-win',
    file: `${paths.output.root}${fileName}`,
    format: 'esm'
  },
  plugins: [
    json(),
    resolve(),
    commonjs(),
    babel({
      exclude: 'node_modules/**',
      runtimeHelpers: true,
      include: '**/*.js'
    }),
    uglify(),
  ],
  ignore: [
    "node_modules/**"
  ]
  
}
```

`package.json` 文件如下，`script` 脚本配置使用的就是 `rollup` 执行命令

```js
{
  "name": "view-win",
  "version": "0.1.0",
  "description": "视窗显示",
  "main": "src/index.js",
  "scripts": {
    "dev": "rollup -c rollup.config.js -w",
    "build": "rollup -c rollup.config.js"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/cgbin24/view-win.git"
  },
  "keywords": [
    "tab",
    "list",
    "scroll",
    "x-scroll",
    "tab scroll",
    "list scroll",
    "In-window display",
    "Center display"
  ],
  "author": "cgbin24",
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/cgbin24/view-win/issues"
  },
  "homepage": "https://github.com/cgbin24/view-win#readme",
  "devDependencies": {
    "@babel/core": "^7.20.12",
    "@babel/plugin-transform-runtime": "^7.19.6",
    "@babel/preset-env": "^7.20.2",
    "rollup-plugin-babel": "^4.4.0",
    "rollup-plugin-commonjs": "^10.1.0",
    "rollup-plugin-json": "^4.0.0",
    "rollup-plugin-node-resolve": "^5.2.0",
    "rollup-plugin-uglify": "^6.0.4"
  }
}

```

一切准备就绪，开始打包测试

```shell
npm run build
```

执行结果如下：

> 执行结果是这样的

<img src="/Users/cgbin24/Library/Application Support/typora-user-images/image-20230219111405966.png" alt="image-20230219111405966" style="zoom:20%;" />

刚开始我以为是由于 `rollup` 配置问题，无法识别，然后开始这样做，频频操作后还是找不到思路，困扰了许久

<img src="/Users/cgbin24/Library/Application Support/typora-user-images/image-20230219110330777.png" alt="image-20230219110330777" style="zoom:20%;" />

<img src="/Users/cgbin24/Library/Application Support/typora-user-images/image-20230219112752568.png" alt="image-20230219112752568" style="zoom:20%;" />

。。。此处省略诸多搜索关键词，及诸多搜索结果，简略截图

<img src="/Users/cgbin24/Library/Application Support/typora-user-images/image-20230219112350984.png" alt="image-20230219112350984" style="zoom:20%;" />

好不容易解决了上述问题，然后。。。

> 无法识别 `ES6` `class` 语法糖中的私有属性表示符 `#` 

<img src="/Users/cgbin24/Library/Application Support/typora-user-images/image-20230219105901709.png" alt="image-20230219105901709" style="zoom:20%;" />

然后我便再次尝试这样搜

<img src="/Users/cgbin24/Library/Application Support/typora-user-images/image-20230219111050872.png" alt="image-20230219111050872" style="zoom:20%;" />

<img src="/Users/cgbin24/Library/Application Support/typora-user-images/image-20230219113203149.png" alt="image-20230219113203149" style="zoom:20%;" />

好家伙，又是一顿疯狂操作，直到我再一篇文章中看到这么一句话

> 这里的 `plugin` 有序，结合上下文便知道指的就是 `rollup.config.json` 中 `rollup` 插件的配置次序

<img src="/Users/cgbin24/Library/Application Support/typora-user-images/image-20230219113340991.png" alt="image-20230219113340991" style="zoom:40%;" />

紧接着，我将配置文件中的插件次序修改如下

**特别说明：**

> `rollup-plugin-commonjs` 插件的功能：node_modules中的包大部分都是commonjs格式的，要在rollup中使用必须先转为ES6语法，为此需要安装插件 rollup-plugin-commonjs

```js

import resolve from 'rollup-plugin-node-resolve'; // 依赖引用插件
import commonjs from 'rollup-plugin-commonjs'; // commonjs模块转换插件
import json from 'rollup-plugin-json'; // json
import babel from "rollup-plugin-babel"; // babel 插件
import { uglify } from 'rollup-plugin-uglify';

const paths = {
  input: {
      root:  'src/index.js',
  },
  output: {
      root:  'dist/',
  },
};
const fileName = `view-win.js`

export default {
  input: `${paths.input.root}`,
  output:{
    name: 'view-win',
    file: `${paths.output.root}${fileName}`,
    format: 'esm'
  },
  plugins: [
    babel({
      exclude: 'node_modules/**',
      runtimeHelpers: true,
      include: '**/*.js'
    }),
    json(),
    resolve(),
    commonjs(),
    uglify(),
  ],
  ignore: [
    "node_modules/**"
  ]
  
}
```

然后，再次执行打包操作

```shell
npm run build
```

接着就会发现，这很完美

<img src="/Users/cgbin24/Library/Application Support/typora-user-images/image-20230219115238564.png" alt="image-20230219115238564" style="zoom:20%;" />



#### 衍生阅读

[rollup.js打包vue组件[!] (plugin commonjs) SyntaxError: Unexpected token (2:2)](https://blog.csdn.net/kalrase/article/details/110186870)

[使用rollup+es6+class 打包类库](https://www.ngui.cc/el/2226356.html)

[开发一个规范的 npm 包](https://juejin.cn/post/6945376222863949831)

[npm包 搭建，打包，调试，发布](https://juejin.cn/post/6871591252417216520)

[都3202年了，不会有人还不会发布npm包吧](https://juejin.cn/post/7172240485778456606)