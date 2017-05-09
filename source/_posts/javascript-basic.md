---
title: JavaScript高级程序设计 -- 数据类型
date: 2017-04-19 13:55:41
categories: JavaScript高级程序设计
tags: 数据类型
---


> 摘录：《JavaScript高级程序设计》第三版

## 数据类型

ECMAScript 中有5 种简单数据类型（也称为基本数据类型）：`Undefined`、`Null`、`Boolean`、`Number` 和 `String`。还有1 种复杂数据类型—— `Object`，Object 本质上是由一组无序名值对组成的。ECMAScript 不支持任何创建自定义类型的机制，而所有值最终都将是上述6 种数据类型之一。乍一看，好像只有6种数据类型不足以表示所有数据；但是，由于ECMAScript 数据类型具有动态性，因此的确没有再定义其他数据类型的必要了。

### typeof 操作符

鉴于ECMAScript 是松散类型的，因此需要有一种手段来检测给定变量的数据类型—— `typeof` 就是负责提供这方面信息的操作符。对一个值使用typeof 操作符可能返回下列某个字符串：

- `undefined` 如果这个值未定义
- `boolean`   如果这个值是布尔值
- `string`    如果这个值是字符串
- `number`    如果这个值是数值
- `object`    如果这个值是对象或null
- `function`  如果这个值是函数

下面是几个使用typeof 操作符的例子：

```javascript
var message = "some string";
console.log(typeof message); // "string"
console.log(typeof(message)); // "string"
console.log(typeof 95); // "number"
```

这几个例子说明，`typeof` 操作符的操作数可以是变量（message），也可以是数值字面量。注意，`typeof` 是一个操作符而不是函数，因此例子中的圆括号尽管可以使用，但不是必需的。
有些时候，`typeof` 操作符会返回一些令人迷惑但技术上却正确的值。比如，调用 `typeof null` 会返回 `object`，因为特殊值 `null` 被认为是一个空的对象引用。Safari 5 及之前版本、Chrome 7 及之前版本在对正则表达式调用 `typeof` 操作符时会返回 `function`，而其他浏览器在这种情况下会返回 `object`。

> 从技术角度讲，函数在 ECMAScript 中是对象，不是一种数据类型。然而，函数也确实有一些特殊的属性，因此通过 `typeof` 操作符来区分函数和其他对象是有必要的。


### Undefined 类型

`Undefined` 类型只有一个值，即特殊的 `undefined` 。在使用 `var` 声明变量但未对其加以初始化时，这个变量的值就是 `undefined`，例如：

```javascript
var message;
console.log(message == undefined); //true
```

这个例子只声明了变量message，但未对其进行初始化。比较这个变量与 undefined 字面量，结果表明它们是相等的。这个例子与下面的例子是等价的：

```javascript
var message = undefined;
console.log(message == undefined); //true
```

这个例子使用 `undefined` 值显式初始化了变量message。但我们没有必要这么做，因为未经初始化的值默认就会取得 `undefined` 值。

> 一般而言，不存在需要显式地把一个变量设置为 `undefined` 值的情况。字面值 `undefined` 的主要目的是用于比较，而ECMA-262 第3 版之前的版本中并没有规定这个值。第3 版引入这个值是为了正式区分空对象指针与未经初始化的变量。


不过，包含 `undefined` 值的变量与尚未定义的变量还是不一样的。看看下面这个例子：

```javascript
var message; // 这个变量声明之后默认取得了undefined 值
// 下面这个变量并没有声明
// var age
console.log(message); // "undefined"
console.log(age); // 产生错误
```

运行以上代码，第一个警告会显示变量 message 的值，即"undefined"。而第二个警告——由于传递给 console.log() 是尚未声明的变量 age ——则会导致一个错误。
对于尚未声明过的变量，只能执行一项操作，即使用typeof 操作符测其数据类型（对未经声明的变量调用delete 不会导致错误，但这样做没什么实际意义，而且在严格模式下确实会导致错误）。

然而，令人困惑的是：对未初始化的变量执行 `typeof` 操作符会返回 `undefined` 值，而对未声明的变量执行 `typeof` 操作符同样也会返回 `undefined` 值。来看下面的例子：

```javascript
var message; // 这个变量声明之后默认取得了undefined 值
// 下面这个变量并没有声明
// var age
console.log(typeof message); // "undefined"
console.log(typeof age); // "undefined"
```

结果表明，对未初始化和未声明的变量执行 `typeof` 操作符都返回了 `undefined` 值；这个结果有其逻辑上的合理性。因为虽然这两种变量从技术角度看有本质区别，但实际上无论对哪种变量也不可能执行真正的操作。

> 即便未初始化的变量会自动被赋予 `undefined` 值，但显式地初始化变量依然是明智的选择。如果能够做到这一点，那么当 `typeof` 操作符返回 `undefined` 值时，我们就知道被检测的变量还没有被声明，而不是尚未初始化。


### Null 类型

`Null` 类型是第二个只有一个值的数据类型，这个特殊的值是 `null` 。从逻辑角度来看，`null` 值表示一个空对象指针，而这也正是使用 `typeof` 操作符检测 `null` 值时会返回 `object` 的原因，如下面的例子所示：

```javascript
var car = null;
console.log(typeof car); // "object"
```

**如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为 `null` 而不是其他值。这样一来，只要直接检查 `null` 值就可以知道相应的变量是否已经保存了一个对象的引用**，如下面的例子所示：

```javascript
if (car != null){
// 对car 对象执行某些操作
}
```
实际上，`undefined` 值是派生自 `null` 值的，因此ECMA-262 规定对它们的相等性测试要返回true：

```javascript
console.log(null == undefined); //true
```

这里，位于 `null` 和 `undefined` 之间的相等操作符（==）总是返回 `true`，不过要注意的是，这个操作符出于比较的目的会转换其操作数（本章后面将详细介绍相关内容）。尽管 `null` 和 `undefined` 有这样的关系，但它们的用途完全不同。如前所述，无论在什么情况下都没有必要把一个变量的值显式地设置为 `undefined`，可是同样的规则对 `null` 却不适用。换句话说，**只要意在保存对象的变量还没有真正保存对象，就应该明确地让该变量保存null 值。这样做不仅可以体现null 作为空对象指针的惯例，而且也有助于进一步区分null 和undefined**。


### Boolean 类型

`Boolean` 类型是 ECMAScript 中使用得最多的一种类型，该类型只有两个字面值：`true` 和 `false`。这两个值与数字值不是一回事，因此 `true` 不一定 等于1，而 `false` 也不一定等于0。以下是为变量赋 `Boolean` 类型值的例子：

```javascript
var found = true;
var lost = false;
```
需要注意的是，`Boolean` 类型的字面值 `true` 和 `false` 是区分大小写的。也就是说，True 和 False（以及其他的混合大小写形式）都不是 Boolean 值，只是标识符。虽然 Boolean 类型的字面值只有两个，但ECMAScript 中所有类型的值都有与这两个 Boolean 值等价的值。要将一个值转换为其对应的 Boolean 值，可以调用转型函数 `Boolean()`，如下例所示：

```javascript
var message = "Hello world!";
var messageAsBoolean = Boolean(message);
```

在这个例子中，字符串 message 被转换成了一个 `Boolean` 值，该值被保存在 messageAsBoolean 变量中。可以对任何数据类型的值调用 `Boolean()` 函数，而且总会返回一个 `Boolean` 值。至于返回的这个值是 `true` 还是 `false`，取决于要转换值的数据类型及其实际值。下表给出了各种数据类型及其对应的转换规则。

| 数据类型   | 转换为true的值              | 转换为false的值                    |
| --------- | -------------------------- | --------------------------------- |
| Boolean   | true                       | false                             |
| String    | 任何非空字符串              | ""（空字符串）                     |
| Number    | 任何非零数字值（包括无穷大） | 0和NaN（参见本章后面有关NaN的内容）  |
| Object    | 任何对象                   | null                               |
| Undefined | N/A                       | undefined                          |

这些转换规则对理解流控制语句（如if 语句）自动执行相应的 Boolean 转换非常重要，请看下面的代码：

```javascript
var message = "Hello world!";
if (message){
    alert("Value is true");
}
```

运行这个示例，就会显示一个警告框，因为字符串 message 被自动转换成了对应的 Boolean 值（true）。由于存在这种自动执行的Boolean 转换，因此确切地知道在流控制语句中使用的是什么变量至关重要。错误地使用一个对象而不是一个 Boolean 值，就有可能彻底改变应用程序的流程。


### Number 类型

`Number` 类型应该是ECMAScript 中最令人关注的数据类型了，这种类型使用IEEE754 格式来表示整数和浮点数值（浮点数值在某些语言中也被称为双精度数值）。为支持各种数值类型，ECMA-262 定义了不同的数值字面量格式。

最基本的数值字面量格式是十进制整数，十进制整数可以像下面这样直接在代码中输入：

```javascript
var intNum = 55; // 整数
```

除了以十进制表示外，整数还可以通过八进制（以8 为基数）或十六进制（以16 为基数）的字面值来表示。其中，八进制字面值的第一位必须是零（0），然后是八进制数字序列（0～7）。如果字面值中的数值超出了范围，那么前导零将被忽略，后面的数值将被当作十进制数值解析。请看下面的例子：

```javascript
var octalNum1 = 070; // 八进制的56
var octalNum2 = 079; // 无效的八进制数值——解析为79
var octalNum3 = 08; // 无效的八进制数值——解析为8
```
八进制字面量在严格模式下是无效的，会导致支持该模式的JavaScript 引擎抛出错误。十六进制字面值的前两位必须是0x，后跟任何十六进制数字（0～9 及A～F）。其中，字母A～F可以大写，也可以小写。如下面的例子所示：

```javascript
var hexNum1 = 0xA; // 十六进制的10
var hexNum2 = 0x1f; // 十六进制的31
```

在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值。

> 鉴于 JavaScript 中保存数值的方式，可以保存正零（+0）和负零（-0）。正零和负零被认为相等，但为了读者更好地理解上下文，这里特别做此说明。

#### 浮点数值

所谓浮点数值，就是该数值中必须包含一个小数点，并且小数点后面必须至少有一位数字。虽然小数点前面可以没有整数，但我们不推荐这种写法。以下是浮点数值的几个例子：

```javascript
var floatNum1 = 1.1;
var floatNum2 = 0.1;
var floatNum3 = .1; // 有效，但不推荐
```

由于**保存浮点数值需要的内存空间是保存整数值的两倍**，因此ECMAScript 会不失时机地将浮点数值转换为整数值。显然，如果小数点后面没有跟任何数字，那么这个数值就可以作为整数值来保存。同样地，如果浮点数值本身表示的就是一个整数（如1.0），那么该值也会被转换为整数，如下面的例子所示：

```javascript
var floatNum1 = 1.; // 小数点后面没有数字——解析为1
var floatNum2 = 10.0; // 整数——解析为10
```

对于那些极大或极小的数值，可以用 `e` 表示法（即科学计数法）表示的浮点数值表示。用 e 表示法表示的数值等于 e 前面的数值乘以 10 的指数次幂。ECMAScript 中 e 表示法的格式也是如此，即前面是一个数值（可以是整数也可以是浮点数），中间是一个大写或小写的字母E，后面是 10 的幂中的指数，该幂值将用来与前面的数相乘。下面是一个使用 e 表示法表示数值的例子：

```javascript
var floatNum = 3.125e7; // 等于31250000
```

在这个例子中，使用e 表示法表示的变量floatNum 的形式虽然简洁，但它的实际值则是31250000。在此，e 表示法的实际含义就是“3.125 乘以107”。
也可以使用e 表示法表示极小的数值，如0.00000000000000003，这个数值可以使用更简洁的3e-17表示。在默认情况下，ECMASctipt 会将那些小数点后面带有6 个零以上的浮点数值转换为以e 表示法表示的数值（例如，0.0000003 会被转换成3e-7）。

浮点数值的最高精度是17 位小数，但在进行算术计算时其精确度远远不如整数。例如，0.1 加0.2的结果不是0.3，而是0.30000000000000004。这个小小的舍入误差会导致无法测试特定的浮点数值。例如：

```javascript
if (a + b == 0.3){ // 不要做这样的测试！
    alert("You got 0.3.");
}
```

在这个例子中，我们测试的是两个数的和是不是等于0.3。如果这两个数是0.05 和0.25，或者是0.15和0.15 都不会有问题。而如前所述，如果这两个数是0.1 和0.2，那么测试将无法通过。因此，**永远不要测试某个特定的浮点数值**。

> 关于浮点数值计算会产生舍入误差的问题，有一点需要明确：这是使用基于 IEEE754 数值的浮点计算的通病，ECMAScript 并非独此一家；其他使用相同数值格式的语言也存在这个问题。


#### 数值范围

由于内存的限制，ECMAScript 并不能保存世界上所有的数值。ECMAScript 能够表示的最小数值保存在 `Number.MIN_VALUE` 中——在大多数浏览器中，这个值是5e-324；能够表示的最大数值保存在 `Number.MAX_VALUE` 中——在大多数浏览器中，这个值是1.7976931348623157e+308。如果某次计算的结果得到了一个超出JavaScript 数值范围的值，那么这个数值将被自动转换成特殊的Infinity 值。具体来说，如果这个数值是负数，则会被转换成 `-Infinity`（负无穷），如果这个数值是正数，则会被转换成 `Infinity`（正无穷）。

如上所述，如果某次计算返回了正或负的Infinity 值，那么该值将无法继续参与下一次的计算，因为Infinity 不是能够参与计算的数值。要想确定一个数值是不是有穷的（换句话说，是不是位于最小和最大的数值之间），可以使用`isFinite()` 函数。这个函数在参数位于最小与最大数值之间时会返回true，如下面的例子所示：

```javascript
var result = Number.MAX_VALUE + Number.MAX_VALUE;
alert(isFinite(result)); //false
```

尽管在计算中很少出现某些值超出表示范围的情况，但在执行极小或极大数值的计算时，检测监控这些值是可能的，也是必需的。

> 访问 `Number.NEGATIVE_INFINITY` 和 `Number.POSITIVE_INFINITY` 也可以得到负和正Infinity 的值。可以想象，这两个属性中分别保存着 `-Infinity` 和 `Infinity`。


#### NaN

NaN，即非数值（Not a Number）是一个特殊的数值，这个数值用于表示一个本来要返回数值的操作数未返回数值的情况（这样就不会抛出错误了）。例如，在其他编程语言中，任何数值除以非数值都会导致错误，从而停止代码执行。但在ECMAScript 中，任何数值除以非数值会返回NaN，因此不会影响其他代码的执行。

NaN 本身有两个非同寻常的特点。首先，任何涉及 NaN 的操作（例如NaN/10）都会返回 NaN，这个特点在多步计算中有可能导致问题。其次，**NaN 与任何值都不相等，包括 NaN 本身**。例如，下面的代码会返回false：

```javascript
alert(NaN == NaN); //false
```

针对NaN 的这两个特点，ECMAScript 定义了 `isNaN()` 函数。这个函数接受一个参数，该参数可以是任何类型，而函数会帮我们确定这个参数是否“不是数值”。isNaN()在接收到一个值之后，会尝试将这个值转换为数值。某些不是数值的值会直接转换为数值，例如字符串"10"或 Boolean 值。而任何不能被转换为数值的值都会导致这个函数返回true。请看下面的例子：

```javascript
alert(isNaN(NaN)); //true
alert(isNaN(10)); //false（10 是一个数值）
alert(isNaN("10")); //false（可以被转换成数值10）
alert(isNaN("blue")); //true（不能转换成数值）
alert(isNaN(true)); //false（可以被转换成数值1）
```

这个例子测试了5 个不同的值。测试的第一个值是NaN 本身，结果当然会返回true。然后分别测试了数值10 和字符串"10"，结果这两个测试都返回了false，因为前者本身就是数值，而后者可以被转换成数值。但是，字符串"blue"不能被转换成数值，因此函数返回了true。由于Boolean 值true可以转换成数值1，因此函数返回false。

> 尽管有点儿不可思议，但 isNaN() 确实也适用于对象。在基于对象调用 `isNaN()` 函数时，会首先调用对象的 `valueOf()` 方法，然后确定该方法返回的值是否可以转换为数值。如果不能，则基于这个返回值再调用 `toString()` 方法，再测试返回值。

#### 数值转换

有3 个函数可以把非数值转换为数值：`Number()`、`parseInt()` 和 `parseFloat()`。第一个函数，即转型函数Number()可以用于任何数据类型，而另两个函数则专门用于把字符串转换成数值。这3 个函数对于同样的输入会有返回不同的结果。

**Number()函数的转换规则如下：**

* 如果是Boolean 值，true 和false 将分别被转换为1 和0。
* 如果是数字值，只是简单的传入和返回。
* 如果是null 值，返回0。
* 如果是undefined，返回NaN。
* 如果是字符串，遵循下列规则：
 - 如果字符串中只包含数字（包括前面带正号或负号的情况），则将其转换为十进制数值，即"1"会变成1，"123"会变成123，而"011"会变成11（注意：前导的零被忽略了）；
 - 如果字符串中包含有效的浮点格式，如"1.1"，则将其转换为对应的浮点数值（同样，也会忽略前导零）；
 - 如果字符串中包含有效的十六进制格式，例如"0xf"，则将其转换为相同大小的十进制整数值；
 - 如果字符串是空的（不包含任何字符），则将其转换为0；
 - 如果字符串中包含除上述格式之外的字符，则将其转换为NaN。
* 如果是对象，则调用对象的valueOf()方法，然后依照前面的规则转换返回的值。如果转换的结果是NaN，则调用对象的toString()方法，然后再次依照前面的规则转换返回的字符串值。

根据这么多的规则使用Number()把各种数据类型转换为数值确实有点复杂。下面还是给出几个具体的例子吧。

```javascript
var num1 = Number("Hello world!"); //NaN
var num2 = Number(""); //0
var num3 = Number("000011"); //11
var num4 = Number(true); //1
```

首先，字符串"Hello world!"会被转换为NaN，因为其中不包含任何有意义的数字值。空字符串会被转换为0。字符串"000011"会被转换为11，因为忽略了其前导的零。最后，true 值被转换为1。

> 一元加操作符的操作与Number()函数相同。

由于Number()函数在转换字符串时比较复杂而且不够合理，因此在处理整数的时候更常用的是parseInt()函数。parseInt()函数在转换字符串时，更多的是看其是否符合数值模式。它会忽略字符串前面的空格，直至找到第一个非空格字符。如果第一个字符不是数字字符或者负号，parseInt()就会返回NaN；也就是说，用parseInt()转换空字符串会返回NaN（Number()对空字符返回0）。如果第一个字符是数字字符，parseInt()会继续解析第二个字符，直到解析完所有后续字符或者遇到了一个非数字字符。例如，"1234blue"会被转换为1234，因为"blue"会被完全忽略。类似地，"22.5"会被转换为22，因为小数点并不是有效的数字字符。

如果字符串中的第一个字符是数字字符，parseInt()也能够识别出各种整数格式（即前面讨论的十进制、八进制和十六进制数）。也就是说，如果字符串以"0x"开头且后跟数字字符，就会将其当作一个十六进制整数；如果字符串以"0"开头且后跟数字字符，则会将其当作一个八进制数来解析。

为了更好地理解parseInt()函数的转换规则，下面给出一些例子：

```javascript
var num1 = parseInt("1234blue"); // 1234
var num2 = parseInt(""); // NaN
var num3 = parseInt("0xA"); // 10（十六进制数）
var num4 = parseInt(22.5); // 22
var num5 = parseInt("070"); // 56（八进制数）
var num6 = parseInt("70"); // 70（十进制数）
var num7 = parseInt("0xf"); // 15（十六进制数）
```

在使用parseInt()解析像八进制字面量的字符串时，ECMAScript 3 和5 存在分歧。例如：

```javascript
//ECMAScript 3 认为是56（八进制），ECMAScript 5 认为是70（十进制）
var num = parseInt("070");
```

在ECMAScript 3 JavaScript 引擎中，"070"被当成八进制字面量，因此转换后的值是十进制的56。而在ECMAScript 5 JavaScript 引擎中，parseInt()已经不具有解析八进制值的能力，因此前导的零会被认为无效，从而将这个值当成"70"，结果就得到十进制的70。在ECMAScript 5 中，即使是在非严格模式下也会如此。

为了消除在使用parseInt()函数时可能导致的上述困惑，可以为这个函数提供第二个参数：转换时使用的基数（即多少进制）。如果知道要解析的值是十六进制格式的字符串，那么指定基数16 作为第二个参数，可以保证得到正确的结果，例如：

```javascript
var num = parseInt("0xAF", 16); //175
```

实际上，如果指定了16 作为第二个参数，字符串可以不带前面的"0x"，如下所示：

```javascript
var num1 = parseInt("AF", 16); //175
var num2 = parseInt("AF"); //NaN
```

这个例子中的第一个转换成功了，而第二个则失败了。差别在于第一个转换传入了基数，明确告诉parseInt()要解析一个十六进制格式的字符串；而第二个转换发现第一个字符不是数字字符，因此就自动终止了。

指定基数会影响到转换的输出结果。例如：

```javascript
var num1 = parseInt("10", 2); //2 （按二进制解析）
var num2 = parseInt("10", 8); //8 （按八进制解析）
var num3 = parseInt("10", 10); //10 （按十进制解析）
var num4 = parseInt("10", 16); //16 （按十六进制解析）
```

不指定基数意味着让parseInt()决定如何解析输入的字符串，因此为了避免错误的解析，我们建议无论在什么情况下都明确指定基数。

> 多数情况下，我们要解析的都是十进制数值，因此始终将10 作为第二个参数是非常必要的。

与parseInt()函数类似，parseFloat()也是从第一个字符（位置0）开始解析每个字符。而且也是一直解析到字符串末尾，或者解析到遇见一个无效的浮点数字字符为止。也就是说，字符串中的第一个小数点是有效的，而第二个小数点就是无效的了，因此它后面的字符串将被忽略。举例来说，"22.34.5"将会被转换为22.34。

除了第一个小数点有效之外，parseFloat()与parseInt()的第二个区别在于它始终都会忽略前导的零。parseFloat()可以识别前面讨论过的所有浮点数值格式，也包括十进制整数格式。但十六进制格式的字符串则始终会被转换成0。由于parseFloat()只解析十进制值，因此它没有用第二个参数指定基数的用法。最后还要注意一点：如果字符串包含的是一个可解析为整数的数（没有小数点，或者小数点后都是零），parseFloat()会返回整数。以下是使用parseFloat()转换数值的几个典型示例。

```javascript
var num1 = parseFloat("1234blue"); //1234 （整数）
var num2 = parseFloat("0xA"); //0
var num3 = parseFloat("22.5"); //22.5
var num4 = parseFloat("22.34.5"); //22.34
var num5 = parseFloat("0908.5"); //908.5
var num6 = parseFloat("3.125e7"); //31250000
```

### String 类型

String 类型用于表示由零或多个16 位Unicode 字符组成的字符序列，即字符串。字符串可以由双引号（"）或单引号（'）表示，因此下面两种字符串的写法都是有效的：

```javascript
var firstName = "Nicholas";
var lastName = 'Zakas';
```

与 PHP 中的双引号和单引号会影响对字符串的解释方式不同，ECMAScript 中的这两种语法形式没有什么区别。用双引号表示的字符串和用单引号表示的字符串完全相同。不过，以双引号开头的字符串也必须以双引号结尾，而以单引号开头的字符串必须以单引号结尾。例如，下面这种字符串表示法会导致语法错误：

```javascript
var firstName = 'Nicholas"; 
// 语法错误 左右引号必须匹配
```

#### 字符字面量

String 数据类型包含一些特殊的字符字面量，也叫转义序列，用于表示非打印字符，或者具有其他用途的字符。这些字符字面量如下表所示：

| 字面量  | 含义 |
| ------ | ---- |
| \n     | 换行 |
| \t     | 制表 |
| \b     | 退格 |
| \r     | 回车 |
| \f     | 禁止 |
| \\     | 斜杠 |
| \"     | " |
| \xnn   | 以十六进制代码nn表示的一个字符（其中n为0～F）。例如，\x41表示"A" |
| \unnnn | 以十六进制代码nnnn表示的一个Unicode字符（其中n为0～F）。例如，\u03a3表示希腊字符Σ |


这些字符字面量可以出现在字符串中的任意位置，而且也将被作为一个字符来解析，如下面的例子所示：

```javascript
var text = "This is the letter sigma: \u03a3.";
```

这个例子中的变量text 有28 个字符，其中6 个字符长的转义序列表示1 个字符。
任何字符串的长度都可以通过访问其length 属性取得，例如：

```javascript
alert(text.length); // 输出28
```

这个属性返回的字符数包括16 位字符的数目。如果字符串中包含双字节字符，那么 `length` 属性可能不会精确地返回字符串中的字符数目。

#### 字符串的特点

ECMAScript 中的字符串是不可变的，也就是说，字符串一旦创建，它们的值就不能改变。要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量，例如：

```javascript
var lang = "Java";
lang = lang + "Script";
```

以上示例中的变量lang 开始时包含字符串"Java"。而第二行代码把lang 的值重新定义为"Java"与"Script"的组合，即"JavaScript"。实现这个操作的过程如下：首先创建一个能容纳10 个字符的新字符串，然后在这个字符串中填充"Java"和"Script"，最后一步是销毁原来的字符串"Java"和字符串"Script"，因为这两个字符串已经没用了。这个过程是在后台发生的，而这也是在某些旧版本的浏览器（例如版本低于1.0 的Firefox、IE6 等）中拼接字符串时速度很慢的原因所在。但这些浏览器后来的版本已经解决了这个低效率问题。


#### 转换为字符串

要把一个值转换为一个字符串有两种方式。第一种是使用几乎每个值都有的 `toString()` 方法。这个方法唯一要做的就是返回相应值的字符串表现。来看下面的例子：

```javascript
var age = 11;
var ageAsString = age.toString(); // 字符串"11"
var found = true;
var foundAsString = found.toString(); // 字符串"true"
```

数值、布尔值、对象和字符串值（没错，每个字符串也都有一个 `toString()` 方法，该方法返回字符串的一个副本）都有 `toString()` 方法。但 `null` 和 `undefined` 值没有这个方法。

多数情况下，调用 `toString()` 方法不必传递参数。但是，在调用数值的 toString() 方法时，可以传递一个参数：输出数值的基数。默认情况下，toString() 方法以十进制格式返回数值的字符串表示。而通过传递基数，toString()可以输出以二进制、八进制、十六进制，乃至其他任意有效进制格式表示的字符串值。下面给出几个例子：

```javascript
var num = 10;
alert(num.toString()); // "10"
alert(num.toString(2)); // "1010"
alert(num.toString(8)); // "12"
alert(num.toString(10)); // "10"
alert(num.toString(16)); // "a"
```

通过这个例子可以看出，通过指定基数，toString() 方法会改变输出的值。而数值 10 根据基数的不同，可以在输出时被转换为不同的数值格式。注意，默认的（没有参数的）输出值与指定基数 10 时的输出值相同。

在不知道要转换的值是不是null 或undefined 的情况下，还可以使用转型函数String()，这个函数能够将任何类型的值转换为字符串。**String()函数遵循下列转换规则**：

- 如果值有toString()方法，则调用该方法（没有参数）并返回相应的结果；
- 如果值是null，则返回"null"；
- 如果值是undefined，则返回"undefined"。

下面看几个例子：

```javascript
var value1 = 10;
var value2 = true;
var value3 = null;
var value4;
alert(String(value1)); // "10"
alert(String(value2)); // "true"
alert(String(value3)); // "null"
alert(String(value4)); // "undefined"
```

这里先后转换了4 个值：数值、布尔值、null 和undefined。数值和布尔值的转换结果与调用toString()方法得到的结果相同。因为 **null 和 undefined 没有toString()方法**，所以String()函数就返回了这两个值的字面量。

> 要把某个值转换为字符串，可以使用加号操作符把它与一个字符串（""）加在一起。


### Object 类型

ECMAScript 中的对象其实就是一组数据和功能的集合。对象可以通过执行new 操作符后跟要创建的对象类型的名称来创建。而创建 Object 类型的实例并为其添加属性和（或）方法，就可以创建自定义对象，如下所示：

```javascript
var o = new Object();
```

这个语法与Java 中创建对象的语法相似；但在ECMAScript 中，如果不给构造函数传递参数，则可以省略后面的那一对圆括号。也就是说，在像前面这个示例一样不传递参数的情况下，完全可以省略那对圆括号（但这不是推荐的做法）：

```javascript
var o = new Object; // 有效，但不推荐省略圆括号
```

仅仅创建 Object 的实例并没有什么用处，但关键是要理解一个重要的思想：即在ECMAScript 中，（就像Java 中的java.lang.Object 对象一样）Object 类型是所有它的实例的基础。换句话说，Object 类型所具有的任何属性和方法也同样存在于更具体的对象中。

Object 的每个实例都具有下列属性和方法：

- `constructor`：保存着用于创建当前对象的函数。对于前面的例子而言，构造函数（constructor）就是 Object()。
- `hasOwnProperty(propertyName)`：用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中，作为参数的属性名（propertyName）必须以字符串形式指定（例如：`o.hasOwnProperty("name")`）。
- `isPrototypeOf(object)`：用于检查传入的对象是否是当前对象的原型。
- `propertyIsEnumerable(propertyName)`：用于检查给定的属性是否能够使用for-in 语句来枚举。与 `hasOwnProperty()` 方法一样，作为参数的属性名必须以字符串形式指定。
- `toLocaleString()`：返回对象的字符串表示，该字符串与执行环境的地区对应。
- `toString()`：返回对象的字符串表示。
- `valueOf()`：返回对象的字符串、数值或布尔值表示。通常与 `toString()` 方法的返回值相同。

由于在ECMAScript 中Object 是所有对象的基础，因此所有对象都具有这些基本的属性和方法。

> 从技术角度讲，ECMA-262 中对象的行为不一定适用于JavaScript 中的其他对象。浏览器环境中的对象，比如BOM 和DOM 中的对象，都属于宿主对象，因为它们是由宿主实现提供和定义的。ECMA-262 不负责定义宿主对象，因此宿主对象可能会也可能不会继承Object。