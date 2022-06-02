### wangeditor & vue2简易使用

> `vue2.x` 搭配 `wangeditor` 简易使用

#### 安装

```shell
npm install @wangeditor/editor --save
npm install @wangeditor/editor-for-vue --save
```

#### 基础引入

```js
import '@wangeditor/editor/dist/css/style.css'
import { Editor, Toolbar } from '@wangeditor/editor-for-vue'
```

#### 使用

##### html结构

```vue
<template>
  <div class="contentWrapper">
    <div style="border: 1px solid rgba(37, 40, 50, 0.1);">
      <Toolbar
        style="border-bottom: 1px solid rgba(37, 40, 50, 0.1)"
        :editor="editor"
        :defaultConfig="toolbarConfig"
        :mode="mode"
      />
      <Editor
        style="height: 900px; overflow-y: auto;"
        v-model="html"
        :defaultConfig="editorConfig"
        :mode="mode"
        @onCreated="onCreated"
      />
    </div>
  </div>
</template>
```

#### script - js逻辑

> 初始化、基础配置

- 注：

  **excludeKeys**：

  > 如果仅仅想排除掉某些菜单，其他都保留，可以使用 `excludeKeys` 来配置。
  > 可通过 `toolbar.getConfig().toolbarKeys` 查看工具栏的默认配置

  **MENU_CONF**:

  > 菜单配置，如：`uploadImage` 则为上传图片配置参数

  **customInsert**：

  > 如果你的服务端 response body 无法按照上文规定的格式，则无法插入图片，提示失败。
  > 但你可以使用 `customInsert` 来自定义插入图片

```js
components: { Editor, Toolbar },
data () { 
  return {
    editor: null,
    html: '',
    toolbarConfig: {
      excludeKeys: [
        "uploadVideo",
        "insertVideo",
        "fullScreen"
      ]
    },
    editorConfig: { 
      placeholder: '请输入内容...',
      MENU_CONF: {
        uploadImage: {
          fieldName: 'file',
          server: baseURL + '/v1/file/upload',
          allowedFileTypes: ['image/*'],
          headers: {
            token: getStorage('token'),
          },
          // 自定义插入图片
          customInsert(res, insertFn) {
            insertFn(res.data, '图片', res.data)
          }
        }
      }
    },
    // mode: 'default', // or 'simple'
    mode: 'simple', // or 'simple'
  }
},
```

> 实例化

```js
methods: {
  // 初始化
  onCreated(editor) {
    this.editor = Object.seal(editor)
    console.log(this.editor, this.editor.getAllMenuKeys(), '编辑器')
  },
},
```

> 销毁实例

```js
beforeDestroy() {
  const editor = this.editor
  if (editor == null) return
  editor.destroy() // 组件销毁时，及时销毁编辑器
}
```

#### 链接

> 更多内容请参阅一下内容

[wangEditor](https://www.wangeditor.com/)