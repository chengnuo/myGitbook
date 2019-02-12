# 千位分隔符的完整攻略

## 需求


千位分隔符是很常见的需求，但是输入文本千变万化，如何才能准确添加千分符呢？


## 千分符-正则表达式 [在 CodePen 中打开](https://codepen.io/chengnuo/pen/bzMPZG)

```javascript
// 千分符-正则表达式
function miliFormat(num){

  return num && num.toString().replace(/(^|\s)\d+/g, function(s){
    return s.replace(/(?=(\d))(?=(\d{3})+$)/g, ',')
  });

}
let result = miliFormat('12345678.22235');
// 输出
document.getElementById('layout').innerHTML = result;
```

## 参考

```
https://www.tuicool.com/articles/ArQZfui
https://idiotwu.me/milli-formatting-digitals-with-regex/
```