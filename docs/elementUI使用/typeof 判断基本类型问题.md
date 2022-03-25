##### 问题

> 使用 `typeof` 判断基本数据类型，同时在判断时用 `+` 号进行数据类型转换

###### 字符串

- 情况1，字符串中非数字

```js
let str = '你好'
// undefined
+str
// NaN
```

- 情况2，字符串中是数字

```js
let num = '123'
// undefined
+num
// 123
```

###### 使用 `typeof` 判断

```js
typeof +str
// 'number'
typeof +num
// 'number'
```

> 结果如上，都是 `number` 类型，所以不能直接使用 `typeof` 来判断，这样达不到效果

##### 推荐方式

> 因为需要对字符串中的内容进行类型转换，是数字的转换后还是数字，不是数字的转换后就是 `NaN` ，所以这里推荐使用 `isNaN()` 方法来判断

```js
isNaN(+str)
// true
isNaN(+num)
// false
```



