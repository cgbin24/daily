#### form表单非input框字段change时不会主动触发校验

> elementUI 绑定表单校验时，prop 和 v-model 对应一致，但是自定义规则后不会主动触发校验

```vue
<el-form-item label="星评数量" prop="starNum" :rules="{ required: isRequired, validator: starNumVerify, trigger: 'change' }">
    <el-rate class="stars" v-model="dialogForm.starNum" allow-half :disabled="dialogObj.type === 'detail' && true"></el-rate>
</el-form-item>
```

##### 自定义校验

```js
// 校验星评数量
starNumVerify (rule, value, callback) {
    // { min: 0.5, type: 'number', message: '星评数量不能为空' }]
    this.fieldsVerifyMaxMin(0.5, null, value, callback, '星评数量')
},
    
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

#### 解决

> 内容改变时，主动去触发单个字段校验

[自定义校验规则 --> Form Methods --> validateField](https://element.eleme.io/#/zh-CN/component/form)

```vue
<el-form-item label="星评数量" prop="starNum" :rules="{ required: isRequired, validator: starNumVerify, trigger: 'change' }">
    <el-rate class="stars" v-model="dialogForm.starNum" allow-half :disabled="dialogObj.type === 'detail' && true" @change="handleVerifyfields('starNum')"></el-rate>
</el-form-item>
```

```js
// 触发单个字段校验
handleVerifyfields (props) {
    this.$refs.dialogForm.validateField(props)
},
```

##### 参考

[ELEMENT表单校验不生效问题](https://www.freesion.com/article/93441052591/)

