#### ES6 Class类 事件的注册与销毁

<hr/>

`Es6` 中使用 `Class` 语法糖注册监听事件、销毁监听事件的使用小结

> **注：**本文主要探讨**事件注册后无法注销**的问题，**未涵盖事件可选参数的使用**



##### 普通函数使用

> 以下为简单使用案例

```js
// 被监听事件
function handleClick(e) {
  console.log('注册', e)
}
const app = document.querySelector('#app')
// 注册监听器
app.addEventListener('click', handleClick)
// 销毁监听器
app.removeEventListener('click', handleClick)
```



##### Class类中使用

> 以下为 `ES6` `Class` 语法糖中使用案例，为方便管理和还原踩坑场景，故将原生事件注册和销毁简单抽取封装了一下

```js
class Test {
  ele = null
  constructor(ele) {
    this.ele = ele
    this.init()
  }
  init() {
    this.regListener(this.ele, 'mouseenter', this.handleEnter)
    this.regListener(this.ele, 'click', this.handleClick)
  }
  // 事件注册
  regListener(target, event, callback) {
    target.addEventListener(event, callback)
  }
  // 事件销毁
  rmListener(target, event, callback) {
    target.removeEventListener(event, callback)
  }
  // 鼠标移入
  handleEnter(e) {
    console.log('enter', e)
  }
  // 点击
  handleClick(e) {
    console.log('click', e)
    // this.ele.removeEventListener('mouseenter', this.handleEnter)
    this.rmListener(this.ele, 'mouseenter', this.handleEnter)
  }
}
```

如上代码所述，存在以下几个问题

- 当调用执行代码后会报错

  ```shell
  Uncaught TypeError: Cannot read properties of undefined (reading 'removeEventListener')
      at HTMLDivElement.handleClick (test.html:39:18)
      at HTMLDivElement.handleClick (test.html:39:18)
      
  ===OR===
  
  Uncaught TypeError: this.rmListener is not a function
      at HTMLDivElement.handleClick (test.html:40:14)
      at HTMLDivElement.handleClick (test.html:40:14)
  ```

- 监听器事件依旧存在



##### 问题分析🤔

首先，进入排错阶段，因上述绑定事件时未传递任何参数，故推测当前（指`handleClick`）方法的上下文 `this` 指向的是调用者本身（即：事件触发的对象`this.ele`），而并非期望的 `Class` 对象



##### 验证并解决🔬

经实践验证，确实存在上述问题，且可通过以下几种方式解决 `Class` 中 `this` 指向问题

- 声明函数时使用箭头函数 `() => {}`，如下

  > 使用这种方式，既可以解决 `this` 指向问题，同时也更便于解决本文核心问题

  ```js
  // 点击
  handleClick = (e) => {
    console.log('click', e, this)	// this 指代当前 Class 的上下文
  }
  ```

- 注册监听器时使用箭头函数，如下

  > 注意⚠️：使用这种方式可以解决 `this` 指向问题，**但是**，回到本文核心问题**监听事件的注册和销毁**来看，注册是可以，**却无法销毁**，下文提供解决方案可供参考使用

  ```js
  this.regListener(this.ele, 'mouseenter', (e) => this.handleEnter(e))
  ```



##### 解决核心问题🎯

言归正传，上文已解决了 `this` 上下文的问题，下面继续讨论 `事件的注册和销毁问题`，这是MDN上对 `removeEventLister()` 方法的一小段阐述

> 调用 `removeEventListener()` 时，若传入的参数不能用于确定当前注册过的任何一个[事件监听器](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener#事件监听回调)，该函数不会起任何作用。
>
>  [详见EventTarget.removeEventListener()](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/removeEventListener)

也就是说，注册和销毁传递的事件必须为同一个，即 `addEventListener` 和  `removeEventListener` 传递的 `listener` 必须保证是同一个函数对象，而上文中解决 `this` 指向的第二种方式传递的相当于一个匿名函数，内部再调用另一个函数；同样的销毁事件中也要传递一个一样的匿名函数，但本质上两者之间却已不是同一个函数

```js
(() => {}) === (() => {}) // false
```

因此，这样的操作方式便无法成功销毁事件

```js
this.removeListener(this.ele, 'mouseenter', (e) => this.handleEnter(e))
// 无效，事件仍然存在
```

话虽如此，若真的想要解决问题，也并非没有办法，承接上文 `Class` 中的事件抽象我们可以使用一个对象，比如数组，将准备注册的事件对象及其方法等参数一同存储起来，也可以理解为拷贝一份，如此便可在要销毁的地方将其取出销毁即可

```js
class Test {
	eventStack = []
	...
  init() {
    this.regListener(this.ele, 'mouseenter', (e) => this.handleEnter(e))
    ...
  }
	// 事件注册
  regListener(target, event, callback) {
    // 保存注册对象 及其事件函数
    eventStack.push({ target, event, callback })
    target.addEventListener(event, callback)
  }
	...
  // 点击
  handleClick(e) {
    console.log('click', e)
    // 销毁所有注册事件
    this.eventStack.forEach((item, index) => {
      const { target, event, callback } = item
    	this.rmListener(target, event, callback)
    })
    this.eventStack = []
  }
}
```

亦或者，可以通过声明一个函数，通过外部创建的实例对象主动调用，这样处理，同时既避免了上文中的 `this` 指向问题，也可以直接销毁，事例如下：

> 若注册时使用的是  `箭头函数 ()=>{}` 的方式可结合上文 `eventStack` 存储并统一销毁的处理方法

```js
class Test {
  ...
  // 初始化
  init() {
    // this.regListener(this.ele, 'mouseenter', this.handleEnter)
    this.regListener(this.ele, 'mouseenter', (e) => this.handleEnter(e))
    ...
  }
  // 事件注册
  regListener(target, event, callback) {
    target.addEventListener(event, callback)
  }
	// 事件销毁
  rmListener(target, event, callback) {
    target.removeEventListener(event, callback)
  }
	...
	// 移除监听器
  handleRemoveListener() {
    console.log('remove listener')
    // this.rmListener(this.ele, 'mouseenter', this.handleEnter)
    this.rmListener(this.ele, 'mouseenter', (e) => this.handleEnter(e))
  }
}

// 创建实例
const test = new Test()
// 销毁listener
test.handleRemoveListener()
```

若使用了上文中处理 `this` 指向问题时的第一种方式，也可以使用下面的方法直接销毁事件，且这也是最简洁高效的处理方式了

> 当然，若有批量处理的需求，也可以结合类似上文中 `eventStack` 的方式来处理

```js
class Test {
  ...
  // 初始化
  init() {
    this.regListener(this.ele, 'mouseenter', this.handleEnter)
    this.regListener(this.ele, 'click', this.handleClick)
  }
	...
	// 鼠标移入
  handleEnter = (e) => {
    console.log('enter', e)
  }
  // 点击
  handleClick = (e) => {
    console.log('click', e)
    this.rmListener(this.ele, 'mouseenter', this.handleEnter)
  }
}
```



##### 总结📝

> 经实践，主要考虑以下几种情况，问题便可以得到解决

- `this` 上下文指向问题
- `addEventListener` 和 `removeEventListener` 中传递的  `listener` 是否一致问题
- 是否考虑在某个事件内（被监听事件）处理另一个事件的销毁动作
- 是否考虑批量处理，如批量销毁事件
- 是否在 `Class` 外部调用处理销毁事件