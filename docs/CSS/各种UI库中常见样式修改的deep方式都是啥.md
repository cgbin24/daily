### 各种UI库中常见样式修改的deep方式都是啥
> 如题，某些 `UI库` 设定的样式并不容易自定义更改，即便在某个样式上加了 `!important` 也没用，但只要使用某些特定的函数包裹以下选择器就能行，如：`:deep(selectName)`、`::deep(selectName)`、`v-deep(selectName)`、`/deep/(selectName)`、`::ng-deep selectName`等，那么这都是啥意思呢，是原生 `CSS` 提供的函数吗，没见过呀好奇怪？

如果疑惑无法消除，那便往下文寻求答案吧

#### ::背景
为啥会出现这种方式