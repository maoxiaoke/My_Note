# Functions

A function is a `JavaScript` procedure - **a set of statements that performs a task or calculates a value**.

## Defining functions -- 函数定义

### Function declarations -- 函数声明

A **function definition** (also called a **function declaration**, or **function statement**) consists of the `function` keyword, followed by:

+ The name of the function
+ A list of arguments(参数) to the function, enclosed in parentheses(括号) and separated by commas(逗号)
+ The JavaScript statements that define the function, enclosed in curly brackets, `{ }`

> 注意：在`C++`语言中，函数声明和定义可是两码事哦，要记清楚哦。

参数以**值传递**的方式传递给被调函数(比如`a number`)，如果被调函数改变了这个参数的值，这样的改变不会影响到全局或调用函数。

但是，如果`object`作为参数传递(比如`Array`、`user-defined object`)，而函数改变了该对象的属性，则这种改变对外是可见的。

### Function expressions -- 函数表达式

Functions can also be created by a function expression.(函数同样可以由函数表达式创建) Such a function can be **anonymous**(匿名的); it does not have to have a name.

```javascript
var square = function(number) (return number * number;)
var x = square(4); // x gets the value 16
```

However, a name can be provided with a function expression and can be used inside the function to refer to itself, or in a debugger to identify the function in stack traces:

```javascript
var factorial = function fac(n) {return n < 2 ? 1 : n * fac(n-1);}
```

Function expressions are convenient when passing a function as an argument to another function.(当将一个函数作为一个参数传递给另一个函数，函数表达式就十分方便)

> 当一个函数是一个对象的属性时，称之为方法。

---

## Calling functions -- 调用函数

Functions must be in scope when they are called, but the function declaration can be hoisted (appear below the call in the code).(函数必须在被他们被调用的域当中，但是函数声明可以被提升--出现在调用代码的下方)

```javascript
console.log (square(5));
/* ... */
function square(n) {return n * n;}
```

> The scope of a function is the function in which it is declared, or the entire program if it is declared at the top level.

> 注意： 函数提升只对函数声明有效，对函数表达式是无效的。

```javascript
console.log(square); // square is hoisted with an initial value undefined.
console.log(square(5)); // TypeError: square is not a function
var square = function(n) {
  return n * n;
}
```

> **函数也是对象**，也有自己的**方法**。[具体参见](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function)

---

## Function scope -- 函数作用域

+ 定义在函数内部的变量不能被外部访问，因为这个变量仅仅在函数内有定义
+ 函数被定义为**全局**，所以可以访问所有的**全局变量**
+ 在另一个函数中定义的函数可以访问在其父函数中定义的所有变量和父函数有权访问的变量

---

## Scope and the function stack -- 作用域和函数堆栈

### Recursion -- 递归

函数可以指向和调用自身，有三种方法：

+ 使用函数名
+ `arguments.callee` -- 这个属性包含当前正在执行的函数，被`ES5`严格模式删除
+ 作用域内指向函数的变量名(指使用函数表达式的方式)

```javascript
var foo = function bar() {
   // statements go here
};
```

The following are all equivalent:

+ `bar()`
+ `arguments.callee()`
+ `foo()`

A function that calls itself is called a *recursive* function.

> 将递归算法转换成非递归是可能的，但逻辑更复杂，也会需要使用到**堆栈**。

### Nested functions and closures -- 嵌套函数和闭包

可以在一个函数里面嵌套另外一个函数。**嵌套（内部）函数**对其**容器（外部）函数**是私有的。它自身也形成了一个**闭包**(`closure`)。A closure is an expression (typically a function) that can have free variables together with an environment that binds those variables (that "closes" the expression).**一个闭包是一个可以自己拥有独立的环境与变量的的表达式(通常是函数)**。

既然**嵌套函数是一个闭包**，就意味着一个嵌套函数可以”继承“容器函数的参数和变量。换句话说，内部函数包含外部函数的作用域。

+ 内部函数只能在外部函数中访问
+ 内部函数形成一个闭包： 内部函数可以访问外部函数的参数和变量，反之则不行

```javascript
function outside(x) {
  function inside(y) {
    return x + y;
  }
  return inside;
}
fn_inside = outside(3); // Think of it like: give me a function that adds 3 to whatever you give it
result = fn_inside(5); // returns 8

result1 = outside(3)(5); // returns 8
```

> 因为形成闭包，所以你可以调用外部函数并未外部和内部函数指定参数

### Preservation of variables -- 保存变量

一个闭包必须保存它范围内的所有参数和变量。

### Multiply-nested functions -- 多层嵌套函数

Functions can be multiply-nested.

```javascript
function A(x) {
  function B(y) {
    function C(z) {
      console.log(x + y + z);
    }
    C(3);
  }
  B(2);
}
A(1); // logs 6 (1 + 2 + 3)
```

+ B形成了一个包含A的闭包，B可以访问A的参数和变量
+ C形成一个包含B的闭包
+ 所以，C可以访问A和B的任何参数和变量
+ 反之则不行

> 这种递归式成为**域链**(scope chaining)

### Name conflicts -- 命名冲突

When two arguments or variables in the scopes of a closure have the same name, there is a `name conflict`. More inner scopes take precedence, so the inner-most scope takes the highest precedence, while the outer-most scope takes the lowest(更近的作用域有更高的优先权，所以最近的优先权最高，最远的最低). This is the **scope chain**. The first on the chain is the inner-most scope, and the last is the outer-most scope(链的第一个元素是最里面的域，最后一个元素是最外层的域).

```javascript
function outside() {
  var x = 10;
  function inside(x) {
    return x;
  }
  return inside;
}
result = outside()(20); // returns 20 instead of 10
```

> 命名冲突发生在`return x`，此时`inside()`参数`x`与`outside()`的变量`x`发生了冲突。此时域链是`{inside,outside,global object}`，所以`inside()`的`x`有最高的优先权，所以返回的是传递给内部函数的`20`。


---

## Closures -- 闭包

闭包是JavaScript中最强大的特性之一。JavaScript允许函数嵌套，并且内部函数可以访问定义在外部函数中的所有变量和函数，以及外部函数能访问的所有变量和函数。但是，外部函数却不能够访问定义在内部函数中的变量和函数。这给内部函数的变量提供了一定的安全性。而且，当内部函数生存周期大于外部函数时，由于内部函数可以访问外部函数的作用域，定义在外部函数的变量和函数的生存周期就会大于外部函数本身。**当内部函数以某一种方式被任何一个外部函数作用域访问时，一个闭包就产生了**。

外部函数的参数和变量对内嵌函数来说是可取得的，而除了通过内嵌函数本身，没有其它任何方法可以取得内嵌的变量。内嵌函数的内嵌变量就像内嵌函数的保险柜。它们会为内嵌函数保留*稳定*而又安全的数据参与运行。

```javascript
var pet = function(name) {          //外部函数定义了一个变量"name"
  var getName = function() {
    //内部函数可以访问 外部函数定义的"name"
    return name;
  }
  //返回这个内部函数，从而将其暴露在外部函数作用域
  return getName;
};
myPet = pet("Vivie");

myPet();                            // 返回结果 "Vivie"
```

闭包中的神奇变量`this`是非常诡异的。使用它必须十分的小心，因为`this`指代什么完全取决于**函数在何处被调用**，而不是在何处被定义。

---

## Using the arguments object -- 使用arguments对象

函数的参数被保存为一个类似于数组的对象中。在函数内，可以使用如下方式找到传入的参数：

```javascript
arguments[i]
```

> `i`是序号编号，以`0`开始。参数的数量由`arguments.length`表示。

---

## Function parameter -- 函数参数

从`ECMAScript 6`开始，有两个新的类型参数：默认参数(default parameter)和剩余参数(rest parameter).

### default parameter

In JavaScript, parameters of functions default to `undefined`. 你也可以直接在函数头设置默认参数。

```javascript
function multiply(a, b = 1) {
  return a*b;
}

multiply(5); // 5
```

### rest parameter

剩余参数语法允许将不确定数量的参数表示为数组。

语法：

```javascript
function(a, b, ...theArgs) {
  // ...
}
```

---

## Arrow functions -- 箭头函数

An [arrow function expression](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions) has a shorter syntax compared to function expressions and lexically binds the `this` value.(箭头函数表达式相比函数表达式具有较短的语法并以词法的方式绑定`this`)

箭头函数**总是匿名的**。

造成箭头函数引入的两个原因：

+ 更简洁的函数
+ `this`

### 更简洁的函数

```javascript
var a = [
  "Hydrogen",
  "Helium",
  "Lithium",
  "Beryllium"
];

var a2 = a.map(function(s){ return s.length });
console.log(a2); // logs [ 8, 6, 7, 9 ]

var a3 = a.map( s => s.length );
console.log(a3); // logs [ 8, 6, 7, 9 ]
```

### this的词法

在箭头函数出现之前，每一个新函数都重新定义了自己的`this`值。(例如，构造函数的 `this` 指向了一个新的对象；严格模式下的函数的 `this` 值为 `undefined`；如果函数是作为对象的方法被调用的，则其 `this` 指向了那个调用它的对象)

```javascript
function Person() {
  // The Person() constructor defines `this` as itself.
  this.age = 0;

  setInterval(function growUp() {
    // In nonstrict mode, the growUp() function defines `this` 
    // as the global object, which is different from the `this`
    // defined by the Person() constructor.
    this.age++;
  }, 1000);
}

var p = new Person();
```

在`ECMAScript 3/5`里，通过把`this`的值赋值给一个变量可以修复这个问题。

```javascript
function Person() {
  var self = this; // Some choose `that` instead of `self`. 
                   // Choose one and be consistent.
  self.age = 0;

  setInterval(function growUp() {
    // The callback refers to the `self` variable of which
    // the value is the expected object.
    self.age++;
  }, 1000);
}
```

箭头功能捕捉闭包上下文的`this`值，所以下面的代码工作正常:

```javascript
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| properly refers to the person object
  }, 1000);
}

var p = new Person();
```

### 箭头函数的语法

基础语法：

```javascript
(param1, param2, …, paramN) => { statements }
(param1, param2, …, paramN) => expression
         // equivalent to:  => { return expression; }

// 如果只有一个参数，圆括号是可选的:
(singleParam) => { statements }
singleParam => { statements }

// 无参数的函数需要使用圆括号:
() => { statements }
```

高级语法：

```javascript
// 返回对象字面量时应当用圆括号将其包起来:
params => ({foo: bar})

// 支持 Rest parameters 和 default parameters:
(param1, param2, ...rest) => { statements }
(param1 = defaultValue1, param2, …, paramN = defaultValueN) => { statements }

// 参数列表中的解构赋值也是被支持的
var f = ([a, b] = [1, 2], {x: c} = {x: a + b}) => a + b + c;
f();  // 6
```


## IIFE(Immediately Invoked Function Expression) -- 立即执行函数表达式

传统的方法，函数的定义和执行分开写。为了能够在函数定义之后立即执行，我们使用`IIFE`这种语法：

```javascript
var result = (function () {
    return 2 + 2;
}());

// or
var result = (function(){
  return 2 + 2;
})();
```

> 上面这两种写法都是可以的。

也可以为`IIFE`传递参数：

```javascript
(function(who, when) {
    console.log("I met " + who + " on " + when);
} ("Joe Black", new Date()));
```

参考：

+ [https://en.wikipedia.org/wiki/Immediately-invoked_function_expression](https://en.wikipedia.org/wiki/Immediately-invoked_function_expression)
+ [http://benalman.com/news/2010/11/immediately-invoked-function-expression/](http://benalman.com/news/2010/11/immediately-invoked-function-expression/)

> [更多解析参见：](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
