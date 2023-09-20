#### echarts移动端+vue3实例声明导致效果失效问题

#### 环境
| 技术栈 | 版本 |
| ---- | ---- | 
| vue | 3.2.47 |
| echarts | 5.4.2 |
| vant | 4.3.1 |
| vite | 4.3.2 |


> 使用技术栈 `Vue@3.2.47` + `echarts@5.4.2` 实现图表📈可视化面板，引入 `echarts` 后按照正常流程配置完参数信息，与面板信息交互时，`tooltip` 以及滑动效果失效，同时爆出如下错误：

```sh
dataSample.js:112 Uncaught TypeError: Cannot read properties of undefined (reading 'type')
    at Object.reset (dataSample.js:112:34)
    at Task2.seriesTaskReset [as _reset] (Scheduler.js:485:70)
    at Task2._doReset (task.js:202:23)
    at Task2.perform (task.js:117:33)
    at Scheduler.js:272:20
    at util.js:487:16
    at Map.forEach (<anonymous>)
    at HashMap2.each (util.js:486:19)
    at Scheduler.js:255:23
    at Array.forEach (<anonymous>)
```

经过各种配置尝试，始终未生效
最后查看官方 `github` 仓库中的 `issues` 得以解决
原来我在声明实例对象时使用了 `ref` 的方式错误的引用了实例，推荐使用 `shallowRef` 方式声明

```ts
...
const echartsIns = shallowRef()
...
echartsIns.value = echarts.init(document.getElementById('main'))
```

#### 延伸阅读

[[Bug] ECharts.resize() ERROR! Cannot read properties of undefined (reading 'type') #16642](https://github.com/apache/echarts/issues/16642)

[响应式 API：进阶](https://cn.vuejs.org/api/reactivity-advanced.html)
