---
layout: post
title: addEventListener 事件监听
categories: [JavaScript]
description: addEventListener 事件监听
keywords: addEventListener 事件监听
---

# 事件监听执行的顺序

在元素或者浏览器上面添加事件监听，不知道你是否知道各种情况下面他们的执行顺序？如果对于事件流特别清楚，应该不难。对我来说有一点点的难道

## 一句话说事件流

从 `html` 元素到目标元素是捕获阶段，从目标元素到 `html` 是冒泡阶段。

## 事件监听直接绑定在目标元素上面

1. 目标元素上面绑定两个单击监听事件，问两个事件的执行先后顺序？

```
<html>
<body>
<div>
<button id='botton'>qqqq</button>
</div>
</body>
<script>
const botton = document.getElementById('botton')
botton.addEventListener('click',function(){
console.log('true 是事件捕获');
})
botton.addEventListener('click',function(){
console.log('false 是事件冒泡');
})
</script>
</html>
```

在目标元素上添加事件监听事件，不管是捕获(`true`)还是冒泡(`false`),默认是 `false`，都是按照从上到下的执行顺序，谁先定义谁先执行。

原因很简单：对于事件目标上的事件监听器来说，事件会处于“目标阶段”，而不是冒泡阶段或者捕获阶段。在目标阶段的事件会触发该元素（即事件目标）上的所有监听器，而不在乎这个监听器到底在注册时 `useCapture` 参数值是 `true` 还是 `false`

## 事件监听直接绑定在 window 上面

如果默认都是触发类型事件冒泡，默认形式，就是从上往下执行，谁先定义，谁先执行

```
<html>
<body>
<div>
<button id='botton'>qqqq</button>
<span>hjhj</span>
</div>
</body>
<script>
const botton = document.getElementById('botton')
window.addEventListener('click',function(){
console.log('false 是冒泡事件');
})
window.addEventListener('click',function(){
console.log('true 是捕获事件');
})
</script>
</html>
```

如果默定义了某一个是触发类型捕获(`true`)，不管定义顺序的前后，都是先执行捕获事件，然后冒泡事件

```
<html>
<body>
<div>
<button id='botton'>qqqq</button>
</div>
</body>
<script>
const botton = document.getElementById('botton')


window.addEventListener('click',function(){
console.log('false 是冒泡事件');
})
window.addEventListener('click',function(){
console.log('true 是捕获事件');
},true)
</script>
</html>
```

## 事件委托

利用的是事件冒泡机制，点击子元素的时候，一层一层往上找，找到上级元素的时候，发现她身上有事件，那就把时间给执行了
