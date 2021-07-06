[TOC]



## 函数

calc()	# 允许在 CSS 中进行简单的计算

```css
.box {
	width: calc(90% - 30px);
}
```

rotate()	# 旋转

```css
.box {
	transform: rotate(0.8turn);
}
```

## 速记属性

允许一行设置多个属性值

```css
padding: 10px 15px 15px 5px;
等价于
padding-top: 10px;
padding-right: 15px;
padding-bottom: 15px;
padding-left: 5px;
```

```css
background: red url(bg-graphic.png) 10px 10px repeat-x fixed;
等价于
background-color: red;
background-image: url(bg-graphic.png);
background-position: 10px 10px;
background-repeat: repeat-x;
background-attachment: fixed;
```

## 一些属性

* `text-align: -webkit-match-parent`

  /* 匹配父类样式对齐的方式 */

* overflow /* 定义一个元素内容太大而无法适应*/

  /* 为使得效果，需要指定块级容器高度，或white-space=nowrap*/

  ```css
  /* 默认值。内容不会被修剪，会呈现在元素框之外 */ 
  overflow: visible; 
  /* 内容会被修剪，并且其余内容不可见 */ 
  overflow: hidden; 
  /* 内容会被修剪，浏览器会显示滚动条以便查看其余内容 */ 
  overflow: scroll; 
  /* 由浏览器定夺，如果内容被修剪，就会显示滚动条 */ 
  overflow: auto; 
  /* 规定从父元素继承overflow属性的值 */ 
  overflow: inherit;
  ```

  

## 一些CSS样式

[css参考](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference)

- [`font-family`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family)

  `font-family: <family-name>,<generic-name>`

  `<family-name>`	#字体族名字，族名之间包含空格，并且用引号

  `<generic-name>`	#通用字体族名，在指定字体不可用时给出较好的字体

> 示例：`font-family: "Gill Sans Extrabold", sans-serif;`

- [`color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/color)
- [`border-bottom`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-bottom)
- [`font-weight`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-weight)
- [`font-size`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-size)
- [`text-decoration`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration)

## CSS伪类

* `:hover` 层叠样式

按照LVHA的循顺序声明`:link－:visited－:hover－:active`

```css
a:link {color:#FF0000;} /* 未访问的链接 */
a:visited {color:#00FF00;} /* 已访问的链接 */
a:hover {color:#FF00FF;} /* 鼠标划过链接 */
a:active {color:#0000FF;} /* 已选中的链接 */
```

**:hover伪类可以任何伪元素上使用。**

### CSS伪元素

伪类可以是多个，而伪元素在一个选择器中只能出现一次，并且只能出现在末尾

* `::after` 用来创建一个伪元素，通常配合content属性

  ```
  a::after {
    content: "→";
  }
  ```

## 控制和继承

[`inherit`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/inherit)

设置该属性会使子元素属性和父元素相同。实际上，就是 "开启继承".

[`initial`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/initial)

设置属性值和浏览器默认样式相同。如果浏览器默认样式中未设置且该属性是自然继承的，那么会设置为 `inherit` 。

[`unset`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/unset)

将属性重置为自然值，也就是如果属性是自然继承那么就是 `inherit`，否则和 `initial`一样

### 重设所有属性

```css
.example {
	all: unset		# 撤销对样式所作更改
}
```

## 层叠

三个重要的因素：

* 重要程度

* 优先级

  类选择器的权重达于元素选择器，所以可以给基本元素定义通用样式，给不同的元素创建对应的类

* 资源顺序

  后面的规则会覆盖前面定义的规则

一个选择器的优先级可以说是由四个部分相加 (分量)，可以认为是个十百千 — 四位数的四个位数：

1. **千位**： 如果声明在 `style` 的属性（内联样式）则该位得一分。这样的声明没有选择器，所以它得分总是1000。
2. **百位**： 选择器中包含ID选择器则该位得一分。
3. **十位**： 选择器中包含类选择器、属性选择器或者伪类则该位得一分。
4. **个位**：选择器中包含元素、伪元素选择器则该位得一分

> **注**: 通用选择器 (`*`)，组合符 (`+`, `>`, `~`, ' ')，和否定伪类 (`:not`) 不会影响优先级。

------

**警告:** 在进行计算时不允许进行进位，例如，20 个类选择器仅仅意味着 20 个十位，而不能视为 两个百位，也就是说，无论多少个类选择器的权重叠加，都不会超过一个 ID 选择器。

------

### !important

可以用来覆盖所有上面的优先级计算，用于修改特定属性的值，能够覆盖普通规则的的层叠。