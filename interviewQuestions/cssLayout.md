# css布局

## 需求

css 布局方式
```
一栏布局
双栏布局
三栏布局
圣杯布局
双飞翼布局
```

## 一栏布局 [在 CodePen 中打开](https://codepen.io/chengnuo/pen/pBZNwe)

```css
<div class='layout'>
  <div>一栏布局</div>
</div>


.layout{
  width: 1000px;
  height: 500px;
  margin: 0 auto;
  background: #999;
}
```

```
一栏布局主要思路是
1，设置宽度 width: 1000px;
2，设置 margin: 0 auto;
```

## 双栏布局 [在 CodePen 中打开](https://codepen.io/chengnuo/pen/PgBjOj)

```css
<div class="layout">
    <span class="p1">
        float
    </span>
    <div class="p2">
        很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字
    </div>
</div>

<div class="layout layout2">
    <span>写几个字</span>
    <span class="p1">
        float
    </span>
    <span class="p1">
        float
    </span>
</div>
<div class="layout" style="height:200px;background:blue;">
</div>

.layout{
    background:red;
    width:400px;
    margin:20px;
}
.p1{
    background:green;
    float:left;
    width:200px;
    height:50px;
}
.layout2::after{
    content: 'aaa';
    clear:both;
    display: block;
    visibility: hidden;
    height:0;
}
```

## 三栏布局 [在 CodePen 中打开](https://codepen.io/chengnuo/pen/BEPZPJ)

```css

<div class="layout">
    <div class="left">
        左
    </div>
    <div class="right">
        右
    </div>
    <div class="middle">
        中间
    </div>
</div>

.layout{
    width:800px;
    height:200px;
}
.left{
    background:red;
    /* float:left; */
    /* height:100%; */
    width:200px;
    position: absolute;
    height:200px;
}
.right{
    background:blue;
    float:right;
    width:200px;
    height:100%;
}
.middle{
    margin-left:200px;
    margin-right:200px;
}

```

## 圣杯布局 [在 CodePen 中打开](https://codepen.io/chengnuo/pen/LvBxgN)

```css
<div class="layout">
    <div class="main col">Main-css圣杯</div>
    <div class="left col">Left</div>
    <div class="right col">Right</div>
</div>

/* 两边定宽，中间自适用 */
body,html,.layout{
    height: 100%;
    padding:0;
    margin: 0;
}
.col{
    float: left;   /* 三个col都设置float: left,为了把left和right定位到左右部分 */
    position:relative;
}
 /*父元素空出左右栏位子: 因为上一步中，左右栏定位成功了，但是中间栏的内容会被遮盖住*/
.layout{
    padding:0 200px 0 100px;
}
/*左边栏*/
.left{
    left:-100px;
    width: 100px;
    height:100%;
    margin-left: -100%;
    background: #ff69b4;
}
/*中间栏*/
.main{
    width:100%;
    height: 100%;
    background: #659;
}
/*右边栏*/
.right{
    right:-200px;
    width:200px;
    height:100%;
    margin-left: -200px;
    background: #ff69b4;
}
```

## 双飞翼布局 [在 CodePen 中打开](https://codepen.io/chengnuo/pen/VNBPop)


```css
<div class="layout">
    <div class="main col ">
        <div class="main_inner">Main</div>
    </div>
    <div class="left col ">Left</div>
    <div class="right col ">Right</div>
</div>


body,html,.layout{
    height: 100%;
    padding:0;
    margin: 0;
}
.col{
    float: left; /* 把left和right定位到左右部分 */
}
.main{
    width:100%;
    height:100%;
    background: #659;
}
.main_inner{   /* 处理中间栏的内容被遮盖问题 */
    margin:0 200px 0 100px;
}
.left{
    width: 100px;
    height: 100%;
    margin-left: -100%;
    background: #ff69b4;
}
.right{
    height:100%;
    width:200px;
    margin-left: -200px;
    background: #ff69b4;
}
```


## 参考
