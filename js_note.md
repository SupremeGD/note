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

Object.getOwnProperty-Symbols()返回对象实例的符号属性数组。这两个方法的返回值彼此互斥。Object.getOwnProperty-Descriptors()会返回同时包含常规和符号属性描述符的对象。

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













































































































































