#### 为什么要使用tsconfig.json文件

<hr/>

> 若不添加该文件，则项目不完全受`typescript` 规则约束；
>
> `tsconfig.json`文件可以是个空文件，那么所有默认的文件都会以默认配置选项编译



> 添加该文件后，处于管理范围内的的文件都将受到配置规则的约束；
>
> 代码编译时，会对项目进行检测



> `tsconfig.json`包含了工程里TypeScript特定的选项



`tsconfig.json`文件中指定了用来编译这个项目的根文件和编译选项。 一个项目可以通过以下方式之一来编译：

#### 使用tsconfig.json

- 不带任何输入文件的情况下调用`tsc`，编译器会从当前目录开始去查找`tsconfig.json`文件，逐级向上搜索父目录。
- 不带任何输入文件的情况下调用`tsc`，且使用命令行参数`--project`（或`-p`）指定一个包含`tsconfig.json`文件的目录。

当命令行上指定了输入文件时，`tsconfig.json`文件会被忽略。



```shell
17:40:50 [vite] changed tsconfig file detected: /project/tsconfig.json - Clearing cache and forcing full-reload to ensure TypeScript is compiled with updated config values.
```

##### `.d.ts` 格式文件说明

> `.d.ts`文件为指定类型文件的声明文件，作用于让 `Typescript` 知道如何使用该类型文件，如：使用 `Enzyme`测试是需要同时安装 `@types/enzyme` ,而`@types/enzyme`则包含了声明文件（`.d.ts`文件）的包，以便TypeScript能够了解该如何使用Enzyme。



##### 如何创建

让我们通过简单配置编译器测试一下：

- 创建一个新的空文件夹。
- 放置一个`index.html`文件，文件内容是基础的 HTML5 结构。
- 在`index.html`同一层，创建一个空的`index.ts`文件。
- 打开终端，并输入`tsc --init`（假设你是全局安装 TypeScript），便会创建一个 `tsconfig.json`文件。（我们将在下一章详细探讨这个文件）



#### 参考

[tsconfig.json 详解与常用配置（笔记）](https://blog.csdn.net/qq_16051405/article/details/126671093)

[写给 React 开发者的 TypeScript 指南](https://www.freecodecamp.org/chinese/news/typescript-for-react-developers/)

[tsconfig.json - tslang.cn](https://www.tslang.cn/docs/handbook/tsconfig-json.html)