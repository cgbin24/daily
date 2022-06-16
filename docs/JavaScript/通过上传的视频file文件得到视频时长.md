### 通过上传的视频file文件得到视频时长

> 将 `file` 文件转为 `blob` 格式，再通过 `video` 对象获取时长

```js
/**
 * 检测视频时长
 * @param {file} file
 * @return 视频时长
 */
checkVideoDuration(file) {
  const videoBlobObj = URL.createObjectURL(file)
  const videoDom = document.createElement('video')
  videoDom.src = videoBlobObj
  return new Promise((resolve)=>{
    videoDom.addEventListener('loadeddata',()=>{
      resolve(videoDom.duration)
    })
  })
}
```



> 参考 `file` 格式如下

```js
File {uid: 1655374042081, name: 'v3.mp4', lastModified: 1621560855423, lastModifiedDate: Fri May 21 2021 09:34:15 GMT+0800 (中国标准时间), webkitRelativePath: '', …}
uid: 1655374042081
lastModified: 1621560855423
lastModifiedDate: Fri May 21 2021 09:34:15 GMT+0800 (中国标准时间) {}
name: "v3.mp4"
size: 2598702
type: "video/mp4"
webkitRelativePath: ""
[[Prototype]]: File...
```
