# CommonJS的模块规范 #

> CommonJS对模块的定义十分简单，主要分为模块引用、模块定义和模块标识。

## 模块引用 ##

示例如下：

```
var math = require('math');
```

> 存在`require()`方法，这个方法接受模块标识，以此引入一个模块的API。

## 模块定义 ##

上下文提供`exports`对象来提供用于**导出当前模块的方法或者变量**。


	//math.js
	exports.add = function(){
		var sum = 0;
		i = 0;
		args = arguments;
		l = args.length;
		while (i <l){
			sum += args[i++];
		}
		return sum;
	};
>在模块中，还存在一个`moudle`对象，它代表模块自身，而`exports`是`module`属性。在Node中，一个文件就是一个模块。

另一个文件中，我们通过`require()`方法引入模块后，就能调用定义的属性或方法了。

	//program.js
	var math = require('math');
	exports.increment = function(val) {
		return math.add(val,1);
	};

## 模块标识 ##

模块标识就是传递给`require()`方法的参数，它必须是符合**小驼峰命名**的字符串，或者以`.` / `..`开头的**相对路径**，或者是**绝对路径**。它可以没有后缀`.js`。

1/17/2017 3:23:37 PM 

> 驼峰式命名法
>
> ![驼峰式命名来源](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ef/CamelCase.svg/368px-CamelCase.svg.png)
>
> 规则如下：单字之间不以空格断开或连字符(`-`)或下划线(`_`)连接。有两种格式。
>
>小驼峰命名法(lower camel case): 第一个单字以小写字母开始，第二个单字的首字母大写。
>
> 大驼峰命名法(upper camel case): 每一个单字的首字母都采用大写字母。

1/17/2017 3:33:19 PM 