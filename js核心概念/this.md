# this
在JavaScript中，this总是指向一个对象，而具体指向哪个对象是在函数运行时基于函数的执行环境动态绑定的，而非函数被声明时的环境。

函数在运行时，this的指向分为以下几种情况：
1. 默认绑定: this指向window或undefined(严格模式下)
```
var a = 1;
function fn () {
	console.log(this.a);
}
fn(); // 1,this指向window

function fnStrict() {
	"use strict"
	console.log(this);
}
fnStrict(); // undefined,this指向undefined
```

2. 隐式绑定

```
var obj = {
	a: 2,
	fn: function() {
		console.log(this.a);
	}
}
obj.fn(); // 2, this指向obj
```

```
var obj1 = {
	name: "one",
	obj2: {
		name: "two",
		fn: function() {
			return this.name;
		}
	}
}
obj1.obj2.fn(); // two, 嵌套调用时，this指向最后一个对象
```

3. 硬绑定: call，bind,  apply

```
var obj1 = {
	name: "obj1",
	fn: function () {
		return this.name;
	}
}
var obj2 = {
	name: "obj2",
	fn: function() {
		return this.name;
	}
}
obj1.fn(); // "obj1"
obj2.fn(); // "obj2"
obj2.fn.apply(obj1); //"obj1"
```

4. new构造绑定

```
function Person(name) {
	this.name=name;
}
const person = new Person("person1"); // person1
```

**绑定优先级**

new构造绑定>硬绑定>隐式绑定>默认绑定

**其他情况：箭头函数**

在箭头函数中，this的指向是由外层（函数或全局）作用域来决定的。

```
const obj = {
	fn: function() {
		setTimeout(function() {
			console.log(this)
		})
	}
}
console.log(obj.fn()) // window
```
如果需要this指向obj这个对象，则可以使用箭头函数来解决

```
const obj = {
	fn: function() {
		setTimeout(() => {
			console.log(this)
		})
	}
}
console.log(obj.fn()) // {fn:f}
```