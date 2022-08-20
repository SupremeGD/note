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















































































