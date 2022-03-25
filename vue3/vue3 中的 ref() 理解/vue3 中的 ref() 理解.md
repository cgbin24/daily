### vue3 中的 ref(params) 理解

> 刚接触 `vue3.x` ，在很多地方见到 `ref()`， 通过查阅资料更多的理解它是做什么用的

##### 场景

```ts
// 1、传递一个普通值
const testRef = ref(1)
console.log(testRef, '======testRef=====');

// 2、传递一个 `null`
const testRef = ref(null)
console.log(testRef, '======testRef=====');

// 3、传递一个对象
const testRef = ref(obj)
console.log(testRef, '======testRef=====');

// 4、传递一个指定泛型的普通值
const testRef = ref<T>('')
console.log(testRef, '======testRef=====');
```

> [疑问]：这些都代表的啥意思呢？？

##### 查阅资料

###### 官网

> ## `ref`
>
> 接受一个内部值并返回一个响应式且可变的 ref 对象。ref 对象仅有一个 `.value` property，指向该内部值。

> 看完后明白了其用于操作一个响应式对象，且该对象只包含一个`value`属性，但还是一头雾水

###### 博客

1、[vue3中ref的理解 - 山竹回家了](https://blog.csdn.net/weixin_47886687/article/details/112919563)

> - **1.什么是ref?**
>   - ref和reactive一样,也是用来实现响应式数据的方法
>   - 由于reactive必须传递一个对象,所以在实际开发中如果只是想让某个变量实现响应式的时候回非常麻烦
>   - 所以Vue3提供了ref方法实现简单值得监听
> - **2.ref本质**
>   - ref底层其实还是reactive,所以当运行时系统会自动根据传入的ref转换成reactive.
> - **3.ref注意点**
>   - 在vue中使用ref的值不用通过value获取
>   - 在js中使用ref的值必须通过value获取

> 少许有点了解它的使用，知道了它底层还是基于`reactive`（什么是`reactive`，请参阅下一篇），主要是为了解决`reactive`不支持普通值实现响应式变化监听

2、[vue3之ref()理解 - 昕昕念念](https://zhuanlan.zhihu.com/p/346096502)

> ref()实际就是把传入的值对象或引用对象统一转换成Proxy对象，**用于监听对象的变动**，因为**Proxy只支持引用对象**，所以**对于值对象， 会转换成{ value: 值 } 再转换成Proxy对象**，此时就可以监听到value值的变化
>
> Proxy介绍可以参见官网:
>
> [Proxy - JavaScript | MDNdeveloper.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy](https://link.zhihu.com/?target=https%3A//developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)
>
> **目前Proxy的监听是单层的，new Proxy(obj) 只会把obj转成Proxy对象，obj内部的引用对象并不会转换，所以obj.a的变动可以监测到，而obj.a.b的变动就无法监测到了**， 这个是刚使用Proxy时一个容易迷惑的点， 如果**想要监听全部变动，只需要递归把对象内多有的引用对象全部转成Proxy对象就可以了**

3、[Vue3.x中的ref属性剖析（一看就懂） - BUG制造顶级工程师](https://blog.csdn.net/weixin_56658592/article/details/121598876)

##### 总结

> 刚开始一直不明白 ref(params) 方法中 传递的参数 是什么意思，其实可以将其理解为传递的参数就是 当前声明的变量的初始值，同时是响应式的数据，又因ref 对象仅有一个 .value property，并指向其内部值，所以可以通过 .value来进行访问，vue 会在当前变量值改变时立刻更新数据并渲染

```js
// 这个例子可以看看
const test = ref(1)
console.log(test, test.value, 'test1');
test.value++
console.log(test, test.value, 'test2');
```

