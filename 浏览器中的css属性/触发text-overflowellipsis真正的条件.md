#### 问题

> 触发text-overflowellipsis真正的条件

- 元素设置文本超出显示省略号，效果实现，但是想要通过JavaScript事件监听text-overflow的触发时机或触发条件
- 参考文档如下，当未能实现

```javascript
// 超出聚焦滚动
overflowScroll() {
    const ele = document.getElementsByClassName("ellipsis")[0];
    ele.style.overflow = "initial";
    const clientWidth = ele.clientWidth;
    const scrollWidth = ele.scrollWidth;
    const offsetWidth = ele.offsetWidth;
    const domPadding = ele.style.padding;

    if (clientWidth < scrollWidth) {
        alert("已省略……");
    } else {
        alert("为省略");
    }
    console.log("比较：", clientWidth, scrollWidth, offsetWidth, domPadding);
},
```



https://segmentfault.com/a/1190000017830610

https://www.cnblogs.com/agansj/p/7885469.html





```js
{
    num1: 1,
    num2: 2,
    num3: 2,
    symbole1: "+",
    symbole2: "+",
    symbleList: ["+", "-", "×", "÷", "+"],
}
          
// 产生随机计算式
randomCount() {
    this.num1 = Math.round(Math.random() * 20);
    this.num2 = Math.round(Math.random() * 20);
    this.num3 = Math.round(Math.random() * 20);
    let randomS1 = Math.round((Math.random() * 10) / 2);
    let randomS2 = Math.round((Math.random() * 10) / 2);
    this.symbole1 = this.symbleList[randomS1 > 0 ? randomS1 - 1 : 0];
    this.symbole2 = this.symbleList[randomS2 > 0 ? randomS2 - 1 : 0];
    let s1 = this.symbole1;
    let s2 = this.symbole2;

    // 计算结果
    if (this.symbole1 == "×") {
        s1 = "*";
    }
    if (this.symbole2 == "×") {
        s2 = "*";
    }
    if (this.symbole1 == "÷") {
        s1 = "/";
    }
    if (this.symbole2 == "÷") {
        s2 = "/";
    }
    // 转为字符串
    let str = this.num1 + "" + s1 + "" + this.num2 + "" + s2 + "" + this.num3;
    console.log(str, "===");
    // 使用eval函数进行计算，后期将使用其他方式进行优化
    this.randomResult = Math.round(eval(str));
    console.log(this.randomResult);
},
```



```js
let str = '你好，世界'
let len = str.replace(/[^\x00-\xff]/g, "01").length;
// 把双字节的替换成两个单字节的然后再获得长度
```



```js
let str = '这是一段滚动的描述文本'
let strList = str.split("");
// 截取第一个
let first = strList.splice(0, 1);
// 将截取到的值添加到末尾
strList.splice(strList.length, 0, ...first);
// 转为字符串
let newList = strList.join("");
console.log(str, strList, first, newList, "------");
```

