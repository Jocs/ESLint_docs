3.4 [consistent-return](http://eslint.org/docs/rules/consistent-return) - require `return` statements to either always or never specify values

这条规则的目的就是为了在函数中有分支时，保证所有的`return` 语句要么指定一个数值，要么都不指定返回值。这样来避免错误的发生。比如下面的代码将被视为错误：

``` javascript
/*eslint consistent-return: 2*/

function doSomething(condition) {

    if (condition) {
        return true;
    } else {
        return;      /*error Expected a return value.*/
    }
}

function doSomething(condition) {

    if (condition) {
        return;
    } else {
        return true; /*error Expected no return value.*/
    }
}
```

而下面代码将被视为合法：

``` javascript
/*eslint consistent-return: 2*/

function doSomething(condition) {

    if (condition) {
        return true;
    } else {
        return false;
    }
}
```

3.5 [curly](http://eslint.org/docs/rules/curly) - specify curly brace conventions for all control statements

这条规则的目的就是为了防止代码出错和使得代码更加简洁，该规则保证了所有的代码块语句需要被大括号包围，当代码块没有使用大括号时就会报错。举个栗子：

``` javascript
/*eslint curly: 2*/

if (foo) foo++; /*error Expected { after 'if' condition.*/

while (bar)     /*error Expected { after 'while' condition.*/
    baz();

if (foo) {      /*error Expected { after 'else'.*/
    baz();
} else qux();
```



3.6 [default-case](http://eslint.org/docs/rules/default-case) - require `default` case in `switch` statements

该条规则保证了所有的switch语句都必须要有一个default分支。举个栗子：

``` javascript
/*eslint default-case: 2*/

switch (a) {       /*error Expected a default case.*/
    case 1:
        /* code */
        break;
}
```



3.7 [dot-location](http://eslint.org/docs/rules/dot-location) - enforces consistent newlines before or after dots

在JavaScript中，在书写对象的属性或方法时，新的一行代码可以以`.` 开头，也可以以`.` 结束，统一`.` 的位置可以大大提高代码的可读性。

该规则接受两个option，分别是`object` 和`property` ,当选则`object` 的时候，`.` 应该放在object一行，当选择`property` 时候，`.` 应该放在property一行。

3.8 [dot-notation](http://eslint.org/docs/rules/dot-notation) - encourages use of dot notation whenever possible

在JavaScript中，我们可以用过点表示法（dot notation）或者中括号表示法（square-bracket）来读取对象的属性。但是在JavaScript中更推荐使用dot notation方法，因为这种方法更易读，也不显得冗长，并且在大多数代码压缩时工作正常。

**Rule Details**

这条规则的目的就是通过尽量使用dot notation来保证代码的一致性并且提高代码的可读性。正因为如此，当我们在不必要的时候使用了square-bracket notation的时，会抛出错误。

例如下面代码将会抛出错误：

``` javascript
/*eslint dot-notation: 2*/

var x = foo["bar"]; /*error ["bar"] is better written in dot notation.*/
```

而下面代码将显示正常：

``` javascript
/*eslint dot-notation: 2*/

var x = foo.bar;

var x = foo[bar];    // Property name is a variable, square-bracket notation required
```

这条规则接受一个对象作为选项。默认情况如下：

``` javascript
{
    "rules": {
        "dot-notation": [2, {"allowKeywords": true, "allowPattern": ""}]
    }
}
```

这儿主要说说`"allowPattern"` ,该属性接受一个正则表达式，该选项属性让我们可以通过bracket notation来获取属性值，该属性名必须和allowPattern中的正则表达式匹配。

配置文件如下：

``` javascript
{
    "rules": {
        "camelcase": 2
        "dot-notation": [2, {"allowPattern": "^[a-z]+(_[a-z]+)+$"}]
    }
}
```

实例代码：

``` javascript
/*eslint camelcase: 2*/
/*eslint dot-notation: [2, {"allowPattern": "^[a-z]+(_[a-z]+)+$"}]*/

var data = {};
data.foo_bar = 42;    /*error Identifier 'foo_bar' is not in camel case.*/

var data = {};
data["fooBar"] = 42;  /*error ["fooBar"] is better written in dot notation.*/

var data = {};
data["foo_bar"] = 42; // no warning
```

3.9 [eqeqeq](http://eslint.org/docs/rules/eqeqeq) - require the use of `===` and `!==` (fixable)

顾名思义，在进行比较时，使用全等`===` 和完全不等`!==`.

3.10 [guard-for-in](http://eslint.org/docs/rules/guard-for-in) - make sure `for-in` loops have an `if` statement

顾名思义，保证在`for-in` 循环中使用了`if` 语句。

3.11 [no-alert](http://eslint.org/docs/rules/no-alert) - disallow the use of `alert`, `confirm`, and `prompt`

顾名思义，保证了在代码中没有使用`alert`, `confirm`, and `prompt`

3.12 [no-caller](http://eslint.org/docs/rules/no-caller) - disallow use of `arguments.caller` or `arguments.callee`

顾名思义，保证在代码中不使用 `arguments.caller` or `arguments.callee`，因为者两个方法在严格模式下是禁止使用的，我们应该尽量使用严格模式。

3.13 [no-case-declarations](http://eslint.org/docs/rules/no-case-declarations) - disallow lexical declarations in case clauses

禁止在case/default语句中使用lexical declarations，例如`let`, `const`, `function` and `class` .因为在case/default中的声明，在整个switch语句中都能够访问到，如果实在需要声明变量，可以加大括号。

举个栗子：

``` javascript
switch (foo) {
    case 1: {
        let x = 1;
        break;
    }
    case 2: {
        const y = 2;
        break;
    }
    case 3: {
        function f() {}
        break;
    }
    default: {
        class C {}
    }
}
```



3.14 [no-div-regex](http://eslint.org/docs/rules/no-div-regex) - disallow division operators explicitly at beginning of regular expression

这条规则主要目的用来消除`/` (除号)操作符对程序员的迷惑，比如在正则表达式`/=foo/` 中，我们并不能够确定第一个`/` 是除号还是正则表达式，因此我们需要在等号前面加一个转移符。写成如下形式就是正确的了。

``` javascript
/*eslint no-div-regex: 2*/

function bar() { return /\=foo/; }
```



3.15 [no-else-return](http://eslint.org/docs/rules/no-else-return) - disallow `else` after a `return` in an `if`

在if else语句中，如果else语句中只含有一个return语句，那么完全可以不适用else语句，直接return。

具体参见官方文档。



3.16 [no-empty-label](http://eslint.org/docs/rules/no-empty-label) - disallow use of labels for anything other than loops and switches

labeled 语句只有在结合`break` 和`continue` 语句时一起使用，这条规则的目的就是保证了在必须得时候才能够使用labeled语句。



3.17 [no-empty-pattern](http://eslint.org/docs/rules/no-empty-pattern) - disallow use of empty destructuring patterns

故名思议，在结构赋值时，模式不能为空，为空该规则就会报错。

在ECMAScript2015的结构赋值中，模式为空是不会报错的，只是这样的结构赋值没有任何效果，该条规则就保证了模式不能为空，也就保证了结构赋值的有效性。

举个栗子，下面的代码将被视为不合法：

``` javascript
/*eslint no-empty-pattern: 2*/

var {} = foo;
var [] = foo;
var {a: {}} = foo;
var {a: []} = foo;
function foo({}) {}
function foo([]) {}
function foo({a: {}}) {}
function foo({a: []}) {}
```

下面的代码就不会报错了。

``` javascript
/*eslint no-empty-pattern: 2*/

var {a = {}} = foo;
var {a = []} = foo;
function foo({a = {}}) {}
function foo({a = []}) {}
```



3.18 [no-eq-null](http://eslint.org/docs/rules/no-eq-null) - disallow comparisons to null without a type-checking operator

该条规则保证了在和`null` 比较时使用`===` 和 `!==` 而不能够使用`==` 和`!=` .



3.19 [no-eval](http://eslint.org/docs/rules/no-eval) - disallow use of `eval()`

不解释。



3.20 [no-extend-native](http://eslint.org/docs/rules/no-extend-native) - disallow adding to native types

不能够向native的对象上面添加属性。下面向内部对象添加属性的方式都将被视为不合法的。

- `Object.prototype.a = "a";`
- `Object.defineProperty(Array.prototype, "times", {value: 999});`


- `var x = Object; x.prototype.thing = a;`
- `eval("Array.prototype.forEach = 'muhahaha'");`
- `with(Array) { prototype.thing = 'thing'; };`
- `window.Function.prototype.bind = 'tight';`



3.21 [no-extra-bind](http://eslint.org/docs/rules/no-extra-bind) - disallow unnecessary function binding

`bind()` 方法用来给bind的函数添加一个特定的this 值，也就是说，bind的对象就是函数中的this对象，在我们使用了bind方法时，我们应该确保我们的函数体内有this值。而这条规则的作用就是保证了调用bind方法的函数体内有this对象。规避了不必要的使用bind方法的情况。

**注意：箭头函数中没有this对象，也就不能够使用bind()方法。该规则保证了在所有的箭头函数中使用bind方法将被视为错误。

举个栗子，下面的代码将被视为错误：

``` javascript
/*eslint no-extra-bind: 2*/
/*eslint-env es6*/

var x = function () {   /*error The function binding is unnecessary.*/
    foo();
}.bind(bar);

var x = (() => {        /*error The function binding is unnecessary.*/
    foo();
}).bind(bar);

var x = (() => {        /*error The function binding is unnecessary.*/
    this.foo();
}).bind(bar);

var x = function () {   /*error The function binding is unnecessary.*/
    (function () {
      this.foo();
    }());
}.bind(bar);

var x = function () {   /*error The function binding is unnecessary.*/
    function foo() {
      this.bar();
    }
}.bind(baz);

```

而下面的代码将不会报错

``` javascript
/*eslint no-extra-bind: 2*/

var x = function () {
    this.foo();
}.bind(bar);

var x = function (a) {
    return a + 1;
}.bind(foo, bar);
```



3.22 [no-fallthrough](http://eslint.org/docs/rules/no-fallthrough) - disallow fallthrough of `case` statements (recommended)

在JavaScript中，最容易出错的一个语法结构就是`switch`语句了，因为如果`case` 语句中如果没有添加`break` 等语句，该`case` 往往可能会「fall through」到下一个`case` .如果这不是我们需要的，往往会导致错误。该条规则就是为了防止「fall through」而设计的。

该规则的目的就是为了消除从一个case到另一个case的非故意的「fall through」。如果没有添加break等终止语句或者没有添加注释语句，将会抛出错误。

举个栗子，下面的代码将会认为错误：

``` javascript
/*eslint no-fallthrough: 2*/

switch(foo) {
    case 1:            /*error Expected a "break" statement before "case".*/
        doSomething();

    case 2:
        doSomething();
}
```

而下面的代码将不被视为错误：

``` javascript
/*eslint no-fallthrough: 2*/

switch(foo) {
    case 1:
        doSomething();
        break;

    case 2:
        doSomething();
}

function bar(foo) {
    switch(foo) {
        case 1:
            doSomething();
            return;

        case 2:
            doSomething();
    }
}

switch(foo) {
    case 1:
        doSomething();
        throw new Error("Boo!");

    case 2:
        doSomething();
}

switch(foo) {
    case 1:
    case 2:
        doSomething();
}

switch(foo) {
    case 1:
        doSomething();
        // falls through

    case 2:
        doSomething();
}
```

**注意：上面的代码中最后一个case并不会抛出错误，这是因为最后一个case也没法`fall through` .



3.23 [no-floating-decimal](http://eslint.org/docs/rules/no-floating-decimal) - disallow the use of leading or trailing decimal points in numeric literals

该条规则保证了在使用浮点小数时，不能够省略小数点前面的数或者后面的数，必须写全。

举个栗子，下面书写都是不合法的：

``` javascript
/*eslint no-floating-decimal: 2*/

var num = .5;  /*error A leading decimal point can be confused with a dot.*/
var num = 2.;  /*error A trailing decimal point can be confused with a dot.*/
var num = -.7; /*error A leading decimal point can be confused with a dot.*/
```

而下面的书写是合法了：

``` javascript
/*eslint no-floating-decimal: 2*/

var num = 0.5;
var num = 2.0;
```



3.24 [no-implicit-coercion](http://eslint.org/docs/rules/no-implicit-coercion) - disallow the type conversions with shorter notations

在JavaScript中有很多方法用来转换值得类型，其中一些方法可能很难理解。比如如下：

``` javascript
var b = !!foo;
var b = ~foo.indexOf(".");
var n = +foo;
var n = 1 * foo;
var s = "" + foo;
foo += "";
```

上面的方法可以通过如下方法替换：

``` javascript
var b = Boolean(foo);
var b = foo.indexOf(".") !== -1;
var n = Number(foo);
var n = Number(foo);
var s = String(foo);
foo = String(foo);
```

该条规则的目的就是为了消除简写的类型转换，而推荐使用一种更加「自解释」的转换方法。

该规则有三个选项。如下：

``` javascript
{
  "rules": {
    "no-implicit-coercion": [2, {"boolean": true, "number": true, "string": true}]
  }
}
```

默认情况下，三个选项都为true。当某个选项为true时，该项简写转换形式将被报错。



3.25 [no-implied-eval](http://eslint.org/docs/rules/no-implied-eval) - disallow use of `eval()`-like methods

不解释，请参见官网



3.26 [no-invalid-this](http://eslint.org/docs/rules/no-invalid-this) - disallow `this` keywords outside of classes or class-like objects

在严格模式下，在classes或者classes-like对象外部使用this关键词this将被视为`undefined` 并且抛出`TypeError`错误。这条规则的目的就是为了消除在classes和classes-like外面使用`this` 关键词。

这条规则主要地是判断构造函数或者方法中是否包含this关键词。

该规则通过如下条件来判断一个函数是否是构造函数。

* 函数名开头大写
* 函数是ES2015 Classes中构造函数

该规则通过如下条件判断一个函数是否是对象中的方法

* 该函数在对象字面量中
* 该函数被赋值给了一个属性
* 该函数是ES2015 Classes中的一个方法/getter/setter.

该规则还允许`this` 关键字出现在以下函数内部：

* 调用`call/apply/bind` 的函数中
* this可以出现在array 方法的回调函数中（例如forEach()），当然在制定了thisArg的情况下。
* 如果一个函数的JSDoc 注释中包含@this，那么this也可以出现在该函数中。

其他情况下出现this关键词将会被视为错误。

举个栗子，下面的情况将被视为错误：

``` javascript
/*eslint no-invalid-this: 2*/
/*eslint-env es6*/

this.a = 0;            /*error Unexpected `this`.*/
baz(() => this);       /*error Unexpected `this`.*/

(function() {
    this.a = 0;        /*error Unexpected `this`.*/
    baz(() => this);   /*error Unexpected `this`.*/
})();

function foo() {
    this.a = 0;        /*error Unexpected `this`.*/
    baz(() => this);   /*error Unexpected `this`.*/
}

var foo = function() {
    this.a = 0;        /*error Unexpected `this`.*/
    baz(() => this);   /*error Unexpected `this`.*/
};

foo(function() {
    this.a = 0;        /*error Unexpected `this`.*/
    baz(() => this);   /*error Unexpected `this`.*/
});

obj.foo = () => {
    // `this` of arrow functions is the outer scope's.
    this.a = 0;        /*error Unexpected `this`.*/
};

var obj = {
    aaa: function() {
        return function foo() {
            // There is in a method `aaa`, but `foo` is not a method.
            this.a = 0;      /*error Unexpected `this`.*/
            baz(() => this); /*error Unexpected `this`.*/
        };
    }
};

class Foo {
    static foo() {
        this.a = 0;      /*error Unexpected `this`.*/
        baz(() => this); /*error Unexpected `this`.*/
    }
}

foo.forEach(function() {
    this.a = 0;          /*error Unexpected `this`.*/
    baz(() => this);     /*error Unexpected `this`.*/
});
```

 而下面的代码将被视为合法：

``` javascript
/*eslint no-invalid-this: 2*/
/*eslint-env es6*/

function Foo() {
    // OK, this is in a legacy style constructor.
    this.a = 0;
    baz(() => this);
}

class Foo {
    constructor() {
        // OK, this is in a constructor.
        this.a = 0;
        baz(() => this);
    }
}

var obj = {
    foo: function foo() {
        // OK, this is in a method (this function is on object literal).
        this.a = 0;
    }
};

var obj = {
    foo() {
        // OK, this is in a method (this function is on object literal).
        this.a = 0;
    }
};

var obj = {
    get foo() {
        // OK, this is in a method (this function is on object literal).
        return this.a;
    }
};

var obj = Object.create(null, {
    foo: {value: function foo() {
        // OK, this is in a method (this function is on object literal).
        this.a = 0;
    }}
});

Object.defineProperty(obj, "foo", {
    value: function foo() {
        // OK, this is in a method (this function is on object literal).
        this.a = 0;
    }
});

Object.defineProperties(obj, {
    foo: {value: function foo() {
        // OK, this is in a method (this function is on object literal).
        this.a = 0;
    }}
});

function Foo() {
    this.foo = function foo() {
        // OK, this is in a method (this function assigns to a property).
        this.a = 0;
        baz(() => this);
    };
}

obj.foo = function foo() {
    // OK, this is in a method (this function assigns to a property).
    this.a = 0;
};

Foo.prototype.foo = function foo() {
    // OK, this is in a method (this function assigns to a property).
    this.a = 0;
};

class Foo {
    foo() {
        // OK, this is in a method.
        this.a = 0;
        baz(() => this);
    }
}

var foo = (function foo() {
    // OK, the `bind` method of this function is called directly.
    this.a = 0;
}).bind(obj);

foo.forEach(function() {
    // OK, `thisArg` of `.forEach()` is given.
    this.a = 0;
    baz(() => this);
}, thisArg);

/** @this Foo */
function foo() {
    // OK, this function has a `@this` tag in its JSDoc comment.
    this.a = 0;
}
```



3.27 [no-iterator](http://eslint.org/docs/rules/no-iterator) - disallow usage of `__iterator__` property

不能够使用SpiderMonkey 的扩展， `__iterator__` 属性，使用会报错。因为这条属性在很多浏览器中也没有被实施。



3.28 [no-labels](http://eslint.org/docs/rules/no-labels) - disallow use of labeled statements

该条规则保证了代码中不适用labeled 语句。



3.29 [no-lone-blocks](http://eslint.org/docs/rules/no-lone-blocks) - disallow unnecessary nested blocks

该条规则的目的就是为了消除不必要的和可能会导致疑惑的代码块，该代码块可以在代码最外层，也可以在其他代码块内。

**在严格模式下，当我们配置了`ecamFeatures: {blockBindings: true },时，下面的代码将不会抛出错误。

``` javascript
/*eslint-env es6*/
/*eslint no-lone-blocks: 2*/
"use strict";

{
    function foo() {}
}
```





3.30 [no-loop-func](http://eslint.org/docs/rules/no-loop-func) - disallow creation of functions within loops

在循环中书写函数通常会导致错误，因为这种方式下，函数会创造一个包含循环的闭包(???)。例如：

``` javascript
for (var i = 0; i < 10; i++) {
    funcs[i] = function() {
        return i;
    };
}
```

在上面的例子中，本意是希望循环中的每个函数都能够返回一个不同的数字，但实际上，每个函数都返回10，因为10是i在该作用域中的最终值。

下面的例子将被视为不合法，因为在循环中定义了函数，并且函数引用了外部变量。

``` javascript
/*eslint no-loop-func: 2*/
/*eslint-env es6*/

for (var i=10; i; i--) {
    (function() { return i; })();     /*error Don't make functions within a loop*/
}

while(i) {
    var a = function() { return i; }; /*error Don't make functions within a loop*/
    a();
}

do {
    function a() { return i; };      /*error Don't make functions within a loop*/
    a();
} while (i);

let foo = 0;
for (let i=10; i; i--) {
    // Bad, function is referencing block scoped variable in the outer scope.
    var a = function() { return foo; }; /*error Don't make functions within a loop*/
    a();
}
```

下面的例子将被视为合法：因为即使在循环中定义了函数，但是函数内部没有引用外部变量，或者使用let定义的代码块变量。

``` javascript
/*eslint no-loop-func: 2*/
/*eslint-env es6*/

var a = function() {};

for (var i=10; i; i--) {
    a();
}

for (var i=10; i; i--) {
    var a = function() {}; // OK, no references to variables in the outer scopes.
    a();
}

for (let i=10; i; i--) {
    var a = function() { return i; }; // OK, all references are referring to block scoped variable in the loop.
    a();
}
```

如果想了解更多，请参考[Don't make functions within a loop](http://jslinterrors.com/dont-make-functions-within-a-loop/)



3.31 [no-magic-numbers](http://eslint.org/docs/rules/no-magic-numbers) - disallow the use of magic numbers

详见官网，顾名思义，就是使用变量来代替数字。



3.32 [no-multi-spaces](http://eslint.org/docs/rules/no-multi-spaces) - disallow use of multiple spaces (fixable)

顾名思义，该规则保证了在逻辑表达式、条件表达式、申明语句、数组元素、对象属性、sequences、函数参数中不使用超过一个的空白符。



3.33 [no-multi-str](http://eslint.org/docs/rules/no-multi-str) - disallow use of multiline strings

顾名思义，该规则保证了字符串不分两行书写。



3.34 [no-native-reassign](http://eslint.org/docs/rules/no-native-reassign) - disallow reassignments of native objects

顾名思义，该规则保证了不重写原生对象。



3.35 [no-new-func](http://eslint.org/docs/rules/no-new-func) - disallow use of new operator for `Function` object

该规则保证了不适用`new Function();` 语句。



3.36 [no-new-wrappers](http://eslint.org/docs/rules/no-new-wrappers) - disallows creating new instances of `String`,`Number`, and `Boolean`

简单来说，就是不用如下书写代码：

``` javascript
/*eslint no-new-wrappers: 2*/

var stringObject = new String("Hello world"); /*error Do not use String as a constructor.*/
var numberObject = new Number(33);            /*error Do not use Number as a constructor.*/
var booleanObject = new Boolean(false);       /*error Do not use Boolean as a constructor.*/

var stringObject = new String;                /*error Do not use String as a constructor.*/
var numberObject = new Number;                /*error Do not use Number as a constructor.*/
var booleanObject = new Boolean;              /*error Do not use Boolean as a constructor.*/
```



3.37 [no-new](http://eslint.org/docs/rules/no-new) - disallow use of the `new` operator when not part of an assignment or comparison

简单来说，在使用new来调用构造函数时，必须把生成的实例赋值给一个变量，否者将会报错。举个栗子，下面的代码将会报错。

``` javascript
/*eslint no-new: 2*/

new Thing(); /*error Do not use 'new' for side effects.*/
```



3.38 [no-octal-escape](http://eslint.org/docs/rules/no-octal-escape) - disallow use of octal escape sequences in string literals, such as `var foo = "Copyright \251";`

简单来说，使用Unicode escapes代替octal escapes。具体请参见官网。



3.39 [no-octal](http://eslint.org/docs/rules/no-octal) - disallow use of octal literals (recommended)

简单来说，不要使用八进制的语法。



3.40 [no-param-reassign](http://eslint.org/docs/rules/no-param-reassign) - disallow reassignment of function parameters

通过对函数参数进行赋值通常会产生误会或者导致代码难以理解，同时修改函数的参数也会改变arguments对象，因此通常对函数参数进行赋值都是无意的，是一种错误的行为。

这条规则的目的就是为了防止无意的对函数参数进行重写。

该规则接受一个选项，`"prop"`.默认值是false。当设置为true时，即使函数参数的属性改变时也会报错。

举个栗子，下面的代码将被视为不合法：

``` javascript
/*eslint no-param-reassign: 2*/

function foo(bar) {
    bar = 13;       /*error Assignment to function parameter 'bar'.*/
}

function foo(bar) {
    bar++;          /*error Assignment to function parameter 'bar'.*/
}
```

而当选项`"props"` 设置为ture时，下面代码也是不合法的：

``` javascript
/*eslint no-param-reassign: [2, { "props": true }]*/

function foo(bar) {
    bar.prop = "value"; /*error Assignment to function parameter 'bar'.*/
}

function foo(bar) {
    delete bar.aaa;     /*error Assignment to function parameter 'bar'.*/
}

function foo(bar) {
    bar.aaa++;          /*error Assignment to function parameter 'bar'.*/
}
```

下面代码将被视为合法：

``` javascript
/*eslint no-param-reassign: 2*/

function foo(a) {
    var b = a;
}
```

当`{"props": false}`:

``` javascript
/*eslint no-param-reassign: [2, { "props": false }]*/

function foo(bar) {
    bar.prop = "value";
}

function foo(bar) {
    delete bar.aaa;
}

function foo(bar) {
    bar.aaa++;
}

```



3.41 [no-process-env](http://eslint.org/docs/rules/no-process-env) - disallow use of `process.env`

参见官网。顾名思义是不推荐使用`process.env` 



3.42 [no-proto](http://eslint.org/docs/rules/no-proto) - disallow usage of `__proto__` property

不解释。



3.43 [no-redeclare](http://eslint.org/docs/rules/no-redeclare) - disallow declaring the same variable more than once (recommended)

顾名思义，避免重复申明一个变量



3.44 [no-return-assign](http://eslint.org/docs/rules/no-return-assign) - disallow use of assignment in `return` statement

顾名思义，不要再return语句中使用赋值语句。



3.45 [no-script-url](http://eslint.org/docs/rules/no-script-url) - disallow use of `javascript:` urls.

顾名思义，不要使用`javascript:` URLs.也就是说下面代码不推荐：

``` javascript
/*eslint no-script-url: 2*/

location.href = "javascript:void(0)"; /*error Script URL is a form of eval.*/
```



3.46 [no-self-compare](http://eslint.org/docs/rules/no-self-compare) - disallow comparisons where both sides are exactly the same

顾名思义，代码中不能够出现和本身作比较的比较语句。



3.47 [no-sequences](http://eslint.org/docs/rules/no-sequences) - disallow use of the comma operator

顾名思义，不要使用逗号操作符，详见官网。



3.48 [no-throw-literal](http://eslint.org/docs/rules/no-throw-literal) - restrict what can be thrown as an exception

通过throw语句抛出的对象必须是Error对象本身或者通过Error对象定义的对象。

但是下列情况除外：

``` javascript
/*eslint no-throw-literal: 2*/

var err = "error";
throw err;

function foo(bar) {
    console.log(bar);
}
throw foo("error");

throw new String("error");

var foo = {
    bar: "error"
};
throw foo.bar;
```

以上通过throw抛出的错误，都不是通过Error对象生成。



3.49 [no-unused-expressions](http://eslint.org/docs/rules/no-unused-expressions) - disallow usage of expressions in statement position

Unused expressions是指那些代码中没有被使用到的表达式或值，例如：

``` javascript
"Hello world";
```

上面的字符串是合法的JavaScript表达式，但是却没有被使用到，尽管它不是语法错误，但是很明显是一种逻辑错误，并对代码执行没有任何帮助。

**Rule Details**

该规则的目的是用于消除没用的表达式，表达式的值应该始终被用到，除了一些会带来代码副作用的表达式。例如函数调用、赋值语句、以及`new`操作符。这些表达式也是有用的。

**注意**： Sequence 表达式（those using a comma, such as `a = 1, b = 2`）通常被认为是没用的表达式，除非被赋值或者函数调用时作为参数传入。

该规则可以接受两个选项：

- `allowShortCircuit` set to `true` will allow you to use short circuit evaluations in your expressions (Default: `false`).
- `allowTernary` set to `true` will enable you use ternary operators in your expressions similarly to short circuit evaluations (Default: `false`).

举个栗子，下面的表达式将被视为不合法：

``` javascript
/*eslint no-unused-expressions: 2*/

0         /*error Expected an assignment or function call and instead saw an expression.*/

if(0) 0   /*error Expected an assignment or function call and instead saw an expression.*/

{0}       /*error Expected an assignment or function call and instead saw an expression.*/

f(0), {}  /*error Expected an assignment or function call and instead saw an expression.*/

a && b()  /*error Expected an assignment or function call and instead saw an expression.*/

a, b()    /*error Expected an assignment or function call and instead saw an expression.*/

c = a, b; /*error Expected an assignment or function call and instead saw an expression.*/
```

而下面的例子，将被视为合法：

``` javascript
/*eslint no-unused-expressions: 2*/

{}

f()

a = 0

new C

delete a.b

void a
```

当把allowShortCircuit设置为true时，下面的代码将被视为合法：

``` javascript
/*eslint no-unused-expressions: [2, { allowShortCircuit: true }]*/

a && b()

a() || (b = c)
```

当把allowTernary设置为true时，下面的代码将被视为合法：

``` javascript
/*eslint no-unused-expressions: [2, { allowTernary: true }]*/

a ? b() : c()

a ? (b = c) : d()
```

当把allowShortCircuit和allowTernary都设置为true时，下面的代码将被视为合法：

``` javascript
/*eslint no-unused-expressions: [2, { allowShortCircuit: true, allowTernary: true }]*/

a ? b() || (c = d) : e()
```

但是即使allowShortCircuit和allowTernary都设置为true时，下面的代码由于没有side effect，依然被认为是不合法的：

``` javascript
/*eslint no-unused-expressions: [2, { allowShortCircuit: true, allowTernary: true }]*/

a || b         /*error Expected an assignment or function call and instead saw an expression.*/

a ? b : 0      /*error Expected an assignment or function call and instead saw an expression.*/
```



3.50 [no-useless-call](http://eslint.org/docs/rules/no-useless-call) - disallow unnecessary `.call()` and `.apply()`

顾名思义，避免使用没有意义的`call()` 和 `apply()`.



3.51 [no-useless-concat](http://eslint.org/docs/rules/no-useless-concat) - disallow unnecessary concatenation of literals or template literals

该规则保证了没有必要的字符串拼接。

举个栗子，下面代码被视为不合法：

``` javascript
/*eslint no-useless-concat: 2*/
/*eslint-env es6*/

// these are the same as "10"
var a = `some` + `string`; /*error Unexpected string concatenation of literals.*/
var a = '1' + '0';         /*error Unexpected string concatenation of literals.*/
var a = '1' + `0`;         /*error Unexpected string concatenation of literals.*/
var a = `1` + '0';         /*error Unexpected string concatenation of literals.*/
var a = `1` + `0`;         /*error Unexpected string concatenation of literals.*/
```



3.52 [no-void](http://eslint.org/docs/rules/no-void) - disallow use of the `void` operator

不要使用void操作符。



3.53 [no-warning-comments](http://eslint.org/docs/rules/no-warning-comments) - disallow usage of configurable warning terms in comments - e.g. `TODO` or `FIXME`

很少用到详见官网。

3.54 [no-with](http://eslint.org/docs/rules/no-with) - disallow use of the `with` statement

顾名思义，不要使用`with` 语句。



3.55 [radix](http://eslint.org/docs/rules/radix) - require use of the second argument for `parseInt()`

顾名思义，在使用`parseInt()` 方法时，需要传递第二个参数，来帮助解析，告诉方法解析成多少进制。



3.56[vars-on-top](http://eslint.org/docs/rules/vars-on-top) - require declaration of all vars at the top of their containing scope

顾名思义，在通过var申明变量时，应该放在代码块的顶部。



3.57[wrap-iife](http://eslint.org/docs/rules/wrap-iife) - require immediate function invocation to be wrapped in parentheses

顾名思义，立即执行函数需要通过圆括号包围。

该规则接受三个选项：`outside`  `inside`  `any` .默认选项时`outside` .



3.58[yoda](http://eslint.org/docs/rules/yoda) - require or disallow Yoda conditions

yoda条件语句就是对象字面量应该写在比较操作符的左边，而变量应该写在比较操作符的右边。如下：

``` javascript
if ("red" === color) {
    // ...
}
```

该规则接受两个参数，`never` 和`always` .默认值是`never`.

举个栗子，下面的代码将被视为不合法：因为默认变量放前面。

``` javascript
/*eslint yoda: 2*/

if ("red" === color) {          /*error Expected literal to be on the right side of ===.*/
    // ...
}

if (true == flag) {             /*error Expected literal to be on the right side of ==.*/
    // ...
}

if (5 > count) {                /*error Expected literal to be on the right side of >.*/
    // ...
}

if (-1 < str.indexOf(substr)) { /*error Expected literal to be on the right side of <.*/
    // ...
}

if (0 <= x && x < 1) {          /*error Expected literal to be on the right side of <=.*/
    // ...
}

```

当参数为`always`时。下面代码不合法：

``` javascript
/*eslint yoda: [2, "always"]*/

if (color == "blue") { /*error Expected literal to be on the left side of ==.*/
    // ...
}
```

该规则还接受一些选项，如下：

``` javascript
"yoda": [2, "never", {
    "exceptRange": false,
    "onlyEquality": false
}]
```

使用方法参见官网。