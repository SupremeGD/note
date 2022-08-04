## 第3章

### 3.1严格模式

​		ECMAScript5增加了严格模式的概念，严格模式是一种不同的JavaScript解析和执行模型，ES3的一些不规范写法在这种模式下会被处理，对于不安全的活动将抛出错误。

​		要对整个脚本启用严格模式，在脚本开头加上这一行：“ use  strict ” ；这是一个预处理指令。任何支持的JavaScript引擎看到他都会切换到严格模式。（这种语法形式不破坏ES3语法）

​		单独指定一个函数在严格模式下执行，只要把这个预处理指令放到函数体开头：

​		function  doSome() {

​				" use  strcit "；

​				// 函数体

​		}



#### 3.1.5语句

在控制语句中使用代码块可以让内容更清晰，在需要修改代码时也可以减少出错的可能性。





### 3.2关键字与保留字

保留的关键字不能用作标识符或属性名。ECMA-262第6版规定的所有关键字如下：

```
break	   do		 	in		 		typeof 
case	   else	 		instanceof		var 
catch	   export	 	new				void 
class	   extends 		return 			while 
const      finally      super           with 
continue   for 			switch  		yield 
debugger   function 	this 
default    if 			throw 
delete	   import 		try
```

```
始终保留: 
enum 
严格模式下保留: 
implements 		package 	public 
interface 		protected 	static 
let 			private 
模块代码中保留: 
await
```

**不要使用关键字和保留字作为标识符和属性名，以确保兼容过去和未来的ECMAScript版本。**





### 3.3变量

​		ECMAScript 变量是松散类型的，意思是变量可以用于保存任何类型的数据。每个变量只不过是一

个用于保存任意值的命名占位符。有 3 个关键字可以声明变量：var、const 和 let。其中，var 在

ECMAScript 的所有版本中都可以使用，而 const 和 let 只能在 ECMAScript 6 及更晚的版本中使用。

##### 1.var的声明和变量

使用 var 操作符定义的变量会成为包含它的函数的局部变量。比如，使用 var

在一个函数内部定义一个变量，就意味着该变量将在函数退出时被销毁：

```
function test() { 
 var message = "hi"; // 局部变量
} 
test(); 
console.log(message); // 出错！
```

在函数内定义变量时省

略 var 操作符，可以创建一个全局变量：

```
function test() { 
 message = "hi"; //全局变量
} 
test(); 
console.log(message); // "hi" 
```

**注意**

**不能一下子断定省略var是不是有意义而为之。在严格模式下，如果像这样给未声明的变量赋值，则会抛出异常ReferenceError。**



##### 2.let声明

let 跟 var 的作用差不多，但有着非常重要的区别。最明显的区别是，let 声明的范围是块作用域，

而 var 声明的范围是函数作用域。

```
let age; 
let age; // SyntaxError；标识符 age 已经声明过了
```

这两个关键字声明的并不是不同类型的变量，

它们只是指出变量在相关作用域如何存在。

```
var name; 
let name; // SyntaxError 
let age; 
var age; // SyntaxError 
```



###### 暂时性死区

let与var 的另一个重要的区别，就是 let 声明的变量不会在作用域中被提升。

```
// name 会被提升
console.log(name); // undefined 
var name = 'Matt'; 

// age 不会被提升
console.log(age); // ReferenceError：age 没有定义
let age = 26;
```

在解析时，JavaScript 引擎也会注意出现在后面的 let 声明，只不过在此之前不能以任何方式来引用未声明的变量。在 let 声明之前的执行瞬间被称为 “ 暂时性死区 ”，在此阶段任何后面才声明的变量都会抛出 ReferenceError。



##### 3.全局声明

与 var 关键字不同，使用 let 在全局作用域中声明的变量不会成为 window 对象的属性（var 声

明的变量则会）。

```
var name = 'Matt'; 
console.log(window.name); // 'Matt' 

let age = 26; 
console.log(window.age); // undefined
```



##### 4.条件声明

​		在使用 var 声明变量时，由于声明会被提升，JavaScript 引擎会自动将多余的声明在作用域顶部合

并为一个声明。因为 let 的作用域是块，所以不可能检查前面是否已经使用 let 声明过同名变量，同

时也就不可能在没有声明的情况下声明它。

```
 var name = 'Nicholas'; 
 let age = 26; 
</script> 

<script> 
 // 假设脚本不确定页面中是否已经声明了同名变量
 // 那它可以假设还没有声明过
 
 var name = 'Matt'; 
 // 这里没问题，因为可以被作为一个提升声明来处理
 // 不需要检查之前是否声明过同名变量
 
 let age = 36; 
 // 如果 age 之前声明过，这里会报错
</script>
```

使用 try/catch 语句或 typeof 操作符也不能解决，因为条件块中 let 声明的作用域仅限于该块。

```
<script> 
 let name = 'Nicholas'; 
 let age = 36; 
</script> 

<script> 
 // 假设脚本不确定页面中是否已经声明了同名变量
 // 那它可以假设还没有声明过
 if (typeof name === 'undefined') { 
 	let name; 
 } 
 // name 被限制在 if {} 块的作用域内
 // 因此这个赋值形同全局赋值
 name = 'Matt'; 

 try { 
 console.log(age); // 如果 age 没有声明过，则会报错
 } 
 catch(error) { 
 let age; 
} 

 // age 被限制在 catch {}块的作用域内
 // 因此这个赋值形同全局赋值
 age = 26; 
</script> 
```

**注意**

**不能使用 let 进行条件式声明，因为条件声明是一种反模式，它让程序变得更难理解。**





##### 5.for 循环中的 let 声明

使用 let 之后迭代变量的作用域仅限于 for 循环块内部



在使用 var 的时候，最常见的问题就是对迭代变量的奇特声明和修改：

```
for (var i = 0; i < 5; i++) {
	setTimeout(() => console.log(i), 0)
}
// 你可能以为会输出0、1、2、3、4
// 实际上会输出 5、5、5、5、5

这是因为在退出循环时，迭代变量保存的是导致循环退出的值：5。在执行超时逻辑时，所有的 i 都是同一个变量，
所以输出的都是同一个值。
```

只需将 var 改为 let 即可。



##### 6.const声明

​		const 与 let 基本相同，唯一的区别是用它声明变量时必须初始化变量，且修改const 声明的变量会导致运行时错误。

​		如果 const 变量引用的是一个对象，那么修改这个对象内部的属性并不违反 const 的限制。

​		虽然 const 变量跟 let 变量很相似，但是不能用 const 来声明迭代变量（因为迭代变量会自增）

```
for (const i = 0; i < 10; ++i) {} // TypeError：给常量赋值
```

​		如果你只想用 const 声明一个不会被修改的 for 循环变量，那也是可以的。也就是说，每

次迭代只是创建一个新变量。这对 for-of 和 for-in 循环特别有意义：

```
let i = 0; 
for (const j = 7; i < 5; ++i) { 
 console.log(j); 
} 
// 7, 7, 7, 7, 7 

for (const key in {a: 1, b: 2}) { 
 console.log(key); 
} 
// a, b 

for (const value of [1,2,3,4,5]) { 
 console.log(value); 
} 
// 1, 2, 3, 4, 5 
```



##### 7.声明风格及最佳实践

（1）	不使用 var

​		限制自己只使用 let 和 const 有助于提升代码质量，因为变量有了明确的作用域、声明位置，以及不变的值。

（2）	const 优先，let 次之

​		const 声明可以强制保持变量不变，也可以让静态代码分析工具提前发现不合法的赋值操作。因此，很多开发者认为应该优先使用 const 来声明变量，只在提前知道未来会有修改时，再使用 let。这样可以让开发者更有信心地推断某些变量的值永远不会变，同时也能迅速发现因意外赋值导致的非预期行为。





### 3.4数据类型

​		ECMAScript 有 6 种简单数据类型（也称为原始类型）：Undefined、Null、Boolean、Number、

String 和 Symbol。Symbol（符号）是 ECMAScript 6 新增的。还有一种复杂数据类型叫 Object（对

象）。Object 是一种无序名值对的集合。但 ECMAScript 的数据类型很灵活，一种数据类型可以当作多种数据类型来使用。

##### 1.typeof 操作符

因为 ECMAScript 的类型系统是松散的，所以需要一种手段来确定任意变量的数据类型。

对一个值使用 typeof 操作符会返回下列字符串之一：

```
 "undefined"表示值未定义；
 "boolean"表示值为布尔值；
 "string"表示值为字符串；
 "number"表示值为数值；
 "object"表示值为对象（而不是函数）或 null； 
 "function"表示值为函数；
 "symbol"表示值为符号。
```

​		typeof 是一个操作符而不是函数，所以不需要参数（但可以使用参数）。

​		typeof在某些情况下返回的结果可能会让人费解，但技术上讲还是正确的。比如，调用typeof 

null 返回的是"object"。这是因为特殊值 null 被认为是一个对空对象的引用。

**注意**

​		**严格来讲，函数在 ECMAScript 中被认为是对象，并不代表一种数据类型。可是，**

**函数也有自己特殊的属性。为此，就有必要通过 typeof 操作符来区分函数和其他对象。**















































