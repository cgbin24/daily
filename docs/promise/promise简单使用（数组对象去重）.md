#### 数组对象列表去重

> 数组中存放多个对象，向数组中添加时需要将重复的清除掉（不添加）
>
> 注：这里使用promise是因为不知道对数组的操作什么时候结束

```js
// 去重方法
filterObj(newObj, ObjList) {
    let flag = false // 添加的对象是否已经存在
    let len  = ObjList.length
    let pObj = new Promise((resolve, reject)=>{
        if(len > 0) {
            flag = ObjList.some((item)=>{
                if (item.title == newObj.title) {
                    return true
                }
            })

            if(!flag) {
                resolve()
            } else {
                reject()
            }
        } else {
            resolve()
        }
    })

    return pObj
},
```

```js
// 使用
let list = []
let obj = {
    title: 'wang'
    name: '小王'
}
this.filterUser(user, this.volMngList2).then(res=>{
    this.list = [...this.list, obj];
}).catch(err=>{
    console.log(err);
})
```



##### 参考

https://www.runoob.com/jsref/jsref-some.html

> some() 方法用于检测数组中的元素是否满足指定条件（函数提供）。
>
> some() 方法会依次执行数组的每个元素：
>
> - 如果有一个元素满足条件，则表达式返回*true* , 剩余的元素不会再执行检测。
> - 如果没有满足条件的元素，则返回false。
>
> **注意：** some() 不会对空数组进行检测。
>
> **注意：** some() 不会改变原始数组。