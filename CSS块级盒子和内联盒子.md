[TOC]

## 块级盒子和内联盒子

* 块级盒子

> 绝大数情况下意味着盒子会和父容器一样宽
>
> 每个盒子都会换行
>
> [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width) 和 [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height) 属性可以发挥作用
>
> 内边距（padding）, 外边距（margin）和边框（border）会将其他元素从当前盒子周围“推开”

* 内联盒子

> 盒子不会换行
>
> [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width) 和 [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height) 属性将不起作用
>
> 处于 `inline` 状态的盒子不会被内边距、外边距和边框作用

`display` 属性可以改变盒子的外部显示类型是块级还是内联

> `block`	块级
>
> `inline`	内联
>
> `flex`	灵活盒子
>
> `inline-flex`	内联的灵活盒子

### 边距折叠

块的[上外边距(margin-top)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-top)和[下外边距(margin-bottom)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-bottom)有时合并(折叠)为单个边距，其大小为单个边距的最大值(或如果它们相等，则仅为其中一个)，

```css
p {
    border: 2px solid green;
    margin: 20px 10px 40px 0;
}
```

```html
/* 块元素p的下边距会发生折叠，所以并不是60px，而是40px*/
<div class="box">
    <p>One November night in the year 1782</p>
    <p>Before that night—a memorable night</p>
</div>
```

## CSS盒模型

完整的css盒模型应用于块级盒子。

组成部分：

* content：显示内用，属性有`width`和`height`
* padding：在内容外部的空白区域，属性有`padding`
* border：盒子包括内容和内边距，属性有`border`
* margin：盒子与其他元素之间的空白区域，属性有 `margin`

![](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20210119165907474.png)

### 标准盒模型（宽度仅仅是内容宽度）

```css
.box {
  width: 350px;
  height: 150px;
  margin: 25px;
  padding: 25px;
  border: 5px solid black;
}
```

标准模型宽度 = 410px (350 + 25 + 25 + 5 + 5)，高度 = 210px (150 + 25 + 25 + 5 + 5)，padding 加 border 再加 content box。

![](C:\Users\91372\AppData\Roaming\Typora\typora-user-images\image-20210119170511915.png)

### 替代盒模型（宽度包括内边距和内容宽度）

```css
.box {
  box-sizing: border-box;
} 
```

```css
/* 所有元素都使用替代盒模型 */
html {
  box-sizing: border-box;
}
*, *::before, *::after {
  box-sizing: inherit;
}
```

### 外边距

```css
/* 上边 | 右边 | 下边 | 左边 */
margin: 2px 1em 0 auto;	/* 1em 等于字体高 */
等价于
margin-top: 2px;
margin-right: 1em;
margin-bottom: o;
margin-left: auto;
```

### 内边距

```css
/* 上边 | 右边 | 下边 | 左边 */
padding: 0 30px 40px 4em;	/* 1em 等于字体高 */
等价于
padding-top: 0;
padding-right: 30px;
padding-bottom: 40px;
padding-left: 4em;
```

## 盒子模型和内联模型

```css
span {
  margin: 20px;	/* 有效果，有可能会覆盖其他内容 */
  padding: 20px;	/* 有效果，有可能会覆盖其他内容 */	
  width: 80px;	/* 将不会有效果 */
  height: 50px;	/* 将不会有效果 */
  background-color: lightblue;
  border: 2px solid blue;
}
```

##  内联和块之间的中间状态

内联模型的升级版，即可设置宽度和高度的模型，不会像内联模型，可能造成重叠。

**display: inline-block;**

```css
span {
  margin: 20px;
  padding: 20px;
  width: 80px;	/* 将会有效果 */
  height: 50px;	/* 将会有效果 */
  background-color: lightblue;
  border: 2px solid blue;
  display: inline-block;
}
```

示例，在ul元素中，用`inline-block`推开ul上的边框，不然会被覆盖

```css
.links-list a {
  background-color: rgb(179,57,81);
  color: #fff;
  text-decoration: none;
  padding: 1em 2em;
  display: inline-block;	/* 不然会被覆盖*/
}

.links-list a:hover {
  background-color: rgb(66, 28, 40);
  color: #fff;

}
```

```html
<nav>
  <ul class="links-list">
    <li><a href="">Link one</a></li>
    <li><a href="">Link two</a></li>
    <li><a href="">Link three</a></li>
  </ul>
</nav>    
```

