## 事件对象

事件对象 `e` 的`target`属性始终是事件刚刚发生的元素的引用。（更加精确选择）

## **JavaScript事件委托的工作方式**

源于[DWB的博客](https://davidwalsh.name/event-delegate)

1.当有`ul`元素中有很多的`li`元素

```html
<ul id="parent-list">
	<li id="post-1">Item 1</li>
	<li id="post-2">Item 2</li>
	<li id="post-3">Item 3</li>
	<li id="post-4">Item 4</li>
	<li id="post-5">Item 5</li>
	<li id="post-6">Item 6</li>
</ul>
```

当要对多个`li`元素添加事件监听时，对每个`li`添加和删除事件监听器太复杂。通过将监听器添加到`ul`元素上。使用`target`属性获得当前点击元素的引用。

```js
// 在ul上添加一个点击监听器
document.getElementById("parent-list").addEventListener("click", function(e) {
	// 当点击的li元素时，选择当前这个li的引用
	if(e.target && e.target.nodeName == "LI") {
		// 打印当前用li元素的id
		console.log("List item ", e.target.id.replace("post-", ""), " was clicked!");
	}
});
```

2.当`div`中有很多`a`元素，选择其中类名为snake的

```html
<div>
    <a>1</a>
    <a class="snake">2</a>
    <a>3</a>
    <a>4</a>
</div>
```

```js
// 同样的在父元素上添加监听器
document.getElementById("myDiv").addEventListener("click",function(e) {
	// 选择a中的class为snake的元素
  if (e.target && e.target.matches("a.snake")) {
    console.log("Anchor element clicked!");
	}
});
```

