[TOC]

## 背景图片

### 控制背景平铺

* `background-repeat` 

> no-repeat —— 不重复
>
> repeat-x —— 水平重复
>
> repeat-y —— 垂直重复
>
> repeat —— 在两个方向上重复

### 调整大小

* `background-size`

> cover -- 使图像足够大
>
> contain -- 使图像适合盒子

### 渐变背景

* 线性渐变

  颜色值沿着一条隐式的直线过渡

* 径向渐变

  颜色值由一个中心点向外扩散

```css
/* 线性渐变 */
/* 格式linear-gradient(渐变形式,颜色 占比,颜色 占比) */
.a {
  background-image: linear-gradient(105deg, rgba(0,249,255,1) 39%, rgba(51,56,57,1) 96%);
}
/* 径向渐变 */
/* 格式radial-gradient(渐变形式,颜色 占比,颜色 占比) */
.b {
  background-image: radial-gradient(circle, rgba(0,249,255,1) 39%, rgba(51,56,57,1) 96%);
  background-size: 100px 50px;
}
```

### 背景附加

* `background-attachment`

> `scroll`: 使元素的背景在页面滚动时滚动
>
> `fixed`: 使元素的背景固定在视图端口上
>
> `local`: 滚动元素，背景也滚动

### background简写

- `background-color` 只能在逗号之后指定。
- `background-size` 值只能包含在背景位置之后，用'/'字符分隔，例如：`center/80%`。

### 边框

设置边框

```css
.box {
  border: 1px solid black;
} 
等价于
.box {
  border-width: 1px;
  border-style: solid;
  border-color: black;
} 
```

仅设置一边

```css
.box {
  border-top: 1px solid black;
} 
```

### 圆角

```css
/* 圆角半径 */
.box {
  border-radius: 10px;
} 
/* 右上角垂直半径*/
.box {
  border-top-right-radius: 1em 10%;
} 
```

### RGBA(0,0,0,0)

前三个值表示颜色，最后一个值alpha值，表示色彩的透明度。

* `rgba(0,0,0,.5)` # 半透明黑

* `rgba(0,0,0,0)`  # 全透明