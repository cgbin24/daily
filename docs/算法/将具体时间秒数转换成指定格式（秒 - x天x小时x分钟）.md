#### 将具体时间秒数转换成指定格式（秒 - x天x小时x分钟）

```js
// 格式化时间 (秒 -> x天x时x分)
formatSecToDHM  (second) {
  let d = 0
  let h = 0
  let m = 0
  let transM = 0
  if (second > 0) {
    m = Math.round(second / 60) // 存在30s误差
    console.log('second: ' + second, 'minuts: ' + m, '分钟');
  }
  if (m >= 60) {
    transM = m / 60
    h = transM.toString().split('.')[0]
    m = Math.round((transM - h) * 60)
    console.log('transM: ' + transM, 'hour: ' + h, 'minuts: ' + m, '小时');
  }
  if (h >= 24) {
    const transH = h / 24
    d = transH.toString().split('.')[0]
    h = Math.round((transH - d) * 24)
    console.log('transH: ' + transH, 'day: ' + d, 'hour: ' + h, '天');
  }
  return d + '天' + h + '小时' + m + '分钟'
}
```

