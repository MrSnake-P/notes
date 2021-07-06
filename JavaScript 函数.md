## 匿名函数

匿名函数没有函数名

```javascript
function() {
  alert('hello');
}
```

通常将匿名函数与事件处理程序一起使用

```javascript
var myButton = document.querySelector('button');

myButton.onclick = function() {		// 点击按钮，打印hello
  alert('hello');
}
```

将匿名函数分配为变量的值

```javascript
var myGreeting = function() {
  alert('hello');
}
// 调用
myGreeting();
```

## 作用域

如果一个**[变量](https://developer.mozilla.org/en-US/docs/Glossary/variable)**或者其他表达式不 "在当前的作用域中"，那么它就是不可用的。

https://developer.mozilla.org/zh-CN/docs/Glossary/Scope

## DOM

DOM（文档对象模型）是一种[API](https://developer.mozilla.org/en-US/docs/Glossary/API)，代表任何[HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML)或[XML](https://developer.mozilla.org/en-US/docs/Glossary/XML)文档并与之交互。DOM是加载到[浏览器中](https://developer.mozilla.org/en-US/docs/Glossary/browser)的文档模型，并将文档表示为节点树，其中每个节点代表文档的一部分（例如，[元素](https://developer.mozilla.org/en-US/docs/Glossary/element)，文本字符串或注释）。

DOM是[Web](https://developer.mozilla.org/en-US/docs/Glossary/World_Wide_Web)上最常用的[API](https://developer.mozilla.org/en-US/docs/Glossary/API)之一，因为它允许在浏览器中运行的代码访问文档中的每个节点并与之交互。可以创建，移动和更改节点。可以将事件侦听器添加到节点，并在发生给定事件时触发。

DOM最初并未指定-它是在浏览器开始实现[JavaScript时出现的](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript)。这个旧版DOM有时称为DOM0。今天，WHATWG维护DOM生活标准。