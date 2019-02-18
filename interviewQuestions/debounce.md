# 防抖

## 需求

防抖的原理就是：你尽管触发事件，但是我一定在事件停止触发 n 秒后才执行。

这意味着如果你在一个事件触发的 n 秒内又触发了这个事件，那我就以新的事件触发的时间为准，在此时间 n 秒后才执行。

总之，就是要等你触发完事件 n 秒内不再触发事件，我才执行，真是任性呐!

## 防抖 [在 CodePen 中打开](https://codepen.io/chengnuo/pen/PVXKaR)

```javascript
var count = 1;
var container = document.getElementById('container');

function getUserAction() {
    container.innerHTML = count++;
};


function debounce(func, wait, immediate){
  var timeout;
  return function (){
    let self = this;
    let args = arguments
    clearTimeout(timeout);
    timeout = setTimeout(function(){
      func.apply(self, args)
    }, wait)
  }
};


container.onmousemove = debounce(getUserAction, 300, true);
```

## 参考

```
https://github.com/mqyqingfeng/Blog/issues/22
https://github.com/component/debounce/blob/f4afbd34e3e15be33d2f261b3ec67ecddaf4a4f6/index.js
```