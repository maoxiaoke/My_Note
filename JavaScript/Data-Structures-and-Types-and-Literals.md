# [Data structures and types][grammar-and-types] #
## Data types ##

> The lastest ECMAScript standard defines **seven(七种)** data types.

#### Six data types that are [primitives](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) ####

> A **primitive**(基元) is data that is not an object and has no methods. 

- **Boolean**. `true` and `false`.
- **null**. A special keyword denoting a null value. And `null` is not the same as Null, NULL ,or any other variant for JavaScript is case-sensitive.
- **undefined**. A top-level property whose value is undefined.
- **Number**. 42 or 3.14159.
- **String**.
- **Symbol** (new in ECMAScript 2015). A data type whose instances are unique and immutable.

#### Object ####

**Objects** and **functions** are the other fundamental elements in the language.

> You can think of objects as **named container for values**, and functions as **procedures that your application can perform**.

### Data type conversion ###

**JavaScript is a dynamically typed language**. That means you don't have to specify the data type of a variable when you declare it.

> In expressions involving numeric and string values with the `+` operator, JavaScript converts numeric values to strings.

	x = "The answer is" + 42; //"The answer is 42"

**In statements involving other operators, JavaScript does not convert numeric values to strings.**

	"37" - 7 //30
	"37" + 7 //377

Functions [`parseInt()`][parseInt] and [`parseFloat()`][parseFloat] is used for converting strings to numbers.


## Literals ##

> Literals are fixed values, not variables, that you literally provide in your script.

- [Array literals](#array-literals)
- [Boolean literals](#boolean-literals)
- [Floating-point literals](#floating-pointing)
- [Integers](#intergers)
- [Object literals](#object-literals)
- [RegExp literals](#regexp-literals)
- [String literals](#string-literals)

### <a name='array-literals'> Array literals </a> ###

An array literal is a list of zero or more expressions, each of which represents an array element, enclosed in square brackets ([]).

> Note: [An array literal is a type of object initializer.][object-initializer]

**Extra commas in array literals**

	var fish = ["Lion", , "Angel"]; //fish[1] is undefined

is the same as:

	var fish = ["Lion", undefined,"Angel"];

If you include a trailling(尾随) comma at the end of elements, the comma is ignored.

	var myList = ['home', , 'school',];

> The length of the array is three. And trailing commas can create errors in older browser versions.


### <a name='boolean-literals'> Boolean literals </a> ###

The Boolean type has two literal values: `true` and `false`.

### <a name='intergers'> Intergers </a> ###

- Decimal integer literal consists of a sequence of digits without a leading 0 (zero).
- Leading 0 (zero) on an integer literal, or leading 0o (or 0O) indicates it is in octal. Octal integers can include only the digits 0-7.
- Leading 0x (or 0X) indicates hexadecimal. Hexadecimal integers can include digits (0-9) and the letters a-f and A-F.
- Leading 0b (or 0B) indicates binary. Binary integers can include digits only 0 and 1.

### <a name='floating-pointing'> Floating-point literals </a> ###

A floating-point literal can have the following parts:

- A decimal integer which can be signed (preceded by `+` or `-`),
- A decimal point (`.`),
- A fraction (another decimal number),
- An exponent.

### <a name='object-literals'> Object literals </a> ###
An object literal is a list of zero or more pairs of property names and associated values of an object, enclosed in curly braces `{}`.

> You should not use an object literal at the beginning of a statement. This will lead to an error or not behave as you expect.

	var sales = "Toyota";
	function carTypes(name) { ... }
	
	var car = {myCar: "Saturn", getCar: carType("Honda"), special: sales};

> The first element of the car object defines a property `myCar`, and assigns to ti a new string value `"Saturn"`. It's called pairs of (property/value). the second element, the `getCar` property, is immediately assigned the result of invoking the function `carTypes("Honda")`; the third element, the `special` property, uses an existing variable `sales`.

Additionally, you can use a numeric or string literal for the name of a property or nest an object inside another.

	var car = { manyCars: {a: "Saab", "b": "Jeep"}, 7: "Mazda" };
	console.log(car.manyCars.b); //Jeep

Object **property names** can be any string, including the empty string.

> If the property name would not be a valid JavaScript identifier or number, it must be enclosed in quotes. Property names that are not valid identifiers also cannot be accessed as a dot `.` property, but can be accessed and set with the array-like notation `[]`.

	var unusualPropertyNames = {
	"!": "Bang!"
	}
	console.log(unusualPropertyNames.!); // SyntaxError: Unexpected token !
	console.log(unusualPropertyNames["!"]); // Bang!

### <a name='regexp-literals'> RegExp literals </a> ###

A regex literal is a pattern enclosed between slashes.

	var re = /ab+c/;


### <a name='string-literals'> String literals </a> ###

A string literal is zero or more characters enclosed in double `"` or single `'` quotation marks.

You can call any of the methods of the String object on a string literal value : JavaScript automatically converts the string literal to a temporary String object, calls the method, then discards the temporary String object.

	console.log("John's cat".length);

#### Using special characters in strings ####

- `\n`
- `\t`
- `\b`
- ...

#### Escaping characters ####

`\` is used as a Escaping characters(转义字符).


[grammar-and-types]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types
[parseInt]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt 
[parseFloat]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat
[object-initializer]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects#Using_object_initializers


1/18/2017 5:00:02 PM 