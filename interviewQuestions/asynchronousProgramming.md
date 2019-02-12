# JavaScript 异步编程

## 需求

向服务器发起数次请求，每次请求的结果作为下次请求的参数。

## 解决

1、callback

2、promise


## callback

最先想到也是最常用的便是回调函数了，我们来进行简单的封装：

```javascript
const makeAjaxCall = (url, cb) => {
    // do some ajax
    // callback with result
}

makeAjaxCall('http://url1', (result) => {
    result = JSON.parse(result)
})
```

嗯，看起来还不错！但是当我们尝试嵌套多个任务时，代码看起来会是这样的：
```javascript
makeAjaxCall('http://url1', (result) => {
    result = JSON.parse(result)

    makeAjaxCall(`http://url2?q=${result.query}`, (result) => {
        result = JSON.parse(result)

        makeAjaxCall(`http://url3?q=${result.query}`, (result) => {
            // ...
        })
    })
})
```

``` 地狱式嵌套 ```，虽然解决了问题。导致的问题是代码无限循环，既不美观也不好维护。

## Promise
真正带来革命性改变的是 Promise 规范。借助 Promise，我们可以这样完成异步任务：

```javascript
const makeAjaxCall = (url) => {
    return new Promise((resolve, reject) => {
        // do some ajax
        resolve(result)
    })
}

makeAjaxCall('http://url1')
    .then(JSON.parse)
    .then((result) => makeAjaxCall(`http://url2?q=${result.query}`))
    .then(JSON.parse)
    .then((result) => makeAjaxCall(`http://url3?q=${result.query}`))
```

## async function
ES7 中，引入了一个更自然的特性 async function。利用 async function 我们可以这样完成任务：

```javascript
const makeAjaxCall = (url) => {
    return new Promise((resolve, reject) => {
        // do some ajax
        resolve(result)
    })
}

;(async () => {
    let result = await makeAjaxCall('http://url1')
    result = JSON.parse(result)
    result = await makeAjaxCall(`http://url2?q=${result.query}`)
    result = JSON.parse(result)
    result = await makeAjaxCall(`http://url3?q=${result.query}`)
})()
```

## 总结
参考链接作者写的太好了，我这边用了拿来主义。后面会加上codepen链接作为demo查看和promise的单独讲解


## 参考

```
https://idiotwu.me/going-async-with-javascript/
```