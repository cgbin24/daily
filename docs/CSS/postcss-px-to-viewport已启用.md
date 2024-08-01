### postcss-px-to-viewport已弃用

> `postcss-px-to-viewport` 插件将 px 单位转换为视口单位的 (vw, vh, vmin, vmax) 的 [PostCSS](https://github.com/postcss/postcss) 插件. 因该插件依赖的`PostCSS 8.x`版本发生了重大更新，推出了更多高性能API接口供开发者调用，同时`postcss-px-to-viewport`插件停滞版本（`v1.1.1`）已久，建议使用新插件（postcss-px-to-viewport-8-plugin）替换

```sh
postcss-px-to-viewport: postcss.plugin was deprecated. Migration guide:
https://evilmartians.com/chronicles/postcss-8-plugin-migration

```

#### 解决方案

> 使用 `postcss-px-to-viewport-8-plugin` 替换，具体使用方式请参照下方链接

```sh
npm install postcss-px-to-viewport-8-plugin -D
or
yarn add postcss-px-to-viewport-8-plugin -D
```
[postcss-px-to-viewport-8-plugin](https://www.npmjs.com/package/postcss-px-to-viewport-8-plugin)
