### css边框内圆角

> 使用伪元素 `box-shadow` 实现

```
.active::before {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: calc(100%);
    content: "";
    border-radius: 5px;
    box-shadow: 0px 0 0 5px #fff; // 内外圆角
    z-index: 3;
}

.active::after {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: calc(100%);
    content: "";
    border-radius: 4px;
    box-shadow: 0px 3px 20px 7px rgba(255, 255, 255, 0.5); // 阴影
    z-index: 2;
}
```

