[TOC]

## 对象基础

对象是一个包含相关数据和方法的集合（通常由一些变量和函数组成，我们称之为对象里面的属性和方法）。

对象有时被称之为关联数组(associative array)——对象做了字符串到值的映射，而数组做的是数字到值的映射。

## this

关键字"this"指向了当前代码运行时的对象。

## 两种方法访问对象的属性和方法

```javascript
var person = {
  name : ['Bob', 'Smith'],
  age : 32,
  gender : 'male',
  interests : ['music', 'skiing'],
  bio : function() {
    alert(this.name[0] + ' ' + this.name[1] + ' is ' + this.age + ' years old. He likes ' + this.interests[0] + ' and ' + this.interests[1] + '.');
  },
  greeting: function() {
    alert('Hi! I\'m ' + this.name[0] + '.');
  }
};
```

1.点表示法

属性：`person.name`	方法：`person.bio()`

2.方括号表示法

属性：`person["name"]`	方法：`person["bio"]()`

3.点表示法只能接受字面量的成员的名字，不接受变量作为名字。

```javascript
var myDataName = 'height'
var myDataValue = '1.78m'
person[myDataName] = myDataValue
```

## 多态

**多态**——用来描述多个对象拥有实现共同方法的能力。

## [构造函数和对象](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Object-oriented_JS#构建函数和对象)

```javascript
function Person(name) {
  this.name = name;		// 从实参传入形参的自己的值，而不是其他值
  this.greeting = function() {
    alert('Hi! I\'m ' + this.name + '.');
  };
}

var person1 = new Person('Bob');
var person2 = new Person('Sarah');

person1.name
person1.greeting()
person2.name
person2.greeting()
```

> 一个构造函数通常是大写字母开头，这样便于区分构造函数和普通函数。

### 实参和形参

形参相当于函数中定义的变量，实参是运行函数传入的参数。

## Object()构造函数

1.创建一个空的对象，在添加属性和方法

`var person1 = new Object();`

```javascript
person1.name = 'Chris';
person1['age'] = 38;
person1.greeting = function() {
  alert('Hi! I\'m ' + this.name + '.');
}
```

2.用对象文本传递的方式构造

```javascript
var person1 = new Object({
  name : 'Chris',
  age : 38,
  greeting : function() {
    alert('Hi! I\'m ' + this.name + '.');
  }
});
```

## create()方法

基于现有对象创建新的对象，该对象将拥有相同的属性和方法。

`var person2 = Object.create(person1);`

## 原型（prototype）属性

* **Object.getPrototypeOf()返回对象原型**

```javascript
const prototype1 = {};
const object1 = Object.create(prototype1);

console.log(Object.getPrototypeOf(object1) === prototype1);
// output: true
```

* **每一个对象都有一个特殊的原型属性**

```javascript
function doSomething(){};
var doSomeInstancing = new doSomething();
doSomething.prototype.foo = "snake"
doSomeInstancing.prop = "something"
// 这会生成原型链
> doSomeInstancing
<·doSomething
{
    prop: "something"
    __proto__:{
        foo: "snake"
        constructor: ƒ doSomething()
        __proto__: {
            constructor: ƒ Object()
            hasOwnProperty: ƒ hasOwnProperty()
            isPrototypeOf: ƒ isPrototypeOf()
            propertyIsEnumerable: ƒ propertyIsEnumerable()
            toLocaleString: ƒ toLocaleString()
            toString: ƒ toString()
            valueOf: ƒ valueOf()
    	}
	}
}
```

## 修改原型

一种极其常见的对象定义模式是，在构造器（函数体）中定义属性、在 `prototype` 属性上定义方法。如此，构造器只包含属性定义，而方法则分装在不同的代码块，代码更具可读性。

```javascript
// 构造器及其属性定义

function Test(a,b,c,d) {
  // 属性定义
};

// 定义第一个方法

Test.prototype.x = function () { ... }

// 定义第二个方法

Test.prototype.y = function () { ... }

```

## json对象和文本间的转换

- `parse()`: 以文本字符串形式接受JSON对象作为参数，并返回相应的对象。
- `stringify()`: 接收一个对象作为参数，返回一个对应的JSON字符串。

```javascript
var myJSON = { "name" : "Chris", "age" : "38" }; 	对象
var myString = JSON.stringify(myJSON); 	字符串
var myobject = JSON.parse(myString);	在转化为对象
```