#### form表单未全部清空

> 调用 `resetFields()` 方法未能将表单完全重置清空

| resetFields | 对整个表单进行重置，将所有字段值重置为初始值并移除校验结果 |
| ----------- | ---------------------------------------------------------- |

根据官网说明，该方法只是将内容重置到初始状态，并不是想要的将内容直接清空

于是，想要达到清空的效果，还得手动实现

> 下面实现了一个兼容的方法

```js
// 清空表单数据
resetForm () {
    if (this.$refs.dialogForm) {
        this.$refs.dialogForm.resetFields()
        // 检测字段是否清空
        for (const formItem in this.dialogForm) {
            // 过滤数组、对象
            if (this.dialogForm[formItem] && typeof this.dialogForm[formItem] !== 'object') {
            this.dialogForm[formItem] = ''
          }
        }
    }
},
```



#### 参考

[ElementUI嵌套Form的Dialog如何重置Form](https://segmentfault.com/a/1190000019733787?utm_source=tag-newest)