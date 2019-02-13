# 冒泡排序

## 需求

冒泡排序

## 描述

冒泡排序算法的原理如下：
比较相邻的元素。如果第一个比第二个大，就交换他们两个。
对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。
针对所有的元素重复以上的步骤，除了最后一个。
持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

## 语法 [在 CodePen 中打开](https://codepen.io/chengnuo/pen/rPKXpY)
```javascript
/*
* @param arr 数组
*/
function bubbleSort(arr){
  let temp = 0; // 中间变量
  let i = 0;
  let j = 0;

  for(i=0; i<arr.length; i++ ){
    for(j=0; j<arr.length-i-1; j++ ){
      if(arr[j]>arr[j+1]){
        temp = arr[j+1];
        arr[j+1] = arr[j]
        arr[j] = temp;
      }
    }
  }

  return arr;
}

let bubbleSortArr = [11,1,8,4,5,2,5,7,62,3,6];
let result = bubbleSort(bubbleSortArr);

// 输出
document.getElementById('layout').innerHTML = result;
```


## 参考

```
```