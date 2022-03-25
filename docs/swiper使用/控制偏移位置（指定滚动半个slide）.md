#### 需求

> 使用 `swiper` 时需要自定义只滚动一半

#### 实现思路

> 通过参考 `swiper` 官网发现可以通过设置偏移量 **mySwiper.setTranslate(translate)** 实现

```js
translateCount: 0

// 设置偏移量
setTranslate () {
    // 获取视窗宽度
    const wwidth = window.innerWidth
    if (this.translateCount < 1) {
        this.translateCount++
    } else {
        this.translateCount--
    }
    // console.log(wwidth, '宽度', this.translateCount);
    this.searchSwiperObj.setTranslate(-(wwidth / 2) * this.translateCount)
},
```

调用

```js
const _this = this
_this.searchSwiperObj = new Swiper(".swiperContent", {
    speed: 200,
    // autoplay: true,
    on: {
        slideChange: function () {
            // this.activeIndex
            // this.realIndex
            _this.setTranslate()
        }
    }
})
```

