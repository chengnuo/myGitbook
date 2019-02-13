# promise

## 需求

Promise 是异步编程的一种解决方案，用于一个异步操作的最终完成(或失败)及其结果值的表示，比传统的回调函数方案更加合理。

## 描述

Promise 对象是一个代理对象（代理一个值），被代理的值在Promise对象创建时可能是未知的。它允许你为异步操作的成功和失败分别绑定相应的处理方法（handlers）。
一个 Promise有以下几种状态:

(1) pending: 意味着操作正在进行。

(2) fulfilled: 意味着操作成功。

(3) rejected: 意味着操作失败。

pending 状态的 Promise 对象可能触发fulfilled 状态并传递一个值给相应的状态处理方法，也可能触发失败状态（rejected）并传递失败信息。当其中任一种情况出现时，Promise 对象的then 方法绑定的处理方法（handlers ）就会被调用（then方法包含两个参数：onfulfilled 和 onrejected（可选参数），它们都是 Function 类型。

## 语法
```javascript
var promise = new Promise((resolve, reject) => {/* executor函数 */
    // ... some code
    if (/* 异步操作成功 */){
        resolve(value);
    } else {
        reject(error);
    }
});
promise.then((value) => {
    //success
}, (error) => {
    //failure
})
```


## 参考

```
https://www.jianshu.com/p/270fec5b33ce
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
```