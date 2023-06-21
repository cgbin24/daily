## 解决Naive UI和Tailwindcss样式覆盖问题

> 当项目中同时使用到 `Naive UI` 和 `Tailwindcss` 时，出现了样式覆盖问题，`Tailwindcss` 样式直接将 `Naive UI` 样式覆盖导致无法正常使用

#### :: 解决方案
- 方式一
> 在 `main.js / main.ts` 文件中添加以下代码，主要原理是需要在应用初始化之前将 `naive-ui-style` 提前注入到应用当中，避免被覆盖

```ts
import { createApp } from "vue"
import App from "./App.vue"
import router from "./router"
...

// 初始化应用
const init = () => {
  // 添加 meta 标签，用于处理使用 Naive UI 和 Tailwind CSS 时的样式覆盖问题
  const meta = document.createElement('meta')
  meta.name = 'naive-ui-style'
  document.head.appendChild(meta)

  // 创建应用
  createApp(App)
  .use(router)
  ...
  .mount("#app")
}
init()
```

- 方式二
> 或者，可以在 `index.html` 根路径下的 `head` 元素中加入一个 `<meta name="naive-ui-style" />` 元素
```html
<html>
  ...
  <meta name="naive-ui-style" />
  ...
</html>
```

#### :: 衍伸阅读
[潜在的样式冲突](https://www.naiveui.com/zh-CN/os-theme/docs/style-conflict)
