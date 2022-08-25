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



##### 2.Undefined 类型

Undefined 类型只有一个值，就是特殊值 undefined。当使用 var 或 let 声明了变量但没有初始

化时，就相当于给变量赋予了 undefined 值

**注意**

​		**一般来说，永远不用显式地给某个变量设置 undefined 值。字面值 undefined**

**主要用于比较，而且在 ECMA-262 第 3 版之前是不存在的。增加这个特殊值的目的就是为**

**了正式明确空对象指针（null）和未初始化变量的区别。**



​		**即使未初始化的变量会被自动赋予 undefined 值，但我们仍然建议在声明变量的同时进行初始化。这样，当 typeof 返回"undefined"时，你就会知道那是因为给定的变量尚未声明，而不是声明了但未初始化。**



##### 3.Null 类型

null 值表示一个空对象指针，这也是给

typeof 传一个 null 会返回"object"的原因





##### 4.Boolean 类型

Boolean（布尔值）类型是 ECMAScript 中使用最频繁的类型之一，有两个字面值：true 和 false。

这两个布尔值不同于数值，因此 true 不等于 1，false 不等于 0。



**注意**：布尔值字面值量 true 和 false 是区分大小写的，因此 True 和 False（及其他大小混写形式）是有效的标识符，但不是布尔值。



不同类型与布尔值之间的转换规则。

```
数据类型 			转换为 true的值 		转换为false的值

Boolean				true 				false

String 				非空字符串 			""（空字符串）

Number 			非零数值（包括无穷值） 	0、NaN（参见后面的相关内容）

Object 				任意对象 				null

Undefined 			N/A（不存在） 		undefined
```

像 if 等流控制语句会自动执行其他类型值到布尔值的转换



##### 5.Number 类型

Number 类型使用 IEEE 754 格式表示整数和浮点值（在某些语言中也叫双精度值）。

​		八进制字面量在严格模式下是无效的，会导致 JavaScript 引擎抛出语法错误。要创建十六进制，要在数值前加上 0x（区分大小写）。

​		**使用八进制和十六进制格式创建的数值在所有数学操作中都被视为十进制数值。**



###### （1）浮点值

浮点值数值中必须包含小数点，小数点后必须至少有一个数字。浮点值使用的内存空间是存储整数值的两倍。

​		对于非常大或非常小的数值，浮点值可以用科学记数法来表示。ECMAScript 中科学记数法的格式要求是一个数（整数或浮点数）后跟一个大写或小写的字母 e，再加上一个要乘的 10 的多少次幂。

```
let floatNum = 3.125e7; // 等于 31250000

0.000 000 3 会被转换为 3e-7
```

**注意**

​		**之所以存在这种舍入错误，是因为使用了 IEEE 754 数值，这种错误并非 ECMAScript**

**所独有。其他使用相同格式的语言也有这个问题。**



###### （2）值的范围

最小的值保存在Number.MIN_VALUE 中。最大数值保存在Number.MAX_VALUE 中

​		任何无法表示的负数以-Infinity（负无穷大）表示，任何无法表示的正数以 Infinity（正

无穷大）表示。

​		要确定一个值是不是有限大（即介于 JavaScript 能表示的最小值和最大值之间），可以使用 isFinite()函数

```
let result = Number.MAX_VALUE + Number.MAX_VALUE; 
console.log(isFinite(result)); // false
```

**注意**

​		使用 Number.NEGATIVE_INFINITY（包含 -Infinity ） 和 Number.POSITIVE_INFINITY （包含 Infinity  ）也可以获取正、负 Infinity。



###### （3）NaN

特殊值NaN，意思是 “ 不是数值 ”（Not a Number）。

在 ECMAScript 中，0、+0 或0 相除会返回 NaN：

```
console.log(0/0); // NaN 
console.log(-0/+0); // NaN 
```

如果分子是非 0 值，分母是有符号 0 或无符号 0，则会返回 Infinity 或-Infinity：

```
console.log(5/0); // Infinity 
console.log(5/-0); // -Infinity 
```

​		ECMAScript 提供了 isNaN()函数，判断此参数是否 “ 不是数值 ”

任何不能转换为数值的值都会导致这个函数返回true。举例如下：

```
console.log(isNaN(NaN)); // true 
console.log(isNaN(10)); // false，10 是数值
console.log(isNaN("10")); // false，可以转换为数值 10 
console.log(isNaN("blue")); // true，不可以转换为数值
console.log(isNaN(true)); // false，可以转换为数值 1 
```



**注意**

​		isNaN（）可以用于测试对象，首先会调用对象的 valueOf（）方法，然后再确定返回的值是否可以转换为数值。不能则调用 toString（）方法，并返回值。



###### （4）数值转换

可以将非数值转换为数值：Number()、parseInt()和 parseFloat()。

Number()函数基于如下规则执行转换。

```
布尔值，true 转换为 1，false 转换为 0。 
数值，直接返回。
null，返回 0。 
undefined，返回 NaN。
```

​		考虑到用 Number()函数转换字符串时相对复杂且有点反常规，通常在需要得到整数时可以优先使

用 parseInt()函数，字符串最前面的空格会被忽略，从第一个非空格字符开始转换。

提供了十六进制参数，那么字符串前面的"0x"可以省掉：

```
let num1 = parseInt("AF", 16); // 175 
let num2 = parseInt("AF"); // NaN 
```

**注意**

​		避免出现错误，要始终传给他第二个参数。



##### 6.String 类型

​		String（字符串）数据类型表示零或多个 16 位 Unicode 字符序列。字符串可以使用双引号（"）、单引号（'）或反引号（`）标示。

###### （1）字符字面量

字符串数据类型包含一些字符字面量，用于表示非打印字符或有其他用途的字符，如下表所示：

字 面 量 						含 义

------

\n								换行

\t 								制表

\b								退格

\r 								回车

\f 								换页

\\				  			  反斜杠（\）

\'								单引号（'），在字符串以单引号标示时使用，例如'He said, \'hey.\''

\" 								 双引号（"），在字符串以双引号标示时使用，例如"He said, \"hey.\""

\` 								反引号（`），在字符串以反引号标示时使用，例如`He said, \`hey.\``

\x*nn* 						  以十六进制编码 *nn* 表示的字符（其中 *n* 是十六进制数字 0~F），例如\x41 等于"A"

\u*nnnn* 					以十六进制编码 *nnnn* 表示的 Unicode 字符（其中 *n* 是十六进制数字 0~F），例如\u03a3 等								 于希腊字符"Σ"

------

###### （2）字符串的特点

​		ECMAScript 中的字符串是不可变的，一旦创建它的值就不能变了，要修改变量中的字符串值，要先销毁原始的字符串，然后将新的值保存到给变量中。

```
let lang = "Java"; 
lang = lang + "Script";
```



###### （3）转换为字符串

使用几乎所有的值都有的 toString（）方法，唯一用途返回当前值的字符串等价物。

```
let age = 11; 
let ageAsString = age.toString(); // 字符串"11" 
let found = true; 
let foundAsString = found.toString(); // 字符串"true" 
```

toString()方法可见于数值、布尔值、对象和字符串值。

传入参数，可以得到数值的二进制、八进制、十六进制，或其它任何有效的字符串

```
let num = 10; 
console.log(num.toString()); // "10" 
console.log(num.toString(2)); // "1010" 
console.log(num.toString(8)); // "12" 
console.log(num.toString(10)); // "10" 
console.log(num.toString(16)); // "a"
```

​		不确定一个值是不是 null 或 undefined，可以使用 String()转型函数，它始终会返回表

示相应类型值的字符串。



###### （4）模板字面量

​		**ECMAScript 6 新增了使用模板字面量定义字符串的能力。与使用单引号或双引号不同，模板字面量**

**保留换行字符，可以跨行定义字符串**

```
let myMultiLineString = 'first line\nsecond line'; 
let myMultiLineTemplateLiteral = `first line 
second line`; 
console.log(myMultiLineString); 
// first line 
// second line" 
console.log(myMultiLineTemplateLiteral); 
// first line 
// second line
```



###### （5）字符串插值

​		模板字面量最常用的一个特性是支持字符串插值，也就是可以在一个连续定义中插入一个或多个

值。模板字面量不是字符串，而是特殊的 JavaScript 表达式，不过求值后得到的是字符串。

​		示例：

```
let value = 5; 
let exponent = 'second'; 
// 以前，字符串插值是这样实现的：
let interpolatedString = 
 value + ' to the ' + exponent + ' power is ' + (value * value); 
 
// 现在，可以用模板字面量这样实现：
let interpolatedTemplateLiteral = 
 `${ value } to the ${ exponent } power is ${ value * value }`; 
 
console.log(interpolatedString); // 5 to the second power is 25 
console.log(interpolatedTemplateLiteral); // 5 to the second power is 25
```

**注意**

​		所有插值都会使用 toString（）方法强制转换为字符串，任何 JS 表达式都可以用于插值。



​		将表达式转换为字符串时会调用 toString()：

```
let foo = { toString: () => 'World' }; 
console.log(`Hello, ${ foo }!`); // Hello, World! 
```

在插值表达式中可以调用函数和方法：

```
function capitalize(word) { 
 return `${ word[0].toUpperCase() }${ word.slice(1) }`; 
} 
console.log(`${ capitalize('hello') }, ${ capitalize('world') }!`); // Hello, World! 
```

此外，模板也可以插入自己之前的值：

```
let value = ''; 
function append() { 
 value = `${value}abc` 
 console.log(value); 
} 
append(); // abc 
append(); // abcabc 
append(); // abcabcabc 
```



###### （6）模板字面量标签函数

​		模板字面量支持定义标签函数，通过标签函数可以自定义插值函数行为。标签函数会接收被插值记号分隔后的模板和对每个表达式求值的结果。

​		标签函数会接收被插值记号分隔后的模板和对每个表达式求值的结果。标签函数本身是一个常规函数，通过前缀到模板字面量来应用自定义行为，标签函数接收到的参数依次是原始字符串数组和对每个表达式求值的结果。这个函数的返回值是对模板字面量求值得到的字符串。

```
function simpleTag(strings,...values){
	console.log(strings);
	console.log(values);
	return "foobar";
}
let a = 6;
let b = 9;
let tagResult = simpleTag`${a}aaaa${b}bbbb`;
//标签函数后面可以紧跟一个模板字面量
//tagResult的值是simpleTag函数的返回值
//标签函数的第一个参数是模板字面量不包含插值的字符串数组，在上面中，strings为：["","aaaa","bbbb"]
//从第二个参数开始，依次是模板字面量中间的插值
————————————————
版权声明：本文为CSDN博主「木子 旭」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_45895576/article/details/117248067
```



###### （7）原始字符串

​		使用模板字面量可以直接获取原始的模板字面量内容（换行符或 Unicode 字符），而不是转换后的字符。

​		String.raw 是一个标签函数，返回一个转义前的该字符串（原始字符串）。但是对于实际的换行符不行。

```
let str = String.raw`\u0049`;//str的值为\u0049
```

​		可以通过raw 属性获取字符串数组中的每一个原始字符串。

```
for(const rawString of strings.raw){//string为一个字符串数组
	console.log(rawString);//获取到的为字符串数组中每一个元素的原始字符串
}
```



##### 7.Symbol

​		Symbol 是ECMAScript 6 新增的数据类型。符号是原始值，符号实例是唯一、不可变的。

​		符号用途是确保对象属性使用唯一标识符，不会发生属性冲突。符号是用来创建唯一记号，进而作用非字符串形式的对象属性。



###### （1）  符号的基本用法

​		符号需要使用 Symbol（）函数初始化。typeof 操作符对符号返回的是 symbol，符号没有字面量语法。

​		Symbol（）函数可以传入字符串参数，但是字符串参数与符号定义域或标识无关。

```
let a = Symbol();
let b = Symbol();
console.log( a == b )	// false

let c = Symbol('fffff');
let d = Symbol('fffff');
console.log( c == d )  // false
```

​		将创建的Symbol（）实例用作对象的新属性，它就不会覆盖已有的对象属性（无论是字符串还是符号）。

​		Symbol（）不能与 new 一起作为构造函数使用。符号包装对象可以借用 Object（）。



###### （2）  使用全局符号注册表

​		Symbol.for（）不同部分需要共享和重用符号实例。

​		Symbol.for（）对每个字符串都执行幂等操作。第一次使用某个字符串时，他会检擦全局运行的注册表，要是发现没有对应的，他就会生成一个新的符号实例添加到注册表中；如果有对应的，他就会返回与该符号实例。

```
let a = Symbol('abcd');
let b = Symbol('abcd');
console.log( a == b );	//true
```

**全局注册表中的符号必须使用字符串创建，注册表中使用的键同时也会被用作符号描述。**

**作为参数传给 Symbol.for（）的任何值都会被转换为字符串。**



​		Symbol.keyFor（）接收符号返回该全局符号对应的字符串，用来查询全局注册表。如查询的不是全局符号，则返回 undefined。

```
// 创建全局符号
let a = Symbol.for('abc'); 
console.log(Symbol.keyFor(a)); // foo 

// 创建普通符号
let b = Symbol('ddd'); 
console.log(Symbol.keyFor(b)); // undefined
```



###### （3）  使用符号作为属性

​		凡是可以使用字符串或数值作为属性的地方，都可以使用符号（包括对象字面量属性和Object.defineProperty()/Object.defineProperties()定义的属性）。

​		对象字面量只能再计算属性语法中使用符号作为属性。

```
let s1 = Symbol('foo'), 
 	s2 = Symbol('bar'), 
 	s3 = Symbol('baz'), 
 	s4 = Symbol('qux'); 
let o = { 
 [s1]: 'foo val' 
}; 
// 这样也可以：o[s1] = 'foo val'; 

console.log(o); 
// {Symbol(foo): foo val} 

Object.defineProperty(o, s2, {value: 'bar val'}); 
console.log(o); 
// {Symbol(foo): foo val, Symbol(bar): bar val} 

Object.defineProperties(o, { 
 [s3]: {value: 'baz val'}, 
 [s4]: {value: 'qux val'} 
}); 

console.log(o); 
// {Symbol(foo): foo val, Symbol(bar): bar val, 
// Symbol(baz): baz val, Symbol(qux): qux val}
```



​		Object.getOwnPropertyNames()返回对象实例的常规属性数组，

Object.getOwnPropertySymbols()返回对象实例的符号属性数组。这两个方法的返回值彼此互斥。Object.getOwnPropertyDescriptors()会返回同时包含常规和符号属性描述符的对象。

Reflect.ownKeys()会返回两种类型的键

```
let s1 = Symbol('foo'), 
 s2 = Symbol('bar'); 
let o = { 
 [s1]: 'foo val', 
 [s2]: 'bar val', 
 baz: 'baz val', 
 qux: 'qux val' 
}; 
console.log(Object.getOwnPropertySymbols(o)); 
// [Symbol(foo), Symbol(bar)] 

console.log(Object.getOwnPropertyNames(o)); 
// ["baz", "qux"] 

console.log(Object.getOwnPropertyDescriptors(o)); 
// {baz: {...}, qux: {...}, Symbol(foo): {...}, Symbol(bar): {...}} 

console.log(Reflect.ownKeys(o)); 
// ["baz", "qux", Symbol(foo), Symbol(bar)]
```

​		符号属性是内存中符号的一个引用直接创建用作属性的符号不会丢失。没有显式的保护属性的引用，需要遍历所有符号属性才能找到对应的属性键。



###### （4）常用内置符号

​		ECMAScript 6 引入了一批常用内置符号，用于暴露语言内部行为，开发者可以直接访问、重写或模拟这些行为。

​		for-of 循环会在相关对象上使用 Symbol.iterator 属性，可以通过自定义对象上重新定义 Symbol.iterator 的值，来改变 for-of 在迭代该对象时的行为。

​		所有内置符号属性都是不可写、不能枚举、不可配置的。

**注意**

​		**提到 ECMAScript 规范时，经常会引用符号在规范中的名称，前缀为@@。比如，**

**@@iterator 指的就是 Symbol.iterator。**



###### （5）Symbol.asyncIterator

​		符号作为属性表示 “ 一个方法，返回对象默认的 AsyncIterator，有 for-await-of 语句使用 ”。（这个符号表示实现异步迭代器API 的函数）。

​		for-await-of 循环会利用这个函数执行异步迭代操作。循环时，它们会调用以 Symbol.asyncIterator

为键的函数，返回的对象是实现该 API 的 AsyncGenerator。

```
class Foo { 
 	async *[Symbol.asyncIterator]() {} 
} 
let f = new Foo(); 
console.log(f[Symbol.asyncIterator]()); 
// AsyncGenerator {<suspended>}
```

​		由 Symbol.asyncIterator 函数生成的对象应该通过其 next()方法陆续返回Promise 实例。

​		可以调用 next()方法返回，也可以隐式地通过异步生成器函数返回

**注意** 

​		**Symbol.asyncIterator 是 ES2018 规范定义的，因此只有版本非常新的浏览器支持它。**



###### （6）Symbol.hasInstance

​		instanceof 操作符可以用来确定一个对象实例的原型链上是否有原型。

​		在 ES6 中，instanceof 操作符会使用 Symbol.hasInstance 函数来确定关系。

```
function Foo() {} 
let f = new Foo(); 
console.log(Foo[Symbol.hasInstance](f)); // true
```

​		属性定义在 Function 的原型上，因此默认在所有函数和类上都可以调用。由于 instanceof

操作符会在原型链上寻找这个属性定义可以在继承的类上通过静态方法重新定义这个函数

```
class Bar {} 
class Baz extends Bar { 
 static [Symbol.hasInstance]() { 
 return false; 
 } 
} 
let b = new Baz(); 
console.log(Bar[Symbol.hasInstance](b)); // true 
console.log(b instanceof Bar); // true 
console.log(Baz[Symbol.hasInstance](b)); // false 
console.log(b instanceof Baz); // false
```



###### （7）Symbol.isConcatSpreadable

​		作为属性表示“一个布尔值，如果是 true，则意味着对象应该用 Array.prototype.concat()打平其数组元素”，

​		Array.prototype.concat()方法会根据接收到的对象类型选择如何将一个类数组对象拼接成数组实例。覆盖Symbol.isConcatSpreadable 的值可以修改这个行为。

​		数组对象默认情况下会被打平到已有的数组，false 或假值会导致整个对象被追加到数组末尾。类数组对象默认情况下会被追加到数组末尾，true 或真值会导致这个类数组对象被打平到数组实例。其他不是类数组对象的对象在 Symbol.isConcatSpreadable 被设置为 true 的情况下将被忽略。

```
let initial = ['foo']; 
let array = ['bar']; 
console.log(array[Symbol.isConcatSpreadable]); // undefined 
console.log(initial.concat(array)); // ['foo', 'bar'] 
array[Symbol.isConcatSpreadable] = false;
console.log(initial.concat(array)); // ['foo', Array(1)] 

let arrayLikeObject = { length: 1, 0: 'baz' }; 
console.log(arrayLikeObject[Symbol.isConcatSpreadable]); // undefined 
console.log(initial.concat(arrayLikeObject)); // ['foo', {...}] 
arrayLikeObject[Symbol.isConcatSpreadable] = true; 
console.log(initial.concat(arrayLikeObject)); // ['foo', 'baz'] 

let otherObject = new Set().add('qux'); 
console.log(otherObject[Symbol.isConcatSpreadable]); // undefined 
console.log(initial.concat(otherObject)); // ['foo', Set(1)] 
otherObject[Symbol.isConcatSpreadable] = true; 
console.log(initial.concat(otherObject)); // ['foo']
```



###### （8）Symbol.iterator

​		这个符号表示迭代器 API 的函数。for-of 循环这样的语言结构会利用这个函数执行迭代操作，返回的对象是实现该 API 的 Generator。

```
class Foo { 
 	*[Symbol.iterator]() {} 
} 
let f = new Foo(); 
console.log(f[Symbol.iterator]()); 
// Generator {<suspended>}
```

​		Symbol.iterator 函数生成的对象应该通过其 next()方法陆续返回值。可以通过显式地调用 next()方法返回，也可以隐式地通过生成器函数返回



###### （9）Symbol.match

​		表示“一个正则表达式方法，该方法用正则表达式去匹配字符串。由 String.prototype.match()方法使用”，会使用以 Symbol.match 为键的函数来对正则表达式求值。

​		传入非正则表达式值会导致该值被转换为 RegExp 对象。让方法直接使用参数，就可以重新定义函数，从而让 match（）方法使用非正则表达式实例。

​		Symbol.match（）接收一个参数，就是调用 match（）方法的字符串实例（返回值没有限制）。

```
class FooMatcher { 
 	static [Symbol.match](target) { 
 		return target.includes('foo'); 
 	} 
} 
console.log('foobar'.match(FooMatcher)); // true 
console.log('barbaz'.match(FooMatcher)); // false 
class StringMatcher { 
 	constructor(str) { 
 	this.str = str; 
 } 
 	[Symbol.match](target) { 
 		return target.includes(this.str); 
 	} 
} 
console.log('foobar'.match(new StringMatcher('foo'))); // true 
console.log('barbaz'.match(new StringMatcher('qux'))); // false
```



###### （10）Symbol.replace

​		替换一个字符串中匹配的子串。 由String.prototype.replace()方法使用。String.prototype.replace()方法会使用以 Symbol.replace 为键的函数来对正则表达式求值。

```
console.log(RegExp.prototype[Symbol.replace]); 
// ƒ [Symbol.replace]() { [native code] } 
console.log('foobarbaz'.replace(/bar/, 'qux')); 
// 'fooquxbaz'
```

​		传入非正则表达式值会导致该值被转换为 RegExp 对象，Symbol.replace 函数接收两个参数，即调用 replace()方法的字符串实例和替换字符串。返回的值没有限制。（与（9）写法一致）



###### （11）Symbol.search

​		表示“一个正则表达式方法，该方法返回字符串中匹配正则表达式的索引。由 String.prototype.search()方法使用”。所有正则表达式实例默认是这个 String 方法的有效参数。

```
console.log(RegExp.prototype[Symbol.search]); 
// ƒ [Symbol.search]() { [native code] } 
console.log('foobar'.search(/bar/)); 
// 3
```

​		传入非正则表达式值会导致该值被转换为 RegExp 对象。Symbol.search 函数接收一个参数，就是调用 match()方法的字符串实例。返回的值没有限制。（与（9）写法一致）



###### （12）Symbol.species

​		表示“一个函数值，该函数作为创建派生对象的构造函数”。

```
class Bar extends Array {} 
class Baz extends Array { 
 static get [Symbol.species]() { 
 return Array; 
 } 
} 
let bar = new Bar(); 
console.log(bar instanceof Array); // true 
console.log(bar instanceof Bar); // true
bar = bar.concat('bar'); 
console.log(bar instanceof Array); // true 
console.log(bar instanceof Bar); // true 

let baz = new Baz(); 
console.log(baz instanceof Array); // true 
console.log(baz instanceof Baz); // true 
baz = baz.concat('baz'); 
console.log(baz instanceof Array); // true 
console.log(baz instanceof Baz); // false
```



###### （13）Symbol.split

​		表示“一个正则表达式方法，该方法在匹配正则表达式的索引位置拆分字符串。由 String.prototype.split()方法使用”。所有正则表达式实例默认是这个 String 方法的有效参数。

```
console.log(RegExp.prototype[Symbol.split]); 
// ƒ [Symbol.split]() { [native code] } 
console.log('foobarbaz'.split(/bar/)); 
// ['foo', 'baz']
```

​		传入非正则表达式值会导致该值被转换为 RegExp 对象。Symbol.split 函数接收一个参数，就是调用 match()方法的字符串实例。返回的值没有限制。



###### （14）Symbol.toPrimitive

​		表示“一个方法，该方法将对象转换为相应的原始值。由 ToPrimitive 抽象操作使用”。自定义对象实例，可以通过 Symbol.toPrimitive 属性上定义一个可以改变默认行为。

```
class Foo {} 
let foo = new Foo(); 
console.log(3 + foo); // "3[object Object]" 
console.log(3 - foo); // NaN 
console.log(String(foo)); // "[object Object]" 
class Bar { 
	constructor() { 
 		this[Symbol.toPrimitive] = function(hint) { 
 			switch (hint) { 
 				case 'number': 
 					return 3; 
 				case 'string': 
 					return 'string bar'; 
 				case 'default': 
 				default: 
 					return 'default bar'; 
 			} 
 		} 
 	 } 
}
let bar = new Bar(); 
console.log(3 + bar); // "3default bar" 
console.log(3 - bar); // 0 
console.log(String(bar)); // "string bar"
```



###### （15）Symbol.toStringTag

​		表示“一个字符串，该字符串用于创建对象的默认字符串描述。由内置方法 Object.prototype.toString()使用”。

​		通过 toString()方法获取对象标识时，会检索由 Symbol.toStringTag 指定的实例标识符，默认为"Object"。

```
let s = new Set(); 
console.log(s); // Set(0) {} 
console.log(s.toString()); // [object Set] 
console.log(s[Symbol.toStringTag]); // Set
```



###### （16）Symbol.unscopables

​		表示“一个对象，该对象所有的以及继承的属性，都会从关联对象的 with 环境绑定中排除”。设置这个符号并让其映射对应属性的键值为 true，就可以阻止该属性出现在 with 环境绑定中

```
let o = { foo: 'bar' }; 
with (o) { 
 	console.log(foo); // bar 
} 
o[Symbol.unscopables] = { 
 	foo: true 
}; 
with (o) { 
 	console.log(foo); // ReferenceError 
}
```

**注意**

​		**不推荐使用 with ，因此也不推荐使用 Symbol.unscopables。**





##### 8.Object 类型

​		ECMAScript 中的对象其实就是一组数据和功能的集合。

​		Object 实例都有如下属性和方法。

```
constructor：用于创建当前对象的函数。在前面的例子中，这个属性的值就是 Object() 
函数。
 hasOwnProperty(propertyName)：用于判断当前对象实例（不是原型）上是否存在给定的属
性。要检查的属性名必须是字符串（如 o.hasOwnProperty("name")）或符号。
 isPrototypeOf(object)：用于判断当前对象是否为另一个对象的原型。（第 8 章将详细介绍
原型。）
 propertyIsEnumerable(propertyName)：用于判断给定的属性是否可以使用（本章稍后讨
论的）for-in 语句枚举。与 hasOwnProperty()一样，属性名必须是字符串。
 toLocaleString()：返回对象的字符串表示，该字符串反映对象所在的本地化执行环境。
 toString()：返回对象的字符串表示。
 valueOf()：返回对象对应的字符串、数值或布尔值表示。通常与 toString()的返回值相同。
因为在 ECMAScript 中 Object 是所有对象的基类，所以任何对象都有这些属性和方法。
```



### 3.5 操作符

​		ECMA-262 描述了一组可用于操作数据值的操作符，包括数学操作符（如加、减）、位操作符、关系操作符和相等操作符等。在应用给对象时，操作符通常会调用 valueOf()和/或 toString()方法来取得可以计算的值。



##### 1.一元操作符

​		只操作一个值的操作符叫做一元操作符，一元操作符是 ECMAScript中最简单的操作符。



###### （1）递增 / 递减操作符

​		递增和递减操作符直接照搬自 C 语言，但有两个版本：前缀版和后缀版。前缀版就是位于要操作的变量前头，后缀版就是位于要操作的变量后头。（最好用后缀版，前缀版有副作用：无论使用前缀递增还是前缀递减操作符，变量的值都会在语句被求值之前改变）

​		这 4 个操作符可以作用于任何值，意思是不限于整数——字符串、布尔值、浮点值，甚至对象都可

以。递增和递减操作符遵循如下规则。

```
 对于字符串，如果是有效的数值形式，则转换为数值再应用改变。变量类型从字符串变成数值。
 对于字符串，如果不是有效的数值形式，则将变量的值设置为 NaN 。变量类型从字符串变成
数值。
 对于布尔值，如果是 false，则转换为 0 再应用改变。变量类型从布尔值变成数值。
 对于布尔值，如果是 true，则转换为 1 再应用改变。变量类型从布尔值变成数值。
 对于浮点值，加 1 或减 1。 
 如果是对象，则调用其（第 5 章会详细介绍的）valueOf()方法取得可以操作的值。对得到的
值应用上述规则。如果是 NaN，则调用 toString()并再次应用其他规则。变量类型从对象变成
数值。
```



###### （2）一元加和减

​		一元加由一个加号（+）表示，放在变量前头，对数值没有任何影响，一元加应用到非数值，则会执行与使用 Number()转型函数一样的类型转换。

​		一元减由一个减号（-）表示，放在变量前头，主要用于把数值变成负值，应用到非数值时，一元减会遵循与一元加同样的规则，先对它们进行转换，然后再取负值。



##### 2.位操作符

​		用于数值的底层操作，也就是操作内存中表示数据的比特（位）。ECMAScript中的所有数值都以 IEEE 754 64 位格式存储，但位操作并不直接应用到 64 位表示，而是先把值转换为32 位整数，再进行位操作，之后再把结果转换为 64 位。但这个转换也导致了一个奇特的副作用，即特殊值NaN 和Infinity在位操作中都会被当成 0 处理。		（对开发者而言，就好像只有 32 位整数一样，因为 64 位整数存储格式是不可见的。既然知道了这些，就只需要考虑 32 位整数即可）

​		有符号整数使用 32 位的前 31 位表示整数值。第 32 位表示数值的符号，如 0 表示正，1 表示负。这一位称为符号位（sign bit），它的值决定了数值其余部分的格式。

​		负值以一种称为二补数（或补码）的二进制编码存储。



​		一个数值的二补数通过如下 3 个步骤计算得到：

(1) 确定绝对值的二进制表示（如，对于18，先确定 18 的二进制表示）；

(2) 找到数值的一补数（或反码），换句话说，就是每个 0 都变成 1，每个 1 都变成 0；

(3) 给结果加 1。

**注意**

​		**默认情况下，ECMAScript 中的所有整数都表示为有符号数，不过，确实存在无符号整数。对无符号整数来说，第 32 位不表示符号，因为只有正值。无符号整数比有符号整数的范围更大，因为符号位被用来表示数值了。**



###### （1）按位符

​		~ 表示，作用是返回数值的一补数，按位非是 ECMAScript 中为数不多的几个二进制数学操作符之一。

```
let num1 = 25; // 二进制 00000000000000000000000000011001 
let num2 = ~num1; // 二进制 11111111111111111111111111100110 
console.log(num2); // -26

let num1 = 25; 
let num2 = -num1 - 1; 
console.log(num2); // "-26"
```

两者返回结果一样，但是位操作符的速度快得多。因为位操作符是在数值的底层表示上完成的。



###### （2）按位与

​		用  &  表示，按位与就是将两个数的每一个对齐，然后基于真值表中的规矩，对每一位执行相应的与操作。

```
第一个数值的位 		第二个数值的位 		结 果
	1 					1				 1 
	1					0				 0 
	0 					1				 0 
	0 					0				 0
```

```
let result = 25 & 3; 
console.log(result); // 1 
```

25 和 3 的按位与操作的结果是 1。为什么呢？看下面的二进制计算过程：

```
 25 = 0000 0000 0000 0000 0000 0000 0001 1001 
 3 = 0000 0000 0000 0000 0000 0000 0000 0011 
--------------------------------------------- 
AND = 0000 0000 0000 0000 0000 0000 0000 0001 

25 和 3 的二进制表示中，只有第 0 位上的两个数都是 1。于是结果数值的所有其他位都
会以 0 填充，因此结果就是 1。
```



###### （3）按位或

​		用  |  表示，同样有两个操作数。

```
第一个数值的位 		第二个数值的位 		结 果
	1 					1				  1 
	1 					0				  1 
	0 					1				  1 
	0 					0				  0
```

```
按位或操作在至少一位是 1 时返回 1，两位都是 0 时返回 0。
对 25 和 3 执行按位或，代码如下所示：
let result = 25 | 3; 
console.log(result); // 27 

可见 25 和 3 的按位或操作的结果是 27：
 25 = 0000 0000 0000 0000 0000 0000 0001 1001 
 3 = 0000 0000 0000 0000 0000 0000 0000 0011 
--------------------------------------------- 
 OR = 0000 0000 0000 0000 0000 0000 0001 1011 
 
在参与计算的两个数中，有 4 位都是 1，因此它们直接对应到结果上。二进制码 11011 等于 27。
```



###### （4）按位异或

​		用  ^  表示，有两个操作数。

```
第一个数的位 		第二个数的位 		结 果
	1 			  1 			 0 
	1 			  0 			 1 
	0			  1				 1 
	0 			  0				 0
```

按位异或与按位或的区别是：按位异或在一位上是 1 的时候返回 1（两位都是1或0，则返回 0）。

```
let result = 25 ^ 3; 
console.log(result); // 26 

可见，25 和 3 的按位异或操作结果为 26，如下所示：
 25 = 0000 0000 0000 0000 0000 0000 0001 1001 
 3 = 0000 0000 0000 0000 0000 0000 0000 0011 
--------------------------------------------- 
XOR = 0000 0000 0000 0000 0000 0000 0001 1010

二进制码 11010 等于 26。（注意，这比对同样两个值执行按位或操作得到的结果小 1。）
```



###### （5）左移

​		用  << 符号表示，按照指定的位数将数值的所有位向左移动。

```
let oldValue = 2; // 等于二进制 10 
let newValue = oldValue << 5; // 等于二进制 1000000，即十进制 64
```



###### （6）有符号右移

​		用  >> 符号表示，会将数值的所有 32 位都向右移，保留符号（正或负）。

```
let oldValue = 64; // 等于二进制 1000000 
let newValue = oldValue >> 5; // 等于二进制 10，即十进制 2
```



###### （7）无符号右移

​		用  >>> 表示，会将数值的所有 32 位都向右移，对于正数，无符号右移与有符号右移结果相同。

```
let oldValue = 64; // 等于二进制 1000000 
let newValue = oldValue >>> 5; // 等于二进制 10，即十进制 2
```

​		对于负数，有时候差距非常大，与有符号右移不同，无符号右移会给空位补 0。不管符号位是什么，正数，跟有符号右移相同。无符号右移操作符将负数的二进制表示成正数的二进制表示来处理。

```
let oldValue = -64; // 等于二进制 11111111111111111111111111000000 
let newValue = oldValue >>> 5; // 等于十进制 134217726

为64 的二进制表示是 1111111111111111111 1111111000000，无符号右移却将它当成正值，也就是 4 294 967 232。把这个值右移 5 位后，结果是00000111111111111111111111111110，即 134 217 726。
```



##### 3.布尔操作符

​		布尔操作符一共有 3 个：逻辑非、逻辑与和逻辑或。



###### （1）逻辑非

​		用 ！表示，可应用给ECMAScript 中的任何值。始终返回布尔值，无论应用的是什么类型。

逻辑非操作符会先将操作数转换为布尔值，然后再对其取反。

```
 如果操作数是对象，则返回 false。 
 如果操作数是空字符串，则返回 true。 
 如果操作数是非空字符串，则返回 false。 
 如果操作数是数值 0，则返回 true。 
 如果操作数是非 0 数值（包括 Infinity），则返回 false。  如果操作数是 null，则返回 true。 
 如果操作数是 NaN，则返回 true。  如果操作数是 undefined，则返回 true。
```

​		**逻辑非操作符可以用于把任意值转换为布尔值。同时使用两个感叹号时（ ！！），相当于调用转型函数 Boolean（）。无论是什么类型，第一个感叹号总会返回布尔值，第二个感叹号对该布尔值取反，给出变量正真对应的布尔值。**



###### （2）逻辑与

​		用  && 表示，应用到两个值。

```
let result = true && false;
```

​		逻辑与操作符遵循：

```
第一个操作数 			第二个操作数 			结 果
  true 				true 				true
  true 				false 				false
  false 			true 				false
  false 			false				false
```

​		逻辑与操作符可用于任何类型的操作数。如果操作数不是布尔值，逻辑与不一定会返回布尔值。



​		逻辑与遵循：

```
 如果第一个操作数是对象，则返回第二个操作数。
 如果第二个操作数是对象，则只有第一个操作数求值为 true 才会返回该对象。
 如果两个操作数都是对象，则返回第二个操作数。
 如果有一个操作数是 null，则返回 null。  如果有一个操作数是 NaN，则返回 NaN。  如果有一个操作数是 undefined，则返回 undefined。
```

​		逻辑与操作符是一种短路操作符，如果第一个操作数决定了结果，那么永远不会对第二个操作数求值。

第一个是false，那么结果不可能是 true。



###### （3）逻辑或

​		用 || 表示。

逻辑或操作符遵循：

```
第一个操作数 			第二个操作数 			结 果
  true 					true 			true
  true 					false 			true
  false 				true 			true
  false 				false 			false
```

​		与逻辑与类似，如果有一个操作数不是布尔值，那么逻辑或操作符也不一定返回布尔值。遵循：

```
 如果第一个操作数是对象，则返回第一个操作数。
 如果第一个操作数求值为 false，则返回第二个操作数。
 如果两个操作数都是对象，则返回第一个操作数。
 如果两个操作数都是 null，则返回 null。  如果两个操作数都是 NaN，则返回 NaN。  如果两个操作数都是 undefined，则返回 undefined。
```



##### 4.乘性操作符

​		ECMAScript 定义了 3 个乘性操作符：乘法、除法和取模。如果乘性操作符有不是数值的操作数，该操作数会在后台被使用 Number（）转型函数转换为数值（空字符串会被当成 0，true 会被当成 1 ）。



###### （1）乘法操作符

​		用 * 表示

​		乘法操作符处理特殊值的特殊行为：

```
 如果操作数都是数值，则执行常规的乘法运算，即两个正值相乘是正值，两个负值相乘也是正
值，正负符号不同的值相乘得到负值。如果 ECMAScript 不能表示乘积，则返回 Infinity 或
-Infinity。 
 如果有任一操作数是 NaN，则返回 NaN。
 如果是 Infinity 乘以 0，则返回 NaN。 
 如果是 Infinity 乘以非 0的有限数值，则根据第二个操作数的符号返回 Infinity 或-Infinity。 
 如果是 Infinity 乘以 Infinity，则返回 Infinity。 
 如果有不是数值的操作数，则先在后台用 Number()将其转换为数值，然后再应用上述规则。
```



###### （2）除法操作符

​		用 / 表示

​		与乘法操作符一样，针对特殊值有特殊行为：

```
 如果操作数都是数值，则执行常规的除法运算，即两个正值相除是正值，两个负值相除也是正
值，符号不同的值相除得到负值。如果ECMAScript不能表示商，则返回Infinity或-Infinity。 
 如果有任一操作数是 NaN，则返回 NaN。 
 如果是 Infinity 除以 Infinity，则返回 NaN。 
 如果是 0 除以 0，则返回 NaN。 
 如果是非 0 的有限值除以 0，则根据第一个操作数的符号返回 Infinity 或-Infinity。 
 如果是 Infinity 除以任何数值，则根据第二个操作数的符号返回 Infinity 或-Infinity。 
 如果有不是数值的操作数，则先在后台用 Number()函数将其转换为数值，然后再应用上述规则。
```



（3）取模操作符

​		取模（余数）操作符用  %  表示。

取模操作符的特使行为：

```
 如果操作数是数值，则执行常规除法运算，返回余数。
 如果被除数是无限值，除数是有限值，则返回 NaN。 
 如果被除数是有限值，除数是 0，则返回 NaN。 
 如果是 Infinity 除以 Infinity，则返回 NaN。 
 如果被除数是有限值，除数是无限值，则返回被除数。
 如果被除数是 0，除数不是 0，则返回 0。 
 如果有不是数值的操作数，则先在后台用 Number()函数将其转换为数值，然后再应用上述规则。
```



##### 5.指数操作符

​		ECMAScript 7 新增指数操作符，Math.pow（）有了自己的操作符 **。

```
console.log(Math.pow(3, 2); // 9 
console.log(3 ** 2); // 9 

console.log(Math.pow(16, 0.5); // 4 
console.log(16** 0.5); // 4
```

​		指数操作符有自己的指数赋值操作符 **=。

```
let squared = 3; 
squared **= 2; 
console.log(squared); // 9
```



##### 6.加性操作符

​		加性操作符，即加法和减法操作符。在 ECMAScript 中，这两个操作符拥有一些特殊的行为。与乘法操作符类似，加性操作符在后台会发生不同数据类型的转换，只不过这两个操作符转换规则不是那么直观。



###### （1）加法操作符

​		用  +  表示。

​		执行加法运算返回结果：

```
 如果有任一操作数是 NaN，则返回 NaN； 
 如果是 Infinity 加 Infinity，则返回 Infinity； 
 如果是-Infinity 加-Infinity，则返回-Infinity； 
 如果是 Infinity 加-Infinity，则返回 NaN； 
 如果是+0 加+0，则返回+0； 
 如果是-0 加+0，则返回+0； 
 如果是-0 加-0，则返回-0。

不过，如果有一个操作数是字符串，则要应用如下规则：
 如果两个操作数都是字符串，则将第二个字符串拼接到第一个字符串后面；
 如果只有一个操作数是字符串，则将另一个操作数转换为字符串，再将两个字符串拼接在一起。
```

​		任一操作数是对象、数值或布尔值，则调用它们的 toString()方法以获取字符串，然后再应用前面的关于字符串的规则。对于 undefined 和 null，则调用 String()函数，分别获取"undefined"和"null"。



###### （2）减法操作符

​		用  -  表示。

​		剑法操作符处理 ECMAScript 中不同类型之间的转换。

```
 如果两个操作数都是数值，则执行数学减法运算并返回结果。
 如果有任一操作数是 NaN，则返回 NaN。 
 如果是 Infinity 减 Infinity，则返回 NaN。 
 如果是-Infinity 减-Infinity，则返回 NaN。 
 如果是 Infinity 减-Infinity，则返回 Infinity。 
 如果是-Infinity 减 Infinity，则返回-Infinity。 
 如果是+0 减+0，则返回+0。  如果是+0 减-0，则返回-0。 
 如果是-0 减-0，则返回+0。  如果有任一操作数是字符串、布尔值、null 或 undefined，则先在后台使用 Number()将其转换为数值，然后再根据前面的规则执行数学运算。如果转换结果是 NaN，则减法计算的结果是NaN。 
 如果有任一操作数是对象，则调用其 valueOf()方法取得表示它的数值。如果该值是 NaN，则减法计算的结果是 NaN。如果对象没有 valueOf()方法，则调用其 toString()方法，然后再将得到的字符串转换为数值。
```



##### 7.关系操作符

​		关系操作符执行比较两个值的操作，包括小于（<）、大于（>）、小于等于（<=）和大于等于（>=）。

​		不同数据类型时也会发生类型转换和其他行为。

```
 如果操作数都是数值，则执行数值比较。
 如果操作数都是字符串，则逐个比较字符串中对应字符的编码。
 如果有任一操作数是数值，则将另一个操作数转换为数值，执行数值比较。
 如果有任一操作数是对象，则调用其 valueOf()方法，取得结果后再根据前面的规则执行比较。
如果没有 valueOf()操作符，则调用 toString()方法，取得结果后再根据前面的规则执行比较。
 如果有任一操作数是布尔值，则将其转换为数值再执行比较。
```

​		字符串"Brick"被认为小于字符串"alphabet"，因为字母 B 的编码是 66，字母 a 的编码是 97。要得到确实按字母顺序比较的结果，就必须把两者都转换为相同的大小写形式（全大写或全小写），然后再比较：

```
let result = "Brick".toLowerCase() < "alphabet".toLowerCase(); // false
```



##### 8.相等操作符

​		ECMAScript提供了两组操作符。第一组是等于和不等于，它们在比较之前执行转换。第二组是全等和不等，它们在比较之前不执行转换。



###### （1）等于和不等于

​		用  ==  表示，相等返回 true。不等于用  ！=  表示，两个操作数不想等，则返回  true。

这两个操作符会先进行类型转换（强制类型转换）再确定操作数是否相等。



转换操作数的类型时，相等和不相等操作符遵循：

```
 如果任一操作数是布尔值，则将其转换为数值再比较是否相等。false 转换为 0，true 转换
为 1。 
 如果一个操作数是字符串，另一个操作数是数值，则尝试将字符串转换为数值，再比较是否相等。
 如果一个操作数是对象，另一个操作数不是，则调用对象的 valueOf()方法取得其原始值，再根据前面的规则进行比较。在进行比较时，这两个操作符会遵循如下规则。
 null 和 undefined 相等。
 null 和 undefined 不能转换为其他类型的值再进行比较。
 如果有任一操作数是 NaN，则相等操作符返回 false，不相等操作符返回 true。记住：即使两个操作数都是 NaN，相等操作符也返回 false，因为按照规则，NaN 不等于 NaN。 
 如果两个操作数都是对象，则比较它们是不是同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回 true。否则，两者不相等。
```

​		**特殊情况及比较结果：**

```
表 达 式 							结 果
null == undefined 				  true
"NaN" == NaN 					  false
5 == NaN 						  false
NaN == NaN 						  false
NaN != NaN 						  true
false == 0 						  true
true == 1   					  true
true == 2   					  false
undefined == 0 					  false
null == 0 					  	  false
"5" == 5 						  true
```



###### （2）全等和不全等

​		比较相等时不转换操作数，用  ===  表示。只有两个操作数在不转换的前提下相等才返回  true  。

​		不全等用  ！==  表示，只有两个操作数在不转换的前提下不相等才返回  true 。



**注意**

​		由于相等和不相等操作符存在类型转换问题，因此推荐使用全等和不全等操作符。这样有助于在代码中保持数据类型的完整性。





##### 9.条件操作符

​		条件操作符是 ECMAScript  中用途最为广泛的操作符之一。

```
variable = boolean_expression ? true_value : false_value;
```



##### 10.赋值操作符

​		用 = 表示，将右边的值赋给左边的变量。

```
 乘后赋值（*=） 
 除后赋值（/=） 
 取模后赋值（%=） 
 加后赋值（+=） 
 减后赋值（-=） 
 左移后赋值（<<=） 
 右移后赋值（>>=） 
 无符号右移后赋值（>>>=）
```



##### 11.逗号操作符

​		逗号操作符可以用来在一条语句中执行多个操作。

​		在赋值时使用逗号操作符分隔值，最终会返回表达式中最后一个值：

```
let num = (5, 1, 4, 8, 0); // num 的值为 0 
```



### 3.6 语句

​		ECMA-262 描述了一些语句（也称为流控制语句），而 ECMAScript 中的大部分语法都体现在语句中。



##### 1.  if语句

​		if  语句是用的最频繁的语句之一。

```
if (condition) statement1 else statement2
```

​		这里的条件（condition）可以是任何表达式，并且求值结果不一定是布尔值。ECMAScript 会自动调用Boolean()函数将这个表达式的值转换为布尔值。如果条件求值为 true，则执行语句statement1；如果条件求值为 false，则执行语句 statement2。



##### 2.do-while 语句

​	do-while 语句是一种后测试循环语句，循环体中的代码执行后才会退出条件进行求值，循环体内的代码至少执行一次。

**注意**

​		**后测试循环经常用于循环体内代码在推出之前至少要执行一次。**



##### 3.while 语句

​		while 语句是一种先测试循环语句，即先检测退出条件，再执行循环体内的代码。



##### 4.for 语句

​		for  语句也是先测试语句，不过增加了进入循环之前的初始化代码，以及循环执行后要执行的表达式。

​		无法通过  while  循环实现的逻辑，同样也无法使用 for  循环实现。

初始化、条件表达式和循环后表达式都不是必需的。因此，下面这种写法可以创建一个无穷循环：

```
for (;;) { // 无穷循环
	 doSomething(); 
} 
```



##### 5.for-in  语句

​		for-in  语句是一种严格的迭代语句，用于枚举对象中的非符号键属性。

```
for (const propName in window) { 
 	document.write(propName); 
} 
```

​		这个例子使用 for-in 循环显示了 BOM 对象 window 的所有属性。每次执行循环，都会给变量propName 予一个 window 对象的属性作为值，直到 window 的所有属性都被枚举一遍。与 for 循环一样，这里控制语句中的 const 也不是必需的。但为了确保这个局部变量不被修改，推荐使用 const。

​		ECMAScript 中对象的属性是无序的，因此 for-in 语句不能保证返回对象属性的顺序。换句话说，所有可枚举的属性都会返回一次，但返回的顺序可能会因浏览器而异。如果 for-in 循环要迭代的变量是 null 或 undefined，则不执行循环体。



##### 6.for-of  语句

​		for-of 语句是一种严格的迭代语句，用于遍历可迭代对象的元素。

```
for (const el of [2,4,6,8]) { 
 	document.write(el); 
}
```

​		在这个例子中，我们使用 for-of 语句显示了一个包含 4 个元素的数组中的所有元素。循环会一直持续到将所有元素都迭代完。与 for 循环一样，这里控制语句中的 const 也不是必需的。但为了确保这个局部变量不被修改，推荐使用 const。

​		for-of 循环会按照可迭代对象的 next()方法产生值的顺序迭代元素。关于可迭代对象，在

第 7 章详细介绍。

​		如果尝试迭代的变量不支持迭代，则 for-of 语句会抛出错误。



**注意** 

​		**ES2018 对 for-of 语句进行了扩展，增加了 for-await-of 循环，以支持生成期**

**约（promise）的异步可迭代对象。**





##### 7.标签语句

​		标签语句用于给语句加标签。

```
label: statement
```

```
start: for (let i = 0; i < count; i++) { 
 console.log(i); 
}
```

​		**在这个例子中，start 是一个标签，可以在后面通过 break 或 continue 语句引用。标签语句的**

**典型应用场景是嵌套循环。**





##### 8.break 和  continue  语句

​		break  和  continue  语句为执行循环代码提供了更严谨的控制手段。break 语句立即退出循环，强制执行循环后的下一条语句。continue  语句立即退出本次循环，执行下个循环。



​		break 和 continue 都可以与标签语句一起使用，返回代码中特定的位置。这通常是在嵌套循环中。

```
let num = 0; 
outermost: 
for (let i = 0; i < 10; i++) { 
 	for (let j = 0; j < 10; j++) { 
 		if (i == 5 && j == 5) { 
 			break outermost; 
 		} 
		num++; 
 	} 
} 
console.log(num); // 55
```

​		在这个例子中，outermost 标签标识的是第一个 for 语句。正常情况下，每个循环执行 10 次，意味着 num++语句会执行 100 次，而循环结束时 console.log 的结果应该是 100。但是，break 语句带来了一个变数，即要退出到的标签。添加标签不仅让 break 退出（使用变量 j 的）内部循环，也会退出（使用变量 i 的）外部循环。当执行到 i 和 j 都等于 5 时，循环停止执行，此时 num 的值是 55。



continue：

```
let num = 0; 
outermost: 
for (let i = 0; i < 10; i++) { 
 	for (let j = 0; j < 10; j++) {
 		if (i == 5 && j == 5) { 
 			continue outermost; 
 		} 
 		num++; 
 	} 
} 
console.log(num); // 95
```

​		continue 语句会强制循环继续执行，但不是继续执行内部循环，而是继续执行外部循环。当 i 和 j 都等于 5 时，会执行 continue，跳到外部循环继续执行，从而导致内部循环少执行 5 次，结果 num 等于 95。



**注意**

​		**组合使用标签语句和  break、continue  能实现复杂的逻辑，但是容易出错，不要嵌套太深。（标签要使用描述性强的文本）。**



##### 9.with  语句

​		with  语句的用途是将代码作用域设置为特定的对象。

```
with(location) { 
 let qs = search.substring(1); 
 let hostName = hostname; 
 let url = href; 
}
```

​		with 语句用于连接 location 对象。这意味着在这个语句内部，每个变量首先会被认为是一个局部变量。如果没有找到该局部变量，则会搜索 location 对象，看它是否有一个同名的属性。如果有，则该变量会被求值为location 对象的属性。

​		严格模式不允许使用 with 语句，否则会抛出错误。



**注意**

​		with 语句影响性能且难于调试代码，不推荐在产品代码中使用 with 语句。





##### 10.switch 语句

​		switch 语句是与 if 语句紧密相关的一种流控制语句。

```
let num = 25; 
switch (true) { 
 	case num < 0: 
   		console.log("Less than 0."); 
 		break; 
 	case num >= 0 && num <= 10: 
 		console.log("Between 0 and 10."); 
 		break; 
 	case num > 10 && num <= 20: 
 		console.log("Between 10 and 20."); 
 		break; 
 	default: 
 		console.log("More than 20."); 
}

外部定义了变量 num，而传给 switch 语句的参数之所以是 true，就是因为每个条件的表达式都会返回布尔值。条件的表达式分别被求值，直到有表达式返回 true；否则，就会一直跳到 default 语句（这个例子正是如此）。
```

​		这里的每个 case（条件/分支）相当于：“如果表达式等于后面的值，则执行下面的语句。”break关键字会导致代码执行跳出 switch 语句。如果没有 break，则代码会继续匹配下一个条件。default关键字用于在任何条件都没有满足时指定默认执行的语句（相当于 else 语句）。

​		为避免不必要的条件判断，最好给每个条件后面都加上 break 语句。



**注意**

​		**switch  语句在比较每个条件的值时会使用全等操作符，因此不会强制转换数据类型（比如，字符串"10"不等于数值 10）。**





### 3.7函数

​		函数对于任何语言来说都是核心组件，因为他们可以封装语句，然后在任何地方、任何时间执行。ECMAScript 中的函数使用 function 关键字声明，后跟一组参数，然后是函数体。

ECMAScript 中的函数不需要指定是否返回值。

只要碰到 return 语句，函数就会立即停止执行并退出。因此，return 语句后面的代码不会被执行。

```
function sum(num1, num2) { 
 	return num1 + num2; 
 	console.log("Hello world"); // 不会执行
}
```



严格模式对函数的限制：

```
 函数不能以 eval 或 arguments 作为名称；
 函数的参数不能叫 eval 或 arguments； 
 两个命名参数不能拥有同一个名称。
如果违反上述规则，则会导致语法错误，代码也不会执行。
```



**注意**

​		**函数要么返回值，要么不返回值。在某个条件下返回值的函数会带来麻烦，特别是调试时。**





### 3.8小结

​		JS 的核心语言特性在 ECMA-262 中以伪类语言 ECMAScript 的形式来定义。ECMAScript 包含所有基本语法、操作符、数据类型和对象，能完成基本的计算任务，但没有提供获得输入和产生输出的机制。

​		ECMAScript  中的基本数据类型包括 Undefined、Null、Boolean、Number、String 和 Symbol 。

​		ECMAScript  不区分整数和浮点值，只有 Number 一种数值数据类型。

​		Object 是一种复杂数据类型，是这门语言中所有对象的基类。

​		严格模式为某些容易出错的部分施加了限制。

​		ECMAScript  提供了很多基本操作符，包括数学操作符、布尔操作符、关系操作符、相等操作符和赋值操作符等。

​		语言中的流控制语句大多是从其它语言借鉴而来的，如 if 语句、for 语句和 switch 语句等。

​		ECMAScript  中的函数和其它语言中的函数不一样。

​		不需要指定函数的返回值，函数可以在任意时候返回任何值。

​		不指定返回值的函数会返回特殊值 underfined。







## 第4章

​		JavaScript 变量是松散类型的，而且变量不过就是特定时间点一个特定值的名称而已。由于没有规则定义变量必须包含什么数据类型，变量的值和数据类型在脚本生命期内可以改变。



##### 1.原始值与引用值

​		ECMAScript 变量包含两种不同类型的数据：原始值和引用值。原始值就是最简单的数据，引用值是由多个值构成的对象。把一个值赋给变量时，JavaScript 引擎必须确定这个值是原始值还是引用值。保存的原始值的变量是按值（by  value）访问的。

​		引用值是保存在内存中的对象。JavaScript 不允许直接访问内存位置，不能直接操作对象所在的内存空间。保存引用值的变量是按引用（by  reference ）访问的。

**注意** 

​		**在很多语言中，字符串是使用对象表示的，因此被认为是引用类型。ECMAScript**

**打破了这个惯例。**



###### （1）动态属性

​		引用值可以随时添加、修改和删除其属性和方法。

```
let person = new Object(); 
person.name = "Nicholas"; 
console.log(person.name); // "Nicholas"
```

​		原始值不能有属性，尽管尝试给原始值添加属性不会报错。

只有引用值可以动态添加后面可以使用的属性。

```
let name = "Nicholas"; 
name.age = 27; 
console.log(name.age); // undefined
```



###### （2）复制值

​		原始值和引用值的复制也有所不同。原始值会被复制到新变量的位置。

引用值从一个变量赋给另一个变量时，存储在变量中的值也会被复制到新变量所在的位置，这里复制的是一个指针，它指向存储在堆内存中的对象。



###### （3）传递参数

​		ECMAScript 中所有函数的参数都是按值传递的。

​		在按值传递参数时，值会被复制到一个局部变量（即一个命名参数，或者用 ECMAScript 的话说，就是 arguments 对象中的一个槽位）。在按引用传递参数时，值在内存中的位置会被保存在一个局部变量，这意味着对本地变量的修改会反映到函数外部。



```
function addTen(num) { 
 	num += 10; 
 	return num; 
} 
let count = 20;
let result = addTen(count); 
console.log(count); // 20，没有变化
console.log(result); // 30
```

​		函数 addTen()有一个参数 num，它其实是一个局部变量。在调用时，变量 count 作为参数传入。count 的值是 20，这个值被复制到参数 num 以便在 addTen()内部使用。在函数内部，参数 num的值被加上了 10，但这不会影响函数外部的原始变量 count。参数 num 和变量 count 互不干扰，它们只不过碰巧保存了一样的值。如果 num 是按引用传递的，那么 count 的值也会被修改为 30。



```
function setName(obj) { 
 obj.name = "Nicholas"; 
 obj = new Object(); 
 obj.name = "Greg"; 
} 
let person = new Object(); 
setName(person); 
console.log(person.name); // "Nicholas"
```

​		setName()中多了两行代码，将 obj 重新定义为一个有着不同 name的新对象。当 person 传入 setName()时，其 name 属性被设置为"Nicholas"。然后变量 obj 被设置为一个新对象且 name 属性被设置为"Greg"。如果 person 是按引用传递的，那么 person 应该自动将指针改为指向 name 为"Greg"的对象。可是，当我们再次访问 person.name 时，它的值是"Nicholas"，这表明函数中参数的值改变之后，原始的引用仍然没变。当 obj 在函数内部被重写时，它变成了一个指向本地对象的指针。而那个本地对象在函数执行结束时就被销毁了。



**注意** 

​		**ECMAScript 中函数的参数就是局部变量。**



###### （4）确定类型

​		是判断一个变量是否为字符串、数值、布尔值或 undefined 的最好方式。是对象或 null ，那么  typeof  返回 “ object ” 。

​		typeof  对原始值有用，但是对引用值的用处不大。为此，ECMAScript  提供了  instanceof  操作符。

```
result = variable instanceof constructor
```



```
console.log(person instanceof Object); // 变量 person 是 Object 吗？
console.log(colors instanceof Array); // 变量 colors 是 Array 吗？
console.log(pattern instanceof RegExp); // 变量 pattern 是 RegExp 吗？
```

​		所有引用值都是 Object 的实例，因此通过 instanceof 操作符检测任何引用值和Object 构造函数都会返回 true。类似地，如果用 instanceof 检测原始值，则始终会返回 false，因为原始值不是对象。

**注意**

​		**typeof  操作符在用于检测函数时也会返回 “ function ”。ECMA-262 规定，任何实现内部[[Call]]方法的对象都应该在 typeof 检测时返回"function"。因为上述浏览器中的正则表达式实现了这个方法，所以 typeof 对正则表达式也返回"function"。在 IE 和 Firefox 中，typeof 对正则表达式返回"object"。**



##### 2.执行上下文与作用域

​		执行上下文的概念在 JavaScript 中颇为重要。每个上下文都有一个关联的变量对象，而这个上下文中定义的所有变量和函数都存在于这个对象上。

​		全局上下文时最外层的上下文。根据 ECMAScript 实现的宿主环境，表示全局上下文的对象可能不一样。在浏览器中，全局上下文就是我们常说的 window 对象，因此所有通过 var 定义的全局变量和函数都会成为 window 对象的属性和方法。使用 let 和 const 的顶级声明不会定义在全局上下文中，但在作用域链解析上效果是一样的。

上下文在其所有代码都执行完毕后会被销毁，包括定义在它上面的所有变量和函数（全局上下文在应用程序退出前才会被销毁，比如关闭网页或退出浏览器）。

​		每个函数调用都有自己的上下文。当代码执行流进入函数时，函数的上下文被推到一个上下文栈上。在函数执行完之后，上下文栈会弹出该函数上下文，将控制权返还给之前的执行上下文。ECMAScript程序的执行流就是通过这个上下文栈进行控制的。

​		上下文中的代码在执行的时候，会创建变量对象的一个作用域链，作用域链决定了各级上下文中的代码在访问变量和函数时的顺序。这个作用域链决定了各级上下文中的代码在访问变量和函数时的顺序。

​		如果上下文时函数，则其活动对象用作变量对象。

活动对象最初只有一个定义变量：arguments 。（全局上下文没有这个变量。），全局上下文的变量对象始终时作用域链的最后一个变量对象。



```
var color = "blue"; 
function changeColor() { 
 	let anotherColor = "red"; 
 	function swapColors() { 
 		let tempColor = anotherColor; 
 		anotherColor = color; 
 		color = tempColor; 
 		// 这里可以访问 color、anotherColor 和 tempColor 
	} 
 	// 这里可以访问 color 和 anotherColor，但访问不到 tempColor 
 	swapColors(); 
} 
// 这里只能访问 color 
changeColor();
```

​		以上代码涉及 3 个上下文：全局上下文、changeColor()的局部上下文和 swapColors()的局部上下文。全局上下文中有一个变量 color 和一个函数 changeColor()。changeColor()的局部上下文中有一个变量 anotherColor 和一个函数 swapColors()，但在这里可以访问全局上下文中的变量 color。swapColors()的局部上下文中有一个变量 tempColor，只能在这个上下文中访问到。全局上下文和changeColor()的局部上下文都无法访问到 tempColor。而在 swapColors()中则可以访问另外两个上下文中的变量，因为它们都是父上下文。



**注意**

​		**函数参数被认为是当前上下文中的变量，因此也跟上下文中的其它变量遵循相同的访问规则。**



###### （1）作用域链增强

​		执行上下文主要有全局上下文和函数上下文两种（ eval（）调用内部存在第三种上下文），有其他方式来增强作用域链。某些语句会导致在作用域链前端临时添加一个上下文，这个上下文在代码执行后会被删除。通常在两种情况下会出现这个现象。

```
try/catch 语句的catch
with 语句
```

​		两种情况都会在作用域链前端添加一个变量对象。with 语句，会向作用域链前端添加指定的对象；catch 语句，会创建一个新的变量对象，这个变量对象会包含要抛出的错误对象的声明。



```
function buildUrl() { 
 	let qs = "?debug=true"; 
 	with(location){ 
 		let url = href + qs; 
 	} 
 	return url; 
}
```

​		with 语句将 location 对象作为上下文，因此 location 会被添加到作用域链前端。with 语句中的代码引用变量 href 时，实际上引用的是  location.href ，就是自己变量对象的属性。这里使用  let  声明的变量  url ，所以在with 块之外没有意义。



**注意**

​		**IE 的实现在 IE8 之前是有偏差的，即他们会将 catch 语句中捕捉的错误添加到执行上下文的变量对象上，而不是 catch 语句的变量对象上，导致在 catch 块外部都可以访问到错误。IE9 纠正了这个问题。**



###### （2）变量声明

​		ES6 之后，JavaScript 的变量声明经历了翻天覆地的变化。



**1 使用 var 的函数作用域声明**

​		使用 var 声明变量时，会被自动添加到最接近的上下文。在函数中最接近的上下文时函数的局部上下文，with 语句中，最接近的上下文也是函数上下文。变量未经声明就被初始化，他会自动被添加到全局上下文。

```
function add(num1, num2) { 
 	var sum = num1 + num2; 
 	return sum; 
} 
let result = add(10, 20); // 30 
console.log(sum); // 报错：sum 在这里不是有效变量
```

​		变量 sum 在函数外部是访问不到的。如果省略上面例子中的关键字 var，那么 sum 在 add()被调用

之后就变成可以访问的了。



**注意**

​		**未经声明而初始化变量是 JavaScript 编程中一个非常常见的错位u，会导致很多问题。在初始化变量之前一定要先声明变量。在严格模式下，未经声明就初始化变量会报错。**



var 声明会被拿到函数或全局作用域的顶部，位于作用域中所有代码之前。这个现象叫作“提升”。提升让同一作用域中的代码不必考虑变量是否已经声明就可以直接使用。



​		通过在声明之前打印变量，可以验证变量会被提升。声明的提升意味着会输出 undefined 而不是

Reference Error。

```
console.log(name); // undefined 
var name = 'Jake'; 
function() { 
 	console.log(name); // undefined 
 	var name = 'Jake'; 
}
```



**2  使用 let 的块级作用域声明**

​		ES6 新增的 let 关键字跟 var 很相似，但它的作用域是块级的，这也是 JavaScript 中的新概念。

​		let 与 var 的另一个不同之处是在同一作用域内不能声明两次。重复的 var 声明会被忽略，而重

复的 let 声明会抛出 SyntaxError。

​		let 的行为非常适合在循环中声明迭代变量。使用 var 声明的迭代变量会泄漏到循环外部，这种情

况应该避免。

​		严格来讲，let 在 JavaScript 运行时中也会被提升，但由于“暂时性死区”（temporal dead zone）的

缘故，实际上不能在声明之前使用 let 变量。因此，从写 JavaScript 代码的角度说，let 的提升跟 var

是不一样的。



**3  使用 const 的常量声明**

​		ES6 同时还增加了 const 关键字。使用 const 声明的变量必须同时初始化为某个值。一经声明，在其生命周期的任何时候都不能再重新赋予新值。const 除了要遵循以上规则，其他方面与 let 声明是一样的。

​		赋值为对象的 const 变量不能再被重新赋值为其他引用值，但对象的键则不受限制。

​		如果想让整个对象都不能修改，可以使用 Object.freeze()，这样再给属性赋值时虽然不会报错，

但会静默失败。

```
const o3 = Object.freeze({}); 
o3.name = 'Jake'; 
console.log(o3.name); // undefined
```

​		const 声明变量的值是单一类型且不可修改，JavaScript 运行时编译器可以将所有实例都替换成实际的值，而不会通过查询表进行变量查询。谷歌的 V8 引擎就执行这种优化。



**注意** 

​		**开发实践表明，如果开发流程并不会因此而受很大影响，就应该尽可能地多使用**

**const 声明，除非确实需要一个将来会重新赋值的变量。这样可以从根本上保证提前发现**

**重新赋值导致的 bug。**



**4  标识符查找**

​		特定上下文中为读取或写入而引用一个标识符时，必须通过搜索确定这个表示符表示声明。搜索开始于作用域链前端，以给定的名称搜索对应的标识符。如果在局部上下文中找到该标识符，则搜索停止，变量确定；如果没有找到变量名，则继续沿作用域链搜索。（注意，作用域链中的对象也有一个原型链，因此搜索可能涉及每个对象的原型链。）这个过程一直持续到搜索至全局上下文的变量对象。如果仍然没有找到标识符，则说明其未声明。

​		如果局部上下文中有一个同名的标识符，那就不能在该上下文中引用父上下文中的同名标识符。使用块级作用域声明并不会改变搜索流程，但可以给词法层级添加额外的层次。

```
var color = 'blue'; 
function getColor() { 
 	let color = 'red'; 
 	{ 
 		let color = 'green'; 
 		return color; 
 	} 
} 
console.log(getColor()); // 'green'
```

​		变量已找到，搜索随即停止，所以就使用这个局部变量。这意味着函数会返回'green'。在局部变量 color 声明之后的任何代码都无法访问全局变量color，除非使用完全限定的写法 window.color。



**注意**

​		**标识符查找并非没有代价。访问局部变量比访问全局变量要快，因为不用切换作用域。JavaScript 引擎在优化标识符查找上做了很多工作，将来这个差异可能就微不足道了。**



##### 3.垃圾回收

​		JavaScript 是使用垃圾回收的语言，执行环境负责在代码执行时管理内存。通过自动内存管理实现内存分配和闲置资源回收。基本思路：确定哪个变量不会再使用，然后释放它占有的内存。这个过程是周期性的，即垃圾回收程序每隔一定时间（或者说在代码执行过程中某个预定的收集时间）就会自动运行。

​		垃圾回收过程是一个近似且不完美的方案，因为某块内存是否还有用，属于 “ 不可判断的 ” 问题，意味着靠算法是解决不了的。

​		在浏览器的发展史上，用到过两种主要的标记策略：标记清理和引用计数。



###### （1）标记清理

​		JavaScript 最常用的垃圾回收策略是标记清理。当变量进入上下文，比如在函数内部声明一个变量时，这个变量会被加上存在于上下文中的标记。逻辑上讲，在上下文中的变量永远不应该释放他们的内存，因为只要上下文中的代码在运行，就可能用到。。变量离开上下文时，也会被加上裂开上下文的标记。

​		垃圾回收程序运行的时候，会标记内存中存储的所有变量（标记方法有很多种）。

​		它会将所有在上下文中的变量，以及被在上下文中的变量引用的变量的标记去掉。在此之后再被加上标记的变量就是待删除的了，原因是任何在上下文中的变量都访问不到它们了。随后垃圾回收程序做一次内存清理，销毁带标记的所有值并收回它们的内存。



###### （2）引用计数

​		另一种没那么常用的垃圾回收策略是引用计数。思路是：对每个值都记录它被引用的次数。声明变量并给它赋一个引用值时，这个值的引用数为 1。如果同一个值又被赋给另一个变量，那么引用数加 1。

​		引用计数最早由 Netscape Navigator 3.0 采用，但很快就遇到了严重的问题：循环引用。所谓循环引用，就是对象 A 有一个指针指向对象 B，而对象 B 也引用了对象 A。

```
function problem() { 
 	let objectA = new Object(); 
 	let objectB = new Object(); 
	objectA.someOtherObject = objectB; 
 	objectB.anotherObject = objectA; 
}
```

​		objectA 和 objectB 通过各自的属性相互引用，意味着它们的引用数都是 2。在标记清理策略下，这不是问题，因为在函数结束后，这两个对象都不在作用域中。在引用计数下，objectA 和 objectB 在函数结束后还会存在，因为它们的引用数永远不会变成 0。



​		

​		只要涉及 COM 对象，就无法避开循环引用问题。

```
let element = document.getElementById("some_element"); 
let myObject = new Object(); 
myObject.element = element; 
element.someObject = myObject;

myObject.element = null; 
element.someObject = null;
```

​		在一个 DOM 对象（element）和一个原生 JavaScript 对象（myObject）之间制造了循环引用。myObject 变量有一个名为 element 的属性指向 DOM 对象 element，而 element 对象有一个someObject 属性指回 myObject 对象。由于存在循环引用，因此 DOM 元素的内存永远不会被回收，即使它已经被从页面上删除了也是如此。

​		为避免类似的循环引用问题，应该在确保不使用的情况下切断原生 JavaScript 对象与 DOM 元素之间的连接。把变量设置为 null 实际上会切断变量与其之前引用值之间的关系。当下次垃圾回收程序运行时，这些值就会被删除，内存也会被回收。



###### （3）性能

​		垃圾回收程序会周期性运行，如果内存中分配了很多变量，可能造成性能损失，因此垃圾回收的时间调度很重要。开发者不知道什么时候运行时会收集垃圾，因此最好的办法是在写代码时就要做到：无论什么时候开始收集垃圾，都能让它尽快结束工作。

​		现代垃圾回收程序会基于对 JavaScript 运行时环境的探测来决定何时运行。探测机制因引擎而异，但基本上都是根据已分配对象的大小和数量来判断的。

​		由于调度垃圾回收程序方面的问题会导致性能下降，IE 曾饱受诟病。分配那么多变量的脚本，很可能在其整个生命周期内始终需要那么多变量，结果就会导致垃圾回收程序过于频繁地运行。



**注意**

​		**在某些浏览器中是有可能主动触发垃圾回收的。（但是不推荐）在 IE 中，window.CollectGarbage（）方法会立即触发垃圾回收。在 Opera 7 及更高的版本中，调用 window.opera.collect（）也会启动垃圾回收程序。**



###### （4）内存管理

​		使用垃圾回收的编程环境中，开发者通常无须关心内存管理。JavaScript 运行在一个内存管理于垃圾回收都很特殊的环境。分配给浏览器的内存通常比分配给桌面软件的要少很多，分配给移动浏览器的就更少了。这更多出于安全考虑而不是别的，就是为了避免运行大量 JavaScript 的网页耗尽系统内存而导致操作系统崩溃。这个内存限制不仅影响变量分配，也影响调用栈以及能够同时在一个线程中执行的语句数量。

​		将内存占用量保持在一个较小的值可以让页面性能更好。优化内存占用的最佳手段就是保证在执行代码时只保存必要的数据。如果数据不再必要，那么把它设置为 null，从而释放其引用。这也可以叫作解除引用。这个建议最适合全局变量和全局对象的属性。

```
function createPerson(name){ 
 	let localPerson = new Object(); 
 	localPerson.name = name; 
 	return localPerson; 
} 
let globalPerson = createPerson("Nicholas"); 
// 解除 globalPerson 对值的引用
globalPerson = null;
```

​		变量 globalPerson 保存着 createPerson()函数调用返回的值。在 createPerson()内部，localPerson 创建了一个对象并给它添加了一个 name 属性。然后，localPerson 作为函数值被返回，并被赋值给 globalPerson。localPerson 在 createPerson()执行完成超出上下文后会自动被解除引用，不需要显式处理。

​		globalPerson 是一个全局变量，应该在不再需要时手动解除其引用，最后一行就是这么做的。

​		**解除对一个值的引用并不会自动导致相关内存被回收。解除引用的关键在于确保相关的值已经不在上下文里了，因此它在下次垃圾回收时会被回收。**



**1  通过 const 和 let 声明提升性能**

​		ES6 增加这两个关键字不仅有助于改善代码风格，而且同样有助于改进垃圾回收的过程。因为 const 和 let 都以块（而非函数）为作用域，所以相比于使用 var，使用这两个新关键字可能会更早地让垃圾回收程序介入，尽早回收应该回收的内存。



**2  隐藏类和删除类**

​		JavaScript 所在的运行环境，有时候需要根据浏览器使用的 JavaScript 引擎来采取不同的性能优化策略。截至 2017 年，Chrome 是最流行的浏览器，使用 V8 JavaScript 引擎。V8 在将解释后的 JavaScript代码编译为实际的机器码时会利用“隐藏类”。

​		运行期间， V8 会将创建的对象与隐藏类联系起来，以跟踪他们的属性特征。能够共享相同隐藏类的对象性能会更好，V8 会针对这种情况进行优化，但不一定总能够做到。

```
function Article() { 
 this.title = 'Inauguration Ceremony Features Kazoo Band'; 
} 
let a1 = new Article(); 
let a2 = new Article();
```

​		V8 会在后台配置，让这两个类实例共享相同的隐藏类，因为这两个实例共享同一个构造函数和原

型。假设之后又添加了下面这行代码：

```
a2.author = 'Jake'; 
```

​		

​		使用 delete 关键字会导致生成相同的隐藏类片段。

​		在代码结束后，即使两个实例使用了同一个构造函数，它们也不再共享一个隐藏类。动态删除属性与动态添加属性导致的后果一样。最佳实践是把不想要的属性设置为 null。这样可以保持隐藏类不变和继续共享，同时也能达到删除引用值供垃圾回收程序回收的效果。

```
function Article() { 
 	this.title = 'Inauguration Ceremony Features Kazoo Band'; 
 	this.author = 'Jake'; 
} 
let a1 = new Article(); 
let a2 = new Article(); 
delete a1.author;

//a1.author = null;
```



**3  内存泄漏**

​		写的不好的 JavaScript 可能出现难以察觉且有害的内存泄露问题。在内存有限的设备上，或者在函数会被调用很多次的情况下，内存泄漏可能是个大问题。JavaScript 中的内存泄漏大部分是由不合理的引用导致的。

​		意外声明全局变量是最常见但也最容易修复的内存泄漏问题。

```
function setName() {
	name = 'Jake';
}
```

​		这时，解释器会把变量 name 当作  window 的属性来创建（相当于 window.name = ' Jake '）。在 window 对象上创建的属性，只要 window 本身不被清理就不会消失。只要在变量声明前头加上 var、let 或 const  关键字即可。



```
let name = 'Jake'; 
setInterval(() => { 
 	console.log(name); 
}, 100);
```

​		只要定时器一直运行，回调函数中引用的 name 就会一直占用内存。垃圾回收程序知道这一点，所以就不会清理外部变量。



​		使用 JavaScript 闭包很容易在不知不觉间造成内存泄漏。

​		调用  outer（）会导致分配给 name 的内存被泄漏。代码执行后创建了一个内部闭包，只要返回的函数存在就不能清理 name，因为闭包一直在引用它。

```
let outer = function() { 
 	let name = 'Jake'; 
 	return function() { 
 		return name; 
 	}; 
};
```



**4  静态分配与对象池**

​		为了提升 JavaScript 性能，最后要考虑的一点往往就是压榨浏览器了。理论上，如果能够合理使用分配的内存，同时避免多余的垃圾回收，那就可以保住因释放内存而损失的性能。



​		浏览器决定何时运行垃圾回收程序的一个标准就是对象更替的速度。调用这个函数时，会在堆上创建一个新对象，然后修改它，最后再把它返回给调用者。如果这个矢量对象的生命周期很短，那么它会很快失去所有对它的引用，成为可以被回收的值。假如这个矢量加法函数频繁被调用，那么垃圾回收调度程序会发现这里对象更替的速度很快，从而会更频繁地安排垃圾回收。

```
function addVector(a, b) { 
 	let resultant = new Vector(); 
 	resultant.x = a.x + b.x; 
 	resultant.y = a.y + b.y; 
 	return resultant; 
}

该问题的解决方案是不要动态创建矢量对象，比如可以修改上面的函数，让它使用一个已有的矢量对象
这需要在其他地方实例化矢量参数 resultant，但这个函数的行为没有变。
function addVector(a, b, resultant) { 
 	resultant.x = a.x + b.x; 
 	resultant.y = a.y + b.y; 
 	return resultant; 
}
```



​		一个策略是使用对象池。在初始化的某一时刻，可以创建一个对象池，用来管理一组可回收的对象。应用程序可以向这个对象池请求一个对象、设置其属性、使用它，然后在操作完成后再把它还给对象池。由于没发生对象初始化，垃圾回收探测就不会发现有对象更替，因此垃圾回收程序就不会那么频繁地运行。

​		如果对象池只按需分配矢量（在对象不存在时创建新的，在对象存在时则复用存在的），那么这个实现本质上是一种贪婪算法，有单调增长但为静态的内存。

```
// vectorPool 是已有的对象池 
let v1 = vectorPool.allocate(); 
let v2 = vectorPool.allocate(); 
let v3 = vectorPool.allocate(); 

v1.x = 10; 
v1.y = 5; 
v2.x = -3; 
v2.y = -6; 

addVector(v1, v2, v3); 

console.log([v3.x, v3.y]); // [7, -1] 

vectorPool.free(v1); 
vectorPool.free(v2); 
vectorPool.free(v3); 

// 如果对象有属性引用了其他对象
// 则这里也需要把这些属性设置为 null 
v1 = null; 
v2 = null; 
v3 = null;
```

不过，使用数组来实现，必须留意不要招致额外的垃圾回收。

```
let vectorList = new Array(100); 
let vector = new Vector(); 
vectorList.push(vector);
```



**注意**

​		**静态分配是优化的一种极端形式。如果你的应用程序被垃圾回收严重地拖了后腿，**

**可以利用它提升性能。但这种情况并不多见。大多数情况下，这都属于过早优化，因此不**

**用考虑。**





##### 4.小结

​		JavaScript 变量可以保存两种类型的值：原始值和引用值。原始值可能是以下 6 种原始数据类型之一：Undefined、Null、Boolean、Number、String 和 Symbol。

原始值和引用值的特点：

​		1.原始值大小固定，因此保存在栈内存上。

​		2.从一个变量到另一个变量复制原始值会创建该值的第二个副本。

​		3.引用值是对象，存储在堆内存上。

​		4.包含引用值的变量实际上只包含指向相应对象的一个指针，而不是对象本身。

​		5.从一个变量到另一个变量复制引用值只会复制指针，因此结果是两个变量都指向同一个对象。

​		6.typeof 操作符可以确定值的原始类型，而 instanceof 操作符用于确保值的引用类型。





任何变量（不管包含的是原始值还是引用值）都存在于某个执行上下文中（也称为作用域）。这个上下文（作用域）决定了变量的生命周期，以及它们可以访问代码的哪些部分。

​		1.执行上下文分全局上下文、函数上下文和块级上下文。

​		2.代码执行流每进入一个新上下文，都会创建一个作用域链，用于搜索变量和函数。

​		3.函数或块的局部上下文不仅可以访问自己作用域内的变量，而且也可以访问任何包含上下文乃至全局上下文中的变量。

​		4.全局上下文只能访问全局上下文中的变量和函数，不能直接访问局部上下文中的任何数据。

​		5.变量的执行上下文用于确定什么时候释放内存。





JavaScript 是使用垃圾回收的编程语言，开发者不需要操心内存分配和回收。

​		1.离开作用域的值会被自动标记为可回收，然后在垃圾回收期间被删除。

​		2.主流的垃圾回收算法是标记清理，即先给当前不使用的值加上标记，再回来回收它们的内存。

​		3.引用计数是另一种垃圾回收策略，需要记录值被引用了多少次。JavaScript 引擎不再使用这种算法，但某些旧版本的 IE 仍然会受这种算法的影响，原因是 JavaScript 会访问非原生 JavaScript 对象（如 DOM 元素）。

​		4.引用计数在代码中存在循环引用时会出现问题。

​		5.解除变量的引用不仅可以消除循环引用，而且对垃圾回收也有帮助。为促进内存回收，全局对

​			象、全局对象的属性和循环引用都应该在不需要时解除引用。







## 第5章  基本引用类型

​		引用类型（或对象）是某个特定引用类型的实例。在ECMAScript 中，引用类型是把数据和功能组织到一起的结构，经常被错误的称作 “ 类 ”。虽然从技术上讲 JavaScript 是一门面向对象语言，但ECMAScript 缺少传统的面向对象编程语言所具备的某些基本结构，包括类和接口。引用类型有时候也被称为对象定义，因为它们描述了自己的对象应有的属性和方法。

​		对象被认为是某个特定引用类型的实例。新对象通过使用 new 操作符后跟一个构造函数来创建。



#### 1.Date

​		Date 类型将日期保存为自协调世界时（UTC，Universal Time Coordinated）时间 1970 年 1 月 1 日午夜（零时）至今所经过的毫秒数。

​		ECMAScript 提供了两个辅助方法：Date.parse（）和 Date.UTC（）。

​		Date.parse（）方法接收一个表示日期的字符串参数，尝试将这个字符串转换为表示日期的毫秒数。

```
“月/日/年”，如"5/23/2019"； 
“月名 日, 年”，如"May 23, 2019"； 
“周几 月名 日 年 时:分:秒 时区”，如"Tue May 23 2019 00:00:00 GMT-0700"； 
ISO 8601 扩展格式“YYYY-MM-DDTHH:mm:ss.sssZ”，如 2019-05-23T00:00:00（只适用于
兼容 ES5 的实现）。
```

​		如果传给 Date.parse()的字符串并不表示日期，则该方法会返回 NaN。

这两行代码得到的日期对象相同。

```
let someDate = new Date(Date.parse("May 23, 2019"));

let someDate = new Date("May 23, 2019");
```



**注意**

​		**不同的浏览器对 Date 类型的实现有很多问题。比如，很多浏览器会选择用当前日期替代越界的日期，因此有些浏览器会将"January 32, 2019"解释为"February 1, 2019"。Opera 则会插入当前月的当前日，返回"January 当前日, 2019"。就是说，如果是在 9 月 21 日运行代码，会返回"January 21, 2019"。**





​		Date.UTC（）方法也是返回日期的毫秒表示，传给 Date.UTC()的参数是年、零起点月数（1 月是 0，2 月是 1，以此类推）、日（1~31）、时（0~23）、分、秒和毫秒。这些参数中，只有前两个（年和月）是必需的。如果不提供日，那么默认为 1 日。其他参数的默认值都是 0。

​		第一个日期是 2000 年 1 月 1 日零点（GMT），2000 代表年，0 代表月（1 月）。因为没有其他参数（日取 1，其他取 0），所以结果就是该月第 1 天零点。第二个日期表示 2005年 5 月 5 日下午 5 点 55 分 55 （GMT）。虽然日期里面涉及的都是 5，但月数必须用 4，因为月数是零起点的。小时也必须是 17，因为这里采用的是 24 小时制，即取值范围是 0~23。

```
// GMT 时间 2000 年 1 月 1 日零点
let y2k = new Date(Date.UTC(2000, 0)); 

// GMT 时间 2005 年 5 月 5 日下午 5 点 55 分 55 秒
let allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));

还可以这样写：
两个日期是（由于系统设置决定的）本地时区的日期。
// 本地时间 2000 年 1 月 1 日零点
let y2k = new Date(2000, 0); 
// 本地时间 2005 年 5 月 5 日下午 5 点 55 分 55 秒
let allFives = new Date(2005, 4, 5, 17, 55, 55);
```



​		与 Date.parse()一样，Date.UTC()也会被 Date 构造函数隐式调用，但有一个区别：这种情况下创建的是本地日期，不是 GMT 日期。不过 Date 构造函数跟 Date.UTC()接收的参数是一样的。



​		ECMAScript 还提供了 Date.now()方法，返回表示方法执行时日期和时间的毫秒数。

```
// 起始时间
let start = Date.now(); 
// 调用函数
doSomething(); 
// 结束时间
let stop = Date.now(), 
result = stop - start;
```



##### 1 继承的方法

​		Date 类型重写了 toLocaleString()、toString()和 valueOf()方法。但与其他类型不同，重写后这些方法的返回值不一样。

 **toLocaleString()**：Date 类型的 toLocaleString()方法返回与浏览器运行的本地环境一致的日期和时间。这通常意味着格式中包含针对时间的 AM（上午）或 PM（下午），但不包含时区信息（具体格式可能因浏览器而不同）

**toString()**：toString()方法通常返回带时区信息的日期和时间，而时间也是以 24 小时制（0~23）表示的。

**valueOf()**：Date 类型的 valueOf() 方法根本就不返回字符串，这个方法被重写后返回的是日期的毫秒表示。所以

操作符（如小于号和大于号）可以直接使用它返回的值。

```
let date1 = new Date(2019, 0, 1); // 2019 年 1 月 1 日
let date2 = new Date(2019, 1, 1); // 2019 年 2 月 1 日

console.log(date1 < date2); // true 
console.log(date1 > date2); // false
```



地区为"en-US"的 PST，即 Pacific Standard Time，太平洋标准时间：

```
toLocaleString() - 2/1/2019 12:00:00 AM 
toString() - Thu Feb 1 2019 00:00:00 GMT-0800 (Pacific Standard Time)
```

​		这些差异意味着 toLocaleString()和 toString()可能只对调试有用，不能用于显示。





##### 2 日期格式化方法

​		Date 类型有几个专门用于格式化日期的方法，他们都会返回字符串：

```
toDateString()显示日期中的周几、月、日、年（格式特定于实现）；
toTimeString()显示日期中的时、分、秒和时区（格式特定于实现）；
toLocaleDateString()显示日期中的周几、月、日、年（格式特定于实现和地区）；
toLocaleTimeString()显示日期中的时、分、秒（格式特定于实现和地区）；
toUTCString()显示完整的 UTC 日期（格式特定于实现）。
```

​		**这些方法的输出与 toLocaleString()和 toString()一样，会因浏览器而异。因此不能用于在用户界面上一致地显示日期。**



**注意**

​		**还有一个方法叫 toGMTString()，这个方法跟 toUTCString()是一样的，目的是为了向后兼容。不过，规范建议新代码使用 toUTCString()。**



##### 3  日期/时间组件方法

​		Date 类型剩下的方法直接涉及取得或设置日期值的特定部分，“ UTC 日期 ” 指的是没有时间偏移（将日期转换为 GMT）时的日期。

```
方法											说明
getTime() 							返回日期的毫秒表示；与 valueOf()相同
setTime(milliseconds) 				设置日期的毫秒表示，从而修改整个日期
getFullYear() 						返回 4 位数年（即 2019 而不是 19）
getUTCFullYear() 					返回 UTC 日期的 4 位数年
setFullYear(year) 					设置日期的年（year 必须是 4 位数）
setUTCFullYear(year) 				设置 UTC 日期的年（year 必须是 4 位数）
getMonth() 							返回日期的月（0 表示 1 月，11 表示 12 月）
getUTCMonth() 						返回 UTC 日期的月（0 表示 1 月，11 表示 12 月）
setMonth(month) 					设置日期的月（month 为大于 0 的数值，大于 11 加年）
setUTCMonth(month) 					设置 UTC 日期的月（month 为大于 0 的数值，大于 11 加年）
getDate() 							返回日期中的日（1~31）
getUTCDate() 						返回 UTC 日期中的日（1~31）
setDate(date) 						设置日期中的日（如果 date 大于该月天数，则加月）
setUTCDate(date) 					设置 UTC 日期中的日（如果 date 大于该月天数，则加月）
getDay() 							返回日期中表示周几的数值（0 表示周日，6 表示周六）
getUTCDay() 						返回 UTC 日期中表示周几的数值（0 表示周日，6 表示周六）
getHours() 							返回日期中的时（0~23）
getUTCHours() 						返回 UTC 日期中的时（0~23）
setHours(hours) 					设置日期中的时（如果 hours 大于 23，则加日）
setUTCHours(hours) 					设置 UTC 日期中的时（如果 hours 大于 23，则加日）
getMinutes() 						返回日期中的分（0~59）
getUTCMinutes() 					返回 UTC 日期中的分（0~59）
setMinutes(minutes) 				设置日期中的分（如果 minutes 大于 59，则加时）
setUTCMinutes(minutes) 				设置 UTC 日期中的分（如果 minutes 大于 59，则加时）
getSeconds() 						返回日期中的秒（0~59）
getUTCSeconds() 					返回 UTC 日期中的秒（0~59）
setSeconds(seconds) 				设置日期中的秒（如果 seconds 大于 59，则加分）
setUTCSeconds(seconds) 				设置 UTC 日期中的秒（如果 seconds 大于 59，则加分）
getMilliseconds() 					返回日期中的毫秒
getUTCMilliseconds() 				返回 UTC 日期中的毫秒
setMilliseconds(milliseconds) 		设置日期中的毫秒
setUTCMilliseconds(milliseconds) 	设置 UTC 日期中的毫秒
getTimezoneOffset() 				返回以分钟计的 UTC 与本地时区的偏移量（如美国 EST 即“东部标准									时间”返回 300，进入夏令时的地区可能有所差异）
```



#### 2.RegExp

​		ECMAScript 通过 RegExp 类型支持正则表达式。正则表达式使用类似 Perl 的简洁语法来创建

```
let expression = /pattern/flags;
```

​		这个正则表达式的 pattern （模式）可以是任何简单或复杂的正则表达式，包括字符类、限定类、分组、向前查找和反向引用。每个正则表达式可以带零个或多个 flags （标记），用于控制正则表达式的行为。

​		表示匹配模式的标记：

```
 g：全局模式，表示查找字符串的全部内容，而不是找到第一个匹配的内容就结束。
 i：不区分大小写，表示在查找匹配时忽略 pattern 和字符串的大小写。
 m：多行模式，表示查找到一行文本末尾时会继续查找。
 y：粘附模式，表示只查找从 lastIndex 开始及之后的字符串。
 u：Unicode 模式，启用 Unicode 匹配。
 s：dotAll 模式，表示元字符.匹配任何字符（包括\n 或\r）。
```



```
// 匹配字符串中的所有"at" 
let pattern1 = /at/g; 
// 匹配第一个"bat"或"cat"，忽略大小写
let pattern2 = /[bc]at/i; 
// 匹配所有以"at"结尾的三字符组合，忽略大小写
let pattern3 = /.at/gi;
```



​		与其他语言中的正则表达式类似，所有元字符在模式中也必须转义包括：

```
( [ { \ ^ $ | ) ] } ? * + .
```



​		正则表达式也可以使用 RegExp 构造函数来创建，它接收两个参数：模式字符串和（可选的）标记字符串。任何使用字面量定义的正则表达式也可以通过构造函数来创建。

这里的 pattern1 和 pattern2 是等效的正则表达式。RegExp 构造函数的两个参数都是字符串。因为 RegExp 的模式参数是字符串，所以在某些情况下需要二次转义。

```
// 匹配第一个"bat"或"cat"，忽略大小写
let pattern1 = /[bc]at/i;

// 跟 pattern1 一样，只不过是用构造函数创建的
let pattern2 = new RegExp("[bc]at", "i");
```

​		所有元字符都必须二次转义，包括转义字符序列，如\n（\转义后的字符串是\\，在正则表达式字符串中则要写成\\\\）。使用 RegExp 构造函数创建时对应的模式字符串。

```
字面量模式 							对应的字符串
/\[bc\]at/ 							"\\[bc\\]at"
/\.at/ 								"\\.at"
/name\/age/ 						"name\\/age"
/\d.\d{1,2}/ 						"\\d.\\d{1,2}"
/\w\\hello\\123/ 					"\\w\\\\hello\\\\123"
```



​		使用 RegExp 也可以基于已有的正则表达式实例，并可选择性地修改它们的标记：

```
const re1 = /cat/g; 
console.log(re1); // "/cat/g" 

const re2 = new RegExp(re1); 
console.log(re2); // "/cat/g"

const re3 = new RegExp(re1, "i"); 
console.log(re3); // "/cat/i"
```



##### 1  RegExp 实例属性

​		每个 RegExp 实例都有以下属性：

```
 global：布尔值，表示是否设置了 g 标记。
 ignoreCase：布尔值，表示是否设置了 i 标记。
 unicode：布尔值，表示是否设置了 u 标记。
 sticky：布尔值，表示是否设置了 y 标记。
 lastIndex：整数，表示在源字符串中下一次搜索的开始位置，始终从 0 开始。
 multiline：布尔值，表示是否设置了 m 标记。
 dotAll：布尔值，表示是否设置了 s 标记。
 source：正则表达式的字面量字符串（不是传给构造函数的模式字符串），没有开头和结尾的
斜杠。
 flags：正则表达式的标记字符串。始终以字面量而非传入构造函数的字符串模式形式返回（没
有前后斜杠）
```



​		注意，虽然第一个模式是通过字面量创建的，第二个模式是通过 RegExp 构造函数创建的，但两个模式的 source 和 flags 属性是相同的。source 和 flags 属性返回的是规范化之后可以在字面量中使用的形式。

```
let pattern1 = /\[bc\]at/i; 
console.log(pattern1.global); // false 
console.log(pattern1.ignoreCase); // true 
console.log(pattern1.multiline); // false 
console.log(pattern1.lastIndex); // 0 
console.log(pattern1.source); // "\[bc\]at" 
console.log(pattern1.flags); // "i" 

let pattern2 = new RegExp("\\[bc\\]at", "i"); 
console.log(pattern2.global); // false 
console.log(pattern2.ignoreCase); // true 
console.log(pattern2.multiline); // false 
console.log(pattern2.lastIndex); // 0 
console.log(pattern2.source); // "\[bc\]at" 
console.log(pattern2.flags); // "i"
```



##### 2 RegExp 实现方法

​		RegExp 实例的主要方法是 exec()，主要用于配合捕获组使用。这个方法只接收一个参数，即要应用模式的字符串。如果找到了匹配项，则返回包含第一个匹配信息的数组；如果没找到匹配项，则返回null。返回的数组虽然是 Array 的实例，但包含两个额外的属性：index 和 input。index 是字符串中匹配模式的起始位置，input 是要查找的字符串。这个数组的第一个元素是匹配整个模式的字符串，其他元素是与表达式中的捕获组匹配的字符串。如果模式中没有捕获组，则数组只包含一个元素。

```
let text = "mom and dad and baby"; 
let pattern = /mom( and dad( and baby)?)?/gi; 
let matches = pattern.exec(text); 
console.log(matches.index); // 0 
console.log(matches.input); // "mom and dad and baby" 
console.log(matches[0]); // "mom and dad and baby" 
console.log(matches[1]); // " and dad and baby" 
console.log(matches[2]); // " and baby"
```

​		

​		如果模式设置了全局标记，则每次调用 exec()方法会返回一个匹配的信息。如果没有设置全局标记，则无论对同一个字符串调用多少次 exec()，也只会返回第一个匹配的信息。模式没有设置全局标记，因此调用 exec()只返回第一个匹配项（"cat"）。lastIndex在非全局模式下始终不变。

```
let text = "cat, bat, sat, fat"; 
let pattern = /.at/; 

let matches = pattern.exec(text); 
console.log(matches.index); // 0 
console.log(matches[0]); // cat 
console.log(pattern.lastIndex); // 0 

matches = pattern.exec(text); 
console.log(matches.index); // 0 
console.log(matches[0]); // cat 
console.log(pattern.lastIndex); // 0
```

​		模式设置了全局标记，因此每次调用 exec()都会返回字符串中的下一个匹配项，直到搜索到字符串末尾。注意模式的 lastIndex 属性每次都会变化。在全局匹配模式下，每次调用 exec()都会更新 lastIndex 值，以反映上次匹配的最后一个字符的索引。

```
let text = "cat, bat, sat, fat"; 
let pattern = /.at/g; 

let matches = pattern.exec(text); 
console.log(matches.index); // 0 
console.log(matches[0]); // cat 
console.log(pattern.lastIndex); // 3

matches = pattern.exec(text); 
console.log(matches.index); // 5 
console.log(matches[0]); // bat 
console.log(pattern.lastIndex); // 8 
matches = pattern.exec(text); 
console.log(matches.index); // 10 
console.log(matches[0]); // sat 
console.log(pattern.lastIndex); // 13
```



​		如果模式设置了粘附标记 y，则每次调用 exec()就只会在 lastIndex 的位置上寻找匹配项。粘附标记覆盖全局标记。

```
let text = "cat, bat, sat, fat"; 
let pattern = /.at/y; 

let matches = pattern.exec(text); 
console.log(matches.index); // 0 
console.log(matches[0]); // cat 
console.log(pattern.lastIndex); // 3 

// 以索引 3 对应的字符开头找不到匹配项，因此 exec()返回 null 
// exec()没找到匹配项，于是将 lastIndex 设置为 0 
matches = pattern.exec(text); 
console.log(matches); // null 
console.log(pattern.lastIndex); // 0 

// 向前设置 lastIndex 可以让粘附的模式通过 exec()找到下一个匹配项：
pattern.lastIndex = 5; 
matches = pattern.exec(text); 
console.log(matches.index); // 5 
console.log(matches[0]); // bat 
console.log(pattern.lastIndex); // 8
```



​		正则表达式的另一个方法是 test（），接收一个字符串参数。如果输入的文本与模式匹配，则参数返回 true，否则返回 false。这个方法适用于只想测试模式是否匹配，而不需要实际匹配内容的情况。

​		这个例子中，正则表达式用于测试特定的数值序列。如果输入的文本与模式匹配，则显示匹配成功的消息。这个用法常用于验证用户输入，此时我们只在乎输入是否有效，不关心为什么无效。

```
let text = "000-00-0000"; 
let pattern = /\d{3}-\d{2}-\d{4}/; 
if (pattern.test(text)) { 
 	console.log("The pattern was matched."); 
}
```



​		无论正则表达式是怎么创建的，继承的方法 toLocaleString()和 toString()都返回正则表达式的字面量表示。

​		这里的模式是通过 RegExp 构造函数创建的，但 toLocaleString()和 toString()返回的都是其字面量的形式。

```
let pattern = new RegExp("\\[bc\\]at", "gi"); 
console.log(pattern.toString()); // /\[bc\]at/gi 
console.log(pattern.toLocaleString()); // /\[bc\]at/gi
```



**注意**

​		**正则表达式的 valueOf（）方法返回正则表达式本身。**





##### 3 RegExp 构造函数属性

​		RegExp 构造函数本身也有几个属性。（其他语言被称为静态属性）。这些属性适用于作用域中的所有正则表达式，而且会根据最后执行的正则表达式操作而变化。可以通过两种不同的方式访问他们。每一个属性都有一个全名和一个简写。

```
全 名 				简 写 						说 明
input 				  $_ 						最后搜索的字符串（非标准特性）
lastMatch 			  $& 						最后匹配的文本
lastParen 			  $+ 						最后匹配的捕获组（非标准特性）
leftContext 		  $` 						input 字符串中出现在 lastMatch 前面的文本
rightContext 		  $' 						input 字符串中出现在 lastMatch 后面的文本
```



​		通过这些属性可以提取出与 exec()和 test()执行的操作相关的信息。

​		用于搜索任何后跟"hort"的字符，并把第一个字符放在了捕获组中。

```
let text = "this has been a short summer"; 
let pattern = /(.)hort/g; 
if (pattern.test(text)) { 
 	console.log(RegExp.input); // this has been a short summer 
 	console.log(RegExp.leftContext); // this has been a 
 	console.log(RegExp.rightContext); // summer 
 	console.log(RegExp.lastMatch); // short 
 	console.log(RegExp.lastParen); // s 
}
```



​		不同属性包含的内容：

```
 input 属性中包含原始的字符串。
 leftContext 属性包含原始字符串中"short"之前的内容，rightContext 属性包含"short"
之后的内容。
 lastMatch 属性包含匹配整个正则表达式的上一个字符串，即"short"。  lastParen 属性包含捕获组的上一次匹配，即"s"。
```

​		这些属性名也可以替换成简写形式，只不过要使用中括号语法来访问，如下面的例子所示，因为大

多数简写形式都不是合法的 ECMAScript 标识符：

```
let text = "this has been a short summer"; 
let pattern = /(.)hort/g; 
/* 
 * 注意：Opera 不支持简写属性名
 * IE 不支持多行匹配
 */ 
if (pattern.test(text)) { 
 	console.log(RegExp.$_); // this has been a short summer 
 	console.log(RegExp["$`"]); // this has been a 
 	console.log(RegExp["$'"]); // summer 
 	console.log(RegExp["$&"]); // short 
 	console.log(RegExp["$+"]); // s 
}
```



​		RegExp 还有其他几个构造函数属性，可以存储最多 9 个捕获组的匹配项。这些属性通过 RegExp. 

$1~RegExp.$9 来访问，分别包含第 1~9 个捕获组的匹配项。在调用 exec()或 test()时，这些属性就会被填充。

​		模式包含两个捕获组。调用 test()搜索字符串之后，因为找到了匹配项所以返回true，而且可以打印出通过 RegExp 构造函数的$1 和$2 属性取得的两个捕获组匹配的内容。

```
let text = "this has been a short summer"; 
let pattern = /(..)or(.)/g; 
if (pattern.test(text)) { 
 	console.log(RegExp.$1); // sh 
 	console.log(RegExp.$2); // t 
}
```



**注意**

​		**RegExp 构造函数的所有属性都没有任何 Web 标准出处，所以不要在生产环境中使用他们。**





##### 4 模式局限

​		虽然 ECMAScript 对正则表达式的支持有了长足的进步，但仍然缺少 Perl 语言中的一些高级特性。

下列特性目前还没有得到 ECMAScript 的支持：

```
 \A 和\Z 锚（分别匹配字符串的开始和末尾）
 联合及交叉类
 原子组
 x（忽略空格）匹配模式
 条件式匹配
 正则表达式注释
```





#### 3.原始值包装类型

​		为了方便操作原始值，ECMAScript 提供了 3 种特殊的引用类型：Boolean、Number 和 String。这些其他引用类型一样的特点，但也具有与各自原始类型对应的特殊行为。每当用到某个原始值的方法或属性时，后台都会创建一个相应原始包装类型的对象，从而暴露出操作原始值的各种方法。

```
let s1 = "some text"; 
let s2 = s1.substring(2);
```

​		具体来说，当第二行访问 s1 时，是以读模式访问的，也就是要从内存中读取变量保存的值。在以读模式访问字符串值的任何时候，后台都会执行以下 3 步：

​		(1) 创建一个 String 类型的实例；

​	(2) 调用实例上的特定方法；

​	(3) 销毁实例。

可以把这 3 步想象成执行了如下 3 行 ECMAScript 代码：

```
let s1 = new String("some text"); 
let s2 = s1.substring(2); 
s1 = null; 

这种行为可以让原始值拥有对象的行为。对布尔值和数值而言，以上 3 步也会在后台发生，只不过使用的是 Boolean 和 Number 包装类型而已。
```



​		引用类型与原始值包装类型的主要区别在于对象的生命周期。在通过 new 实例化引用类型后，得到的实例会在离开作用域时被销毁，而自动创建的原始值包装对象则只存在于访问它的那行代码执行期间。这意味着不能在运行时给原始值添加属性和方法。

```
let s1 = "some text"; 
s1.color = "red"; 
console.log(s1.color); // undefined
```

​		原因就是第二行代码运行时会临时创建一个 String 对象，而当第三行代码执行时，这个对象已经被销毁了。实际上，第三行代码在这里创建了自己的 String 对象，但这个对象没有 color 属性。



​		可以显式地使用 Boolean、Number 和 String 构造函数创建原始值包装对象。不过应该在确实必要时再这么做，否则容易让开发者疑惑，分不清它们到底是原始值还是引用值。在原始值包装类型的实例上调用 typeof 会返回"object"，所有原始值包装对象都会转换为布尔值 true。

​		另外，Object 构造函数作为一个工厂方法，能够根据传入值的类型返回相应原始值包装类型的实例。

```
let obj = new Object("some text"); 
console.log(obj instanceof String); // true
```



​		如果传给 Object 的是字符串，则会创建一个 String 的实例。如果是数值，则会创建 Number 的

实例。布尔值则会得到 Boolean 的实例。

​		注意，使用 new 调用原始值包装类型的构造函数，与调用同名的转型函数并不一样。

```
let value = "25"; 
let number = Number(value); // 转型函数
console.log(typeof number); // "number" 
let obj = new Number(value); // 构造函数
console.log(typeof obj); // "object"
```

​		在这个例子中，变量 number 中保存的是一个值为 25 的原始数值，而变量 obj 中保存的是一个Number 的实例。





##### 1 Boolean

​		Boolean 是对应布尔值的引用类型。要创建一个 Boolean 对象，就使用 Boolean 构造函数并传入true 或 false，如下例所示：

```
let booleanObject = new Boolean(true);
```



​		Boolean 的实例会重写 valueOf() 方法，返回一个原始值 true 或 false 。toString()方法被调用时也会被覆盖，返回字符串"true"或"false"。不过，Boolean 对象在 ECMAScript 中用得很少。

```
let falseObject = new Boolean(false); 
let result = falseObject && true; 
console.log(result); // true 

let falseValue = false; 
result = falseValue && true; 
console.log(result); // false
```

​		创建一个值为 false 的 Boolean 对象。然后，在一个布尔表达式中通过&&操作将这个对象与一个原始值 true 组合起来。在布尔算术中，false && true 等于 false。可是，这个表达式是对 falseObject 对象而不是对它表示的值（false）求值。前面刚刚说过，所有对象在布尔表达式中都会自动转换为 true，因此 falseObject 在这个表达式里实际上表示一个 true 值。那么true && true 当然是 true。





​		原始值和引用值（Boolean 对象）还有几个区别。首先，typeof 操作符对原始值返回"boolean"，但对引用值返回"object"。同样，Boolean 对象是 Boolean 类型的实例，在使用instaceof 操作符时返回 true，但对原始值则返回 false，

```
console.log(typeof falseObject); // object 
console.log(typeof falseValue); // boolean 
console.log(falseObject instanceof Boolean); // true 
console.log(falseValue instanceof Boolean); // false
```



**注意**

​		**理解原始布尔值和 Boolean 对象之间的区别非常重要，强烈建议永远不要使用后者。**



##### 2 Number

​		Number 是对应数值的引用类型。  let numberObject = new Number(10); 

​		与 Boolean 类型一样，Number 类型重写了 valueOf()、toLocaleString() 和 toString() 方法。

valueOf() ：valueOf()方法返回 Number 对象表示的原始数值，另外两个方法返回数值字符串。

toString() ：toString()方法可选地接收一个表示基数的参数，并返回相应基数形式的数值字符串。

```
let num = 10; 
console.log(num.toString()); // "10" 
console.log(num.toString(2)); // "1010" 
console.log(num.toString(8)); // "12" 
console.log(num.toString(10)); // "10" 
console.log(num.toString(16)); // "a"
```



​		Number 类型还提供了几个用于将数值格式化为字符串的方法。

toFixed() 方法返回包含指定小数点位数的数值字符串。果数值本身的小数位超过了参数指定的位数，则四舍五入到最接近的小数位。

toFixed() 自动舍入的特点可以用于处理货币。需要注意多个浮点数值的数学计算不一定得到精确的结果。如：

​		0.1 + 0.2 = 0.30000000000000004。

```
let num = 10; 
console.log(num.toFixed(2)); // "10.00"

let num = 10.005; 
console.log(num.toFixed(2)); // "10.01"
```

**注意**

​		**toFixed()方法可以表示有 0~20 个小数位的数值。某些浏览器可能支持更大的范围，但这是通常被支持的范围。**



​		另一个用于格式化数值的方法是 toExponential()，返回科学记数法（指数记数法）表示数值字符串。与 toFixed() 一样，toExponential() 也接收一个参数，表示结果中小数的位数。这么小的数不用表示为科学记数法形式。如果想得到数值最适当的形式，那么可以使用 toPrecision()。

```
let num = 10; 
console.log(num.toExponential(1)); // "1.0e+1"
```



​		toPrecision() 方法会根据情况返回最合理的输出结果，可能是固定长度，也可能是科学记数法形式。这个方法接收一个参数，表示结果中数字的总位数（不包含指数）。

```
let num = 99; 
console.log(num.toPrecision(1)); // "1e+2" 
console.log(num.toPrecision(2)); // "99" 
console.log(num.toPrecision(3)); // "99.0"
```

​		首先要用 1 位数字表示数值 99，得到"1e+2"，也就是 100。因为 99 不能只用 1 位数字来精确表示，所以这个方法就将它舍入为 100，这样就可以只用 1 位数字（及其科学记数法形式）来表示了。用 2 位数字表示 99 得到"99"，用 3 位数字则是"99.0"。本质上，toPrecision()方法会根据数值和精度来决定调用 toFixed()还是 toExponential()。为了以正确的小数位精确表示数值，这 3 个方法都会向上或向下舍入。

**注意**

​		**toPrecision()方法可以表示带 1~21 个小数位的数值。某些浏览器可能支持更大的范围，但这是通常被支持的范围。**



​		与 Boolean 对象类似，Number 对象也为数值提供了重要能力。不建议直接实例化 Number 对象。在处理原始数值和引用数值时，typeof  和  instacnceof  操作符会返回不同的结果。

原始数值在调用 typeof 时始终返回"number"，而 Number 对象则返回"object"。类似地，Number对象是Number 类型的实例，而原始数值不是。

```
let numberObject = new Number(10); 
let numberValue = 10; 
console.log(typeof numberObject); // "object" 
console.log(typeof numberValue); // "number" 
console.log(numberObject instanceof Number); // true 
console.log(numberValue instanceof Number); // false
```



​		**isInteger() 方法与安全整数**

​		ES6 新增了 Number.isInteger() 方法，用于辨别一个数值是否保存完整。有时，小数位的 0 可能会让人误以为数值是一个浮点值：

```
console.log(Number.isInteger(1)); // true 
console.log(Number.isInteger(1.00)); // true 
console.log(Number.isInteger(1.01)); // false
```



​		IEEE 754 数值格式有一个特殊的数值范围，在这个范围内二进制值可以表示一个整数值。这个数值范围从 Number.MIN_SAFE_INTEGER（253 + 1）到 Number.MAX_SAFE_INTEGER（253  1）。对超出这个范围的数值，即使尝试保存为整数，IEEE 754 编码格式也意味着二进制值可能会表示一个完全不同的数值。为了鉴别整数是否在这个范围内，可以使用 Number.isSafeInteger()方法：

```
console.log(Number.isSafeInteger(-1 * (2 ** 53))); // false 
console.log(Number.isSafeInteger(-1 * (2 ** 53) + 1)); // true 

console.log(Number.isSafeInteger(2 ** 53)); // false 
console.log(Number.isSafeInteger((2 ** 53) - 1)); // true
```





##### 3 String

​		String 是对应字符串的引用类型，要创建一个 String 对象，使用 String 构造函数并传入一个数值。

​		**let stringObject = new String("hello world");**



​		String 对象的方法可以在所有字符串原始值上调用。3个继承的方法 valueOf()、toLocaleString()  和 toString() 都返回对象的原始字符串值。

​		每个 String 对象都有一个 length 属性，表示字符串中字符的数量。



###### （1）JavaScript 字符

​		JavaScript 字符串由 16 位码元组成。对多数字符来说，每 16 位码元对应一个字符。换句话说，字符串的 length 属性表示字符串包含多少 16 位码元

```
let message = "abcde"; 
console.log(message.length); // 5
```



​		charAt() 方法返回给定索引位置的字符，由传给方法的整数参数指定。具体来说，这个方法查找指定索引位置的 16 位码元，并返回该码元对应的字符。



​		JavaScript 字符串使用了两种 Unicode 编码混合的策略：UCS-2 和 UTF-16。对于可以采用 16 位编码

的字符（U+0000~U+FFFF），这两种编码实际上是一样的。



​		使用 charCodeAt() 方法可以查看指定码元的字符编码。这个方法返回指定索引位置的码元值，索引以整数指定。

```
let message = "abcde"; 

// Unicode "Latin small letter C"的编码是 U+0063 
console.log(message.charCodeAt(2)); // 99

// 十进制 99 等于十六进制 63 
console.log(99 === 0x63); // true
```



​		fromCharCode() 方法用于根据给定的 UTF-16 码元创建字符串中的字符这个方法可以接受任意多个数值，并返回将所有数值对应的字符拼接起来的字符串：

```
// Unicode "Latin small letter A"的编码是 U+0061 
// Unicode "Latin small letter B"的编码是 U+0062 
// Unicode "Latin small letter C"的编码是 U+0063 
// Unicode "Latin small letter D"的编码是 U+0064 
// Unicode "Latin small letter E"的编码是 U+0065 
console.log(String.fromCharCode(0x61, 0x62, 0x63, 0x64, 0x65)); // "abcde"

// 0x0061 === 97 
// 0x0062 === 98 
// 0x0063 === 99 
// 0x0064 === 100 
// 0x0065 === 101 
console.log(String.fromCharCode(97, 98, 99, 100, 101)); // "abcde"
```

​		对于 U+0000~U+FFFF 范围内的字符，length、charAt()、charCodeAt()和 fromCharCode()返回的结果都跟预期是一样的。这是因为在这个范围内，每个字符都是用 16 位表示的，而这几个方法也都基于 16 位码元完成操作。只要字符编码大小与码元大小一一对应，这些方法就能如期工作。

​		这个对应关系在扩展到 Unicode 增补字符平面时就不成立了。问题很简单，即 16 位只能唯一表示65 536 个字符。这对于大多数语言字符集是足够了，在 Unicode 中称为基本多语言平面（BMP）。为了表示更多的字符，Unicode 采用了一个策略，即每个字符使用另外 16 位去选择一个增补平面。这种每个字符使用两个 16 位码元的策略称为代理对。



​		在涉及增补平面的字符时，前面讨论的字符串方法就会出问题。

```
// "smiling face with smiling eyes" 表情符号的编码是 U+1F60A 
// 0x1F60A === 128522 
let message = "ab☺de";

console.log(message.length); // 6 
console.log(message.charAt(1)); // b

console.log(message.charAt(2)); // <?> 
console.log(message.charAt(3)); // <?> 
console.log(message.charAt(4)); // d 

console.log(message.charCodeAt(1)); // 98 
console.log(message.charCodeAt(2)); // 55357 
console.log(message.charCodeAt(3)); // 56842 
console.log(message.charCodeAt(4)); // 100 

console.log(String.fromCodePoint(0x1F60A)); // ☺
console.log(String.fromCharCode(97, 98, 55357, 56842, 100, 101)); // ab☺de
```

​		这些方法仍然将 16 位码元当作一个字符，事实上索引 2 和索引 3 对应的码元应该被看成一个代理对，只对应一个字符。fromCharCode()方法仍然返回正确的结果，因为它实际上是基于提供的二进制表示直接组合成字符串。浏览器可以正确解析代理对（由两个码元构成），并正确地将其识别为一个Unicode 笑脸字符。



​		为正确解析既包含单码元字符又包含代理对字符的字符串，可以使用 codePointAt()来代替charCodeAt()。与charCodeAt() 类似，codePointAt() 接收 16 位码元的索引并返回该索引位置上的码点。码点是 Unicode 中的一个字符的完整标识。"c"的码点是 0x0063，而"☺"的码点是 0x1F60A。码点可能是 16 位，也可能是 32 位，而codePointAt()方法可以从指定码元位置识别完整的码点。

```
let message = "ab☺de"; 
console.log(message.codePointAt(1)); // 98 
console.log(message.codePointAt(2)); // 128522 
console.log(message.codePointAt(3)); // 56842 
console.log(message.codePointAt(4)); // 100
```

**注意**

​		**如果传入的码元索引并非代理对的开头，就会返回错误的码点。这种错误只有检测单个字符的时候才会出现，可以通过从左到右按正确的码元数遍历字符串来规避。**



​		迭代字符串可以智能地识别代理对的码点：

```
console.log([..."ab☺de"]); // ["a", "b", "☺", "d", "e"]
```

​		与 charCodeAt()有对应的 codePointAt()一样，fromCharCode()也有一个对应的 fromCodePoint()。这个方法接收任意数量的码点，返回对应字符拼接起来的字符串：

```
console.log(String.fromCharCode(97, 98, 55357, 56842, 100, 101)); // ab☺de 
console.log(String.**fromCodePoint**(97, 98, 128522, 100, 101)); // ab☺de
```



###### （2）normalize() 方法

​		某些 Unicode 字符可以有多种编码形式。有的字符既可以通过一个 BMP 字符表示，也可以通过一个代理对表示。

```
// U+00C5：上面带圆圈的大写拉丁字母 A 
console.log(String.fromCharCode(0x00C5)); // Å 

// U+212B：长度单位“埃”
console.log(String.fromCharCode(0x212B)); // Å 

// U+004：大写拉丁字母 A 
// U+030A：上面加个圆圈
console.log(String.fromCharCode(0x0041, 0x030A)); //
```



​		比较操作符不在乎字符看起来是什么样的，因此这 3 个字符互不相等。

```
let a1 = String.fromCharCode(0x00C5), 
 a2 = String.fromCharCode(0x212B), 
 a3 = String.fromCharCode(0x0041, 0x030A); 
 
console.log(a1, a2, a3); // Å, Å, Å 

console.log(a1 === a2); // false 
console.log(a1 === a3); // false 
console.log(a2 === a3); // false
```

​		为解决这个问题，Unicode提供了 4种规范化形式，可以将类似上面的字符规范化为一致的格式，无论底层字符的代码是什么。这 4种规范化形式是：NFD（Normalization Form D）、NFC（Normalization Form C）、NFKD（Normalization Form KD）和 NFKC（Normalization Form KC）。可以使用 normalize()方法对字符串应用上述规范化形式，使用时需要传入表示哪种形式的字符串："NFD"、"NFC"、"NFKD"或"NFKC"。



​		通过比较字符串与其调用 normalize()的返回值，就可以知道该字符串是否已经规范化了：

```
let a1 = String.fromCharCode(0x00C5), 
 a2 = String.fromCharCode(0x212B), 
 a3 = String.fromCharCode(0x0041, 0x030A); 
 
// U+00C5 是对 0+212B 进行 NFC/NFKC 规范化之后的结果
console.log(a1 === a1.normalize("NFD")); // false 
console.log(a1 === a1.normalize("NFC")); // true 
console.log(a1 === a1.normalize("NFKD")); // false 
console.log(a1 === a1.normalize("NFKC")); // true 

// U+212B 是未规范化的
console.log(a2 === a2.normalize("NFD")); // false 
console.log(a2 === a2.normalize("NFC")); // false 
console.log(a2 === a2.normalize("NFKD")); // false 
console.log(a2 === a2.normalize("NFKC")); // false 

// U+0041/U+030A 是对 0+212B 进行 NFD/NFKD 规范化之后的结果
console.log(a3 === a3.normalize("NFD")); // true 
console.log(a3 === a3.normalize("NFC")); // false 
console.log(a3 === a3.normalize("NFKD")); // true 
console.log(a3 === a3.normalize("NFKC")); // false 

选择同一种规范化形式可以让比较操作符返回正确的结果：
let a1 = String.fromCharCode(0x00C5), 
 a2 = String.fromCharCode(0x212B), 
 a3 = String.fromCharCode(0x0041, 0x030A); 
 
console.log(a1.normalize("NFD") === a2.normalize("NFD")); // true 
console.log(a2.normalize("NFKC") === a3.normalize("NFKC")); // true 
console.log(a1.normalize("NFC") === a3.normalize("NFC")); // true
```





###### （3）字符串操作方法

​		concat()，用于将一个或多个字符串拼接成一个新字符串。

```
let stringValue = "hello "; 
let result = stringValue.concat("world"); 

console.log(result); // "hello world" 
console.log(stringValue); // "hello"
```

​		concat()方法可以接收任意多个参数，因此可以一次性拼接多个字符串：

```
let stringValue = "hello "; 
let result = stringValue.concat("world", "!");

console.log(result); // "hello world!" 
console.log(stringValue); // "hello"
```

​		虽然 concat()方法可以拼接字符串，但更常用的方式是使用加号操作符（+）。而且多数情况下，对于拼接多个字符串来说，使用加号更方便。



​		ECMAScript 提供了 3 个从字符串中提取子字符串的方法：slice()、substr()和 substring()。这3个方法都返回调用它们的字符串的一个子字符串，而且都接收一或两个参数。第一个表示起始位置，第二个表示结束位置。

​		对 slice()和 substring()而言，第二个参数是提取结束的位置（即该位置之前的字符会被提取出来）。对 substr()而言，第二个参数表示返回的子字符串数量。任何情况下，省略第二个参数都意味着提取到字符串末尾。

```
let stringValue = "hello world"; 
console.log(stringValue.slice(3)); // "lo world" 
console.log(stringValue.substring(3)); // "lo world" 
console.log(stringValue.substr(3)); // "lo world" 
console.log(stringValue.slice(3, 7)); // "lo w" 
console.log(stringValue.substring(3,7)); // "lo w" 
console.log(stringValue.substr(3, 7)); // "lo worl"
```



​		当某个参数是负值时，这 3 个方法的行为又有不同。比如，slice()方法将所有负值参数都当成字

符串长度加上负参数值。

​		substr()方法将第一个负参数值当成字符串长度加上该值，将第二个负参数值转换为 0。substring()方法会将所有负参数值都转换为 0。

```
let stringValue = "hello world"; 
console.log(stringValue.slice(-3)); // "rld" 
console.log(stringValue.substring(-3)); // "hello world" 
console.log(stringValue.substr(-3)); // "rld" 
console.log(stringValue.slice(3, -4)); // "lo w" 
console.log(stringValue.substring(3, -4)); // "hel" 
console.log(stringValue.substr(3, -4)); // "" (empty string)
```





###### （4）字符串位置方法

​		有两个方法用于在字符串中定位子字符串：indexOf() 和 lastIndexOf()。从字符串中搜索传入的字符串，并返回位置（没找到，返回 -1）。indexOf()方法从字符串开头开始查找子字符串，而 lastIndexOf()方法从字符串末尾开始查找子字符串。

​		两个方法都可以接收可选的第二个参数，表示开始搜索的位置。这意味着，indexOf()会从这个参数指定的位置开始向字符串末尾搜索，忽略该位置之前的字符；lastIndexOf()则会从这个参数指定的位置开始向字符串开头搜索，忽略该位置之后直到字符串末尾的字符。



​		样使用第二个参数并循环调用indexOf()或 lastIndexOf()，就可以在字符串中找到所有的目标子字符串。

```
let stringValue = "Lorem ipsum dolor sit amet, consectetur adipisicing elit"; 
let positions = new Array(); 
let pos = stringValue.indexOf("e"); 

while(pos > -1) { 
 	positions.push(pos); 
 	pos = stringValue.indexOf("e", pos + 1); 
} 
console.log(positions); // [3,24,32,35,52]
```



###### （5）字符串包含方法

​		ECMAScript 6 增加了 3 个用于判断字符串中是否包含另一个字符串的方法：startsWith()、endsWith()和 includes()。这些方法都会从字符串中搜索传入的字符串，并返回一个表示是否包含的布尔值。

startsWith() 检查开始于索引 0 的匹配项。

endsWith() 检查开始于索引(string.length - substring.length)的匹配项。

而 includes()检查整个字符串。

```
let message = "foobarbaz"; 

console.log(message.startsWith("foo")); // true 
console.log(message.startsWith("bar")); // false 

console.log(message.endsWith("baz")); // true 
console.log(message.endsWith("bar")); // false 

console.log(message.includes("bar")); // true 
console.log(message.includes("qux")); // false
```

​		startsWith()和 includes()方法接收可选的第二个参数，表示开始搜索的位置。

​		endsWith()方法接收可选的第二个参数，表示应该当作字符串末尾的位置。



###### （6）trim() 方法

​		ECMAScript 在所有字符串上提供了trim()方法。这个方法会创建字符串的一个副本，删除前、后所有空格符，再返回结果。

```
let stringValue = " hello world "; 
let trimmedStringValue = stringValue.trim(); 

console.log(stringValue); // " hello world " 
console.log(trimmedStringValue); // "hello world"
```

​		由于 trim()返回的是字符串的副本，因此原始字符串不受影响，即原本的前、后空格符都会保留。另外，trimLeft()和 trimRight()方法分别用于从字符串开始和末尾清理空格符。



###### （7）repeat() 方法

​		ECMAScript 在所有字符串上都提供了 repeat()方法。这个方法接收一个整数参数，表示要将字符串复制多少次，然后返回拼接所有副本后的结果。



###### （8）padStart() 和 padEnd() 方法

​		padStart() 和 padEnd() 方法会复制字符串，如果小于指定长度，则在相应一边填充字符，直至满足长度条件。这两个方法的第一个参数是长度，第二个参数是可选的填充字符串，默认为空格（U+0020）。

```
let stringValue = "foo"; 

console.log(stringValue.padStart(6)); // " foo" 
console.log(stringValue.padStart(9, ".")); // "......foo" 

console.log(stringValue.padEnd(6)); // "foo " 
console.log(stringValue.padEnd(9, ".")); // "foo......"
```



​		可选的第二个参数并不限于一个字符。如果提供了多个字符的字符串，则会将其拼接并截断以匹配指定长度。此外，如果长度小于或等于字符串长度，则会返回原始字符串。



###### （9）字符串迭代与解构

​		字符串的原型上暴露了一个@@iterator 方法，表示可以迭代字符串的每个字符。

手动使用迭代器：

```
let message = "abc"; 
let stringIterator = message[Symbol.iterator](); 

console.log(stringIterator.next()); // {value: "a", done: false} 
console.log(stringIterator.next()); // {value: "b", done: false} 
console.log(stringIterator.next()); // {value: "c", done: false} 
console.log(stringIterator.next()); // {value: undefined, done: true}
```



​		在 for-of 循环中可以通过这个迭代器按序访问每个字符。

```
for (const c of "abcde") { 
 	console.log(c); 
} 
// a 
// b 
// c 
// d 
// e
```

​		有了这个迭代器之后，字符串就可以通过解构操作符来解构了。比如，可以更方便地把字符串分割为字符数组：

```
let message = "abcde"; 
console.log([...message]); // ["a", "b", "c", "d", "e"]
```



###### （10）字符串大小写转换

​		涉及大小写转换，包括 4 个方法：toLowerCase()、toLocaleLowerCase()、toUpperCase()、 和 toLocaleUpperCase()。在很多地区，地区特定的方法与通用的方法是一样的。

toLowerCase()和toUpperCase()方法是原来就有的方法，与 java.lang.String 中的方法同名。toLocaleLowerCase()和 toLocaleUpperCase()方法旨在基于特定地区实现。

```
let stringValue = "hello world"; 
console.log(stringValue.toLocaleUpperCase()); // "HELLO WORLD" 
console.log(stringValue.toUpperCase()); // 		 "HELLO WORLD" 
console.log(stringValue.toLocaleLowerCase()); // "hello world" 
console.log(stringValue.toLowerCase()); // 		 "hello world"
```

​		通常，如果不知道代码涉及什么语言，则最好使用地区特定的转换方法。



###### （11）字符串模式匹配

​		String 类型专门为在字符串中实现模式匹配设计了几个方法。

​		第一个就是 match（）方法，这个方法本质上跟 RegExp 对象的 exec（）方法相同。match()方法接收一个参数，可以是一个正则表达式字符串，也可以是一个 RegExp 对象。

```
let text = "cat, bat, sat, fat"; 
let pattern = /.at/; 
// 等价于 pattern.exec(text) 
let matches = text.match(pattern); 
console.log(matches.index); // 0 
console.log(matches[0]); // "cat" 
console.log(pattern.lastIndex); // 0
```

​		另一个查找模式的字符串方法是 search() 。这个方法唯一的参数与 match()方法一样：正则表达

式字符串或 RegExp 对象。这个方法返回模式第一个匹配的位置索引，如果没找到则返回1。search()

始终从字符串开头向后匹配模式。

```
let text = "cat, bat, sat, fat"; 
let pos = text.search(/at/); 
console.log(pos); // 1
```



​		为简化子字符串替换操作，ECMAScript 提供了 replace()方法。这个方法接收两个参数，第一个参数可以是一个 RegExp 对象或一个字符串（这个字符串不会转换为正则表达式），第二个参数可以是一个字符串或一个函数。如果第一个参数是字符串，那么只会替换第一个子字符串。要想替换所有子字符串，第一个参数必须为正则表达式并且带全局标记.

```
let text = "cat, bat, sat, fat"; 
let result = text.replace("at", "ond"); 
console.log(result); // "cond, bat, sat, fat" 

result = text.replace(/at/g, "ond"); 
console.log(result); // "cond, bond, sond, fond"
```



​		第二个参数是字符串的情况下，有几个特殊的字符序列，可以用来插入正则表达式操作的值。

```
字符序列 			 替换文本
$$ 					$ 
$& 					匹配整个模式的子字符串。与 RegExp.lastMatch 相同
$' 					匹配的子字符串之前的字符串。与 RegExp.rightContext 相同
$` 					匹配的子字符串之后的字符串。与 RegExp.leftContext 相同
$n 					匹配第 n 个捕获组的字符串，其中 n 是 0~9。比如，$1 是匹配第一个捕获组的字符串，					  $2 是匹配第二个捕获组的字符串，以此类推。如果没有捕获组，则值为空字符串

$nn 				匹配第 nn 个捕获组字符串，其中 nn 是 01~99。比如，$01 是匹配第一个捕获组的字符  					  串，$02 是匹配第二个捕获组的字符串，以此类推。如果没有捕获组，则值为空字符串
```

​		使用这些特殊的序列，可以在替换文本中使用之前匹配的内容：

```
let text = "cat, bat, sat, fat"; 
result = text.replace(/(.at)/g, "word ($1)"); 
console.log(result); // word (cat), word (bat), word (sat), word (fat)
```



​		replace() 的第二个参数可以是一个函数。函数 htmlEscape()用于将一段 HTML 中的 4 个字符替换成对应的实体：小于号、大于号、和号，还有双引号（都必须经过转义）。

```
function htmlEscape(text) { 
 	return text.replace(/[<>"&]/g, function(match, pos, originalText) { 
 		switch(match) { 
 			case "<": 
 				return "&lt;"; 
 			case ">": 
 				return "&gt;"; 
 			case "&": 
 				return "&amp;"; 
 			case "\"": 
 				return "&quot;"; 
 		} 
 	}); 
} 
console.log(htmlEscape("<p class=\"greeting\">Hello world!</p>")); 
// "&lt;p class=&quot;greeting&quot;&gt;Hello world!</p>"
```



​		最后一个与模式匹配相关的字符串方法是 split()。这个方法会根据传入的分隔符将字符串拆分成数组。作为分隔符的参数可以是字符串，也可以是 RegExp 对象。（字符串分隔符不会被这个方法当成正则表达式。）还可以传入第二个参数，即数组大小，确保返回的数组不会超过指定大小。

```
let colorText = "red,blue,green,yellow"; 
let colors1 = colorText.split(","); // ["red", "blue", "green", "yellow"] 
let colors2 = colorText.split(",", 2); // ["red", "blue"] 
let colors3 = colorText.split(/[^,]+/); // ["", ",", ",", ",", ""]
```

​		最后，使用正则表达式可以得到一个包含逗号的数组。注意在最后一次调用 split()时，返回的数组前后包含两个空字符串。这是因为正则表达式指定的分隔符出现在了字符串开头（"red"）和末尾（"yellow"）。



###### （12）localeCompare() 方法

​		比较两个字符串，返回以下 3 个值中的一个。

```
 如果按照字母表顺序，字符串应该排在字符串参数前头，则返回负值。（通常是-1，具体还要看与实际值相关的实现。）
 如果字符串与字符串参数相等，则返回 0。 
 如果按照字母表顺序，字符串应该排在字符串参数后头，则返回正值。（通常是 1，具体还要看与实际值相关的实现。）
```

​		"brick"按字母表顺序应该排在"yellow"前头，因此 localeCompare()返回 1。

```
let stringValue = "yellow"; 
console.log(stringValue.localeCompare("brick")); // 1 
console.log(stringValue.localeCompare("yellow")); // 0 
console.log(stringValue.localeCompare("zoo")); // -1
```



​		因为返回的具体值可能因具体实现而异，所以最好像下面的示例中一样使用 localeCompare()：

这样一来，就可以保证在所有实现中都能正确判断字符串的顺序了。

```
function determineOrder(value) { 
 	let result = stringValue.localeCompare(value); 
 	if (result < 0) { 
 		console.log(`The string 'yellow' comes before the string '${value}'.`); 
 	} else if (result > 0) { 
 		console.log(`The string 'yellow' comes after the string '${value}'.`); 
 	} else { 
 		console.log(`The string 'yellow' is equal to the string '${value}'.`); 
 	} 
} 
determineOrder("brick"); 
determineOrder("yellow"); 
determineOrder("zoo");
```



​		localeCompare()的独特之处在于，实现所在的地区（国家和语言）决定了这个方法如何比较字符串。在美国，英语是 ECMAScript 实现的标准语言，localeCompare()区分大小写，大写字母排在小写字母前面。但其他地区未必是这种情况。





###### （13）HTML 方法

​		早期的浏览器开发商认为使用 JavaScript 动态生成 HTML 标签是一个需求。因此，早期浏览器扩展了规范，增加了辅助生成 HTML 标签的方法。下表总结了这些 HTML 方法。不过，这些方法基本上已经没有人使用了，因为结果通常不是语义化的标记。

```
方 法 						输 出
anchor(name) 				 <a name="name">string</a>
big() 						 <big>string</big>
bold() 						 <b>string</b>
fixed() 					 <tt>string</tt>
fontcolor(color) 			 <font color="color">string</font>
fontsize(size) 				 <font size="size">string</font>
italics() 					 <i>string</i>
link(url) 					 <a href="url">string</a>
small() 					 <small>string</small>
strike() 					 <strike>string</strike>
sub() 						 <sub>string</sub>
sup() 						 <sup>string</sup>
```





#### 4.单例内置对象

​		ECMA-262 对内置对象的定义是“任何由 ECMAScript 实现提供、与宿主环境无关，并在 ECMAScript程序开始执行时就存在的对象”。开发者不用显式地实例化内置对象，因为它们已经实例化好了。前面我们已经接触了大部分内置对象，包括 Object、Array 和 String。本节介绍 ECMA-262定义的另外两个单例内置对象：Global 和 Math。



##### 1 Global

​		Global 对象是ECMAScript 中最特别的对象，因为代码不会显示地访问它。ECMA-262 规定 Global对象为一种兜底对象，它所针对的是不属于任何对象的属性和方法。事实上，不存在全局变量或全局函数这种东西。在全局作用域中定义的变量和函数都会变成 Global 对象的属性 。本书前面介绍的函数，包括 isNaN()、isFinite()、parseInt()和 parseFloat()，实际上都是 Global 对象的方法。除了这些，Global 对象上还有另外一些方法。



###### （1）URL 编码方式

​		encodeURI () 和 encodeURIComponent () 方法用于编码统一资源标识符（ URI ），以便传给浏览器。有效的 URI 不能包含某些字符，比如空格。使用 URI 编码方法来编码 URI 可以让浏览器能够理解它们，同时又以特殊的 UTF-8 编码替换掉所有无效字符。

​		这两个方法的主要区别是，encodeURI()不会编码属于 URL 组件的特殊字符，比如冒号、斜杠、问号、井号，而 encodeURIComponent()会编码它发现的所有非标准字符。

```
let uri = "http://www.wrox.com/illegal value.js#start"; 

// "http://www.wrox.com/illegal%20value.js#start" 
console.log(encodeURI(uri)); 

// "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.js%23start" 
console.log(encodeURIComponent(uri));
```

​		使用 encodeURI()编码后，除空格被替换为%20 之外，没有任何变化。而 encodeURIComponent()方法将所有非字母字符都替换成了相应的编码形式。这就是使用 encodeURI()编码整个URI，但只使用encodeURIComponent()编码那些会追加到已有 URI 后面的字符串的原因。

**注意**

​		**使用encodeURIComponent() 应该比使用 encodeURI() 的频率更高，因为编码查询字符串参数比编码基准 URI 的次数多。**





​		与 encodeURI() 和 encodeURIComponent() 相对的是 decodeURI() 和 decodeURIComponent()。

decodeURI() 只对使用 encodeURI() 编码过的字符解码。例如，%20 会被替换为空格，但%23 不会被替换为井号（#），因为井号不是由 encodeURI()替换的。

decodeURIComponent()解码所有被 encodeURIComponent()编码的字符，基本上就是解码所有特殊值。

```
let uri = "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.js%23start";

// http%3A%2F%2Fwww.wrox.com%2Fillegal value.js%23start 
console.log(decodeURI(uri)); 

// http:// www.wrox.com/illegal value.js#start 
console.log(decodeURIComponent(uri));
```



**注意**

​		**URI方法 encodeURI()、encodeURIComponent()、decodeURI()和 decodeURIComponent()取代了 escape()和 unescape()方法，后者在 ECMA-262 第 3 版中就已经废弃了。URI 方法始终是首选方法，因为它们对所有 Unicode 字符进行编码，而原来的方法只能正确编码 ASCII 字符。不要在生产环境中使用 escape()和unescape()。**



###### （2）eval() 方法

​		eval() 方法可能是整个 ECMAScript 语言中最强大的了。这个方法就是一个完整的 ECMAScript 解读器，它接收一个参数，即一个要执行的 ECMAScript（JavaScript）字符串。

```
eval("console.log('hi')"); 
上面这行代码的功能与下一行等价：
console.log("hi");
```



​		当解释器发现 eval()调用时，会将参数解释为实际的 ECMAScript 语句，然后将其插入到该位置。定义在包含上下文中的变量可以在 eval()调用内部被引用。第二行代码会被替换成一行真正的函数调用代码。

```
let msg = "hello world"; 
eval("console.log(msg)"); // "hello world"
```

​		类似地，可以在 eval()内部定义一个函数或变量，然后在外部代码中引用。

```
eval("function sayHi() { console.log('hi'); }"); 
sayHi();
```



​		通过 eval()定义的任何变量和函数都不会被提升，这是因为在解析代码的时候，它们是被包含在一个字符串中的。它们只是在 eval()执行的时候才会被创建。

```
eval("let msg = 'hello world';"); 
console.log(msg); // Reference Error: msg is not defined
```

​		在严格模式下，在 eval()内部创建的变量和函数无法被外部访问。换句话说，最后两个例子会报错。同样，在严格模式下，赋值给 eval 也会导致错误

```
"use strict"; 
eval = "hi"; // 导致错误
```

**注意**

​		**解释代码字符串的能力是非常强大的，但也非常危险。在使用 eval()的时候必须极为慎重，特别是在解释用户输入的内容时。因为这个方法会对 XSS 利用暴露出很大的攻击面。恶意用户可能插入会导致你网站或应用崩溃的代码。**





###### （3）Global 对象属性

​		Global 对象有很多属性。 undefined、NaN 和 Infinity 等特殊值都是 Global 对象的属性。此外，所有原生引用类型构造函数，比如 Object 和 Function，也都是Global 对象的属性。

```
属 性 						   说 明
undefined 						特殊值 undefined
NaN 							特殊值 NaN
Infinity 						特殊值 Infinity
Object 							Object的构造函数
Array 							Array 的构造函数
Function 						Function 的构造函数
Boolean 						Boolean 的构造函数
String 							String 的构造函数
Number 							Number 的构造函数
Date 							Date 的构造函数
RegExp 							RegExp 的构造函数
Symbol 							Symbol 的伪构造函数
Error 							Error 的构造函数
EvalError 						EvalError 的构造函数
RangeError 						RangeError 的构造函数
ReferenceError 					ReferenceError 的构造函数
SyntaxError 					SyntaxError 的构造函数
TypeError 						TypeError 的构造函数
URIError 						URIError 的构造函数
```





###### （4）window 对象

​		虽然 ECMA-262 没有规定直接访问 Global 对象的方式，但浏览器将 window 对象实现为 Global对象的代理。因此，所有全局作用域中声明的变量和函数都变成了 window 的属性。

```
var color = "red"; 
function sayColor() { 
 	console.log(window.color); 
} 
window.sayColor(); // "red"
```

**注意**

​		 **window 对象在 JavaScript 中远不止实现了 ECMAScript 的 Global 对象那么简单。**



​		另一种获取 Global 对象的方式是使用如下的代码

```
let global = function() {
	return this; 
}();
```

​		当一个函数在没有明确（通过成为某个对象的方法，或者通过 call()/apply()）指定 this 值的情况下执行时，this 值等于Global 对象。因此，调用一个简单返回 this 的函数是在任何执行上下文中获取 Global 对象的通用

方式。





##### 2 Math

​		ECMAScript 提供了 Math 对象作为保存数学公式、信息和计算的地方。Math 对象提供了一些辅助计算的属性和方法。

**注意**

​		**使用 Math 计算的问题是精度会因浏览器、操作系统、指令集和硬件而异。**



###### （1） Math 对象属性

​		Math 对象有一些属性，主要用于保存数学中的一些特殊值。

```
属 性 				   说 明
Math.E 					自然对数的基数 e 的值
Math.LN10 				10 为底的自然对数
Math.LN2 				2 为底的自然对数
Math.LOG2E 				以 2 为底 e 的对数
Math.LOG10E 			以 10 为底 e 的对数
Math.PI 				π 的值
Math.SQRT1_2 			1/2 的平方根
Math.SQRT2 				2 的平方根
```



###### （2） min（）和 max（）方法

​		min()和 max()方法用于确定一组数值中的最小值和最大值。这两个方法都接收任意多个参数。使用这两个方法可以避免使用额外的循环和 if 语句来确定一组数值的最大最小值。

```
let max = Math.max(3, 54, 32, 16); 
console.log(max); // 54 

let min = Math.min(3, 54, 32, 16); 
console.log(min); // 3
```

​		要知道数组中的最大值和最小值，可以像下面这样使用扩展操作符：

```
let a = [1,2,3,4,5,6,7,8];
let max = Math.max(...a);
```



###### （3）舍入方法

​		用于把小数值舍入为整数的 4 个方法：Math.ceil()、Math.floor()、Math.round()和 Math.fround()。

（1）Math.ceil() 方法始终向上舍入为最接近的整数。

（2）Math.floor() 方法始终向下舍入为最接近的整数。

（3）Math.round() 方法执行四舍五入。

（4）Math.fround() 方法返回数值最接近的单精度（32 位）浮点值表示。

```
console.log(Math.ceil(25.5)); // 26 
console.log(Math.ceil(25.1)); // 26 

console.log(Math.round(25.5)); // 26 
console.log(Math.round(25.1)); // 25 

console.log(Math.fround(0.4)); // 0.4000000059604645 
console.log(Math.fround(0.5)); // 0.5 
console.log(Math.fround(25.9)); // 25.899999618530273 

console.log(Math.floor(25.9)); // 25 
console.log(Math.floor(25.5)); // 25 
console.log(Math.floor(25.1)); // 25
```



###### （4）random（）方法

​		Math.random() 方法返回一个 0~1 范围内的随机数，其中包含 0 但不包含 1。

​		使用 Math.random() 从一组整数中随机选择一个数：

使用了 Math.floor() 方法，因为 Math.random() 始终返回小数，即便乘以一个数再加上一个数也是小数。

```
number = Math.floor(Math.random() * total_number_of_choices + first_possible_value)；
```



​		如果想从 1~10 范围内随机选择一个数。这样就有 10 个可能的值（1~10），其中最小的值是 1。

```
let num = Math.floor(Math.random() * 10 + 1);
```

​		2~10 只有 9 个数，所以可选总数（total_number_of_choices）是 9，而最小可能的值（first_possible_value）是 2。

想选择一个 2~10 范围内的值：

```
let num = Math.floor(Math.random() * 9 + 2);
```



​		通过函数来算出可选总数和最小可能的值可能更方便。调用 selectFrom(2,10)就可以从 2~10（包含）

范围内选择一个值了。

```
function selectFrom(lowerValue, upperValue) {
	let choices = upperValue - lowerValue + 1; 
 	return Math.floor(Math.random() * choices + lowerValue); 
} 
let num = selectFrom(2,10); 
console.log(num); // 2~10 范围内的值，其中包含 2 和 10
```



​		使用这个函数，从一个数组中随机选择一个元素就很容易。传给 selecFrom()的第二个参数是数组长度减 1，即数组最大的索引值。

```
let colors = ["red", "green", "blue", "yellow", "black", "purple", "brown"]; 
let color = colors[selectFrom(0, colors.length-1)];
```

**注意**

​		**如果是为了加密而需要生成随机数（传给生成器的输入需要较高的不确定性），那么建议使用 window.crypto. getRandomValues()。**





###### （5）其他方法

​		Math 对象还有很多涉及各种简单或高阶数运算的方法。下表还是总结了 Math 对象的其他方法。

```
方 法							 说 明
Math.abs(x) 				  返回 x 的绝对值
Math.exp(x) 				  返回 Math.E 的 x 次幂
Math.expm1(x) 				  等于 Math.exp(x) - 1
Math.log(x) 				  返回 x 的自然对数
Math.log1p(x) 				  等于 1 + Math.log(x)
Math.pow(x, power) 			  返回 x 的 power 次幂
Math.hypot(...nums) 		  返回 nums 中每个数平方和的平方根
Math.clz32(x) 				  返回 32 位整数 x 的前置零的数量
Math.sign(x) 				  返回表示 x 符号的 1、0、-0 或-1
Math.trunc(x) 				  返回 x 的整数部分，删除所有小数
Math.sqrt(x) 				  返回 x 的平方根
Math.cbrt(x) 				  返回 x 的立方根
Math.acos(x) 				  返回 x 的反余弦
Math.acosh(x) 				  返回 x 的反双曲余弦
Math.asin(x) 				  返回 x 的反正弦
Math.asinh(x) 				  返回 x 的反双曲正弦
Math.atan(x) 				  返回 x 的反正切
Math.atanh(x) 				  返回 x 的反双曲正切
Math.atan2(y, x) 			  返回 y/x 的反正切
Math.cos(x) 				  返回 x 的余弦
Math.sin(x) 				  返回 x 的正弦
Math.tan(x) 				  返回 x 的正切
```

​		即便这些方法都是由 ECMA-262 定义的，对正弦、余弦、正切等计算的实现仍然取决于浏览器，因为计算这些值的方式有很多种。结果，这些方法的精度可能因实现而异。





#### 5.小结

JavaScript 中的对象称为引用值，几种内置的引用类型可用于创建特定类型的对象。

（1）引用值与传统面向对象编程语言中的类相似，但现实不同。

（2）Date 类型提供关于日期和时间的信息，包括当前日期、时间及相关计算。

（3）RegExp 类型是 ECMAScript 支持正则表达式的接口，提供了大多数基础的和部分高级的正则表达式功能。



​		JavaScript 比较独特的一点是，函数实际上是 Function 类型的实例，也就是说函数也是对象。

​		由于原始值包装类型的存在，JavaScript 中的原始值可以被当成对象来使用。有 3 种原始值包装类型：Boolean、Number 和 String。它们都具备如下特点。

（1）每种包装类型都映射到同名的原始类型。

（2）以读模式访问原始值时，后台会实例化一个原始值包装类型的对象，借助这个对象可以操作相应的数据。

（3）涉及原始值的语句执行完毕后，包装对象就会被销毁。



​		当代码开始执行时，全局上下文中会存在两个内置对象：Global 和 Math。其中，Global 对象在大多数 ECMAScript 实现中无法直接访问。不过，浏览器将其实现为 window 对象。所有全局变量和函数都是 Global 对象的属性。Math 对象包含辅助完成复杂计算的属性和方法。





## 第6章 集合引用类型



#### 1.Object

​		到目前为止，大多数引用值的示例使用的是 Object 类型。Object 是 ECMAScript 中最常用的类型之一。虽然 Object 的实例没有多少功能，但很适合存储和在应用程序间交换数据。

​		显式地创建 Object 的实例有两种方式。第一种是使用 new 操作符和 Object 构造函数。

```
let person = {}; // 与 new Object()相同

let person = new Object(); 
person.name = "Nicholas"; 
person.age = 29;
```

​		另一种方式是使用对象字面量表示法。对象字面量是对象定义的简写形式，目的是为了简化包含大量属性的对象的创建。使用的是对象字面量表示法：

```
let person = { 
 	name: "Nicholas", 
	age: 29 
};
```

​		左大括号（{）表示对象字面量开始，因为它出现在一个表达式上下文（expression context）中。在 ECMAScript 中，表达式上下文指的是期待返回值的上下文。



​		最后一个属性后面加上逗号在非常老的浏览器中会导致报错，但所有现代浏览器都支持这种写法。在对象字面量表示法中，属性名可以是字符串或数值。这个例子会得到一个带有属性 name、age 和 5 的对象。注意，数值属性会自动转换为字符串。

```
let person = { 
 	"name": "Nicholas", 
 	"age": 29, 
 	5: true 
};
```

**注意**

​		**在使用对象字面量表示法定义对象时，并不会实际调用 Object 构造函数。**



​		虽然使用哪种方式创建 Object 实例都可以，但实际上开发者更倾向于使用对象字面量表示法。这是因为对象字面量代码更少，看起来也更有封装所有相关数据的感觉。

​		函数 displayInfo()接收一个名为 args 的参数。这个参数可能有属性 name 或 age，也可能两个属性都有或者都没有。函数内部会使用 typeof 操作符测试每个属性是否存在，然后根据属性有无构造并显示一条消息。

```
function displayInfo(args) { 
 	let output = ""; 
 	if (typeof args.name == "string"){ 
 		output += "Name: " + args.name + "\n"; 
 	} 
 	if (typeof args.age == "number") { 
 		output += "Age: " + args.age + "\n"; 
 	} 
 	alert(output); 
} 
displayInfo({ 
 	name: "Nicholas", 
 	age: 29 
}); 
displayInfo({ 
 	name: "Greg" 
});
```

**注意**

​		**这种模式非常适合函数有大量可选参数的情况。一般来说，命名参数更直观，但在可选参数过多的时候就显得笨拙了。最好的方式是对必选参数使用命名参数，再通过一个对象字面量来封装多个可选参数。**





​		虽然属性一般是通过点语法来存取的，这也是面向对象语言的惯例，但也可以使用中括号来存取属性。在使用中括号时，要在括号内使用属性名的字符串形式。从功能上讲，这两种存取属性的方式没有区别。

```
console.log(person["name"]); // "Nicholas" 
console.log(person.name); // "Nicholas"
```



​		另外，如果属性名中包含可能会导致语法错误的字符，或者包含关键字/保留字时，也可以使用中括号语法。

因为"first name"中包含一个空格，所以不能使用点语法来访问。不过，属性名中是可以包含非字母数字字符的，这时候只要用中括号语法存取它们就行了。

```
person["first name"] = "Nicholas";
```





#### 2.Array

​		除了 Object，Array 应该就是 ECMAScript 中最常用的类型了。ECMAScript 数组也是一组有序的数据，但跟其他语言不同的是，数组中每个槽位可以存储任意类型的数据。这意味着可以创建一个数组，它的第一个元素是字符串，第二个元素是数值，第三个是对象。ECMAScript 数组也是动态大小的，会随着数据添加而自动增长。



##### 1 创建数组

​		有几种基本的方式可以创建数组。		let colors = new Array(); 

​		知道数组中元素的数量，那么可以给构造函数传入一个数值，然后 length 属性就会被自动创建并设置为这个值。

​		也可以给 Array 构造函数传入要保存的元素。

```
let colors = new Array("red", "blue", "green");
```

​		在使用 Array 构造函数时，也可以省略 new 操作符。结果是一样的

```
let colors = Array(3); // 创建一个包含 3 个元素的数组
let names = Array("Greg"); // 创建一个只包含一个元素，即字符串"Greg"的数组
```



​		另一种创建数组的方式是使用数组字面量表示法。数组字面量是在中括号中包含以逗号分隔的元素列表。

```
let colors = ["red", "blue", "green"]; // 创建一个包含 3 个元素的数组
let names = []; // 创建一个空数组
let values = [1,2,]; // 创建一个包含 2 个元素的数组
```

**注意**

​		**与对象一样，在使用数组字面量表示法创建数组不会调用 Array 构造函数。**



​		Array 构造函数还有两个 ES 6 新增的用于创建数组的静态方法：from() 和 of() 。

**from()** 用于将类数组结构转换为数组实例。

**of()** 用于将一组参数转换为数组实例。

​		Array.from()的第一个参数是一个类数组对象，即任何可迭代的结构，或者有一个 length 属性和可索引元素的结构。

```
// 字符串会被拆分为单字符数组
console.log(Array.from("Matt")); // ["M", "a", "t", "t"] 
// 可以使用 from()将集合和映射转换为一个新数组
const m = new Map().set(1, 2) 
 				   .set(3, 4); 
const s = new Set().add(1) 
 				   .add(2) 
 				   .add(3) 
 				   .add(4); 
console.log(Array.from(m)); // [[1, 2], [3, 4]] 
console.log(Array.from(s)); // [1, 2, 3, 4] 
// Array.from()对现有数组执行浅复制
const a1 = [1, 2, 3, 4]; 
const a2 = Array.from(a1); 
console.log(a1); // [1, 2, 3, 4] 
alert(a1 === a2); // false 
// 可以使用任何可迭代对象
const iter = { 
 	*[Symbol.iterator]() { 
 		yield 1; 
 		yield 2; 
 		yield 3; 
 		yield 4; 
 } 
}; 
console.log(Array.from(iter)); // [1, 2, 3, 4]

// arguments 对象可以被轻松地转换为数组
function getArgsArray() { 
 	return Array.from(arguments); 
} 
console.log(getArgsArray(1, 2, 3, 4)); // [1, 2, 3, 4]

// from()也能转换带有必要属性的自定义对象
const arrayLikeObject = { 
 	0: 1, 
 	1: 2, 
 	2: 3, 
 	3: 4, 
 	length: 4 
}; 
console.log(Array.from(arrayLikeObject)); // [1, 2, 3, 4]
```



​		Array.from()还接收第二个可选的映射函数参数。这个函数可以直接增强新数组的值，而无须像调用 Array.from().map()那样先创建一个中间数组。还可以接收第三个可选参数，用于指定映射函数中 this 的值。但这个重写的 this 值在箭头函数中不适用。

```
const a1 = [1, 2, 3, 4]; 
const a2 = Array.from(a1, x => x**2); 
const a3 = Array.from(a1, function(x) {return x**this.exponent}, {exponent: 2}); 
console.log(a2); // [1, 4, 9, 16] 
console.log(a3); // [1, 4, 9, 16]
```



​		Array.of()可以把一组参数转换为数组。这个方法用于替代在 ES6之前常用的 Array.prototype. slice.call(arguments)，一种异常笨拙的将 arguments 对象转换为数组的写法

```
console.log(Array.of(1, 2, 3, 4)); // [1, 2, 3, 4] 
console.log(Array.of(undefined)); // [undefined]
```



##### 2 数组空位

​		使用数组字面量初始化数组时，可以使用一串逗号来创建空位（hole）。ECMAScript 会将逗号之间相应索引位置的值当成空位，ES6 规范重新定义了该如何处理这些空位。

```
const options = [,,,,,]; // 创建包含 5 个元素的数组
console.log(options.length); // 5 
console.log(options); // [,,,,,]
```

​		ES6 新增的方法和迭代器与早期 ECMAScript 版本中存在的方法行为不同。ES6 新增方法普遍将这

些空位当成存在的元素，只不过值为 undefined

```
const options = [1,,,,5]; 
for (const option of options) { 
 	console.log(option === undefined); 
} 
// false 
// true 
// true 
// true 
// false
```



​		ES6 之前的方法则会忽略这个空位，但具体的行为也会因方法而异

```
const options = [1,,,,5]; 

// map()会跳过空位置
console.log(options.map(() => 6)); // [6, undefined, undefined, undefined, 6] 

// join()视空位置为空字符串
console.log(options.join('-')); // "1----5"
```



**注意**

​		**由于行为不一致和存在性能隐患，因此实践中要避免使用数组空位。如果确实需要空位，则可以显式地用 undefined 值代替。**





##### 3 数组索引

​		要取得或设置数组的值，需要使用中括号并提供相应值的数字索引。。如果把一个值设置给超过数组最大索引的索引，就像示例中的 colors[3]，则数组长度会自动扩展到该索引值加 1（示例中设置的索引 3，所以数组长度变成了 4）。

```
let colors = ["red", "blue", "green"]; // 定义一个字符串数组
alert(colors[0]); // 显示第一项
colors[2] = "black"; // 修改第三项
colors[3] = "brown"; // 添加第四项
```

​		数组中元素的数量保存在 length 属性中，这个属性始终返回 0 或大于 0 的值。数组 length 属性的独特之处在于，它不是只读的。通过修改 length 属性，可以从数组末尾删除或添加元素。如果将 length 设置为大于数组元素数的值，则新添加的元素都将以 undefined 填充。

​		使用 length 属性可以方便地向数组末尾添加元素。

```
let colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
colors[colors.length] = "black"; // 添加一种颜色（位置 3）
colors[colors.length] = "brown"; // 再添加一种颜色（位置 4）
```

​		数组中最后一个元素的索引始终是 length - 1，因此下一个新增槽位的索引就是 length。每次

在数组最后一个元素后面新增一项，数组的 length 属性都会自动更新，以反映变化。

**注意**

​		**数组最多可以包含 4 294 967 295 个元素，这对于大多数编程任务应该足够了。如果尝试添加更多项，则会导致抛出错误。以这个最大值作为初始值创建数组，可能导致脚本运行时间过长的错误。**





##### 4 检测数组

​		一个经典的 ECMAScript 问题是判断一个对象是不是数组。在只有一个网页（因而只有一个全局作用域）的情况下，使用 instanceof 操作符就足矣。

```
if (value instanceof Array){ 
	 // 操作数组
}
```

​		使用 instanceof 的问题是假定只有一个全局执行上下文。如果网页里有多个框架，则可能涉及两个不同的全局执行上下文，因此就会有两个不同版本的 Array 构造函数。如果要把数组从一个框架传给另一个框架，则这个数组的构造函数将有别于在第二个框架内本地创建的数组。



​		为解决这个问题，ECMAScript 提供了 Array.isArray()方法。这个方法的目的就是确定一个值是否为数组，而不用管它是在哪个全局执行上下文中创建的

```
if (Array.isArray(value)){ 
 	// 操作数组
}
```



##### 5 迭代器方法

​		在 ES6 中，Array 的原型上暴露了 3 个用于检索数组内容的方法：keys()、values() 和 entries()。

keys() 返回数组索引的迭代器；

values() 返回数组元素的迭代器；

entries() 返回索引/值对的迭代器。

```
const a = ["foo", "bar", "baz", "qux"]; 

// 因为这些方法都返回迭代器，所以可以将它们的内容
// 通过 Array.from()直接转换为数组实例
const aKeys = Array.from(a.keys()); 
const aValues = Array.from(a.values()); 
const aEntries = Array.from(a.entries()); 

console.log(aKeys); // [0, 1, 2, 3] 
console.log(aValues); // ["foo", "bar", "baz", "qux"] 
console.log(aEntries); // [[0, "foo"], [1, "bar"], [2, "baz"], [3, "qux"]] 

使用 ES6 的解构可以非常容易地在循环中拆分键/值对：
const a = ["foo", "bar", "baz", "qux"]; 
for (const [idx, element] of a.entries()) { 
 	alert(idx); 
	alert(element); 
} 
// 0 
// foo 
// 1 
// bar 
// 2 
// baz 
// 3 
// qux
```



##### 6 复制和填充方法

​		ES6 新增了两个方法：批量复制方法 copyWithin()，以及填充数组方法 fill()。这两个方法的函数签名类似，都需要指定既有数组实例上的一个范围，包含开始索引，不包含结束索引。使用这个方法不会改变数组的大小。

​		使用 fill()方法可以向一个已有的数组中插入全部或部分相同的值。开始索引用于指定开始填充的位置，它是可选的。如果不提供结束索引，则一直填充到数组末尾。负值索引从数组末尾开始计算。也可以将负索引想象成数组长度加上它得到的一个正索引

```
const zeroes = [0, 0, 0, 0, 0]; 

// 用 5 填充整个数组
zeroes.fill(5); 
console.log(zeroes); // [5, 5, 5, 5, 5] 
zeroes.fill(0); // 重置

// 用 6 填充索引大于等于 3 的元素
zeroes.fill(6, 3); 
console.log(zeroes); // [0, 0, 0, 6, 6] 
zeroes.fill(0); // 重置

// 用 7 填充索引大于等于 1 且小于 3 的元素
zeroes.fill(7, 1, 3); 
console.log(zeroes); // [0, 7, 7, 0, 0]; 
zeroes.fill(0); // 重置

// 用 8 填充索引大于等于 1 且小于 4 的元素
// (-4 + zeroes.length = 1) 
// (-1 + zeroes.length = 4) 
zeroes.fill(8, -4, -1); 
console.log(zeroes); // [0, 8, 8, 8, 0];
```



​		fill()静默忽略超出数组边界、零长度及方向相反的索引范围。

```
const zeroes = [0, 0, 0, 0, 0]; 
// 索引过低，忽略

zeroes.fill(1, -10, -6); 
console.log(zeroes); // [0, 0, 0, 0, 0] 

// 索引过高，忽略
zeroes.fill(1, 10, 15); 
console.log(zeroes); // [0, 0, 0, 0, 0] 

// 索引反向，忽略
zeroes.fill(2, 4, 2); 
console.log(zeroes); // [0, 0, 0, 0, 0] 

// 索引部分可用，填充可用部分
zeroes.fill(4, 3, 10) 
console.log(zeroes); // [0, 0, 0, 4, 4]
```

​		与 fill()不同，copyWithin()会按照指定范围浅复制数组中的部分内容，然后将它们插入到指定索引开始的位置。开始索引和结束索引则与 fill()使用同样的计算方法。

```
let ints, 
 reset = () => ints = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]; 
reset(); 

// 从 ints 中复制索引 0 开始的内容，插入到索引 5 开始的位置
// 在源索引或目标索引到达数组边界时停止
ints.copyWithin(5); 
console.log(ints); // [0, 1, 2, 3, 4, 0, 1, 2, 3, 4] 
reset(); 

// 从 ints 中复制索引 5 开始的内容，插入到索引 0 开始的位置
ints.copyWithin(0, 5); 
console.log(ints); // [5, 6, 7, 8, 9, 5, 6, 7, 8, 9]
reset(); 
// 从 ints 中复制索引 0 开始到索引 3 结束的内容
// 插入到索引 4 开始的位置
ints.copyWithin(4, 0, 3); 
alert(ints); // [0, 1, 2, 3, 0, 1, 2, 7, 8, 9] 
reset(); 
// JavaScript 引擎在插值前会完整复制范围内的值
// 因此复制期间不存在重写的风险
ints.copyWithin(2, 0, 6); 
alert(ints); // [0, 1, 0, 1, 2, 3, 4, 5, 8, 9] 
reset(); 

// 支持负索引值，与 fill()相对于数组末尾计算正向索引的过程是一样的
ints.copyWithin(-4, -7, -3); 
alert(ints); // [0, 1, 2, 3, 4, 5, 3, 4, 5, 6] 

copyWithin()静默忽略超出数组边界、零长度及方向相反的索引范围：
let ints, 
 reset = () => ints = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]; 
reset(); 

// 索引过低，忽略
ints.copyWithin(1, -15, -12); 
alert(ints); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]; 
reset() 

// 索引过高，忽略
ints.copyWithin(1, 12, 15); 
alert(ints); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]; 
reset(); 

// 索引反向，忽略
ints.copyWithin(2, 4, 2); 
alert(ints); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]; 
reset(); 

// 索引部分可用，复制、填充可用部分
ints.copyWithin(4, 7, 10) 
alert(ints); // [0, 1, 2, 3, 7, 8, 9, 7, 8, 9];
```



##### 7 转换方法

​		所有对象都有 toLocaleString()、toString()和 valueOf()方法。

valueOf() 返回的还是数组本身。

toString() 返回由数组中每个值的等效字符串拼接而成的一个逗号分隔的字符串。也就是说，对数组的每个值都会				  调用其 toString()方法，以得到最终的字符串。



​		toLocaleString() 方法也可能返回跟 toString()和 valueOf() 相同的结果，但也不一定。在调用数组的 toLocaleString() 方法时，会得到一个逗号分隔的数组值的字符串。它与另外两个方法唯一的区别是，为了得到最终的字符串，会调用数组每个值的 toLocaleString() 方法，而不是 toString() 方法。

​		在调用数组的 toLocaleString()方法时，结果变成了"Nikolaos, Grigorios"，这是因为调用了数组每一项的 toLocaleString()方法。

```
let person1 = { 
 	toLocaleString() { 
 		return "Nikolaos"; 
 	}, 
 	toString() { 
 		return "Nicholas"; 
 	} 
}; 
let person2 = { 
 	toLocaleString() { 
 		return "Grigorios"; 
 	}, 
 	toString() { 
 		return "Greg"; 
 	} 
}; 
let people = [person1, person2]; 
alert(people); // Nicholas,Greg 
alert(people.toString()); // Nicholas,Greg 
alert(people.toLocaleString()); // Nikolaos,Grigorios
```



​		继承的方法 toLocaleString()以及 toString()都返回数组值的逗号分隔的字符串。如果想使用不同的分隔符，则可以使用 join()方法。join()方法接收一个参数，即字符串分隔符，返回包含所有项的字符串。

```
let colors = ["red", "green", "blue"]; 
alert(colors.join(",")); // red,green,blue 
alert(colors.join("||")); // red||green||blue
```

**注意**

​		**如果数组中某一项是 null 或 undefined，则在 join()、toLocaleString()、toString() 和 valueOf() 返回的结果中会以空字符串表示。**





##### 8 栈方法

​		ECMAScript 给数组提供几个方法，让它看起来像是另外一种数据结构。数据项的插入（称为推入，push）和删除（称为弹出，pop）只在栈的一个地方发生，即栈顶。ECMAScript 数组提供了 push()和 pop()方法，以实现类似栈的行为。

​		push()方法接收任意数量的参数，并将它们添加到数组末尾，返回数组的最新长度。pop()方法则用于删除数组的最后一项，同时减少数组的 length 值，返回被删除的项。





##### 9 队列方法

​		就像栈是以 LIFO 形式限制访问的数据结构一样，队列以先进先出形式限制访问。队列在列表末尾添加数据，但从列表开头获取数据。

shift()：它会删除数组的第一项并返回它，然后数组长度减 1。使用 shift()和 push()，可以把数组当成队列来使用

```
let item = colors.shift(); // 取得第一项

let item = colors.pop(); // 取得最后一项
```

​		ECMAScript 也为数组提供了 unshift()方法。顾名思义，unshift()就是执行跟 shift()相反的操作：在数组开头添加任意多个值，然后返回新的数组长度。

通过使用 unshift()和 pop()，可以在相反方向上模拟队列，即在数组开头添加新数据，在数组末尾取得数据

```
count = colors.unshift("black"); // 再数组开头推入一项
```





##### 10 排序方法

​		数组有两个方法可以用来对元素重新排序：reverse()和 sort()。

reverse()： 方法就是将数组元素反向排列。

sort()：默认情况下，sort()会按照升序重新排列数组元素，即最小的值在前面，最大的值在后面。为此，sort()会			 在每一项上调用 String()转型函数，然后比较字符串来决定顺序。

```
let values = [1, 2, 3, 4, 5]; 
values.reverse(); 
alert(values); // 5,4,3,2,1
```



**sort()：**

​		即使 5 小于 10，但字符串"10"在字符串"5"的前头，所以 10 还是会排到 5 前面。很明显，这在多数情况下都不是最合适的。为此，sort()方法可以接收一个比较函数

```
let values = [0, 1, 5, 10, 15]; 
values.sort(); 
alert(values); // 0,1,10,15,5
```



​		比较函数接收两个参数，如果第一个参数应该排在第二个参数前面，就返回负值；如果两个参数相

等，就返回 0；如果第一个参数应该排在第二个参数后面，就返回正值。

```
function compare(value1, value2) { 
 	if (value1 < value2) { 
 		return -1; 
 	} else if (value1 > value2) { 
 		return 1; 
 	} else { 
		return 0; 
 	} 
}
```

这个比较函数可以适用于大多数数据类型，可以把它当作参数传给 sort()方法

```
let values = [0, 1, 5, 10, 15]; 
values.sort(compare); 
alert(values); // 0,1,5,10,15
```

​		在给 sort()方法传入比较函数后，数组中的数值在排序后保持了正确的顺序。当然，比较函数也

可以产生降序效果，只要把返回值交换一下即可



​		此外，这个比较函数还可简写为一个箭头函数

```
let values = [0, 1, 5, 10, 15]; 
values.sort((a, b) => a < b ? 1 : a > b ? -1 : 0); 
alert(values); // 15,10,5,1,0
```

**注意**

​		**reverse() 和 sort() 都返回调用他们的数组的引用。**



​		如果数组的元素是数值，或者是其 valueOf()方法返回数值的对象（如 Date 对象），这个比较函数还可以写得更简单，因为这时可以直接用第二个值减去第一个值

```
function compare(value1, value2){ 
 	return value2 - value1; 
}
```





##### 11 操作方法

​		对于数组中的元素，有很多操作方法。concat()方法可以在现有数组全部元素基础上创建一个新数组。它首先会创建一个当前数组的副本，然后再把它的参数添加到副本末尾，最后返回这个新构建的数组。如果传入一个或多个数组，则 concat()会把这些数组的每一项都添加到结果数组。如果参数不是数组，则直接把它们添加到结果数组末尾。

```
let colors = ["red", "green", "blue"]; 
let colors2 = colors.concat("yellow", ["black", "brown"]); 

console.log(colors); // ["red", "green","blue"] 
console.log(colors2); // ["red", "green", "blue", "yellow", "black", "brown"]
```



​		打平数组参数的行为可以重写，方法是在参数数组上指定一个特殊的符号：Symbol.isConcatSpreadable。这个符号能够阻止 concat()打平参数数组。相反，把这个值设置为 true 可以强制打平类数组对象：

```
let colors = ["red", "green", "blue"]; 
let newColors = ["black", "brown"]; 
let moreNewColors = { 
 	[Symbol.isConcatSpreadable]: true, 
 	length: 2, 
 	0: "pink", 
 	1: "cyan" 
}; 
newColors[Symbol.isConcatSpreadable] = false; 

// 强制不打平数组
let colors2 = colors.concat("yellow", newColors);

// 强制打平类数组对象
let colors3 = colors.concat(moreNewColors); 
console.log(colors); // ["red", "green", "blue"] 
console.log(colors2); // ["red", "green", "blue", "yellow", ["black", "brown"]] 
console.log(colors3); // ["red", "green", "blue", "pink", "cyan"]
```



​		

​		 slice()  用于创建一个包含原有数组中一个或多个元素的新数组。slice()方法可以接收一个或两个参数：返回元素的开始索引和结束索引。如果只有一个参数，则 slice()会返回该索引到数组末尾的所有元素。

​		slice() 返回从开始索引到结束索引对应的所有元素，其中不包含结束索引对应的元素。记住，这个操作不影响原始数组。

```
let colors = ["red", "green", "blue", "yellow", "purple"]; 
let colors2 = colors.slice(1); 
let colors3 = colors.slice(1, 4); 

alert(colors2); // green,blue,yellow,purple 
alert(colors3); // green,blue,yellow
```

**注意**

​		**如果 slice()的参数有负值，那么就以数值长度加上这个负值的结果确定位置。比如，在包含 5 个元素的数组上调用 slice(-2,-1)，就相当于调用 slice(3,4)。如果结束位置小于开始位置，则返回空数组。**





​		或许最强大的数组方法就属 splice()了，使用它的方式可以有很多种。splice()的主要目的是在数组中间插入元素，但有 3 种不同的方式使用这个方法。

（1）**删除**：需要给 splice()传 2 个参数：要删除的第一个元素的位置和要删除的元素数量。可以从数组中删除任		 意多个元素，比如 splice(0, 2)会删除前两个元素。

（2）**插入**：需要给 splice()传 3 个参数：开始位置、0（要删除的元素数量）和要插入的元素，可以在数组中指定		 的位置插入元素。第三个参数之后还可以传第四个、第五个参数，乃至任意多个要插入的元素。

​		 比如，splice(2, 0, "red", "green")会从数组位置 2 开始插入字符串"red"和"green"。

（3）**替换**：splice()在删除元素的同时可以在指定位置插入新元素，同样要传入 3 个参数：开始位置、要删除元素		 的数量和要插入的任意多个元素。要插入的元素数量不一定跟删除的元素数量一致。比如，

​		 splice(2, 1,  "red", "green")会在位置 2 删除一个元素，然后从该位置开始向数组中插入"red"和"green"。





##### 12 搜索和位置方法

​		ECMAScript 提供两类搜索数组的方法：按严格相等搜索和按断言函数搜索。



###### （1）严格相等

​		ECMAScript 提供了 3 个严格相等的搜索方法：indexOf()、lastIndexOf()和 includes()。其中，前两个方法在所有版本中都可用，而第三个方法是 ECMAScript 7 新增的。这些方法都接收两个参数：要查找的元素和一个可选的起始搜索位置。indexOf()和 includes()方法从数组前头（第一项）开始向后搜索，而 lastIndexOf()从数组末尾（最后一项）开始向前搜索。

​		indexOf() 和 lastIndexOf() 都返回要查找的元素在数组中的位置，如果没找到则返回1。includes() 返回布尔值，表示是否至少找到一个与指定元素匹配的项。在比较第一个参数跟数组每一项时，会使用全等（===）比较，也就是说两项必须严格相等。

```
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]; 
alert(numbers.indexOf(4)); // 3 
alert(numbers.lastIndexOf(4)); // 5 
alert(numbers.includes(4)); // true

alert(numbers.indexOf(4, 4)); // 5 
alert(numbers.lastIndexOf(4, 4)); // 3 
alert(numbers.includes(4, 7)); // false 

let person = { name: "Nicholas" }; 
let people = [{ name: "Nicholas" }]; 
let morePeople = [person]; 

alert(people.indexOf(person)); // -1 
alert(morePeople.indexOf(person)); // 0 
alert(people.includes(person)); // false 
alert(morePeople.includes(person)); // true
```



###### （2）断言函数

​		ECMAScript 也允许按照定义的断言函数搜索数组，每个索引都会调用这个函数。断言函数的返回值决定了相应索引的元素是否被认为匹配。

​		断言函数接收 3 个参数：元素、索引和数组本身。其中元素是数组中当前搜索的元素，索引是当前元素的索引，而数组就是正在搜索的数组。断言函数返回真值，表示是否匹配。

​		find()和 findIndex()方法使用了断言函数。这两个方法都从数组的最小索引开始。find()返回第一个匹配的元素，findIndex()返回第一个匹配元素的索引。这两个方法也都接收第二个可选的参数，用于指定断言函数内部 this 的值。

```
const people = [ 
 { 
 	name: "Matt", 
 	age: 27 
 }, 
 { 
 	name: "Nicholas", 
 	age: 29 
 } 
]; 
alert(people.find((element, index, array) => element.age < 28)); 
// {name: "Matt", age: 27} 
alert(people.findIndex((element, index, array) => element.age < 28)); 
// 0


找到匹配项后，这两个方法都不再继续搜索。

const evens = [2, 4, 6]; 
// 找到匹配后，永远不会检查数组的最后一个元素
evens.find((element, index, array) => { 
 	console.log(element); 
 	console.log(index); 
 	console.log(array); 
	return element === 4; 
}); 
// 2 
// 0 
// [2, 4, 6] 
// 4 
// 1 
// [2, 4, 6]
```





##### 13 迭代方法

​		ECMAScript 为数组定义了 5 个迭代方法。每个方法接收两个参数：以每一项为参数运行的函数，以及可选的作为函数运行上下文的作用域对象（影响函数中 this 的值）。传给每个方法的函数接收 3个参数：数组元素、元素索引和数组本身。因具体方法而异，这个函数的执行结果可能会也可能不会影响方法的返回值。

（1）every()：对数组每一项都运行传入的函数，如果对每一项函数都返回 true，则这个方法返回 true。

（2）filter()：对数组每一项都运行传入的函数，函数返回 true 的项会组成数组之后返回。

（3）forEach()：对数组每一项都运行传入的函数，没有返回值。

（4）map()：对数组每一项都运行传入的函数，返回由每次函数调用的结果构成的数组。

（5）some()：对数组每一项都运行传入的函数，如果有一项函数返回 true，则这个方法返回 true。

这些方法都不改变调用它们的数组。



**every() 和 some()**

​		在这些方法中，every()和 some()是最相似的，都是从数组中搜索符合某个条件的元素。

调用了 every()和 some()，传入的函数都是在给定项大于 2 时返回 true。every()返 回 false 是因为并不是每一项都能达到要求。而 some()返回 true 是因为至少有一项满足条件。

```
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
let everyResult = numbers.every((item, index, array) => item > 2); 
alert(everyResult); // false 

let someResult = numbers.some((item, index, array) => item > 2); 
alert(someResult); // true
```



**filter()**

​		filter()方法。这个方法基于给定的函数来决定某一项是否应该包含在它返回的数组中。比如，要返回一个所有数值都大于 2 的数组

```
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]; 
let filterResult = numbers.filter((item, index, array) => item > 2); 
alert(filterResult); // 3,4,5,4,3
```



**map()**

​		map()方法也会返回一个数组。这个数组的每一项都是对原始数组中同样位置的元素运行传入函数而返回的结果。例如，可以将一个数组中的每一项都乘以 2，并返回包含所有结果的数组

```
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]; 
let mapResult = numbers.map((item, index, array) => item * 2); 
alert(mapResult); // 2,4,6,8,10,8,6,4,2
```



**forEach()**

​		forEach()方法。这个方法只会对每一项运行传入的函数，没有返回值。本质上，forEach()方法相当于使用 for 循环遍历数组。

```
let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1]; 
numbers.forEach((item, index, array) => { 
 	// 执行某些操作 
});
```





##### 14 归并方法

​		ECMAScript 为数组提供了两个归并方法：reduce()和 reduceRight()。这两个方法都会迭代数组的所有项，并在此基础上构建一个最终返回值。

**reduce()**  方法从数组第一项开始遍历到最后一项。

**reduceRight()**  从最后一项开始遍历至第一项。



​		这两个方法都接收两个参数：对每一项都会运行的归并函数，以及可选的以之为归并起点的初始值。传给 reduce()和 reduceRight()的函数接收 4 个参数：上一个归并值、当前项、当前项的索引和数组本身。这个函数返回的任何值都会作为下一次调用同一个函数的第一个参数。如果没有给这两个方法传入可选的第二个参数（作为归并起点值），则第一次迭代将从数组的第二项开始，因此传给归并函数的第一个参数是数组的第一项，第二个参数是数组的第二项。



**reduce()**

​		第一次执行归并函数时，prev 是 1，cur 是 2。第二次执行时，prev 是 3（1 + 2），cur 是 3（数组第三项）。如此递进，直到把所有项都遍历一次，最后返回归并结果。

```
let values = [1, 2, 3, 4, 5]; 
let sum = values.reduce((prev, cur, index, array) => prev + cur); 

alert(sum); // 15
```



**reduceRight()**

​		reduceRight()方法与之类似，只是方向相反。在这里，第一次调用归并函数时 prev 是 5，而 cur 是 4。当然，最终结果相同，因为归并操作都是简单的加法。

```
let values = [1, 2, 3, 4, 5]; 
let sum = values.reduceRight(function(prev, cur, index, array){ 
 	return prev + cur; 
}); 
alert(sum); // 15
```



​		究竟是使用 reduce()还是 reduceRight()，只取决于遍历数组元素的方向。除此之外，这两个方法没什么区别。





#### 3.定型数组

​		定型数组是ECMAScript 新增的结构，目的是提升向原生库传输数据的效率。。实际上，JavaScript 并没有“TypedArray”类型，它所指的其实是一种特殊的包含数值类型的数组。



##### 1 历史

​		早在 2006 年，Mozilla、Opera 等浏览器提供商就实验性地在浏览器中增加了用于渲染复杂图形应用程序的编程平台，无须安装任何插件。其目标是开发一套 JavaScript API，从而充分利用 3D 图形 API 和 GPU 加速，以便

在<canvas>元素上渲染复杂的图形。



###### （1）WebGL

​		最后的 JavaScript API 是基于 OpenGL ES 2.0 规范的。OpenGL ES是 OpenGL 专注于 2D 和 3D 计算机图形的子集。这个新 API 被命名为 WebGL（Web Graphics Library），于 2011 年发布 1.0 版。有了它，开发者就能够编写涉及复杂图形的应用程序，它会被兼容 WebGL 的浏览器原生解释执行。

​		在 WebGL 的早期版本中，因为 JavaScript 数组与原生数组之间不匹配，所以出现了性能问题。图形驱动程序 API 通常不需要以 JavaScript 默认双精度浮点格式传递给它们的数值，而这恰恰是 JavaScript数组在内存中的式。

因此，每次 WebGL 与 JavaScript 运行时之间传递数组时，WebGL 绑定都需要在目标环境分配新数组，以其当前格式迭代数组，然后将数值转型为新数组中的适当格式，而这些要花费很多时间。



###### （2）定型数组

​		这当然是难以接受的，Mozilla 为解决这个问题而实现了 CanvasFloatArray。这是一个提供JavaScript 接口的、C 语言风格的浮点值数组。JavaScript 运行时使用这个类型可以分配、读取和写入数组。

​		这个数组可以直接传给底层图形驱动程序 API，也可以直接从底层获取到。最终，CanvasFloatArray

变成了 Float32Array，也就是今天定型数组中可用的第一个“类型”。





##### 2 ArrayBuffer

​		Float32Array 实际上是一种“视图”，可以允许 JavaScript 运行时访问一块名为 ArrayBuffer 的预分配内存。ArrayBuffer 是所有定型数组及视图引用的基本单位。

**注意**

​		**SharedArrayBuffer 是 ArrayBuffer 的一个变体，可以无须复制就在执行上下文间传递它。**



​		ArrayBuffer() 是一个普通的 JavaScript 构造函数，可以用于在内存中分配特定数量的字节空间。

```
const buf = new ArrayBuffer(16); // 在内存中分配 16 字节
alert(buf.byteLength); // 16
```

​		ArrayBuffer 一经创建就不能再调整大小，不过，可以使用 slice() 复制其全部或部分到一个新实例中。

```
const buf1 = new ArrayBuffer(16); 
const buf2 = buf1.slice(4, 12); 
alert(buf2.byteLength); // 8
```



​		ArrayBuffer 某种程度上类似于 C++的 malloc()，但也有几个明显的区别。

（1）**malloc()** 在分配失败时会返回一个 null 指针。ArrayBuffer 在分配失败时会抛出错误。

（2）**malloc()** 可以利用虚拟内存，因此最大可分配尺寸只受可寻址系统内存限制。ArrayBuffer分配的内存不能		  超过 Number.MAX_SAFE_INTEGER（253  1）字节。

（3）**malloc()** 调用成功不会初始化实际的地址。声明 ArrayBuffer 则会将所有二进制位初始化为 0。

（4）通过 malloc()分配的堆内存除非调用 free()或程序退出，否则系统不能再使用。而通过声明ArrayBuffer 分配		  的堆内存可以被当成垃圾回收，不用手动释放。



​		**不能仅通过对 ArrayBuffer 的引用就读取或写入其内容。要读取或写入 ArrayBuffer，就必须通过视图。视图有不同的类型，但引用的都是 ArrayBuffer 中存储的二进制数据。**





##### 3 DataView

​		第一种允许你读写 ArrayBuffer 的视图是 DataView。这个视图专为文件 I/O 和网络 I/O 设计，其API 支持对缓冲数据的高度控制，但相比于其他类型的视图性能也差一些。DataView 对缓冲内容没有任何预设，也不能迭代。

​		必须在对已有的 ArrayBuffer 读取或写入时才能创建 DataView 实例。这个实例可以使用全部或部分 ArrayBuffer，且维护着对该缓冲实例的引用，以及视图在缓冲中开始的位置。

```
const buf = new ArrayBuffer(16);

// DataView 默认使用整个 ArrayBuffer 
const fullDataView = new DataView(buf); 
alert(fullDataView.byteOffset); // 0 
alert(fullDataView.byteLength); // 16 
alert(fullDataView.buffer === buf); // true 

// 构造函数接收一个可选的字节偏移量和字节长度
// byteOffset=0 表示视图从缓冲起点开始
// byteLength=8 限制视图为前 8 个字节
const firstHalfDataView = new DataView(buf, 0, 8); 
alert(firstHalfDataView.byteOffset); // 0 
alert(firstHalfDataView.byteLength); // 8 
alert(firstHalfDataView.buffer === buf); // true 

// 如果不指定，则 DataView 会使用剩余的缓冲
// byteOffset=8 表示视图从缓冲的第 9 个字节开始
// byteLength 未指定，默认为剩余缓冲
const secondHalfDataView = new DataView(buf, 8); 
alert(secondHalfDataView.byteOffset); // 8
alert(secondHalfDataView.byteLength); // 8 
alert(secondHalfDataView.buffer === buf); // true
```



​		要通过 DataView 读取缓冲，还需要几个组件。

（1）首先是要读或写的字节偏移量。可以看成 DataView 中的某种“地址”。

（2）DataView 应该使用 ElementType 来实现 JavaScript 的 Number 类型到缓冲内二进制格式的转换。

（3）最后是内存中值的字节序。默认为大端字节序。



###### （1）ElementType

​		DataView 对储存在缓冲内的数据类型没有预定。它暴露的 API 强制开发者在读、写时指定一个 ElementType，然后 DataView 就会忠实地为读、写而完成相应的转换。

​		ECMAScript 6 支持 8 种不同的 ElementType（见下表）。

```
ElementType   字 节 	说 明 			  等价的 C 类型 				值的范围
Int8 			1 	 8 位有符号整数 		    signed char 			-128~127 
Uint8 			1 	 8 位无符号整数 		    unsigned char 			0~255 
Int16 			2 	 16 位有符号整数 		    short 					-32 768~32 767 
Uint16 			2 	 16 位无符号整数		    unsigned short 			0~65 535 
Int32 			4 	 32 位有符号整数 		    int 			-2 147 483 648~2 147 483 647 
Uint32 			4 	 32 位无符号整数		    unsigned int 			0~4 294 967 295 
Float32 	    4 	 32 位 IEEE-754 浮点数   float 				-3.4e+38~+3.4e+38 
Float64			8 	 64 位 IEEE-754 浮点数   double 			-1.7e+308~+1.7e+308
```



​		DataView 为上表中的每种类型都暴露了 get 和 set 方法，这些方法使用 byteOffset（字节偏移量）定位要读取或写入值的位置。类型是可以互换使用的。

```
// 在内存中分配两个字节并声明一个 DataView 
const buf = new ArrayBuffer(2); 
const view = new DataView(buf);

// 说明整个缓冲确实所有二进制位都是 0 
// 检查第一个和第二个字符
alert(view.getInt8(0)); // 0 
alert(view.getInt8(1)); // 0 
// 检查整个缓冲
alert(view.getInt16(0)); // 0 

// 将整个缓冲都设置为 1 
// 255 的二进制表示是 11111111（2^8 - 1）
view.setUint8(0, 255); 

// DataView 会自动将数据转换为特定的 ElementType 
// 255 的十六进制表示是 0xFF 
view.setUint8(1, 0xFF); 

// 现在，缓冲里都是 1 了
// 如果把它当成二补数的有符号整数，则应该是-1 
alert(view.getInt16(0)); // -1
```



###### （2）字节序

​		“字节序”指的是计算系统维护的一种字节顺序的约定。DataView 只支持两种约定：大端字节序和小端字节序。

**大端字节序 **也称为“网络字节序”，意思是最高有效位保存在第一个字节，而最低有效位保存在最后一个字节。

**小端字节序 **正好相反，即最低有效位保存在第一个字节，最高有效位保存在最后一个字节。



​		JavaScript 运行时所在系统的原生字节序决定了如何读取或写入字节，但 DataView 并不遵守这个约定。对一段内存而言，DataView 是一个中立接口，它会遵循你指定的字节序。DataView 的所有 API 方法都以大端字节序作为默认值，但接收一个可选的布尔值参数，设置为 true 即可启用小端字节序。

```
// 在内存中分配两个字节并声明一个 DataView 
const buf = new ArrayBuffer(2); 
const view = new DataView(buf);

// 填充缓冲，让第一位和最后一位都是 1 
view.setUint8(0, 0x80); // 设置最左边的位等于 1 
view.setUint8(1, 0x01); // 设置最右边的位等于 1 

// 缓冲内容（为方便阅读，人为加了空格）
// 0x8 0x0 0x0 0x1 
// 1000 0000 0000 0001 

// 按大端字节序读取 Uint16 
// 0x80 是高字节，0x01 是低字节
// 0x8001 = 2^15 + 2^0 = 32768 + 1 = 32769 
alert(view.getUint16(0)); // 32769 

// 按小端字节序读取 Uint16 
// 0x01 是高字节，0x80 是低字节
// 0x0180 = 2^8 + 2^7 = 256 + 128 = 384 
alert(view.getUint16(0, true)); // 384 

// 按大端字节序写入 Uint16 
view.setUint16(0, 0x0004); 

// 缓冲内容（为方便阅读，人为加了空格）
// 0x0 0x0 0x0 0x4 
// 0000 0000 0000 0100 
alert(view.getUint8(0)); // 0 
alert(view.getUint8(1)); // 4 

// 按小端字节序写入 Uint16 
view.setUint16(0, 0x0002, true);

// 缓冲内容（为方便阅读，人为加了空格）
// 0x0 0x2 0x0 0x0 
// 0000 0010 0000 0000 
alert(view.getUint8(0)); // 2 
alert(view.getUint8(1)); // 0
```



###### （3）边界情形

​		DataView 完成读、写操作的前提是必须有充足的缓冲区，否则就会抛出 RangeError：

```
const buf = new ArrayBuffer(6); 
const view = new DataView(buf);

// 尝试读取部分超出缓冲范围的值
view.getInt32(4); 
// RangeError 

// 尝试读取超出缓冲范围的值
view.getInt32(8); 
// RangeError 

// 尝试读取超出缓冲范围的值
view.getInt32(-1); 
// RangeError 

// 尝试写入超出缓冲范围的值
view.setInt32(4, 123); 
// RangeError
```



​		DataView 在写入缓冲里会尽最大努力把一个值转换为适当的类型，后备为 0。如果无法转换，则抛出错误：

```
const buf = new ArrayBuffer(1); 
const view = new DataView(buf);

view.setInt8(0, 1.5); 
alert(view.getInt8(0)); // 1

view.setInt8(0, [4]); 
alert(view.getInt8(0)); // 4

view.setInt8(0, 'f'); 
alert(view.getInt8(0)); // 0 

view.setInt8(0, Symbol()); 
// TypeError
```





##### 4 定型数组

​		定型数组是另一种形式的 ArrayBuffer 视图。虽然概念上与 DataView 接近，但定型数组的区别在于，它特定于一种 ElementType 且遵循系统原生的字节序。相应地，定型数组提供了适用面更广的API 和更高的性能。设计定型数组的目的就是提高与 WebGL 等原生库交换二进制数据的效率。

```
// 创建一个 12 字节的缓冲
const buf = new ArrayBuffer(12); 
// 创建一个引用该缓冲的 Int32Array 
const ints = new Int32Array(buf); 
// 这个定型数组知道自己的每个元素需要 4 字节
// 因此长度为 3 
alert(ints.length); // 3

// 创建一个长度为 6 的 Int32Array 
const ints2 = new Int32Array(6); 
// 每个数值使用 4 字节，因此 ArrayBuffer 是 24 字节
alert(ints2.length); 			 // 6 
// 类似 DataView，定型数组也有一个指向关联缓冲的引用
alert(ints2.buffer.byteLength);  // 24 

// 创建一个包含[2, 4, 6, 8]的 Int32Array 
const ints3 = new Int32Array([2, 4, 6, 8]); 
alert(ints3.length); 				// 4 
alert(ints3.buffer.byteLength); 	// 16 
alert(ints3[2]); 					// 6 

// 通过复制 ints3 的值创建一个 Int16Array 
const ints4 = new Int16Array(ints3); 
// 这个新类型数组会分配自己的缓冲
// 对应索引的每个值会相应地转换为新格式
alert(ints4.length); 				// 4 
alert(ints4.buffer.byteLength); 	// 8 
alert(ints4[2]); 					// 6

// 基于普通数组来创建一个 Int16Array 
const ints5 = Int16Array.from([3, 5, 7, 9]); 
alert(ints5.length); 			// 4 
alert(ints5.buffer.byteLength); // 8 
alert(ints5[2]); 				// 7 

// 基于传入的参数创建一个 Float32Array 
const floats = Float32Array.of(3.14, 2.718, 1.618); 
alert(floats.length); 			 // 3 
alert(floats.buffer.byteLength); // 12 
alert(floats[2]); 				 // 1.6180000305175781
```



​		定型数组的构造函数和实例都有一个 BYTES_PER_ELEMENT 属性，返回该类型数组中每个元素的大小：

```
alert(Int16Array.BYTES_PER_ELEMENT); // 2 
alert(Int32Array.BYTES_PER_ELEMENT); // 4 

const ints = new Int32Array(1), 
 floats = new Float64Array(1); 
 
alert(ints.BYTES_PER_ELEMENT); // 4 
alert(floats.BYTES_PER_ELEMENT); // 8
```



​		如果定型数组没有用任何值初始化，则其关联的缓冲会以 0 填充：

```
const ints = new Int32Array(4); 
alert(ints[0]); // 0 
alert(ints[1]); // 0 
alert(ints[2]); // 0 
alert(ints[3]); // 0
```





###### （1）定型数组行为

​		很多方面看，定性数组与普通数组都很相似。定型数组支持如下操作符、方法和属性：

 []

 copyWithin()

 entries()

 every()

 fill()

 filter()

 find()

 findIndex()

 forEach()

 indexOf()

 join()

 keys()

 lastIndexOf()

 length

 map()

 reduce()

 reduceRight()

 reverse()

 slice()

 some()

 sort()

 toLocaleString()

 toString()

 values()



​		返回新数组的方法也会返回包含同样元素类型（element type）的新定型数组：

```
const ints = new Int16Array([1, 2, 3]); 
const doubleints = ints.map(x => 2*x); 
alert(doubleints instanceof Int16Array); // true
```

​		定型数组有一个 Symbol.iterator 符号属性，因此可以通过 for..of 循环和扩展操作符来操作：

```
const ints = new Int16Array([1, 2, 3]); 
for (const int of ints) { 
	 alert(int); 
} 
// 1 
// 2 
// 3 
alert(Math.max(...ints)); // 3
```



###### （2）合并、复制和修改定型数组

​		定型数组同样使用数组缓冲来存储数据，而数组缓冲无法调整大小。因此，下列方法不适用于定型数组：

 concat()

 pop()

 push()

 shift()

 splice()

 unshift()



​		定型数组也提供了两个新方法，可以快速向外或向内复制数据：set()和 subarray()。

**set()**  从提供的数组或定型数组中把值复制到当前定型数组中指定的索引位置：

**subarray()**  执行与 set()相反的操作，它会基于从原始定型数组中复制的值返回一个新定型数组。复制值时的开始索引和结束索引是可选的：

**set()：**

```
// 创建长度为 8 的 int16 数组
const container = new Int16Array(8);

// 把定型数组复制为前 4 个值
// 偏移量默认为索引 0 
container.set(Int8Array.of(1, 2, 3, 4)); 
console.log(container); // [1,2,3,4,0,0,0,0]

// 把普通数组复制为后 4 个值
// 偏移量 4 表示从索引 4 开始插入
container.set([5,6,7,8], 4); 
console.log(container); // [1,2,3,4,5,6,7,8] 

// 溢出会抛出错误
container.set([5,6,7,8], 7); 
// RangeError
```



**subarray()：**

```
const source = Int16Array.of(2, 4, 6, 8);

// 把整个数组复制为一个同类型的新数组
const fullCopy = source.subarray(); 
console.log(fullCopy); // [2, 4, 6, 8]

// 从索引 2 开始复制数组
const halfCopy = source.subarray(2); 
console.log(halfCopy); // [6, 8]

// 从索引 1 开始复制到索引 3 
const partialCopy = source.subarray(1, 3); 
console.log(partialCopy); // [4, 6]
```



​		定型数组没有原生的拼接能力，但使用定型数组 API 提供的很多工具可以手动构建：

```
// 第一个参数是应该返回的数组类型 
// 其余参数是应该拼接在一起的定型数组
function typedArrayConcat(typedArrayConstructor, ...typedArrays) { 
 	// 计算所有数组中包含的元素总数
 	const numElements = typedArrays.reduce((x,y) => (x.length || x) + y.length); 
 	
 	// 按照提供的类型创建一个数组，为所有元素留出空间
 	const resultArray = new typedArrayConstructor(numElements); 
 	
 	// 依次转移数组
 	let currentOffset = 0; 
 	typedArrays.map(x => { 
 		resultArray.set(x, currentOffset); 
 		currentOffset += x.length; 
 	}); 
 	return resultArray; 
} 
const concatArray = typedArrayConcat(Int32Array, 
 									 Int8Array.of(1, 2, 3), 
   									 Int16Array.of(4, 5, 6), 
									 Float32Array.of(7, 8, 9)); 
console.log(concatArray); // [1, 2, 3, 4, 5, 6, 7, 8, 9] 
console.log(concatArray instanceof Int32Array); // true
```





###### （3）下溢和上溢

​		定型数组中值的下溢和上溢不会影响到其他索引，但仍然需要考虑数组的元素应该是什么类型。定型数组对于可以存储的每个索引只接受一个相关位，而不考虑它们对实际数值的影响。

```
// 长度为 2 的有符号整数数组
// 每个索引保存一个二补数形式的有符号整数
// 范围是-128（-1 * 2^7）~127（2^7 - 1）
const ints = new Int8Array(2); 

// 长度为 2 的无符号整数数组
// 每个索引保存一个无符号整数
// 范围是 0~255（2^7 - 1）
const unsignedInts = new Uint8Array(2); 

// 上溢的位不会影响相邻索引
// 索引只取最低有效位上的 8 位
unsignedInts[1] = 256; // 0x100 
console.log(unsignedInts); // [0, 0] 
unsignedInts[1] = 511; // 0x1FF 
console.log(unsignedInts); // [0, 255] 

// 下溢的位会被转换为其无符号的等价值
// 0xFF 是以二补数形式表示的-1（截取到 8 位）, 
// 但 255 是一个无符号整数
unsignedInts[1] = -1 // 0xFF (truncated to 8 bits) 
console.log(unsignedInts); // [0, 255]

// 上溢自动变成二补数形式
// 0x80 是无符号整数的 128，是二补数形式的-128 
ints[1] = 128; // 0x80 
console.log(ints); // [0, -128]

// 下溢自动变成二补数形式
// 0xFF 是无符号整数的 255，是二补数形式的-1 
ints[1] = 255; // 0xFF 
console.log(ints); // [0, -1]
```



**注意**

​		**除了 8 种元素类型，还有一种“夹板”数组类型：Uint8ClampedArray，不允许任何方向溢出。超出最大值 255 的值会被向下舍入为 255，而小于最小值 0 的值会被向上舍入为 0。**

```
const clampedInts = new Uint8ClampedArray([-1, 0, 255, 256]); 
console.log(clampedInts); // [0, 0, 255, 255]
```



​		**按照 JavaScript 之父 Brendan Eich 的说法：“Uint8ClampedArray 完全是 HTML5canvas 元素的历史留存。除非真的做跟 canvas 相关的开发，否则不要使用它。”**





#### 4.Map



​		作为 ECMAScript 6 的新增特性，Map 是一种新的集合类型，为这门语言带来了真正的键/值存储机制。Map 的大多数特性都可以通过 Object 类型实现，但二者之间还是存在一些细微的差异。



##### 1 基本 API 

​		使用 new 关键字和 Map 构造函数可以创建一个空映射。

​		如果想在创建的同时初始化实例，可以给 Map 构造函数传入一个可迭代对象，需要包含键/值对数组。可迭代对象中的每个键/值对都会按照迭代顺序插入到新映射实例中：

```
// 使用嵌套数组初始化映射
const m1 = new Map([ 
 ["key1", "val1"], 
 ["key2", "val2"], 
 ["key3", "val3"] 
]); 
alert(m1.size); // 3 

// 使用自定义迭代器初始化映射
const m2 = new Map({ 
 [Symbol.iterator]: function*() { 
 yield ["key1", "val1"]; 
 yield ["key2", "val2"]; 
 yield ["key3", "val3"]; 
 } 
}); 
alert(m2.size); // 3 

// 映射期待的键/值对，无论是否提供
const m3 = new Map([[]]); 
alert(m3.has(undefined)); // true 
alert(m3.get(undefined)); // undefined
```

​		初始化之后，可以使用 set()方法再添加键/值对。另外，可以使用 get() 和 has() 进行查询，可以通过 size 属性获取映射中的键/值对的数量，还可以使用 delete() 和 clear() 删除值。	

set()方法返回映射实例，因此可以把多个操作连缀起来，包括初始化声明。

```
const m = new Map(); 

alert(m.has("firstName")); // false 
alert(m.get("firstName")); // undefined 
alert(m.size); // 0 

m.set("firstName", "Matt") 
 .set("lastName", "Frisbie"); 
alert(m.has("firstName")); // true 
alert(m.get("firstName")); // Matt 
alert(m.size); // 2 

m.delete("firstName"); // 只删除这一个键/值对
alert(m.has("firstName")); // false 
alert(m.has("lastName")); // true 
alert(m.size); // 1 

m.clear(); // 清除这个映射实例中的所有键/值对
alert(m.has("firstName")); // false 
alert(m.has("lastName")); // false 
alert(m.size); // 0
```



​		

​		Object 只能使用数值、字符串或符号作为键不同，Map 可以使用任何 JavaScript 数据类型作为键。Map 内部使用 SameValueZero 比较操作（ECMAScript 规范内部定义，语言中不能使用），基本上相当于使用严格对象相等的标准来检查键的匹配性。与 Object 类似，映射的值是没有限制的。

```
const m = new Map(); 

const functionKey = function() {}; 
const symbolKey = Symbol(); 
const objectKey = new Object();

m.set(functionKey, "functionValue"); 
m.set(symbolKey, "symbolValue"); 
m.set(objectKey, "objectValue"); 

alert(m.get(functionKey)); // functionValue 
alert(m.get(symbolKey)); // symbolValue 
alert(m.get(objectKey)); // objectValue 
// SameValueZero 比较意味着独立实例不冲突
alert(m.get(function() {})); // undefined
```





​		与严格相等一样，在映射中用作键和值的对象及其他“集合”类型，在自己的内容或属性被修改时仍然保持不变：

```
const m = new Map(); 
const objKey = {}, 
 	  objVal = {}, 
	  arrKey = [], 
	  arrVal = []; 
	  
m.set(objKey, objVal); 
m.set(arrKey, arrVal);

objKey.foo = "foo"; 
objVal.bar = "bar"; 
arrKey.push("foo"); 
arrVal.push("bar");

console.log(m.get(objKey)); // {bar: "bar"} 
console.log(m.get(arrKey)); // ["bar"]
```



​		SameValueZero 比较也可能导致意想不到的冲突

```
const m = new Map(); 
const a = 0/"", // NaN 
	  b = 0/"", // NaN 
 	  pz = +0, 
	  nz = -0;
	  
alert(a === b); // false 
alert(pz === nz); // true 

m.set(a, "foo"); 
m.set(pz, "bar"); 

alert(m.get(b)); // foo 
alert(m.get(nz)); // bar
```



**注意**

​		 **SameValueZero 是 ECMAScript 规范新增的相等性比较算法。**





##### 2 顺序与迭代

​		与 Object 类型的一个主要差异是，Map 实例会维护键值对的插入顺序，因此可以根据插入顺序执行迭代操作。

​		映射实例可以提供一个迭代器（Iterator），能以插入顺序生成[key, value]形式的数组。可以通过 entries()方法（或者 Symbol.iterator 属性，它引用 entries()）取得这个迭代器。

因为 entries()是默认迭代器，所以可以直接对映射实例使用扩展操作，把映射转换为数组。

```
const m = new Map([ 
 	["key1", "val1"], 
 	["key2", "val2"], 
	["key3", "val3"] 
]); 
alert(m.entries === m[Symbol.iterator]); // true

for (let pair of m.entries()) { 
 	alert(pair); 
} 
// [key1,val1] 
// [key2,val2] 
// [key3,val3] 

for (let pair of m[Symbol.iterator]()) { 
 	alert(pair); 
} 
// [key1,val1] 
// [key2,val2] 
// [key3,val3]
```



​		如果不使用迭代器，而是使用回调方式，则可以调用映射的 forEach(callback, opt_thisArg)方法并传入回调，依次迭代每个键/值对。传入的回调接收可选的第二个参数，这个参数用于重写回调内部 this 的值：

```
const m = new Map([ 
 	["key1", "val1"], 
 	["key2", "val2"], 
 	["key3", "val3"] 
]); 
m.forEach((val, key) => alert(`${key} -> ${val}`)); 
// key1 -> val1 
// key2 -> val2 
// key3 -> val3
```

keys() 和 values() 也可以分别返回以插入顺序生成键和值的迭代器。



​	

​		键和值在迭代器遍历时是可以修改的，但映射内部的引用则无法修改。当然，这并不妨碍修改作为键或值的对象内部的属性，因为这样并不影响它们在映射实例中的身份：

```
const m1 = new Map([ 
 	["key1", "val1"] 
]); 

// 作为键的字符串原始值是不能修改的
for (let key of m1.keys()) { 
 	key = "newKey"; 
 	alert(key); // newKey 
 	alert(m1.get("key1")); // val1 
} 

const keyObj = {id: 1}; 
const m = new Map([ 
 	[keyObj, "val1"] 
]); 

// 修改了作为键的对象的属性，但对象在映射内部仍然引用相同的值
for (let key of m.keys()) { 
 	key.id = "newKey"; 
 	alert(key); // {id: "newKey"} 
 	alert(m.get(keyObj)); // val1 
} 
alert(keyObj); // {id: "newKey"}
```



##### 3 选择 Object 还是 Map

​		对于多数 Web 开发任务来说，选择 Object 还是 Map 只是个人偏好问题，影响不大。对于在乎内存和性能的开发者来说，对象和映射之间确实存在显著的差别。

###### （1）内存占用

​		Object 和 Map 的工程级实现在不同浏览器间存在明显差异，但存储单个键/值对所占用的内存数量都会随键的数量线性增加。批量添加或删除键/值对则取决于各浏览器对该类型内存分配的工程实现。不同浏览器的情况不同，但给定固定大小的内存，Map 大约可以比 Object 多存储 50%的键/值对。

###### （2）插入性能

​		向 Object 和 Map 中插入新键 / 值对的消耗大致相当，不过插入 Map 在所有浏览器中一般会稍微快一点。插入速度并不会随着键 / 值对数量而线性增加。如果代码涉及大量操作，那么 Map 的性能更佳。

###### （3）查找速度

​		从大型 Object 和 Map 中查找键/值对的性能差异极小，如果只包含少量键/值对，则 Object 有时候速度更快。在把 Object 当成数组使用的情况下（比如使用连续整数作为属性），浏览器引擎可以进行优化，在内存中使用更高效的布局。这对 Map 来说是不可能的。对这两个类型而言，查找速度不会随着键/值对数量增加而线性增加。如果代码涉及大量查找操作，那么某些情况下可能选择 Object 更好一些。

###### （4）删除性能

​		使用 delete 删除 Object 属性的性能一直以来饱受诟病，目前在很多浏览器中仍然如此。为此，出现了一些伪删除对象属性的操作，包括把属性值设置为 undefined 或 null。但很多时候，这都是一种讨厌的或不适宜的折中。而对大多数浏览器引擎来说，Map 的 delete()操作都比插入和查找更快。如果代码涉及大量删除操作，那么毫无疑问应该选择 Map。





#### 5.WeakMap

​		ECMAScript 6 新增的“弱映射”（WeakMap）是一种新的集合类型，为这门语言带来了增强的键/值对存储机制。WeakMap 是 Map 的“兄弟”类型，其 API 也是 Map 的子集。WeakMap 中的“weak”（弱），描述的是 JavaScript 垃圾回收程序对待“弱映射”中键的方式。



##### 1 基本 API 

​		可以使用 new 关键字实例化一个空的 WeakMap。

​		弱映射中的键只能是 Object 或者继承自 Object 的类型，尝试使用非对象设置键会抛出 TypeError。值的类型没有限制。

​		想在初始化时填充弱映射，则构造函数可以接收一个可迭代对象，其中需要包含键/值对数组。可迭代对象中的每个键/值都会按照迭代顺序插入新实例中：

```
const key1 = {id: 1}, 
 	  key2 = {id: 2},
 	  key3 = {id: 3}; 
// 使用嵌套数组初始化弱映射
const wm1 = new WeakMap([ 
 [key1, "val1"], 
 [key2, "val2"], 
 [key3, "val3"] 
]); 
alert(wm1.get(key1)); // val1 
alert(wm1.get(key2)); // val2 
alert(wm1.get(key3)); // val3

// 初始化是全有或全无的操作
// 只要有一个键无效就会抛出错误，导致整个初始化失败
const wm2 = new WeakMap([ 
 [key1, "val1"], 
 ["BADKEY", "val2"], 
 [key3, "val3"] 
]); 
// TypeError: Invalid value used as WeakMap key 
typeof wm2; 
// ReferenceError: wm2 is not defined 

// 原始值可以先包装成对象再用作键
const stringKey = new String("key1"); 
const wm3 = new WeakMap([ 
 stringKey, "val1" 
]); 
alert(wm3.get(stringKey)); // "val1"
```



​		初始化之后可以使用 set()再添加键/值对，可以使用 get()和 has()查询，还可以使用 delete() 删除。

```
const wm = new WeakMap();

const key1 = {id: 1}, 
 	  key2 = {id: 2}; 
 	  
alert(wm.has(key1)); // false 
alert(wm.get(key1)); // undefined 

wm.set(key1, "Matt") 
  .set(key2, "Frisbie");
  
alert(wm.has(key1)); // true 
alert(wm.get(key1)); // Matt 

wm.delete(key1); // 只删除这一个键/值对
alert(wm.has(key1)); // false 
alert(wm.has(key2)); // true
```

​		set()方法返回弱映射实例，因此可以把多个操作连缀起来，包括初始化声明：

```
const key1 = {id: 1}, 
	  key2 = {id: 2}, 
	  key3 = {id: 3}; 
const wm = new WeakMap().set(key1, "val1");

wm.set(key2, "val2") 
  .set(key3, "val3"); 
alert(wm.get(key1)); // val1 
alert(wm.get(key2)); // val2 
alert(wm.get(key3)); // val3
```



##### 2 弱键

​		WeakMap 中“weak”表示弱映射的键是“弱弱地拿着”的。意思就是，这些键不属于正式的引用，不会阻止垃圾回收。但要注意的是，弱映射中值的引用可不是“弱弱地拿着”的。只要键存在，键/值对就会存在于映射中，并被当作对值的引用，因此就不会被当作垃圾回收。

​		set() 方法初始化了一个新对象并将它用作一个字符串的键。因为没有指向这个对象的其他引用，所以当这行代码执行完成后，这个对象键就会被当作垃圾回收。

```
const wm = new WeakMap(); 
wm.set({}, "val");
```



​		这一次，container 对象维护着一个对弱映射键的引用，因此这个对象键不会成为垃圾回收的目标。不过，如果调用了 removeReference()，就会摧毁键对象的最后一个引用，垃圾回收程序就可以把这个键/值对清理掉。

```
const wm = new WeakMap(); 
const container = { 
 	key: {} 
}; 

wm.set(container.key, "val");

function removeReference() { 
 	container.key = null; 
}
```



##### 3 不可迭代键

​		因为 WeakMap 中的键/值对任何时候都可能被销毁，所以没必要提供迭代其键/值对的能力。

​		WeakMap 实例之所以限制只能用对象作为键，是为了保证只有通过键对象的引用才能取得值。如果允许原始值，那就没办法区分初始化时使用的字符串字面量和初始化之后使用的一个相等的字符串了。



##### 4 使用弱映射

​		WeakMap 实例与现有 JavaScript 对象有着很大不同，可能一时不容易说清楚应该怎么使用它。

###### （1）私有变量

​		弱映射造就了在 JavaScript 中实现真正私有变量的一种新方式。前提很明确：私有变量会存储在弱映射中，以对象实例为键，以私有成员的字典为值。

​		外部代码只需要拿到对象实例的引用和弱映射，就可以取得“私有”变量了。

```

```

























































































