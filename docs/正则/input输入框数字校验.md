```js
// value 为具体校验内容
正整数: /^\d+$/.test(value)
负整数: /^-\d+$/.test(value)   
整　数: /^-?\d+$/.test(value)
正小数: /^\d+\.\d+$/.test(value)
负小数: /^-\d+\.\d+$/.test(value)
小　数: /^-?\d+\.\d+$/.test(value)
实　数: /^-?\d+\.?\d*$/.test(value)
保留1位小数: /^-?\d+\.?\d{0,1}$/.test(value)
保留2位小数: /^-?\d+\.?\d{0,2}$/.test(value)
保留3位小数: /^-?\d+\.?\d{0,3}$/.test(value)
```

