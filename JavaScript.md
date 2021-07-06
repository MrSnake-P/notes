[TOC]



## JavaScript简介

JavaScript是一种脚本语言或编程语言，可以在网页上实现复杂的功能-每次网页要做的不只是显示静态信息供查看而是显示及时的内容更新，交互式地图，动画2D / 3D图形，滚动视频点唱机等-都涉及JavaScript。它是标准Web技术的第三层，其他两层为html和css。

## 脚本加载策略

使用异步或延迟，解决脚本阻塞的问题。

- `async`与`defer`这两个指示浏览器下载一个单独的线程的脚本（S），而网页（DOM中，等）的其余部分被下载，所以网页加载时不会被脚本。
- 如果您的脚本应立即运行并且没有任何依赖关系，请使用`async`。
- 如果您的脚本需要等待解析并且依赖于其他脚本和/或DOM，请使用加载它们`defer` ，并将它们的相应``元素放入浏览器执行它们的顺序。

## var和let的区别

* var可以在变量初始化后，再声明变量，这有点不符合我们的认知。**let则不可以**。

  ```javascript
  bla = 2
  var bla;
  
  // 可以隐式地（implicitly）将以上代码理解为：
  
  var bla;
  bla = 2;
  ```

## 事件

侦听事件发生的结构称为**事件监听器（Event Listener）**，响应事件触发而运行的代码块被称为**事件处理器（Event Handler）**。

## if-else

### 语法

```javascript
if () {
code
}else if () {
         code
}else {
code
}
```

## switch

### 语法

```javascript
switch () {
  case choice1:
    code
    break;
  case choice2:
    code
    break;
  default:
    extra code
}
```

## 三元运算符

### 语法

```javascript
( condition ) ? run this code : run this code instead
```

## 循环

### for语法

```javascript
for (initializer; exit-condition; final-expression) {
  // code to run
}
```

### while和do...while

```javascript
initializer
while (exit-condition) {
  // code to run

  final-expression
}
// do...while，总是会执行一次
initializer
do {
  // code to run

  final-expression
} while (exit-condition)
```

## 循环进阶

### label语句

可以标识一个循环，然后使用 `break` 或者 `continue` 来指出程序是否该停止循环还是继续循环。并且指定跳入哪一个循环，再多层循环时极为清晰。

```javascript
var num = 0;
outPoint:		// 可以是任意名字
for(var i = 0; i < 10; i++) {
  for(var j = 0; j < 10; j++) {
    if(i == 5 && j == 5) {
      continue outPoint;	// 跳出当前循环，并跳转到outPoint下的循环继续
    }
    num++;
  }
}
alert(num);  // 95
```

### break/continue [label]

当使用仅`break`或`continue`时，会终止当前所在的循环或继续执行下一个循环。而在后面指定`label`，终止带标签的语句或跳到带标签的循环。

示例1：使用`break [label]`

```javascript
var x = 0;
var z = 0
labelCancelLoops: while (true) {
  console.log("外部循环: " + x);
  x += 1;
  z = 1;
  while (true) {
    console.log("内部循环: " + z);
    z += 1;
    if (z === 10 && x === 10) {
      break labelCancelLoops;	// 终止labelCancelLoops标签的循环
    } else if (z === 10) {
      break;
    }
  }
}
```

示例2：使用`continue [label]`

```javascript
checkiandj:
  while (i < 4) {
    console.log(i);
    i += 1;
    checkj:
      while (j > 4) {
        console.log(j);
        j -= 1;
        if ((j % 2) == 0) {
          continue checkj;	// 跳出当前循环，并跳转到checkj标签的循环
        }
        console.log(j + ' 是奇数。');
      }
      console.log('i = ' + i);
      console.log('j = ' + j);
  }
```

### for...in语句

[`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/statements/for...in) 语句循环一个指定的变量来循环一个对象所有可枚举的属性。JavaScript 会为每一个不同的属性执行指定的语句。例如：遍历一个数组中所有元素。

### for..of语句

[`for...of`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/statements/for...of) 语句在[可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/iterable)（包括[`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Array)、[`Map`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Map)、[`Set`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)、[`arguments`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/functions/arguments) 等等）上创建了一个循环，对值的每一个独特属性调用一次迭代。

 **`for...in` 循环遍历的结果是数组元素的下标，而 `for...of` 遍历的结果是元素的值。**

```javascript
let arr = [3, 5, 7];
arr.foo = "hello";

for (let i in arr) {
   console.log(i); // 输出 "0", "1", "2", "foo"
}

for (let i of arr) {
   console.log(i); // 输出 "3", "5", "7"
}
```

## 选取节点

| 方法                       | 作用             |
| -------------------------- | ---------------- |
| `document.querySelector()` | 选取元素节点     |
| `document.createElement()` | 创建元素节点     |
| `appendChild()`            | 在最后面添加节点 |
|                            |                  |
|                            |                  |

