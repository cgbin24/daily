### angular form 表单自动提交问题

### :: 问题

> 使用 `angular` `form` 表单，表单内任意 `input`  输入框聚焦时，点击键盘上的 `enter` 键，`form` 表单会自动提交，即：若表单内存在 `button` 控件，且绑定了事件，那个便会执行第一个 `button` 控件的 `click` 事件，事例如下：

```html
<form #form="ngForm" novalidate autocomplete="off">
  <div class="formWrap">
    <div class="fieldBox">
      <div class="label">昵称:</div>
      <div class="value">
        <input class="valInner" name="nickname" [(ngModel)]="formObj.nickname" required placeholder="请输入昵称">
      </div>
    </div>
    <div class="footerWrap">
      <button class="close btn" (click)="handleCloseModal()">关闭</button>
      <button class="submit btn" (click)="handleSubmit()">确定</button>
    </div>
  </div>
</form>
```
### :: 分析
其中的 `handleCloseModal` 和 `handleSubmit` 为自定义方法，这里不需要 `form` 表单自动触发，但是，当表单内的 `input` 输入框聚焦时点击键盘上的 `回车键/enter` 就会触发第一个 `button` 按钮上的 `click` 事件，这是 `form` 表单自身的默认行为，基于 `button` 的 `type` 属性，即这里指 `type='submit'`，所以就被表单主动触发了

**:: 引用说明 ::**

> 当用户点击提交按钮（[button](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button) 或 [input type="submit"](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input/submit)），亦或是在表单里输入时（e.g. [\<input type="text">](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input/text)）按下 `Enter` 键，`submit` 事件将会被触发。

> 一个元素，表示发送 [`submit`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLFormElement/submit_event) 事件的表单元素。通常是 [`type`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input#type) 属性是 `submit` 的 [input](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input) 元素或者 [type](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input#type) 属性是 `submit` 的 [button](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button) 元素，但它也可能是启动提交过程的其他元素。


### :: 解决方式

> 在不使用默认提交行为的情况下，可使用如下方式处理

1、在表单内的 `button` 控件上添加 `type='button'` 属性，强调其作用类型

```html
<div class="footerWrap">
  <button class="close btn" type="button" (click)="handleCloseModal()">关闭</button>
  <button class="submit btn" type="button" (click)="handleSubmit()">确定</button>
</div>
```

2、使用其他标签模拟出一个 `button` 按钮，如：`div`

```html
<div class="footerWrap">
  <div class="close btn" (click)="handleCloseModal()">关闭</div>
  <div class="submit btn" (click)="handleSubmit()">确定</div>
</div>
```
总结，只要表单内不出现 `type="submit"` 类型的 `input` 或 `button` 标签即可，同时为了预防并限制默认类型被指定为 `type="submit"`，可以在使用 `input` 和 `button` 的地方指定其作用类型，或者使用其他标签来模拟 `button`。



### :: 衍伸阅读

[form](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form)

[HTMLFormElement: submit event](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLFormElement/submit_event)

[SubmitEvent.submitter](https://developer.mozilla.org/zh-CN/docs/Web/API/SubmitEvent/submitter)