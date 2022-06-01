#### el-table数据变化后未重新渲染问题

##### 问题描述：

> el-table列表项数据被编辑更改后，数据已更新，但列表未重新触发渲染

##### 解决方式：

> 在标签上添加一个 `key` 属性，每当数据内容变更后，动态修改 `key` 的值即可

```vue
<el-table
  :data="redeemTableData"
  style="width: 100%"
  ref="table"
  :key="tableKey"
> 
</el-table> 

export default {
	data() {
		tableKey: 1
	}
}

// 列表数据变更后修改 tableKey 即可，如：
tableKey++
```
