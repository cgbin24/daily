#### this.$refs[formName].validate失效

> 自定义校验规则必须保证最后都会有一个 `callback()` 返回，否则失效

[自定义校验规则](https://element.eleme.io/#/zh-CN/component/form)

```js
// 校验字段最大最小值
fieldsVerifyMaxMin (min, max, val, callback, objDesc) {
    if (!(val + '')) {
        return callback(new Error(`${objDesc}不能为空`))
    } else {
        return callback()
    }
},
```



#### 基于elementUI自定义校验字段

> 校验内容为数值型

```js
/**
     * 校验字段最大最小值
     * @param {Number} min 最小值
     * @param {Number} max 最大值
     * @param {Number} val 当前校验的值
     * @param {Function} callback 回调
     * @param {String} objDesc 当前字段描述信息
     */
fieldsVerifyMaxMin (min, max, val, callback, objDesc) {
    if (!(val + '')) {
        return callback(new Error(`${objDesc}不能为空`))
    }
    if (max && val > max) {
        return callback(new Error(`${objDesc}不能大于${max}`))
    }
    if (min && val < min) {
        return callback(new Error(`${objDesc}不能小于${min}`))
    }
    callback()
},
```

