# vue浏览器没有问题，在微信报错Loading chunk {n} failed

## 需求

vue浏览器没有问题，在微信打开报错Loading chunk {n} failed


## 定位

各种定位review代码，没有问题，最终定位为电信劫持或者来自于webpack进行code spilt之后某些bundle文件lazy loading失败

## 解决

```javascript
router.onError((error) => {
  const pattern = /Loading chunk (\d)+ failed/g;
  const isChunkLoadFailed = error.message.match(pattern);
  const targetPath = router.history.pending.fullPath;
  if (isChunkLoadFailed) {
    router.replace(targetPath);
  }
});
```



## 参考

```
https://segmentfault.com/a/1190000016382323
```
