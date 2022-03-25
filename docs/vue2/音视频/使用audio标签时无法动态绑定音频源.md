#### 使用audio标签时无法动态绑定音频源

> 在vue中想要实现动态绑定 `audio` 的 `src` 属性

##### 踩坑（无效）

> 动态改变 `sourceUrl` 的值，无效

```vue
<audio ref="remainTipsObj">
    <source :src="sourceUrl" type="audio/mpeg">
    您的浏览器不支持 audio 元素
</audio>
```

##### 推荐（实现）

> 直接通过 `ref` 去操作该节点上的 `src` 属性，为其赋值即可

> `chargeTipsList` 为音频源对象，里面存储多个值

```vue
<audio ref="remainTipsObj" type="audio/mpeg">
    您的浏览器不支持 audio 元素
</audio>
```

```js
/**
     * 剩余时长提醒（5min、3min、1min）
     * @param {Number} min 分钟
     */
handleChargeTipsVol (min) {
    if (min === 1) {
        this.$refs.remainTipsObj.src = this.chargeTipsList.oneMinTips
        this.$refs.remainTipsObj.play()
    } else if (min ===3) {
        this.$refs.remainTipsObj.src = this.chargeTipsList.threeMinTips
        this.$refs.remainTipsObj.play()
    } else if (min === 5) {
        this.$refs.remainTipsObj.src = this.chargeTipsList.fiveMinTips
        this.$refs.remainTipsObj.play()
    }
},
```

