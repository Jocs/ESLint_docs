## ESLint Rules

### 一、Rules

ESLint 中的 Rules 被分成了不同的类来帮助我们理解其含义，所有的规则都有一个默认值。ESLint推荐了一些rules来捕获常见的一些错误，只需要在配置文件中添加 `extends: "eslint:recommended"` 就可以使用ESLint推荐的rules了。

### 二、Possible Errors

2.1 **Disallow or Enforce Dangling Commas (comma-dangle)**

对象字面量申明的对象中的尾部逗号在ECMAScript5（ECMAScript3！）是合法有效的，但是在IE8（when not in IE8 document mode）下面的对象声明在使用了尾部逗号时会抛出一个错误。

``` javascript
var foo = {
  bar: 'baz',
  qux: 'quux',
};
```

**Rule Details**

这条rule强制在对象字面量和数组字面量中使用始终如一的尾部逗号规则。这条rule接受一个参数，可以取如下值：

* `"always"` - 当检测到没有使用尾部逗号时抛出错误
* `"always-multiline"` - 当使用多行的对象字面量申明数组或对象时，如果没有使用尾部逗号就会抛出警告。当单行声明对象或数组时，如果使用了尾部逗号也会抛出错误。（个人建议：在使用这个参数值时，推荐使用多行格式来申明对象，并在每行末尾都加上逗号，对于一维数组单行书写，末尾不加逗号）
* `"never"` - 当检测到使用了尾部逗号时抛出错误

这条rule的默认值是`"never"`

当配置`"never"` 时，下面的栗子将抛出错误：

``` javascript
/*eslint comma-dangle: [2, "never"]*/
var foo = {
    bar: "baz",
    qux: "quux",   /*error Unexpected trailing comma.*/
};
var arr = [1,2,];  /*error Unexpected trailing comma.*/
foo({
  bar: "baz",
  qux: "quux",     /*error Unexpected trailing comma.*/
});
```

当配置`"never"` 时，下面的栗子将不会抛出错误：

``` javascript
/*eslint comma-dangle: [2, "never"]*/

var foo = {
    bar: "baz",
    qux: "quux"
};

var arr = [1,2];

foo({
  bar: "baz",
  qux: "quux"
});
```

当配置`"always"` 时，下面的栗子将不会抛出错误：

``` javascript
/*eslint comma-dangle: [2, "always"]*/

var foo = {
    bar: "baz",
    qux: "quux",
};

var arr = [1,2,];

foo({
  bar: "baz",
  qux: "quux",
});
```

当配置`"always"` 时，下面的栗子将抛出错误：

``` javascript
/*eslint comma-dangle: [2, "always"]*/

var foo = {
    bar: "baz",
    qux: "quux"   /*error Missing trailing comma.*/
};

var arr = [1,2];  /*error Missing trailing comma.*/

foo({
  bar: "baz",
  qux: "quux"     /*error Missing trailing comma.*/
});
```

当配置`"always-multiline"` 时，下面的栗子将被认为是错误的。

``` javascript
/*eslint comma-dangle: [1, "always-multiline"]*/

var foo = {
    bar: "baz",
    qux: "quux"                         /*error Missing trailing comma.*/
};

var foo = { bar: "baz", qux: "quux", }; /*error Unexpected trailing comma.*/

var arr = [1,2,];                       /*error Unexpected trailing comma.*/

var arr = [1,
    2,];                                /*error Unexpected trailing comma.*/

var arr = [
    1,
    2                                   /*error Missing trailing comma.*/
];

foo({
  bar: "baz",
  qux: "quux"                           /*error Missing trailing comma.*/
});
```

当配置`"always-multiline"` 时，下面的栗子将不会抛出错误。

``` javascript
/*eslint comma-dangle: [2, "always-multiline"]*/

var foo = {
    bar: "baz",
    qux: "quux",
};

var foo = {bar: "baz", qux: "quux"};
var arr = [1,2];

var arr = [1,
    2];

var arr = [
    1,
    2,
];

foo({
  bar: "baz",
  qux: "quux",
});
```

2.2 **[no-cond-assign](http://eslint.org/docs/rules/no-cond-assign) - disallow assignment in conditional expressions (recommended)**

在条件语句中，我们很容易把一个比较操作符（比如==）写成一个赋值操作符（比如=），举个栗子：

``` javascript
// Check the user's job title
if (user.jobTitle = "manager") {
    // user.jobTitle is now incorrect
}
```

在条件语句中使用赋值操作符是合法的原因之一就是，代码本身并不知道条件语句中的赋值是无意放进去还是有意为之。

**Rule Details**

这条规则的目的就是消除在 `for`, `if`, `while`, 和`do...while`等条件语句中使用赋值语句这种模棱两可的情况。

该条规则接受一个option，string类型，可以是下面两个值之一：

* `except-parens` (默认值)：禁止在条件语句中使用赋值语句，除非把赋值语句放在一个括号中。
* `always` :禁止在条件语句中使用赋值语句。

在配置`except-parens` 时，下面的语法将会报错

``` javascript
/*eslint no-cond-assign: 2*/

// Unintentional assignment
var x;
if (x = 0) {         /*error Expected a conditional expression and instead saw an assignment.*/
    var b = 1;
}

// Practical example that is similar to an error
function setHeight(someNode) {
    "use strict";
    do {             /*error Expected a conditional expression and instead saw an assignment.*/
        someNode.height = "100px";
    } while (someNode = someNode.parentNode);
}
```

在配置`except-parens` 时，下面的语法书写将被认为是合法的：

``` javascript
/*eslint no-cond-assign: 2*/

// Assignment replaced by comparison
var x;
if (x === 0) {
    var b = 1;
}

// Practical example that wraps the assignment in parentheses
function setHeight(someNode) {
    "use strict";
    do {
        someNode.height = "100px";
    } while ((someNode = someNode.parentNode));
}

// Practical example that wraps the assignment and tests for 'null'
function setHeight(someNode) {
    "use strict";
    do {
        someNode.height = "100px";
    } while ((someNode = someNode.parentNode) !== null);
}
```

在配置`always`时，下面的语法书写将被认为是错误的

``` javascript
/*eslint no-cond-assign: [2, "always"]*/

// Unintentional assignment
var x;
if (x = 0) {         /*error Unexpected assignment within an 'if' statement.*/
    var b = 1;
}

// Practical example that is similar to an error
function setHeight(someNode) {
    "use strict";
    do {             /*error Unexpected assignment within a 'do...while' statement.*/
        someNode.height = "100px";
    } while (someNode = someNode.parentNode);
}

// Practical example that wraps the assignment in parentheses
function setHeight(someNode) {
    "use strict";
    do {             /*error Unexpected assignment within a 'do...while' statement.*/
        someNode.height = "100px";
    } while ((someNode = someNode.parentNode));
}

// Practical example that wraps the assignment and tests for 'null'
function setHeight(someNode) {
    "use strict";
    do {             /*error Unexpected assignment within a 'do...while' statement.*/
        someNode.height = "100px";
    } while ((someNode = someNode.parentNode) !== null);
}
```

在配置`always`时，下面的语法书写将被认为合法的

``` javascript
/*eslint no-cond-assign: [2, "always"]*/

// Assignment replaced by comparison
var x;
if (x === 0) {
    var b = 1;
}

```

**个人建议：在使用这条规则时，推荐使用默认选项，也就是`except-parens` 

2.3 [no-console](http://eslint.org/docs/rules/no-console) - disallow use of `console` in the node environment (recommended)

这条规则很好理解，就是禁止使用console方法，举个栗子：

``` javascript
/*eslint no-console: 2*/

console.log("Hello world!");              /*error Unexpected console statement.*/
console.error("Something bad happened."); /*error Unexpected console statement.*/
```

2.4 [no-constant-condition](http://eslint.org/docs/rules/no-constant-condition) - disallow use of constant expressions in conditions (recommended)

这条规则的目的就是避免在条件语句中使用常量表达式，当在条件语句中使用常量语句时就会抛出错误。

举个栗子,下面的例子将会抛出错误：

``` javascript
/*eslint no-constant-condition: 2*/

if (true) {             /*error Unexpected constant condition.*/
    doSomething();
}

var result = 0 ? a : b; /*error Unexpected constant condition.*/

while (-2) {            /*error Unexpected constant condition.*/
    doSomething();
}

for (;true;) {          /*error Unexpected constant condition.*/
    doSomething();
}

do{                     /*error Unexpected constant condition.*/
    something();
} while (x = -1)
```

下面的例子将不会抛出错误：

``` javascript
/*eslint no-constant-condition: 2*/

if (x === 0) {
    doSomething();
}

do {
    something();
} while (x)

for (;;) {
    something();
}
```

2.4**Disallow Controls Characters in Regular Expressions (no-control-regex)**

Control characters非常特殊，出现于特定的信息文本中，表示某一控制功能的字符。在[ASCII码](http://baike.baidu.com/view/812.htm)中，第0～31号及第127号(共33个)是控制字符或通讯专用字符，如控制符：LF（换行）、CR（回车）、FF（换页）、DEL（删除）、BS（退格)、BEL（振铃）等；通讯专用字符：SOH（文头）、EOT（文尾）、ACK（确认）等。

这类字符通常很少出现在字符串和正则表达式中，因为这类字符很容易导致错误发生。

no-control-regex的目标就是保证在正则表达式中不使用control Characters

举个栗子，下面例子将会导致错误

``` javascript
/*eslint no-control-regex: 2*/

var pattern1 = /\\x1f/;
var pattern2 = new RegExp("\x1f"); /*error Unexpected control character in regular expression.*/
```

举个栗子，下面例子将不会导致错误：

``` javascript
/*eslint no-control-regex: 2*/

var pattern1 = /\\x20/;
var pattern2 = new RegExp("\x20");
```

2.5**(no-empty-character-class) Disallow Empty Character Classes**

空的字符分类（class）在正则表达式中不匹配任何字符，如果我们在正则表达式中使用了空字符分类有可能导致代码不能够正常工作。

举个栗子，下面就是一个使用了控制符分类不匹配任何字符串的正则表达式：

``` javascript
var foo = /^abc[]/;
```

举个栗子：下面代码将被认为错误的：

``` javascript
/*eslint no-empty-character-class: 2*/

var foo = /^abc[]/;  /*error Empty class.*/

/^abc[]/.test(foo);  /*error Empty class.*/

bar.match(/^abc[]/); /*error Empty class.*/
```

举个栗子，下面代码将被认为是正确的：

``` javascript
/*eslint no-empty-character-class: 2*/

var foo = /^abc/;

var foo = /^abc[a-z]/;
```

2.6 **[no-debugger](http://eslint.org/docs/rules/no-debugger) - disallow use of `debugger` (recommended)**

顾名思义，就是在代码中不能够使用`debugger`语句。

2.7 [no-dupe-args](http://eslint.org/docs/rules/no-dupe-args) - disallow duplicate arguments in functions (recommended)

顾名思义，函数参数不能够使用相同的参数名。

2.8 [no-dupe-keys](http://eslint.org/docs/rules/no-dupe-keys) - disallow duplicate keys when creating object literals (recommended)

顾名思义，在使用对象字面量申明对象的时候，不能够使用相同的键名，在增加这条规则后，使用相同键名将会报错，而在不适用这条规则时，申明对象时使用相同键名后面一条会覆盖前面一条相同的键值对。

2.9 [no-duplicate-case](http://eslint.org/docs/rules/no-duplicate-case) - disallow a duplicate case label. (recommended)

在switch语句中，禁止使用相同的case值。

2.10 [no-empty](http://eslint.org/docs/rules/no-empty) - disallow empty statements (recommended)

空语句通常发生在重构代码又没有完成的情况下：例如：

``` javascript
if (foo) {
}
```

这条规则的目的就是为了消除代码中的空代码块，空代码块并不会导致技术性错误，但是在代码review时可能会代码疑惑。带空代码块中包含注释语句时，将不再视为错误。

举个栗子，下面的代码将被视为错误：

``` javascript
/*eslint no-empty: 2*/

if (foo) {         /*error Empty block statement.*/
}

while (foo) {      /*error Empty block statement.*/
}

switch(foo) {      /*error Empty switch statement.*/
}

try {
    doSomething();
} catch(ex) {      /*error Empty block statement.*/

} finally {        /*error Empty block statement.*/

}
```

举个栗子，下面的代码将不会抛出错误

``` javascript
/*eslint no-empty: 2*/

if (foo) {
    // empty
}

while (foo) {
    // test
}

try {
    doSomething();
} catch (ex) {
    // Do nothing
}

try {
    doSomething();
} finally {
    // Do nothing
}
```

2.11**Disallow Assignment of the Exception Parameter (no-ex-assign)**

当错误发生并通过一个catch代码块去捕获错误时，有可能意外的重写了捕获的错误，比如：

``` javascript
try {
    // code
} catch (e) {
    e = 10;
}

```

这就会导致无法跟踪到捕获的错误信息。

举个栗子，下面的代码在此条规则下将会报错：

``` javascript
/*eslint no-ex-assign: 2*/

try {
    // code
} catch (e) {
    e = 10;   /*error Do not assign to the exception parameter.*/
}
```

举个栗子，下面的代码在此规则下将不会报错：

``` javascript
/*eslint no-ex-assign: 2*/

try {
    // code
} catch (e) {
    var foo = 'bar';
}
```

2.12 **[no-extra-boolean-cast](http://eslint.org/docs/rules/no-extra-boolean-cast) - disallow double-negation boolean casts in a boolean context (recommended)**

在if语句中，if括号中的表达式结果通常会自动强制转化为布尔值，因此通过两个`!!`来把if括号中的表达式强制转化为布尔值就显得没有必要了，例如下面两个表达式是等价的：

``` javascript
if (!!foo) {
    // ...
}

if (foo) {
    // ...
}
```

**Rule Details**

这条规则的目的就是消除本来不需要使用两个`!!` 来转化布尔值的情况。举个栗子，下面的代码将被视为错误：

``` javascript
/*eslint no-extra-boolean-cast: 2*/

var foo = !!!bar;             /*error Redundant multiple negation.*/

var foo = !!bar ? baz : bat;  /*error Redundant double negation in a ternary condition.*/

var foo = Boolean(!!bar);     /*error Redundant double negation in call to Boolean().*/

var foo = new Boolean(!!bar); /*error Redundant double negation in Boolean constructor call.*/

if (!!foo) {                  /*error Redundant double negation in an if statement condition.*/
    // ...
}

while (!!foo) {               /*error Redundant double negation in a while loop condition.*/
    // ...
}

do {
    // ...
} while (!!foo);              /*error Redundant double negation in a do while loop condition.*/

for (; !!foo; ) {             /*error Redundant double negation in a for loop condition.*/
    // ...
}
```

而下面的代码将不会报错：

``` javascript
/*eslint no-extra-boolean-cast: 2*/

var foo = !!bar;

function foo() {
    return !!bar;
}

var foo = bar ? !!baz : !!bat;
```

2.13 **[no-extra-parens](http://eslint.org/docs/rules/no-extra-parens) - disallow unnecessary parentheses**

这条规则用来限制圆括号的使用，当需要使用到圆括号的时候，才使用。其也可以用来只限制于函数表达式中。

**Rule Detail**

在少数情况下，多余的圆括号也被视为合法的：

* RegExp literals: `(/abc/).test(var)` 总是合法的。
* IIFES：`var x = (function () {})();`, `((function foo() {return 1;})())` 总是合法的。

这条规则可以指定选项，默认值是`all` ，当没有指定选项或者指定`all`时，在代码中只要有多余的圆括号就会警告，举个栗子，下面的代码将会报错：

``` javascript
/*eslint no-extra-parens: 2*/

a = (b * c); /*error Gratuitous parentheses around expression.*/

(a * b) + c; /*error Gratuitous parentheses around expression.*/

typeof (a);  /*error Gratuitous parentheses around expression.*/
```

下面的代码将不会报错：

``` javascript
/*eslint no-extra-parens: 2*/

(0).toString();

({}.toString.call());

(function(){} ? a() : b())

(/^a$/).test(x);
```

如果我们指定选项为`"functions"` 那么就只有函数表达式会被检测是否包含不必要的圆括号。举个栗子，下面的代码将被认为不合法：

``` javascript
/*eslint no-extra-parens: [2, "functions"]*/

((function foo() {}))();           /*error Gratuitous parentheses around expression.*/

var y = (function () {return 1;}); /*error Gratuitous parentheses around expression.*/
```

而下面代码将被认为合法的：

``` javascript
/*eslint no-extra-parens: [2, "functions"]*/

(0).toString();

({}.toString.call());

(function(){} ? a() : b());

(/^a$/).test(x);

a = (b * c);

(a * b) + c;

typeof (a);
```

2.14 **Disallow Extra Semicolons (no-extra-semi)**

顾名思义，这条规则就是用来限制使用多余的分号，尽管多余的分号不会导致代码错误，但是会使得代码难以理解。

使用这条规则后下面的代码将被认为不合法：

``` javascript
/*eslint no-extra-semi: 2*/

var x = 5;;      /*error Unnecessary semicolon.*/

function foo() {
    // code
};               /*error Unnecessary semicolon.*/
```

而下面的代码将被视为合法：

``` javascript
/*eslint no-extra-semi: 2*/

var x = 5;

var foo = function() {
    // code
};
```

2.15 **Disallow Function Assignment (no-func-assign)**

JavaScript中的函数可以通过函数声明：

``` javascript
function foo(){}
```

也可以通过函数表达式来书写：

``` javascript
var foo = function() {...};
```

通过函数声明的语法格式，函数有可能被重写或者重新赋值，使用该条规则后，在通过函数声明书写函数时，函数被重写或者被重新赋值就会抛出错误。

如下：

``` javascript
function foo() {}
foo = bar;
```

举个栗子，如下的语法格式将会抛出错误：

``` javascript
/*eslint no-func-assign: 2*/

function foo() {}
foo = bar;        /*error 'foo' is a function.*/

function foo() {
    foo = bar;    /*error 'foo' is a function.*/
}

```

而下面的代码将不会抛出错误：

``` javascript
/*eslint no-func-assign: 2*/

var foo = function () {}
foo = bar;

function foo(foo) { // `foo` is shadowed.
    foo = bar;
}

function foo() {
    var foo = bar;  // `foo` is shadowed.
}
```

2.16 **Declarations in Program or Function Body (no-inner-declarations)**

这条规则要求函数申明或者变量申明需要放在程序或者函数体的最上面。

有两个选项，默认值是`"function"` 表示只检测函数声明，另外一个选项是`"both"` ,表示不仅函数声明，而且变量申明也需要做检测。举个栗子：

下面的代码将被视为有问题的：

``` javascript
/*eslint no-inner-declarations: 2*/

if (test) {
    function doSomething() { }        /*error Move function declaration to program root.*/
}

function doSomethingElse() {
    if (test) {
        function doAnotherThing() { } /*error Move function declaration to function body root.*/
    }
}
```

添加`"all"` 选项时，下面的代码将被视为有问题的：

``` javascript
/*eslint no-inner-declarations: [2, "both"]*/

if (test) {
    var foo = 42;            /*error Move variable declaration to program root.*/
}

function doAnotherThing() {
    if (test) {
        var bar = 81;        /*error Move variable declaration to function body root.*/
    }
}

```

而下面的代码将被视为合法的:

``` javascript
/*eslint no-inner-declarations: 2*/
/*eslint-env es6*/

function doSomething() { }

function doSomethingElse() {
    function doAnotherThing() { }
}

if (test) {
    asyncCall(id, function (err, data) { });
}

var fn;
if (test) {
    fn = function fnExpression() { };
}

var bar = 42;

if (test) {
    let baz = 43;
}

function doAnotherThing() {
    var baz = 81;
}
```

2.18 **Disallow Invalid Regular Expressions (no-invalid-regexp)**

这条规则用来验证传递给`RegExp` 构造函数的字符串参数是否合法。

**Rule Detail**

下面的书写将被视为有问题的：

``` javascript
/*eslint no-invalid-regexp: 2*/

RegExp('[')      /*error Invalid regular expression: /[/: Unterminated character class*/

RegExp('.', 'z') /*error Invalid flags supplied to RegExp constructor 'z'*/

new RegExp('\\') /*error Invalid regular expression: /\/: \ at end of pattern*/
```

而下面的书写将被视为合法的：

``` javascript
/*eslint no-invalid-regexp: 2*/

RegExp('.')

new RegExp

this.RegExp('[')
```

ECMAScript 6增加了`"u"`(unicode)和`"y"` (sticky)flags。我们可以通过在`.eslintrc`文件中添加如下代码，来使得上面增添的两个标签在该条规则中合法。

``` javascript
"ecmaFeatures": {
  "regexYFlag": true,
  "regexUFlag": true
}
```

2.19 **No irregular whitespace (no-irregular-whitespace)**

顾名思义，禁止使用不合法或者不规则的空白符，因为这些空白符可能导致ECMAScript5解析出错，或者代码难以调试，因为这些空白符和`tabs`和`spaces` 难以区分开来。这些不合法的空白符可能会导致如下问题：

- Zero Width Space
  - Is NOT considered a separator for tokens and is often parsed as an `Unexpected token ILLEGAL`
  - Is NOT shown in modern browsers making code repository software expected to resolve the visualisation
- Line Separator
  - Is NOT a valid character within JSON which would cause parse errors

不合法或者不规则的空白符包括如下字符：

``` javascript
\u000B - Line Tabulation (\v) - <VT>
\u000C - Form Feed (\f) - <FF>
\u00A0 - No-Break Space - <NBSP>
\u0085 - Next Line
\u1680 - Ogham Space Mark
\u180E - Mongolian Vowel Separator - <MVS>
\ufeff - Zero Width No-Break Space - <BOM>
\u2000 - En Quad
\u2001 - Em Quad
\u2002 - En Space - <ENSP>
\u2003 - Em Space - <EMSP>
\u2004 - Tree-Per-Em
\u2005 - Four-Per-Em
\u2006 - Six-Per-Em
\u2007 - Figure Space
\u2008 - Punctuation Space - <PUNCSP>
\u2009 - Thin Space
\u200A - Hair Space
\u200B - Zero Width Space - <ZWSP>
\u2028 - Line Separator
\u2029 - Paragraph Separator
\u202F - Narrow No-Break Space
\u205f - Medium Mathematical Space
\u3000 - Ideographic Space
```

2.20 **Disallow negated left operand of `in` operator (no-negated-in-lhs)**

**Rule Details**

这条规则的目的就是为了消除一个潜在的错误，当一个开发者本意是要写如下代码时：

``` javascript
if(!(a in b)) {
    // do something
}
```

他们通常可能会写出：

``` javascript
if(!a in b) {
    // do something
}
```

这往往不能够得到我们想要的结果，因此这条规则就明确说明不能够在`in`操作符左边使用`!` .下面的代码将被视为错误的代码：

``` javascript
/*eslint no-negated-in-lhs: 2*/

if(!a in b) {       /*error The `in` expression's left operand is negated*/
    // do something
}
```

而下面的代码将被视为正确的

``` javascript
/*eslint no-negated-in-lhs: 2*/

if(!(a in b)) {
    // do something
}

if(('' + !a) in b) {
    // do something
}
```

2.21 **Disallow Global Object Function Calls (no-obj-calls)**

这条规则告诉我们，不要像函数一样调用Math和JSOM.举个栗子，下面的代码是非法的:

``` javascript
/*eslint no-obj-calls: 2*/

var x = Math(); /*error 'Math' is not a function.*/
var y = JSON(); /*error 'JSON' is not a function.*/

```

2.22 **Disallow Spaces in Regular Expressions (no-regex-spaces)**

顾名思义，就是在正则表达式中如果需要使用多个空格的时候，尽量使用量词，如{2}。而不是使用多个空格。

举个栗子：下面的例子将被视为不合法：

``` javascript
/*eslint no-regex-spaces: 2*/

var re = /foo   bar/; /*error Spaces are hard to count. Use {3}.*/
```

而下面的栗子将被视为合法的：

``` javascript
/*eslint no-regex-spaces: 2*/

var re = /foo {3}bar/;

var re = new RegExp("foo   bar");
```

2.23 **Disallow Sparse Arrays (no-sparse-arrays)**

所谓Space Array，就是在声明数组时使用多个逗号`,` 来申明空的数组，例如：

``` javascript
var iterm = [,,];
```

Space Array往往会带来一些疑惑，比如上面的iterm，其length为2，但是item[0]和term[1]并没有值，这样往往容易给开发者带来疑惑，因此不建议使用Space Array。

这条规则的目的就是清除通过多余的逗号申明的Space Arrays。

举个栗子，下面的代码将被视为不合法：

``` javascript
/*eslint no-sparse-arrays: 2*/

var items = [,];                 /*error Unexpected comma in middle of array.*/
var colors = [ "red",, "blue" ]; /*error Unexpected comma in middle of array.*/
```

而下面代码将被视为合法：

``` javascript
/*eslint no-sparse-arrays: 2*/

var items = [];
var items = new Array(23);

// trailing comma is okay
var colors = [ "red", "blue", ];
```

2.24 **Avoid unexpected multiline expressions (no-unexpected-multiline)**

在JavaScript中，得益于ASI（automatic semicolon insertion），我们可以在行末选择性加分号。可以通过参考 [semi](http://eslint.org/docs/rules/semi) 来对该特性有个详细的了解。

ASI规则相对比较容易理解：简单来说，正如Lsaac Schlueter描述，只要满足下面描述条件之一，ASI就不会发生，也就是说`\n` 字符不会结束一句代码。

* 代码语句包含一个没有封闭的圆括号、数组申明或者对象声明，或者其他不能够作为行末结束字符而出现在了行末。（比如以`.` `,`结束）
* 代码以`—` ,`++` 结束（这种情况，ASI也不会发生，因为`—`,`++`会对后面的数进行自减或自加）
* 在使用`for()`, `while()`, `do`, `if()`, 或者 `else`时，后面没有接`{`.
* 新的一行代码以`[`, `(`, `+`, `*`, `/`, `-`, `,`, `.`开头，或者其他位操作符，这些只能够在中缀表达式中找到的操作符开头时，ASI也不会发生。

**Rule Details**

这条规则的目的就是为了保证两行不相关的代码不会意外的被当做一行代码来解析了。

下面的代码将被视为错误：

``` javascript
/*eslint no-unexpected-multiline: 2*/

var foo = bar
(1 || 2).baz();               /*error Unexpected newline between function and ( of function call.*/

var hello = 'world'
[1, 2, 3].forEach(addNumber); /*error Unexpected newline between object and [ of property access.*/
```

而下面的代码是合法的：

``` javascript
/*eslint no-unexpected-multiline: 2*/

var foo = bar;
(1 || 2).baz();

var foo = bar
;(1 || 2).baz()

var hello = 'world';
[1, 2, 3].forEach(addNumber);

var hello = 'world'
void [1, 2, 3].forEach(addNumber);
```

2.25 **Disallow Unreachable Code (no-unreachable)**

顾名思义，这条rule就是禁止不会被使用到的代码，这条规则的目的就是检测不会使用到的代码。在代码块中，如果语句出现在了`return`, `throw`, `break`, 或者`continue` 后面，这些代码往往不会被使用到，这条规则就是为了检测代码块、switch语句中不被使用到的代码。

例如下面的代码将会报错：

``` javascript
/*eslint no-unreachable: 2*/

function foo() {
    return true;
    console.log("done");      /*error Found unexpected statement after a return.*/
}

function bar() {
    throw new Error("Oops!");
    console.log("done");      /*error Found unexpected statement after a throw.*/
}

while(value) {
    break;
    console.log("done");      /*error Found unexpected statement after a break.*/
}

throw new Error("Oops!");
console.log("done");          /*error Found unexpected statement after a throw.*/
```

由于JavaScript中的函数提升和变量提升，下代码将不会报错：

``` javascript
/*eslint no-unreachable: 2*/

function foo() {
    return bar();
    function bar() {
        return 1;
    }
}

function bar() {
    return x;
    var x;
}

switch (foo) {
    case 1:
        break;
        var x;
}
```

2.26 **Require isNaN() (use-isnan)**

在JavaScript中，NaN是一个特殊的值，其属于`Number`类型，其用来描述不是数字，NaN有个特性就是`NaN !== NaN` .

因此这条规则的目的就是要求不要和`NaN`做比较，而应该使用isNaN(),方法来进行判断。举个栗子：

下面的代码将被认为有问题的：

``` javascript
/*eslint use-isnan: 2*/

if (foo == NaN) { /*error Use the isNaN function to compare with NaN.*/
    // ...
}

if (foo != NaN) { /*error Use the isNaN function to compare with NaN.*/
    // ...
}
```

而下面的代码将被视为合法：

``` javascript
/*eslint use-isnan: 2*/

if (isNaN(foo)) {
    // ...
}

if (isNaN(NaN)) {
    // ...
}
```

2.27 **Validates JSDoc comments are syntactically correct (valid-jsdoc)**

JSDoc是一个JavaScript API 文档自动生成器，这条规则也就是用来检测JSDoc是否完整和合法。当出现以下情况时，这条规则将会警告：

1. There is a JSDoc syntax error
2. A `@param` or `@returns` is used without a type specified
3. A `@param` or `@returns` is used without a description
4. A comment for a function is missing `@returns`
5. A parameter has no associated `@param` in the JSDoc comment
6. `@param`s are out of order with named arguments

**由于这条规则暂不使用，在此不再详述，请参见[valid-jsdoc](http://eslint.org/docs/rules/valid-jsdoc)

2.28 **Ensures that the results of typeof are compared against a valid string (valid-typeof)**

在多数情况下，typeof操作符返回的结果会是   `"undefined"`,    `"object"`,     `"boolean"`,          `"number"`, `"string"`,  和  `"function"`之一。当typeof操作符返回的结果和不是上面六个字符串之一进行比较时，这通常是一个书写错误，这条规则就是为了保证typeof 操作符返回的结果必须和上面六个字符串作比较。

**Rule Details** 

举个栗子，下面代码将被视为非法的：

``` javascript
/*eslint valid-typeof: 2*/

typeof foo === "strnig"   /*error Invalid typeof comparison value*/
typeof foo == "undefimed" /*error Invalid typeof comparison value*/
typeof bar != "nunber"    /*error Invalid typeof comparison value*/
typeof bar !== "fucntion" /*error Invalid typeof comparison value*/
```

而下面的代码将被视为合法：

``` javascript
/*eslint valid-typeof: 2*/

typeof foo === "string"
typeof bar == "undefined"
typeof foo === baz
typeof bar === typeof qux
```

**上面28条规则，绝大多数是ESLint推荐的规则，除了[no-extra-parens](http://eslint.org/docs/rules/no-extra-parens)、[no-unexpected-multiline](http://eslint.org/docs/rules/no-unexpected-multiline) 、[valid-jsdoc](http://eslint.org/docs/rules/valid-jsdoc) 。因此个人建议在`.eslintrc` 文件中添加`extends: "eslint:recommended"` 。使用ESLint推荐的规则。因为这些规则能够真实地减少我们代码中的错误。

### 三、Best Practices

下面规则的设计就是为了减少代码中错误的产生，下面的规则要么提出一个更好的实践，要么避免错误的发生。

3.1 [accessor-pairs](http://eslint.org/docs/rules/accessor-pairs) - Enforces getter/setter pairs in objects

强制在声明对象时getter和setter需要成对出现。

3.2 [block-scoped-var](http://eslint.org/docs/rules/block-scoped-var) - treat `var` statements as if they were block scoped

根据上面的描述，也应该知道这条规则的意思，当在代码块中使用用var声明变量，并且在代码块外部使用时，该条规则就会抛出错误，这样就更像C语言的block scope。举个栗子，如下代码将会抛出错误：

``` javascript
/*eslint block-scoped-var: 2*/

function doSomething() {
    if (true) {
        var build = true;
    }

    console.log(build); /*error "build" used outside of binding context.*/
}
```

**当使用ECMAScript2015的语言特性后，这条规则也就没什么用处了，因为在ECMAScript2015中不推荐使用`var`声明变量，而应该使用`const` 和`let`,因为`const` 和`let` 只在其声明的代码块中有效，有效地避免了使用`var` 声明变量带来的各种弊端（自动在window对象上添加属性等）。

3.3 **Limit Cyclomatic Complexity (complexity)**

Cyclomatic Complexity主要是用来度量一个函数的复杂度，看其有几个分支。例如下面的函数有三个分支：

``` javascript
function a(x) {
    if (true) {
        return x; // 1st path
    } else if (false) {
        return x+1; // 2nd path
    } else {
        return 4; // 3rd path
    }
}
```

 而这条规则的目的就是用来控制函数的复杂度，也就是可以用过控制分支来控制其负责度。例如下面的代码将被视为不合法：

``` javascript
/*eslint complexity: [2, 2]*/

function a(x) {               /*error Function 'a' has a complexity of 3.*/
    if (true) {
        return x;
    } else if (false) {
        return x+1;
    } else {
        return 4; // 3rd path
    }
}
```

下面的代码将被视为合法：

``` javascript
/*eslint complexity: [2, 2]*/

function a(x) {
    if (true) {
        return x;
    } else {
        return 4;
    }
}
```





















