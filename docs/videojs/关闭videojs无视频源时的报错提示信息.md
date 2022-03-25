#### 问题：

> The media could not be loaded, either because the server or network failed or because the format is not supported.

```js
let player = videojs(video, {}, function(){
    this.on('error', function(){
        player.errorDisplay.close();   //将错误信息不显示
        // 自定义显示方式
    })
});
```

#### 参考

https://segmentfault.com/q/1010000020294059

