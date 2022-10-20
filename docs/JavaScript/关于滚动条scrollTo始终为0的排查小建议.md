### 关于滚动条scrollTo始终为0的排查小建议

> 1、首先确定当前出现的 **滚动条是哪个DOM元素产生的**
>
> 2、**滚动事件是否绑定** 正确 (绑定对象是 **window** / **document** )
>
> 3、**高度继承问题**, 容器高度尽可能减少高度继承 (建议固定, 减少使用 **%** )
>
> 4、若滚动实时**监听不生效**, 建议使用如下方式绑定
>
> ```document.addEventListener("scroll", callbackEvent, true)```

##### 衍生阅读

[为什么scrollTop设置后一直为0的解释和解决方案（精品）](https://blog.csdn.net/kouryoushine/article/details/99745904)
