

[TOC]

# 基本DOM操作

## 选择节点

### 1.元素节点

* 选择元素并将其引用赋值给变量

  `const link = document.querySelector("a")	// 选取a元素赋值给link变量`

  `const link_list = document.querySelectorAll("a")	// 选取所有a元素，返回类似数据的NodeList对象`

* 

### 2.文本节点

### 3.属性节点

* 选择具有`id`属性值的元素

  `const elementRef = docment.getElementById('snake')`

* 返回一个数组形式的对象，其中包含给定类型的页面上所有元素

  `const elementRefArray = document.getElementsByTagName('p')`

## 创建和放置新节点

### 1.元素节点

* 创建元素并且添加文本内容

  ```javascript
  const sect = document.querySelector('section');	// 选定元素
  
  const para = document.createElement('p');
  para.textContent = 'We hope you enjoyed the ride.';
  
  sect.appendChild(para);	// 将创建的元素添加到选定元素的末尾
  ```

### 2.文本节点

```javascript
const linkPara = document.querySelector('p');	// 选定元素

const text = document.createTextNode(' — the premier source for web development knowledge.');

linkPara.appendChild(text);	// 将创建的文本节点，添加p段落后面
```

## 移动和删除元素

* 将元素或文本添加到一个节点底部

  ```
  sect.appendChild(linkPara);
  ```

* 从一个节点中移除元素

  ```javascript
  sect.removeChild(linkPara);
  ```

  ```javascript
  linkPara.remove();	// 基于自身的删除方法
  ```

## 用JavaScript操作CSS样式

1.通过[`HTMLElement.style`](https://developer.mozilla.org/en-US/docs/Web/API/ElementCSSInlineStyle/style)属性完成，该属性包含文档中每个元素的内联样式信息。设置此对象的属性以直接更新元素样式。

```javascript
para.style.color = 'white';
para.style.backgroundColor = 'black';
para.style.padding = '10px';
para.style.width = '250px';
para.style.textAlign = 'center';
```

> **note：JavaScript中操作CSS用小写驼峰书写，而CSS是带字符的，如backgroundColor`vs `background-color**

2.通过[`Element.setAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute)完成，该方法有两个参数，一个是在该元素上设置的属性，另一个是为其设置的值。

```javascript
// 不需要混合使用css和JavaScript，更加清晰，与上面相同
<style>
.highlight {
  color: white;
  background-color: black;
  padding: 10px;
  width: 250px;
  text-align: center;
}
</style>

para.setAttribute('class', 'highlight');
```