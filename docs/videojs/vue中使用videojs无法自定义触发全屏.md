#### 问题：vue中使用videojs无法自定义触发全屏

> [官网描述 --> enterFullScreen()](https://docs.videojs.com/html5#enterFullScreen)

这样用：

```js
var player = Video("myt-video");
player.enterFullScreen();
```

结果：

```shell
[Vue warn]: Error in v-on handler: "TypeError: Object(...)(...).enterFullScreen is not a function"...
```



#### 缓兵之计

暂且使用以下方法实现全屏功能

原生兼容性写法

```js
var element = document.getElementById("my-video");
if (element.requestFullScreen) {
	element.requestFullScreen();
} else if (element.mozRequestFullScreen) {
	element.mozRequestFullScreen();
} else if (element.webkitRequestFullScreen) {
	element.webkitRequestFullScreen();
}
```

总觉得不够优雅，想不明白都已经引用videojs插件了，为啥还用原生写法！！！

琢磨良久，查阅各种资料。。。可以使用以下方式实现

```js
var player = Video("my-video");
player.requestFullscreen()
```

成功实现

到这里还没完？