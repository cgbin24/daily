> 在安装一个 package，而此 package 要打包到生产环境 bundle 中时，你应该使用 `npm install --save`。如果你在安装一个用于开发环境的 package 时（例如，linter, 测试库等），你应该使用 `npm install --save-dev`



> 比起 CLI 这种简单直接的使用方式，配置文件具有更多的灵活性。
>
> 我们可以通过配置方式指定 loader 规则(loader rule)、plugin(插件)、resolve 选项，以及许多其他增强功能



>使用 npm `scripts`，我们可以像使用 `npx` 那样通过模块名引用本地安装的 npm packages。
>
>这是大多数基于 npm 的项目遵循的标准，因为它允许所有贡献者使用同一组通用脚本



> 可以通过在 `npm run build` 命令与参数之间添加两个连接符的方式向 webpack 传递自定义参数，例如：`npm run build -- --color`



> ###### Warning
>
> 不要使用 webpack 编译不可信的代码。它可能会在你的计算机，远程服务器或者在你 web 应用程序使用者的浏览器中执行恶意代码。