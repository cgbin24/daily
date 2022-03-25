#### input框输入过程中转换格式（清除前面多余的0）

> 当输入框中需要输入数字，且考虑到不能出现多个0的情况（如：0000）

可参考下例实现方式

```vue
<el-form-item prop="width" :rules="{ required: isRequired, validator: sizeWidthVerify, trigger: ['blur', 'change'] }">
    宽： <el-input v-model="dialogForm.width"  maxlength="6" oninput="value=value.replace(/[^\d]/g, '')" @input="handleTransType(dialogForm.width, 'width')"></el-input>
</el-form-item>
```



```js
// 转化数据类型
handleTransType (val, objDesc) {
    this.dialogForm[objDesc] = +val
},
```

