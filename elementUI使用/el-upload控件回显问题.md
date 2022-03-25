#### 问题描述

> 使用 `el-upload` 控件，添加和编辑表单公用同一个组件，添加上传回显没有问题（组件内部已经处理好了，使用者不用考虑），问题在于编辑时后端只提供一个图片的url，此时就不知所措了，思考良久始终找不到头绪

##### 初始代码

```vue
<el-upload
    :class="{hideBtn: ishideUploadBtn}"
    :action="uploadImgBaseUrl"
    list-type="picture-card"
    :limit="limitUploadCount"
    :data="{type: 'image'}"
    accept="image/*"
    :on-remove="handleRemove"
    :on-success="getGameImgUrl">
    <!-- accept=".gif,.jpg,.jpeg,.png,.GIF,.JPG,.PNG*" -->
	<i class="el-icon-plus"></i>
</el-upload>
```

##### 改良后

- 说明

  - 使用 `el-upload` 时可以通过控制 `file-list` 属性对上传列表进行控制，该属性对应的值是数组类型，使用时只要 -对应的数据格式- 正确即可

    ```js
    imgFileList: [
    	{
    		name: '123.jpg',
    		url: 'https://cloudneedle-tv.oss-cn-hangzhou.aliyuncs.com/hotel/image/1637757854633987700315.jpg'
    	}
    ]
    ```

  - 参考官方文档

    > 详见下面文档 `Attribute` 栏中的 `file-list` 列
    >
    > https://element.eleme.io/#/zh-CN/component/upload

```vue
<el-upload
    :class="{hideBtn: ishideUploadBtn}"
    :action="uploadImgBaseUrl"
    list-type="picture-card"
    :limit="limitUploadCount"
    :data="{type: 'image'}"
    accept="image/*"vue
    :on-remove="handleRemove"
    :on-success="getGameImgUrl"
    :file-list="imgFileList">
    <!-- accept=".gif,.jpg,.jpeg,.png,.GIF,.JPG,.PNG*" -->
    <i class="el-icon-plus"></i>
</el-upload>
```

##### 部分回调/公用控制方法

```js
// 获取图片地址
getGameImgUrl (res, file, fileList) {
  console.log(file, '获取图片地址')
  if (res.code === 200) {
    this.dialogForm.gameImg = res.data
    this.imgFileList = fileList
    this.controlUploadBtn()
  }
},
// 移除已上传图片
handleRemove (file, fileList) {
  this.imgFileList = fileList
  this.controlUploadBtn()
  this.dialogForm.gameImg = ''
},
// 控制上传图片按钮
controlUploadBtn () {
	this.ishideUploadBtn = this.imgFileList.length >= this.limitUploadCount
},
```

##### 参考

> 以下文章提供解决思路

https://www.cnblogs.com/home-/p/11962347.html