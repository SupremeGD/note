## 第三章

#### 3.3变量

##### 3.1 var

var 声明范围是函数作用域

（局部变量） var 在一个函数内部定义一个变量

（全局变量）函数内部定义变量时省略 var

在严格模式下，不能定义名为 eval 和 arguments 的变量（导致语法错误）。



##### 3.2 let

let 和 var 的作用差不多，let 声明范围是块级作用域。

在同一块作用域中 let 不能重复声明。

let 存在暂时性死区（在 let 声明之前执行瞬间被称为 “ 暂时性死区 ”）

用 let 在全局作用域中声明的变量不会成为 window 对象的属性。

for 循环 var 定义的迭代变量会渗透到循环体外，let 不会。



##### 3.3 const

const 与 let 基本相同，const 声明变量时必须同时初始化变量，并且不能修改 const 声明的变量。

const 声明作用域是块级。

不能用const 声明迭代变量。



##### 3.4 不使用 var，const 优先， let 次之。



#### 3.4 数据类型

​		ECMAScript 有 6 种简单数据类型（也称为原始类型）：Undefined、Null、Boolean、Number、

String 和 Symbol。



##### 3.4.1 typeof

typeof 操作符用来确定任意变量的数据类型。



##### 3.4.2 Undefined

任何未经初始化的变量都会取得 undefined 值。

使用 var 或 let 声明了变量但没有初始化，就相当于变量赋予了 undefined 值。



##### 3.4.3 Null

null 值表示一个空对象指针。



##### 3.4.4 Boolean

​		布尔值字面量 true 和 false 是区分大小写的，所以 True 和 False（及其他大小混写）是有效标识符，不是布尔值。



##### 3.4.5 Number

Number 类型使用 IEEE 754 格式表示整数和浮点数（也叫双精度值）。

非常大或非常小的数值，浮点值可以用科学计数法表示。

最小值保存在：Number.MIN_VALUE 中；负无穷：-Infinity

最大值保存在：Number.MAX_VALUE 中。正无穷：Infinity

isNaN（）函数会尝试把值转换为数值。还可以判断参数是否 “ 不是数值 ”。

有 3 个函数可以将非数值转换为数值：Number()、parseInt()和 parseFloat()。

parseFloat()函数的工作方式跟 parseInt()函数类似，都是从位置 0 开始检测每个字符。

parseFloat()函数的另一个不同之处在于，它始终忽略字符串开头的零。



##### 3.4.6 String





















































































































