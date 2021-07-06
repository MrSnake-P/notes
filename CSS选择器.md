[TOC]

## CSS选择器

​		CSS选择器是CSS规则的第一部分。它是元素和其他部分组合起来告诉浏览器哪个HTML元素应当是被选为应用规则中的CSS属性值的方式。选择器所选择的元素，叫做“选择器的对象”。

### 类型、类和ID选择器

* 选择器组

  `h1{ }`

* class选择器

  `.box{ }·

* id选择器

  `#id{ }`

### 标签属性选择器

* 根据元素上title属性

  `a[title]`

* 根据特定值的标签属性

  `a[href="https://example.com"]{ }`

* 只匹配a类

  `li[class="a"]`

* 匹配a类，也可以匹配用空格分开，包括a类的值

  `li[class~="a"]`

### 运算符

* 子代选择器，选择元素的初代子元素

  `article > p { }`

* 后代选择器，匹配所有由第一元素作为祖先的元素（不一定是父元素）的第二个元素。

  `li li`

> **note:当使用  `>` 子代选择器匹配第一个元素的直接后代(子元素)的第二元素.  [后代选择器 ](https://developer.mozilla.org/en/CSS/Descendant_selectors)相连时, 匹配由第一个元素作为祖先元素(但不一定是父元素)的第二个元素, 无论它在 DOM 中"跳跃" 多少次.**

### 全局选择器

`* {}`

### 相邻兄弟选择器

当第二个元素跟在第一个元素后面，并同属于一个父元素，第二个元素被选中

```css
/* 图片后面紧跟着的段落将被选中 */
img + p {
  font-style: bold;
}
```

### 通用兄弟选择器

同级别的元素，第二个元素被选中

```css
/* span被选中  */
p ~ span {
  color: red;
}
```

### 子字符匹配选择器

- `li[class^="a"]`匹配了任何值开头为`a`的属性，于是匹配了前两项。
- `li[class$="a"]`匹配了任何值结尾为`a`的属性，于是匹配了第一和第三项。
- `li[class*="a"]`匹配了任何值的字符串中出现了`a`的属性，于是匹配了所有项。

**大小写敏感**

* `li[class*="a" i]` 加入i大小写将不敏感

### * 伪类

​		**伪类**是选择器的一种，选择处于特定状态的元素，比如当鼠标悬浮在元素上面的时候，他们表现得会像在某个部分应用了一个类一样。

​		伪类是开头为冒号得关键字。

------

* 动态伪类，鼠标悬浮时，选择元素

  `a:hover`

* 只会在用户使用键盘控制，选定元素的时候激活。

  `a:focus`

* 选中第一个元素

  ```css
  article p:first-child {
      font-size: 120%;
      font-weight: bold;
  }
  ```

------

示例：

```css
a:link,
a:visited {
    color: rebeccapurple;
    font-weight: bold;
}

a:hover {
    color:hotpink;
}
    
<p><a href="">Hover over me</a></p>
```

### * 伪元素	

​		**伪元素**，表现得像往标记文本中加入全新得元素，而不是向现有的元素上应用类。

​		伪元素是开头为双冒号得关键字。

* 选择元素的一部分，选择第一行

  `p::first_line()`

------

示例1：

```css
/* 不管第一行字符或单词多少个，都只会选取第一行*/
article p::first-line {
    font-size: 110%;
    font-weight: bold;
} 
```

```html
<article>
    <p>Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion daikon amaranth tatsoi tomatillo
            melon azuki bean garlic.</p>

    <p>Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette tatsoi pea sprouts fava bean collard
            greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.</p>
</article>
```

示例2：

```css
/* ::before配合content使用，插在元素前面*/
/* ::before配合content使用，插在元素后面*/
.box::before {
    content: "This should show before the other content."
}
.box::after {
    content: " ➥"
}   
<p class="box">Content in the box in my HTML page.</p>
```

### 列组合器

* 匹配属于第一个元素的第二个元素，不管中间有多少层级

  ```css
  /* 匹配属于col.selected的td元素*/
  col.selected || td {
    background: gray;
  }
  ```

