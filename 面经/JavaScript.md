[TOC]

👉：https://juejin.cn/post/6844904116339261447#heading-4

#### 1、原型、原型链的理解

> - js中我们使用构造函数创建对象，每个构造函数内部都有一个`prototype`属性，这个属性值是一个对象，包含了可以通过该构造函数共享的一些实例和方法  👉**原型**
>
> - 当使用构造函数创建一个对象**实例**后，该实例包含一个`__proto__`指针，该指针指向`Prototype`属性的值
>
> - 当我们访问一个对象的某个属性时，如果这个对象内部不存在该属性，那么他就会去该对象的原型对象中找这个属性，
>
>   这个原型对象也有自己的原型 , 就这样一层层往上找，直到找到该属性，或找到原型对象尽头`Object.prototype.__proto__ = null`为null    👉**原型链**

#### 2、闭包

> 简单来说闭包就是函数return一个可以访问该函数内部参数及变量的函数，达到外部访问函数内部变量的目的
>
> 闭包主要有两个用途:
>
> 1/ 使我们可以在函数外部访问到函数内部的变量,通过这个方法,我们可以在函数外部调用闭包函数,在函数外部访问到函数内部变量
>
> 2/ 使已经运行过的函数上下文的变量对象继续保存在内存中,不被垃圾回收机制释放内存,但是操作不当可能会造成**内存泄露**

#### 3、闭包使用场景

> 经典题目：防抖节流
>
> - 防抖： n秒内不能调用，否则重新计时
>
>   ```js
>   function debounce(fun, delay) {
>   	let timer;
>   	return function (args) {
>   		let _this = this;
>   		clearTimeout(timer);
>   		timer = setTimeout(function () {
>   			fun.call(_this, args);
>   		}, delay);
>   	};
>   }
>   ```
>
> - 节流： n秒内只触发一次
>
>   ```js
>   function throttle(fun, delay) {
>   	let last, timer;
>   	return function (args) {
>   		let _this = this;
>   		let _args = args;
>   		let now = +new Date();
>   		if (last && now < last + delay) {
>   			clearTimeout(timer);
>   			timer = setTimeout(function () {
>   				last = now;
>   				fun.call(_this, _args);
>   			}, delay);
>   		} else {
>   			last = now;
>   			fun.call(_this, _args);
>   		}
>   	};
>   }
>   ```
>
>   

#### 4、DOM怎么添加事件

> - dom直接绑定事件 ， onClick等
> - js绑定 （js获取dom元素，并添加事件）
> - addEventListener（）

#### 5、事件&事件循环（event loop）

JavaScript是一门单线程的，非阻塞的脚本语言

> **`事件：`**
>
> **单线程：**任何时候都只有一个主线程（执行栈中）处理所有任务
>
> **非阻塞：**异步任务时，会产生一个任务队列，放入任务队列的事件不会立即执行，当执行栈中任务执行完毕，则查看任务队列中			 是否有回调，有则压入主线程执行
>
> **异步任务：**异步任务有两种类型（宏任务 、微任务），不同任务类型会被分到不同的任务队列中 ， 执行顺序 `微任务`--->`宏任务`--->`微任务`
>
> **`事件循环：`**
>
> 当执行栈中的任务都执行完毕之后，
>
> 会去检查==微任务队列==中是否有事件存在,如果有，则依次执行微任务队列中事件的回调，直到指向栈清空；
>
> 然后去检查==宏任务队列==中是否有事件存在,如果有，则依次执行微任务队列中事件的回调，直到指向栈清空；
>
> 最后再去检查==微任务队列==中是否有事件存在，如此循环重复该过程，叫做事件循环
>
> - 微任务举例
>
>   ```js
>   Promise.then()
>   MutationObserver
>   Object.observe
>   process.nextTick
>   ```
>
> - 宏任务举例
>
>   ```js
>   setTimeOut
>   setInterVal
>   I/O
>   UI交互事件
>   ```
>
>   
>
> 《深入浅出Vue》 p175

#### 6、执行栈、执行上下文

> - [执行上下文](https://juejin.cn/post/6844903682283143181#heading-3)： 
>
>   当我们执行一段代码时，js会生成一个与该方法对象的执行环境（context），又叫做执行上下文
>
> - 执行栈：
>
>   执行上下文中有这个方法的私有作用域、上层作用域指向、参数、私有作用域中的变量、this对象等，该执行环境会被添加进一个栈中，这个栈就是执行栈



#### 7、Js原型原型链的理解

> - **原型：**
>
>   js中使用构造函数创建对象；
>
>   每个js对象中有`prototype`属性，改属性为一个对象，包含着该构造函数所共有的一些属性和方法
>
>   同时，由该构造函数声明的实例中有一个`__proto__`属性，该属性为一个指针，指向构造函数`prototype`所共享的属性和方法
>
> - **原型链：**
>
>   当我们调用一个对象的某一属性时，会先在当前对象内查找，若查不到，则通过`prototype`向上层查找，若还查不到，则继续逐层查，直到找到`object.prototype = null`返回
>
> - **获取原型方法：**
>
>   - `object.__proto__`
>   - `object.construct.protptype`
>   - `object.getPrototypeOf`
> - **原型可以做什么：**
>   - 共享实例和方法
>   - 继承

#### 7、深浅拷贝

> - 浅拷贝：
>
>     - 若拷贝对象为基本数据类型，则拷贝的是这个数据的值
>     - 若拷贝对象为引用数据类型，则拷贝的是该数据的引用指针，当原数据改变，拷贝的数据也会改变
> - 深拷贝：
>     - 完全复制拷贝对象的数据，另存在一个新的内存空间，且原数据改变，不会改变拷贝数据

#### 8、作用域 & 作用域链

> # 作用域
>
> js作用域为词法作用域，词法作用域就是定义在词法阶段的作用， 换句话说，词法作用域是由写代码时将变量和块作用域写在哪里来决定的

#### 9、[执行上下文](https://juejin.cn/post/6844903682283143181#heading-3)

> - 执行上线文的类型
>
>   - 全局执行上下文
>
>     这个是默认上下文，任何不在函数内部的代码都在全局上下文中，他会执行两件事，创建一个全局的windows对象，并且设置this等于全局对象，一个程序中只会有一个全局执行上下文
>
>   - 函数执行上下文
>
>     函数在调用的时候都会创建一个新的执行上下文，每个函数都有自己的函数执行上下文，不过是在函数调用时才会被创建
>
>   - eval执行上下文
>
>     执行在 `eval` 函数内部的代码也会有它属于自己的执行上下文，但由于 JavaScript 开发者并不经常使用 `eval`，所以在这里我不会讨论它。

#### 10、call 、apply、bind

如下js代码

```js
const Person = {
	weight: "180kg"
};

const getPerson = function (name, age) {
	console.log("name: ", name);
	console.log("age: ", age);
	console.log(this.weight);
};

# call
getPerson.call(Person,'KangKang',18)
# apply
getPerson.apply(Person, ['KangKang',18])
# bind
let newPerson = getPerson.bind(Person)
newPerson("Kangkang", 18)

# result 
//	name:  Kangkang
//	age:  18
//	weight: 180kg
```

> - call
>   + call() 方法在使用一个指定的 this 值和若干个指定的参数值的前提下调用某个函数或方法。
> - apply
>   - 与call类似，参数以数组的方式传递参数
> - bind
>   - bind() 方法会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。
>   - 人话： bind声明一个新函数，`let newPerson = getPerson.bind(Person)` , bind后的参数为this指向 ， 其他参数可以在新的实例后面传递，如 `newPerson("Kangkang", 18)`

