### vue3 + vite 打包错误问题

#### 问题描述：

使用vue3 + vite 构建项目后，直接运行 `npm run build` 报错，检查配置项无果，排查依赖项，究其原因还是排错能力不及，**应当报什么错误，就首先找到错误是什么（定位 -- 看报错信息）**

```shell

node_modules/element-plus/lib/utils/objects.d.ts:1:30 - error TS2307: Cannot find module 'type-fest' or 
its corresponding type declarations.

1 import type { Entries } from 'type-fest';
                               ~~~~~~~~~~~

```

##### 解决

```shell
npm install type-fest
```



> 经排查后发现，缺少 `type-fest` 依赖项

```shell

node_modules/element-plus/lib/components/upload/index.d.ts:1:23 - error TS2688: Cannot find type definition file for 'node'.

1 /// <reference types="node" />
                        ~~~~

```

> 经排查后发现，缺少 `` 依赖项

##### 参考

[vscode 升级1.18遇到Cannot find type definition file for 'node' - 洺鱼](https://blog.csdn.net/qq_16660859/article/details/78540525)

##### 解决

```shell
npm install @types/node –save
```

