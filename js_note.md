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







##### 



















































































































