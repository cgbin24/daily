### 微信小程序构建npm使用记录

#### 问题

> 使用原生微信小程序开发时，通过官方 `typescript` 模板构建的小程序无法正确执行 `构建npm` 成功，从而导致我想通过 `npm` 安装并使用第三方库出现问题



#### 开发环境（可参照）

**设备**：`macOS Ventura 13.0`

**微信开发者工具**：`Stable 1.06.2303060`

**创建模板**：`typescript + sass` 【这里使用的官方推荐模板创建，可参照使用，不必纠结】



#### 错误呈现

- 无法 `构建npm`

```
NPM packages not found. Please confirm npm packages which need to build are belong to `miniprogramRoot` directory. Or you may edit project.config.json's `packNpmManually` and `packNpmRelationList` [1.06.2303060][darwin-arm64]
```

```
message： NPM packages not found. Please confirm npm packages which need to build are belong to `miniprogramRoot` directory. Or you may edit project.config.json's `packNpmManually` and `packNpmRelationList`
appid: wx9074ccabd205e09f
openid: o6zAJs_Ipggw1hbEAnsZ3HdgoIzY
ideVersion: 1.06.2303060
osType: darwin-arm64
time: 2023-04-02 10:56:01
```



#### 解决方案

【官方建议】[npm 支持](https://developers.weixin.qq.com/miniprogram/dev/devtools/npm.html)

> 关键的地方就是需要在 `project.config.json` 文件中的 `setting` 对象下配置如下内容，在此之前请确保已初始化【`npm init`】项目

```json
"packNpmManually": true,
"packNpmRelationList": [
  {
    "packageJsonPath": "./package.json",
    "miniprogramNpmDistDir": "./app/"
  }
],
```

**但是**，【**这很重要**‼️】，若你也使用了 `typescript` 构建项目的话，完成上面这一步依旧无法成功，你需要尝试在 `setting` 配置下先将 `typescript` 相关去除，【**重启编辑器**】后 再次尝试

```json
"setting": {
  ...
  "typescript"	// 删除 -> 保存更改 -> 清除缓存 -> 编译 -> 构建npm
}
```

这是可能又会出现【模拟器启动失败】的情况，这是因为，项目使用 `typescript` 创建，而插件被删除了，无法匹配到入口文件，错误如下：

```sh
[ app.json 文件内容错误] app/app.json: 未找到 ["pages"][0] 对应的 pages/index/index.js 文件(env: macOS,mp,1.06.2303060; lib: 2.30.3)
```

> 直接一点的方式，就是通过调整 `app.json` 文件让其自动为你创建一个 `xxx.js` 的文件，这里其实不用修改什么，随便删除一个引号再加回来就行，目的是要让编辑器知道这个文件被改动了，它就会重新生成文件，保存后依次执行 【清缓存 -> 编译】,然后就可以看到项目中多了一个`xxx.js`的文件了，如下事例

```
├── logs
│   ├── logs.js
│   ├── logs.json
│   ├── logs.ts
│   ├── logs.wxml
│   ├── logs.wxss
```

此时模拟器报错将消失，但屏幕上没有内容？不用担心，别忘了做这些的核心是解决什么问题【**构建npm**】，接下来就是见证奇迹的时刻，依次做如下操作【如果不行，请记得 **重启开发者工具** ，try again】

```
工具栏中找到【工具】 -> 找到【构建npm】 并用力点击它
```

💡然后，然后你就会\*\*\*\*，是的，没错，你做到了，`构建成功了`，此时不出意外的话，你的项目里面应该会有这样一个目录【**miniprogram_npm**】



接下来，就需要将之前的内容补全，然后奇迹再次发生，幸运再次降临，项目正常启动【酷炫的页面重获新生】，当然，刚刚生成的 `xxx.js` 文件可删也可不删并没有什么大的影响，存储空间当然会占用，介意的话就用力删除吧，留下来呢也是兼容上文 `typescript` 被删除后模拟器无法运行的情况

```json
"setting": {
  ...
  "typescript"	// 加回来
}
```



#### 重要提醒‼️

> 修改配置后，没效果或是出现报错，请记得【**重启编辑器**】
