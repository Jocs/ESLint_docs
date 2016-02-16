### 四、Strict Mode

下面的规则和使用严格模式有关

4.1 Strict Mode （strict）

在脚本的顶部或者在函数体的顶部使用`"use strict"` 可以开启严格模式。在脚本顶部使用`"use strict"` 表示全局使用严格模式，包括脚本内的所有函数。如果只需在函数内部使用严格模式，可以只在函数体内部使用`"use strict"` .如下：

``` javascript
function foo() {
    "use strict";
    return;
}

var bar = function() {
    "use strict";
    return;
};
```

**Rule Details**

该条规则的目的就是有效的使用严格模式，消除任意滥用和无意省略严格模式指令。

该规则接受三个三个配置选项。

* `never` - don't use `"use strict"` at all


* `global` - require `"use strict"` in the global scope


* `function` - require `"use strict"` in function scopes only

**上面三个选项用意很明显，这儿就不解释了。

### 五、变量相关Rules

5.1[init-declarations](http://eslint.org/docs/rules/init-declarations) - enforce or disallow variable initializations at definition

在JavaScript中，变量可以在一开始声明是赋值，也可以在声明后在使用赋值语句赋值。例如，在下面的代码中，`foo` 在声明时就被赋值，而`bar` 是声明后才被赋值。

``` javascript
var foo = 1;
var bar;

if (foo) {
    bar = 1;
} else {
    bar = 2;
}
```

**Rule Details**

该条规则的目的就是统一变量在何时被赋值，要么在变量申明的时候，要么在声明后赋值。

该规则接受两个配置项。

* 当配置`"always"` 时，变量就需要在声明时就被赋值，当配置为`"never"` 时，就是在变量声明时不能够赋值，这条规则适用于`var`  `let` 和` const` .但是当设置为`never` 时候，对`const` 是无效的，因为const 声明变量时如果没有被赋值，将被报解析错误。（在FF中，报SyntaxError: missing = in const declaration）
* 使用`ignoreForLoopInit` 配置项可以更进一步控制该规则的行为，当该配置项设置为true时，及时设置了`never` 也可以在for循环中初始化变量值。

**`always` 是默认值。

举个栗子，当设置为`always` 时，下面代码报错。

``` javascript
/*eslint init-declarations: [2, "always"]*/
/*eslint-env es6*/

function foo() {
    var bar;     /*error Variable 'bar' should be initialized on declaration.*/
    let baz;     /*error Variable 'baz' should be initialized on declaration.*/
}

```

而下面代码将不会报错。

``` javascript
/*eslint init-declarations: [2, "always"]*/
/*eslint-env es6*/

function foo() {
    var bar = 1;
    let baz = 2;
    const qux = 3;
}
```

当设置`never` 时，下面代码将会报错。

``` javascript
/*eslint init-declarations: [2, "never"]*/
/*eslint-env es6*/

function foo() {
    var bar = 1;   /*error Variable 'bar' should not be initialized on declaration.*/
    let baz = 2;   /*error Variable 'baz' should not be initialized on declaration.*/

    for (var i = 0; i < 1; i++) {}  /*error Variable 'i' should not be initialized on declaration.*/
}
```

而下面代码将不会设置为报错，及时使用了`const` 赋值。

``` javascript
/*eslint init-declarations: [2, "never"]*/
/*eslint-env es6*/

function foo() {
    var bar;
    let baz;
    const buzz = 1;
}
```

当配置了`ignoreForLoopInit` 为true时，下面代码不会报错。

``` javascript
/*eslint init-declarations: [2, "never", { "ignoreForLoopInit": true }]*/
for (var i = 0; i < 1; i++) {}
```



5.2[no-catch-shadow](http://eslint.org/docs/rules/no-catch-shadow) - disallow the catch clause parameter name being the same as a variable in the outer scope

该规则和IE8及以前浏览器有关，详见官网。



5.3[no-delete-var](http://eslint.org/docs/rules/no-delete-var) - disallow deletion of variables (recommended)

该规则的目的就是防止用`delete` 删除`var` 声明的变量。



5.4[no-label-var](http://eslint.org/docs/rules/no-label-var) - disallow labels that share a name with a variable

该条规则的目的就是防止label和声明的变量重名。



5.5[no-shadow-restricted-names](http://eslint.org/docs/rules/no-shadow-restricted-names) - disallow shadowing of names such as `arguments`

顾名思义，该条规则就是希望我们声明变量时不要覆盖JavaScript中的一些保留关键字，如NaN、Infinity、undefined等。（基本上也没人这样做的，所以不解释了）



5.6[no-shadow](http://eslint.org/docs/rules/no-shadow) - disallow declaration of variables already declared in the outer scope

不解释了，因为如果使用（no-var）后，const和let也不能够重复声明同一个变量。



5.7[no-undef-init](http://eslint.org/docs/rules/no-undef-init) - disallow use of undefined when initializing variables

该条规则禁止初始变量为`undefined` .



5.8[no-undef](http://eslint.org/docs/rules/no-undef) - disallow use of undeclared variables unless mentioned in a `/*global */` block (recommended)

禁止使用未被定义的变量，除非已在配置文件global中进行了说明。



5.9[no-undefined](http://eslint.org/docs/rules/no-undefined) - disallow use of `undefined` variable

禁止把`undefined`作为变量名，

**好像也不会有人这样做吧。



5.10[no-unused-vars](http://eslint.org/docs/rules/no-unused-vars) - disallow declaration of variables that are not used in the code (recommended)

该条规则不允许定义了的变量但是在后面的代码中没有被使用到。



5.11[no-use-before-define](http://eslint.org/docs/rules/no-use-before-define) - disallow use of variables before they are defined

顾名思义，所有的变量都应该先定义，后使用。

### 六、Node.js 和 CommonJs

以后补充

### 七、和样式相关的规则

7.1[array-bracket-spacing](http://eslint.org/docs/rules/array-bracket-spacing) - enforce spacing inside array brackets (fixable)

该条规则主要用于定义数组字面量定义数组时，前后是否加空格，接受两个可选配置，`always` 和`never` .如果设置为`always` 那么就应该在在写数组是前后都留空格，举个栗子：

``` javascript
/*eslint array-bracket-spacing: [2, "always"]*/
/*eslint-env es6*/

var arr = [];
var arr = [ 'foo', 'bar', 'baz' ];
var arr = [ [ 'foo' ], 'bar', 'baz' ];
var arr = [
  'foo',
  'bar',
  'baz'
];

var [ x, y ] = z;
var [ x,y ] = z;
var [ x, ...y ] = z;
var [ ,,x, ] = z;
```

如果设置成`never`时，就应该写成如下形式：

``` javascript
/*eslint array-bracket-spacing: [2, "never"]*/
/*eslint-env es6*/

var arr = [];
var arr = ['foo', 'bar', 'baz'];
var arr = [['foo'], 'bar', 'baz'];
var arr = [
  'foo',
  'bar',
  'baz'
];
var arr = [
  'foo',
  'bar'];

var [x, y] = z;
var [x,y] = z;
var [x, ...y] = z;
var [,,x,] = z;

```



7.2[block-spacing](http://eslint.org/docs/rules/block-spacing) - disallow or enforce spaces inside of single line blocks (fixable)

该条规则是指在单行代码块中，代码块前后是否需要留空白。该规则接受两个可选配置，`always`和`never` .always是默认值。当设置为`always`时，下面代码是正确的。

``` javascript
/*eslint block-spacing: 2*/

function foo() { return true; }
if (foo) { bar = 0; }
```

当设置为`never`时，下面代码是正确的。

``` javascript
/*eslint block-spacing: [2, "never"]*/

function foo() {return true;}
if (foo) {bar = 0;}
```



7.3[brace-style](http://eslint.org/docs/rules/brace-style) - enforce one true brace style

大括号的样式和缩进样式徐徐相关，该规则主要是用来描述大括号在控制语句中的位置，而且大括号在编程中经常出现。

* `one true style` :是JavaScript中最常用的一种大括号语法样式，在这种样式中，大括号应该放在与之相对应的语句或者声明语句中（不要把大括号写在单独一行），例如：

``` javascript
if (foo) {
  bar();
} else {
  baz();
}
```

* `Stroustrup`: 在这种写法中，if、else、try、catch都应该单独启一行。例如：

``` javascript
if (foo) {
  bar();
}
else {
  baz();
}
```

* `Allman` 另外一种大括号样式Allmän。在这种样式中，大括号应该单独放在一行，并且不添加任何缩进。例如：

``` javascript
if (foo)
{
  bar();
}
else
{
  baz();
}
```

当然没有那种样式比其他样式更好，很多开发者都认为统一样式只是为了代码的可维护性。

**Rule Details**

该条规则的目的就是为了强制代码使用统一的大括号样式。

**Options**

该规则接受两个可选配置项：

* 第一个配置项是一个字符串，可以是`1tbs`  `stroustrup`和`aleman` 中任意一个，默认使用`1tbs` .
* 第二个配置项是一个对象，目前，该配置对象唯一配置是`allowSingleLine`，该配置参数正如其英文单词的意思，大括号的开始和结束是否可以写在同一行中。例如可以像下面这样配置：

``` javascript
"brace-style": [2, "stroustrup", { "allowSingleLine": true }]
```

当配置`1tbs`时，下面的代码将会报错：

``` javascript
/*eslint brace-style: 2*/
function foo()       /*error Opening curly brace does not appear on the same line as controlling statement.*/
{
  return true;
}

if (foo)             /*error Opening curly brace does not appear on the same line as controlling statement.*/
{
  bar();
}

try                  /*error Opening curly brace does not appear on the same line as controlling statement.*/
{
  somethingRisky();
} catch(e)           /*error Opening curly brace does not appear on the same line as controlling statement.*/
{
  handleError();
}

if (foo) {
  bar();
}
else {              /*error Closing curly brace does not appear on the same line as the subsequent block.*/
  baz();
}
```

而下面是正确的`1tbs` 样式：

``` javascript
/*eslint brace-style: 2*/

function foo() {
  return true;
}

if (foo) {
  bar();
}

if (foo) {
  bar();
} else {
  baz();
}

try {
  somethingRisky();
} catch(e) {
  handleError();
}

// when there are no braces, there are no problems
if (foo) bar();
else if (baz) boom();
```

当单行模式可用时，下面代码是有效的。

``` javascript
/*eslint brace-style: [2, "1tbs", { "allowSingleLine": true }]*/

function nop() { return; }

if (foo) { bar(); }

if (foo) { bar(); } else { baz(); }

try { somethingRisky(); } catch(e) { handleError(); }
```

第二个大括号样式是`stroustrup`。当配置该样式时，下面代码将被报错：

``` javascript
/*eslint brace-style: [2, "stroustrup"]*/

function foo()        /*error Opening curly brace does not appear on the same line as controlling statement.*/
{
  return true;
}

if (foo)              /*error Opening curly brace does not appear on the same line as controlling statement.*/
{
  bar();
}

try                   /*error Opening curly brace does not appear on the same line as controlling statement.*/
{
  somethingRisky();
} catch(e)            /*error Opening curly brace does not appear on the same line as controlling statement.*/ /*error Closing curly brace appears on the same line as the subsequent block.*/
{
  handleError();
}

if (foo) {
  bar();
} else {              /*error Closing curly brace appears on the same line as the subsequent block.*/
  baz();
}
```

而下面代码在stroustrup样式不会报错。

``` javascript
/*eslint brace-style: [2, "stroustrup"]*/

function foo() {
  return true;
}

if (foo) {
  bar();
}

if (foo) {
  bar();
}
else {
  baz();
}

try {
  somethingRisky();
}
catch(e) {
  handleError();
}

// when there are no braces, there are no problems
if (foo) bar();
else if (baz) boom();
```



7.4[camelcase](http://eslint.org/docs/rules/camelcase) - require camel case names

在命名变量时，推荐命名样式分成了两个阵营，一个是驼峰命名，一个是蛇形命名。这条规则强制使用驼峰命名。

该规则会搜索代码中所有的下划线，它会忽略变量名开始和结尾的下划线而只检测变量中间的下划线。如果ESLint认为一个变量是常量（所有字母大写），那么在变量名字母之间添加下划线也是可以而不会报错的。该规则只检测生命和定义时的变量而不检测函数调用时的函数名。

该规则接受一个配置选项，如下：

``` javascript
{
    "rules": {
        "camelcase": [2, {"properties": "always"}]
    }
}
```

`Properties` can have the following values:

1. `always` is the default and checks all property names
2. `never` does not check property names at all

举个栗子，下面的代码将被视为错误：

``` javascript
/*eslint camelcase: 2*/
var my_favorite_color = "#112C85"; /*error Identifier 'my_favorite_color' is not in camel case.*/

function do_something() {          /*error Identifier 'do_something' is not in camel case.*/
    // ...
}

obj.do_something = function() {    /*error Identifier 'do_something' is not in camel case.*/
    // ...
};

var obj = {
    my_pref: 1                     /*error Identifier 'my_pref' is not in camel case.*/
};
```

而下面代码是正确的：

``` javascript
/*eslint camelcase: 2*/
var myFavoriteColor   = "#112C85";
var _myFavoriteColor  = "#112C85";
var myFavoriteColor_  = "#112C85";
var MY_FAVORITE_COLOR = "#112C85";
var foo = bar.baz_boom;
var foo = { qux: bar.baz_boom };

obj.do_something();
```

``` javascript
/*eslint camelcase: [2, {properties: "never"}]*/

var obj = {
    my_pref: 1
};
```



7.5[comma-spacing](http://eslint.org/docs/rules/comma-spacing) - enforce spacing before and after comma (fixable)

该条规则规定了逗号前后的空白。默认配置如下：

``` javascript
"comma-spacing": [2, {"before": false, "after": true}]
```

上面的配置规定逗号前面没有空白，而逗号后面需要留空白。

举个正确栗子：

``` javascript
/*eslint comma-spacing: [2, {"before": false, "after": true}]*/

var foo = 1, bar = 2
    , baz = 3;
var arr = [1, 2];
var obj = {"foo": "bar", "baz": "qur"};
foo(a, b);
new Foo(a, b);
function foo(a, b){}
a, b
```



7.6[comma-style](http://eslint.org/docs/rules/comma-style) - enforce one true comma style

该规则规定了逗号放的位置，默认配置如下：

``` javascript
"comma-style": [2, "last"]
```

上面默认配置逗号应该放在行末，如果设置为first，逗号就应该放在行首。推荐使用默认配置就好。



7.7[computed-property-spacing](http://eslint.org/docs/rules/computed-property-spacing) - require or disallow padding inside computed properties (fixable)

该条规则用来规定是否在对象的动态属性（computed properties： ES6引入）中添加空白。默认配置如下：

``` javascript
"computed-property-spacing": [2, "never"]
```

默认配置下不添加空白，正确事例如下：

``` javascript
/*eslint computed-property-spacing: [2, "never"]*/
/*eslint-env es6*/

obj[foo]
obj['foo']
var x = {[b]: a}
obj[foo[bar]]
```



7.8[consistent-this](http://eslint.org/docs/rules/consistent-this) - enforce consistent naming when capturing the current execution context

经常情况下，我们需要保存执行上下文（this）到闭包内使用，一个经典的使用场景就是jQuery的回调函数。

``` javascript
var self = this;
jQuery('li').click(function (event) {
    // here, "this" is the HTMLElement where the click event occurred
    self.setFoo(42);
});
```

现目前有许多常用的this别名，比如`self` `that` `me` .该条规则就是统一`this` 的别名（this赋值的变量名）保证整个应用程序代码的统一。

**Rule Details**

该规则制定了一个变量作为`this`对象的别名，然后检测下面两种情况：

* 如果一个变量被指定为`this` 对象的别名，那么这个变量就不能够用来赋其他值，只能够用来保存`this`对象。
* 如果`this` 对象明确被赋值给了一个变量，那么这个变量应该是配置中指定的那个变量名。

该规则接受一个字符串作为可选配置项，用来制定`this`对象的别名。

可以如下配置：

``` javascript
"consistent-this": [2, "self"]
```

当如上配置后，下面代码将被视为错误：

``` javascript
/*eslint consistent-this: [2, "self"]*/

var self = 42;   /*error Designated alias 'self' is not assigned to 'this'.*/

var that = this; /*error Unexpected alias 'that' for 'this'.*/

self = 42;       /*error Designated alias 'self' is not assigned to 'this'.*/

that = this;     /*error Unexpected alias 'that' for 'this'.*/
```

而下面代码将被视为正确：

``` javascript
/*eslint consistent-this: [2, "self"]*/

var self = this;

var that = 42;

var that;

self = this;

foo.bar = this;
```

在声明别名是可以不用马上用于保存`this`对象，但是声明和赋值必须在相同的作用域下，下面的代码将被视为正确：

``` javascript
/*eslint consistent-this: [2, "self"]*/

var self;
self = this;

var foo, self;
foo = 42;
self = this;
```

而下面代码将被视为不合法：

``` javascript
/*eslint consistent-this: [2, "self"]*/

var self;        /*error Designated alias 'self' is not assigned to 'this'.*/
function f() {
    self = this;
}
```



7.9[eol-last](http://eslint.org/docs/rules/eol-last) - enforce newline at the end of file, with no multiple empty lines (fixable)

该条规则规定文件最后一行应该留一空白行。详见官网。



7.10[func-names](http://eslint.org/docs/rules/func-names) - require function expressions to have a name

该规则要求给函数表达式都加上一个名字，便于debug。详见官网。

举个栗子下面的代码将被视为错误的：

``` javascript
/* eslint func-names: 2*/

Foo.prototype.bar = function() {}; /*error Missing function expression name.*/

(function() {                      /*error Missing function expression name.*/
    // ...
}())

```



7.11[func-style](http://eslint.org/docs/rules/func-style) - enforce use of function declarations or expressions

在JavaScript中有两种方式定义函数，函数申明和函数表达式。函数申明就是把`function` 关键词写在最前面，后面跟一个函数名。例如：

``` javascript
function doSomething() {
    // ...
}
```

而函数表达式是通过`var` 等申明变量的关键字开头，然后紧跟函数名，在后面是function本身。如下：

``` javascript
var doSomething = function() {
    // ...
};
```

函数申明和函数表达式的区别就在于函数申明有提升，也就是说，我们可以在函数申明代码前调用函数。如下：

``` javascript
doSomething();

function doSomething() {
    // ...
}
```

而函数表达式没有函数提升，在使用函数表达式定义函数前调用函数就会报错。举个栗子：

``` javascript
doSomething();  // error!

var doSomething = function() {
    // ...
};
```

**Rule Detail**

该条规则的目的就是为了统一定义函数是所采用的方式，要么是函数申明要么是函数表达式。可以通过配置来指定所选择的函数定义方式。

下面的代码将被视为错误：

``` javascript
/*eslint func-style: [2, "declaration"]*/

var foo = function() {  /*error Expected a function declaration.*/
    // ...
};
```

``` javascript
/*eslint func-style: [2, "expression"]*/

function foo() {  /*error Expected a function expression.*/
    // ...
}
```



7.12[id-length](http://eslint.org/docs/rules/id-length) - this option enforces minimum and maximum identifier lengths (variable names, property names etc.)

太短或者太长的标识符都可能带来代码的难以理解并且降低了代码的可维护性，因此这条规则就是用来规定标识符的长短的，默认配置标识符最少两个字符。举个栗子下面代码将被视为错误：

``` javascript
/*eslint id-length: 2*/     // default is minimum 2-chars ({ min: 2})
/*eslint-env es6*/

var x = 5;                  /*error Identifier name 'x' is too short. (< 2)*/

obj.e = document.body;      /*error Identifier name 'e' is too short. (< 2)*/

var foo = function (e) { }; /*error Identifier name 'e' is too short. (< 2)*/

try {
    dangerousStuff();
} catch (e) {               /*error Identifier name 'e' is too short. (< 2)*/
    // ignore as many do
}

var myObj = { a: 1 };       /*error Identifier name 'a' is too short. (< 2)*/

(a) => { a * a };           /*error Identifier name 'a' is too short. (< 2)*/

function foo(x = 0) { }     /*error Identifier name 'x' is too short. (< 2)*/

class x { }                 /*error Identifier name 'x' is too short. (< 2)*/

class Foo { x() {} }        /*error Identifier name 'x' is too short. (< 2)*/

function foo(...x) { }      /*error Identifier name 'x' is too short. (< 2)*/

var { x} = {};              /*error Identifier name 'x' is too short. (< 2)*/

var { x: a} = {};           /*error Identifier name 'x' is too short. (< 2)*/

var { a: [x]} = {};         /*error Identifier name 'a' is too short. (< 2)*/

({ a: obj.x.y.z }) = {};    /*error Identifier name 'a' is too short. (< 2)*/ /*error Identifier name 'z' is too short. (< 2)*/

({ prop: obj.x }) = {};     /*error Identifier name 'x' is too short. (< 2)*/
```

当标识符通过引号包围时或者是ES6中的动态属性名，那么将会无视该条规则。下面的代码将被视为正确的。

``` javascript
/*eslint id-length: 2*/     // default is minimum 2-chars ({ min: 2})
/*eslint-env es6*/

var num = 5;

function _f() { return 42; }

function _func() { return 42; }

obj.el = document.body;

var foo = function (evt) { /* do stuff */ };

try {
    dangerousStuff();
} catch (error) {
    // ignore as many do
}

var myObj = { apple: 1 };

(num) => { num * num };

function foo(num = 0) { }

class MyClass { }

class Foo { method() {} }

function foo(...args) { }

var { prop } = {};

var { prop: a } = {};

var { prop: [x] } = {};

({ prop: obj.x.y.something }) = {};

({ prop: obj.longName }) = {};

var data = { "x": 1 };  // excused because of quotes

data["y"] = 3;  // excused because of calculated property access
```



7.13[id-match](http://eslint.org/docs/rules/id-match) - require identifiers to match the provided regular expression

> "There are only two hard things in Computer Science: cache invalidation and naming things." — Phil Karlton

在写作代码的过程中，统一命名方式往往是被忽略的一块，如果命名好的话，可以大大节省团队时间，甚至可以避免代码误导别人。这条规则可以让你精确的定义变量名和函数名应该怎么写，不仅限于驼峰命名、蛇形命名、PascalCase或者oHungarianNotation。Id-match可以满足前面的一切，甚至可以自定义想要的匹配模式。

这条规则将把函数名和变量名和配置中的一个正则表达式匹配，让你在函数、变量命名上给了你最大的灵活性，但是该规则不对函数调用无效。例如可以如下配置：

``` javascript
{
    "rules": {
        "id-match": [2, "^[a-z]+([A-Z][a-z]+)*$", {"properties": false}]
    }
}
```

详尽栗子参见官网。



7.14[indent](http://eslint.org/docs/rules/indent) - specify tab or space width for your code (fixable)

**好吧，这又是个引发讨论的规则了**

这条规则就是为了统一我们的代码究竟该采用什么样的方式来进行缩进。现目前有不同的代码缩进阵营：

- Two spaces, not longer and no tabs: Google, npm, Node.js, Idiomatic, Felix
- Tabs: jQuery
- Four spaces: Crockford

**Rule Detail**

这条规则的目的就是为了统一代码缩进，默认值是`4 spaces`.

该规则接受两个可选配置：

* 第一个配置是缩进的方式，可以是一个正整数或者`"tab"`.
* 第二个配置项是一个对象，用来可选的缩进场景。

​	*`SwitchCase` - Level of switch cases indent, 0 by default.

​	`VariableDeclarator` - Level of variable declaration indent, 1 by default. Can take an object to define separate rules for `var`, `let` and`const` declarations.

举个栗子，下面代码将被视为正确的：

``` javascript
/*eslint indent: [2, 2]*/

if (a) {
  b=c;
  function foo(d) {
    e=f;
  }
}
```

``` javascript
/*indent: [2, "tab"]*/

if (a) {
/*tab*/b=c;
/*tab*/function foo(d) {
/*tab*//*tab*/e=f;
/*tab*/}
}
```

``` javascript
/*eslint indent: [2, 2, {"VariableDeclarator": { "var": 2, "let": 2, "const": 3}}]*/
/*eslint-env es6*/

var a,
    b,
    c;
let a,
    b,
    c;
const a = 1,
      b = 2,
      c = 3;
```

``` javascript
/*eslint indent: [2, 4, {"SwitchCase": 1}]*/

switch(a){
    case "a":
        break;
    case "b":
        break;
}
```



7.15[jsx-quotes](http://eslint.org/docs/rules/jsx-quotes) - specify whether double or single quotes should be used in JSX attributes

这条规则规定了在JSX语法中我们是使用单引号还是双引号。



7.16[key-spacing](http://eslint.org/docs/rules/key-spacing) - enforce spacing between keys and values in object literal properties

该条规则就是为了规定在对象中，冒号前后是否应该留空白，以及空白应该留多少个空格。

可选的配置如下：

``` javascript
/*eslint key-spacing: [2, {"beforeColon": true, "afterColon": false, "mode": "minimum"}]*/
```

上面的配置说明冒号前面有空格，后面不留空白，空白最少一个。



7.17[linebreak-style](http://eslint.org/docs/rules/linebreak-style) - disallow mixed 'LF' and 'CRLF' as linebreaks

该规则用来规定断行采用的方式，详见官网。



7.18[lines-around-comment](http://eslint.org/docs/rules/lines-around-comment) - enforce empty lines around comments

该条规则用于规定注释和代码块之间的空行，可选的配置像如下：

1. `beforeBlockComment` (enabled by default)
2. `afterBlockComment`
3. `beforeLineComment`
4. `afterLineComment`
5. `allowBlockStart`
6. `allowBlockEnd`
7. `allowObjectStart`
8. `allowObjectEnd`
9. `allowArrayStart`
10. `allowArrayEnd`



7.19[max-depth](http://eslint.org/docs/rules/max-depth) - specify the maximum depth that blocks can be nested

这条规则用于规定代码最多可以嵌套多少层。默认的配置如下：

``` 
"max-depth": [2, 4]
```

下面的代码将会报错。

``` javascript
/*eslint max-depth: [2, 2]*/

function foo() {
  for (;;) {
    if (true) {
      if (true) { /*error Blocks are nested too deeply (3).*/

      }
    }
  }
}
```



7.20[max-len](http://eslint.org/docs/rules/max-len) - specify the maximum length of a line in your program

顾名思义，该规则制定了单行最大长度。



7.21[max-nested-callbacks](http://eslint.org/docs/rules/max-nested-callbacks) - specify the maximum depth callbacks can be nested

顾名思义，该规则规定了回调的最大嵌套层数。

比如可以如下配置：

``` javascript
"max-nested-callbacks": [2, 3]
```



7.22[max-params](http://eslint.org/docs/rules/max-params) - limits the number of parameters that can be used in the function declaration.

顾名思义，该条规则规定了函数参数的最大个数。



7.23[max-statements](http://eslint.org/docs/rules/max-statements) - specify the maximum number of statement allowed in a function

顾名思义，该规则规定了函数中代码不能够超过多少行，便于代码的阅读和可维护性。



7.24[new-cap](http://eslint.org/docs/rules/new-cap) - require a capital letter for constructors

构造函数首字母需要大写。



7.25[new-parens](http://eslint.org/docs/rules/new-parens) - disallow the omission of parentheses when invoking a constructor with no arguments

在调用构造函数是必须交圆括号。



7.26[newline-after-var](http://eslint.org/docs/rules/newline-after-var) - require or disallow an empty newline after variable declarations

该规则规定了在变量申明后，是否需要添加一空行。



7.27[no-array-constructor](http://eslint.org/docs/rules/no-array-constructor) - disallow use of the `Array` constructor

该规则规定了禁止使用`Array`  构造函数。



7.28[no-bitwise](http://eslint.org/docs/rules/no-bitwise) - disallow use of bitwise operators

该规则禁止使用位操作符。



7.29[no-continue](http://eslint.org/docs/rules/no-continue) - disallow use of the `continue` statement

禁止使用`continue` 语句



7.30[no-inline-comments](http://eslint.org/docs/rules/no-inline-comments) - disallow comments inline after code

禁止在代码行内添加注释。举个栗子，下面代码将报错。

``` javascript
/*eslint no-inline-comments: 2*/

var a = 1; // declaring a to 1                /*error Unexpected comment inline with code.*/

function getRandomNumber(){
    return 4; // chosen by fair dice roll.    /*error Unexpected comment inline with code.*/
              // guaranteed to be random.
}

/* A block comment before code */ var b = 2;  /*error Unexpected comment inline with code.*/

var c = 3; /* A block comment after code */   /*error Unexpected comment inline with code.*/
```



7.31[no-lonely-if](http://eslint.org/docs/rules/no-lonely-if) - disallow `if` as the only statement in an `else` block

该规则进制在`if-else` 控制语句中，else代码块中仅包含一个if语句。也就是说，下面的写法是非法的：

``` javascript
/*eslint no-lonely-if: 2*/

if (condition) {
    // ...
} else {
    if (anotherCondition) { /*error Unexpected if as the only statement in an else block.*/
        // ...
    }
}

if (condition) {
    // ...
} else {
    if (anotherCondition) { /*error Unexpected if as the only statement in an else block.*/
        // ...
    } else {
        // ...
    }
}
```



7.32[no-mixed-spaces-and-tabs](http://eslint.org/docs/rules/no-mixed-spaces-and-tabs) - disallow mixed spaces and tabs for indentation (recommended)

顾名思义，不要把空格和`tab`键混用来进行缩进。



7.33[no-multiple-empty-lines](http://eslint.org/docs/rules/no-multiple-empty-lines) - disallow multiple empty lines

顾名思义，就是不要留超过规定数目的空白行。举个栗子，下面代码非法：

``` javascript
/*eslint no-multiple-empty-lines: [2, {max: 2}]*/

var foo = 5;


                  /*error Multiple blank lines not allowed.*/
var bar = 3;
```



7.34[no-negated-condition](http://eslint.org/docs/rules/no-negated-condition) - disallow negated conditions

在`if`语句中使用了否定表达式，同时`else`语句又不为空，那么这样的if-else语句将被视为不合法的，为什么不将其反过来，这样代码更容易理解，该规则同样使用三元操作符。

例

``` javascript
if (!a) {
    doSomething();
}
else {
    doSomethingElse();
}
```

最好写成：

``` javascript
if (a) {
    doSomethingElse();
}
else {
    doSomething();
}

```

举个栗子，下面的代码将被视为不合法：

``` javascript
/*eslint no-negated-condition: 2*/

if (!a) {               /*error Unexpected negated condition.*/
    doSomething();
} else {
    doSomethingElse();
}

if (a != b) {           /*error Unexpected negated condition.*/
    doSomething();
} else {
    doSomethingElse();
}

if (a !== b) {          /*error Unexpected negated condition.*/
    doSomething();
} else {
    doSomethingElse();
}


!a ? b : c              /*error Unexpected negated condition.*/
```





7.35[no-nested-ternary](http://eslint.org/docs/rules/no-nested-ternary) - disallow nested ternary expressions

三元操作符禁止嵌套。



7.36[no-new-object](http://eslint.org/docs/rules/no-new-object) - disallow the use of the `Object` constructor

禁止使用Object构造函数来生产对象。



7.37[no-plusplus](http://eslint.org/docs/rules/no-plusplus) - disallow use of unary operators, `++` and `--`

禁止使用`++` 和`-—`.

7.38[no-restricted-syntax](http://eslint.org/docs/rules/no-restricted-syntax) - disallow use of certain syntax in code

禁止使用某些特定的JavaScript语法，例如`FunctionDeclaration` 和 `WithStatement` .

可以如下配置：

``` javascript
{
    "rules": {
        "no-restricted-syntax": [2, "FunctionExpression", "WithStatement"]
    }
}
```



7.39[no-spaced-func](http://eslint.org/docs/rules/no-spaced-func) - disallow space between function identifier and application (fixable)

函数调用时，函数名和括号之间不能有空格。



7.40[no-ternary](http://eslint.org/docs/rules/no-ternary) - disallow the use of ternary operators

禁止使用三元操作符。下面代码非法：

``` javascript
/*eslint no-ternary: 2*/

var foo = isBar ? baz : qux; /*error Ternary operator used.*/

foo ? bar() : baz();         /*error Ternary operator used.*/

function quux() {
  return foo ? bar : baz;    /*error Ternary operator used.*/
}
```



7.41[no-trailing-spaces](http://eslint.org/docs/rules/no-trailing-spaces) - disallow trailing whitespace at the end of lines (fixable)

该规则禁止行每行末尾加空格。



7.42[no-underscore-dangle](http://eslint.org/docs/rules/no-underscore-dangle) - disallow dangling underscores in identifiers

禁止在标识符前后使用下划线。

举个栗子：

``` javascript
/*eslint no-underscore-dangle: 2*/

var foo_;           /*error Unexpected dangling "_" in "foo_".*/
var __proto__ = {}; /*error Unexpected dangling "_" in "__proto__".*/
foo._bar();         /*error Unexpected dangling "_" in "_bar".*/

```



7.43[no-unneeded-ternary](http://eslint.org/docs/rules/no-unneeded-ternary) - disallow the use of ternary operators when a simpler alternative exists

禁止使用没有必要的三元操作符。因为有些三元操作符可以直接使用其他语句替换，例如：

``` javascript
// Bad
var foo = bar ? bar : 1;

// Good
var foo = bar || 1;
```



7.44[object-curly-spacing](http://eslint.org/docs/rules/object-curly-spacing) - require or disallow padding inside curly braces (fixable)

该规则规定对象字面量中大括号内部是否加空格。该规则也是为了统一代码规范而定，有两个可选配置项，`always`和`never` .

当设置为never时，下面代码正确。

``` javascript
/*eslint object-curly-spacing: [2, "never"]*/

var obj = {'foo': 'bar'};
var obj = {'foo': {'bar': 'baz'}, 'qux': 'quxx'};
var obj = {
  'foo': 'bar'
};
var obj = {'foo': 'bar'
};
var obj = {
  'foo':'bar'};
var obj = {};
var {x} = y;
import {foo} from 'bar';
```

当设置为`always`时，下面代码正确。

``` javascript
/*eslint object-curly-spacing: [2, "always"]*/

var obj = {};
var obj = { 'foo': 'bar' };
var obj = { 'foo': { 'bar': 'baz' }, 'qux': 'quxx' };
var obj = {
  'foo': 'bar'
};
var { x } = y;
import { foo } from 'bar';
```

该条规则也适用于ES6中的结构赋值和模块import 和 export。

7.45[one-var](http://eslint.org/docs/rules/one-var) - require or disallow one variable declaration per function

该规则规定了在一个作用域中是否只使用一次`var` .该规则同样适用于`let` 和`const` 可以针对不同申明变量的关键词单独配置。举个栗子，下面代码正确：

``` javascript
/*eslint one-var: [2, { var: "always", let: "always" }]*/
/*eslint-env es6*/

function foo() {
    var a, b;
    const foo = true;
    const bar = true;
    let c, d;
}
```



7.46[operator-assignment](http://eslint.org/docs/rules/operator-assignment) - require assignment operator shorthand where possible or prohibit it entirely

该条规则主要规定了使用赋值操作符的简写形式，举个栗子，需要使用ShortHand代替Separate

``` javascript
Shorthand | Separate
-----------|------------
 x += y    | x = x + y
 x -= y    | x = x - y
 x *= y    | x = x * y
 x /= y    | x = x / y
 x %= y    | x = x % y
 x <<= y   | x = x << y
 x >>= y   | x = x >> y
 x >>>= y  | x = x >>> y
 x &= y    | x = x & y
 x ^= y    | x = x ^ y
 x |= y    | x = x | y

```



7.47[operator-linebreak](http://eslint.org/docs/rules/operator-linebreak) - enforce operators to be placed before or after line breaks

在进行断行时，操作符应该放在行首还是行尾。并且还可以对某些操作符进行重写。例如

``` javascript
"operator-linebreak": [2, "before", { "overrides": { "?": "after" } }]
```



7.48[padded-blocks](http://eslint.org/docs/rules/padded-blocks) - enforce padding within blocks

该规则规定了在代码块中，代码块的开始和结尾是否应该留一个空行。接受两个可选配置`always` 和`never`.



7.49[quote-props](http://eslint.org/docs/rules/quote-props) - require quotes around object literal property names

顾名思义，函数属性名是否应该加引号。该规则接受四种行为。

 `"always"` (default), `"as-needed"`, `"consistent"` and `"consistent-as-needed"`.

我们可以如下配置：

``` javascript
{
    "quote-props": [2, "as-needed"]
}
```

具体配置项的含义如有疑问，参见官网。http://eslint.org/docs/rules/quote-props



7.50[quotes](http://eslint.org/docs/rules/quotes) - specify whether backticks, double or single quotes should be used (fixable)

在JavaScript中有三种方式定义字符串，双引号、单引号、反义符（ECMAScript2015）例如：

``` javascript
/*eslint-env es6*/

var double = "double";
var single = 'single';
var backtick = `backtick`;    // ES6 only

```

而这条规则的目的就是统一字符串定义的方式。

该规则接受两个配置选项：

* 第一个选项是一个字符串，可以配置`double` `single` `backpack` ,分别代表双引号、单引号、反义符，默认配置时双引号。
* 第二个配置项是`avoid-escape`.当我我们使用了avoid-escape时，下面代码是正确的：

``` javascript
/*eslint quotes: [2, "double", "avoid-escape"]*/

var single = 'a string containing "double" quotes';
```

因为，虽然配置了`double` ,但是字符串内也有双引号，因此字符串包围的变成了单引号。



7.51[require-jsdoc](http://eslint.org/docs/rules/require-jsdoc) - Require JSDoc comment

顾名思义，注释格式要求JSDoc格式。（在SubLime中可以安装一个JSDoc包来自动生成JSDoc格式注释）



7.52[semi-spacing](http://eslint.org/docs/rules/semi-spacing) - enforce spacing before and after semicolons

顾名思义，该规则用来规定分号前后是否应该加空格。

例如可以如下配置：(默认配置)

``` javascript
 "semi-spacing": [2, {"before": false, "after": true}]
```



7.53[semi](http://eslint.org/docs/rules/semi) - require or disallow use of semicolons instead of ASI (fixable)

**分号党和无分号党之争来了~~~

JavaScript在C语言风格的程序语言中是独一无二的，因为其不要求在每行末尾加上分号，这是因为JavaScript引擎会决定是否需要在行末加上分号，然后自动帮我们在行末加上分号，这一特性被成为ASI(automatic semicolon insertion) 也是JavaScript语言最富争议的特性之一。例如，下面两行代码都被视为正确的：

``` javascript
var name = "ESLint"
var website = "eslint.org";
```

在第一行代码，JavaScript引擎会自动帮我们加上分号，因此不被认为是语法错误，JavaScript引擎知道怎样去解析一行代码，也知道行末是否指示着一行代码的结束。

在ASI之争中，通常有两方阵营：

**分号党**：我们应该当ASI不存在，然后手动在可以加分号的行末都加上分号。他们认为记住什么时候需要加分号、什么时候不需要加分号，还不如在所有需要加分号的地方都加上分号，这样可以错误的引入。

但是ASI机制对于分号党来说有时难以理解：例如：

``` javascript
return
{
    name: "ESLint"
};
```

看上去，上面的代码是return一个对象，但是实际上，JavaScript引擎是如下解析的：

``` javascript
return;
{
    name: "ESLint";
}
```

实际上，ASI在return后面自动加上了一个分号。使得下面大括号包围的代码块更本就不会被使用到。不过`no-unreachable`规则可以帮助我们检查这种情形的错误，这是题外话了。

再举个栗子：

``` javascript
var globalCounter = { }

(function () {
    var n = 0
    globalCounter.increment = function () {
        return ++n
    }
})()
```

上面的代码是个反模式，运行时会报错的，因为ASI不会在第一行代码后面加一个分号，这是因为第三号代码以圆括号开头，ASI机制为把空对象当成一个函数，然后就报错了。（`no-unexpected-multiline` rule可以帮助我们规避这样的错误。）

尽管ASI 允许我们使用更加自由的代码风格，但是它也可能使得你的代码并不是按你期许的方式运行。但是不论你是分号党，还是无分号党，你都应该了解ASI什么时候会加上分号，什么时候又不会加上分号。ESLint可以帮助你的代码避免以上错误。正如Lsaac Schlueter描述：除了以下四种情况，`\n`总是表示一行代码的结束，ASI会自动帮我们加上分号。

1. 代码包含未结束的括号、未结束的数组字面量、未结束的对象字面量，或者其他不能够作为结束一行代码的字符出现在了行末（比如`.` `,`）
2. 当`++` `- -`出现在行末时，ASI也不会自动加上分号，因为在这种情况下，JavaScript引擎会对后面的操作数进行加减操作。
3. 当`for()`, `while()`, `do`, `if()`, 或者 `else`, 结尾但是后面没有接`{` 时，ASI机制不会自动加上分号。
4. 下一行以 `[`, `(`, `+`, `*`, `/`, `-`, `,`, `.`,开头，或者其他可以放在两个操作数之间的位操作符作为一行开头时，这是ASI也不会自动加上分号。

规则接受两个可选参数，`always` 和`never` 默认配置`always`.

举个栗子：

下面代码是报错的。

``` javascript
/*eslint semi: 2*/

var name = "ESLint"          /*error Missing semicolon.*/

object.method = function() {
    // ...
}                            /*error Missing semicolon.*/
```

而下面代码不会报错。

``` javascript
/*eslint semi: [2, "never"]*/

var name = "ESLint"

object.method = function() {
    // ...
}
```





7.54[sort-vars](http://eslint.org/docs/rules/sort-vars) - sort variables within the same declaration block

该条规则规定在同一个变量申明代码块中，需要对声明的变量进行排序。可以接受`ignoreCase`作为配置项。详见官网。



7.55[space-after-keywords](http://eslint.org/docs/rules/space-after-keywords) - require a space after certain keywords (fixable)

该条规则规定在特定`keywords`后面加上空格，需要加空格的关键字如下：

 `if`, `else`, `for`, `while`, `do`, `switch`, `try`, `catch`, `finally`, and `with`.



7.56[space-before-blocks](http://eslint.org/docs/rules/space-before-blocks) - require or disallow a space before blocks (fixable)

该规则规定了在代码块前是否需要加空格。



7.57[space-before-function-paren](http://eslint.org/docs/rules/space-before-function-paren) - require or disallow a space before function opening parenthesis (fixable)

该规则规定了`function` 关键字后面的小括号前是否需要加空格。举个栗子：(默认值是always)

下面的代码是合法的

``` javascript
/*eslint space-before-function-paren: 2*/
/*eslint-env es6*/

function foo () {
    // ...
}

var bar = function () {
    // ...
};

var bar = function foo () {
    // ...
};

class Foo {
    constructor () {
        // ...
    }
}

var foo = {
    bar () {
        // ...
    }
};

```



7.58[space-before-keywords](http://eslint.org/docs/rules/space-before-keywords) - require a space before certain keywords (fixable)

该条规则的目的是为了统一关键字前面是否加空格，包括的关键字如下：`if`, `else`, `for`, `while`, `do`, `switch`, `throw`, `try`, `catch`, `finally`, `with`,`break`, `continue`, `return`, `function`, `yield`, `class` 

以及变量申明（`let` `const` `var`）和label语句。

接受两个可选配置项，`always`和`never`。默认值是`always`.

7.59[space-in-parens](http://eslint.org/docs/rules/space-in-parens) - require or disallow spaces inside parentheses

该条规则用来规定圆括号内部的空格。规定是否需要在`(` 右边，或者`)`左边加空格。但是无论哪一种要求，`()` 写法都是可以的。

该规则接受两个可选配置，

- `"always"` enforces a space inside of parentheses
- `"never"` enforces zero spaces inside of parentheses (default)



7.60[space-infix-ops](http://eslint.org/docs/rules/space-infix-ops) - require spaces around operators (fixable)

该规则规定了在操作符左右是否添加空格。

详见官网。

7.61[space-return-throw-case](http://eslint.org/docs/rules/space-return-throw-case) - require a space after `return`, `throw`, and `case` (fixable)

如上所述，在`return` `throw` `case` 后面加一个空格。



7.62[space-unary-ops](http://eslint.org/docs/rules/space-unary-ops) - require or disallow spaces before/after unary operators (fixable)

规定在一元操作符前后是否加空格。

可以如下配置：

``` javascript
"space-unary-ops": [1, { "words": true, "nonwords": false }]
```

上面配置的意思，英文单词的操作符加空格，非单词操作符不加空格。

7.63[spaced-comment](http://eslint.org/docs/rules/spaced-comment) - require or disallow a space immediately following the `//` or `/*` in a comment

在写代码注释时，我们可以在`//` `/*` 后面加一个空格来提高代码的可读性，规则的作用就是用来规定是否需要在代码注释符号后面加一个空格。接受`always` 和`never`作为配置项。

7.64[wrap-regex](http://eslint.org/docs/rules/wrap-regex) - require regex literals to be wrapped in parentheses

该规则就是要求在正则表达式的双斜杠外面加一个圆括号，来消除歧义。举个栗子，下面代码被认为是不合法的：

``` javascript
/*eslint wrap-regex: 2*/

function a() {
    return /foo/.test("bar"); /*error Wrap the regexp literal in parens to disambiguate the slash.*/
}
```

而下面代码是合法的：

``` javascript
/*eslint wrap-regex: 2*/

function a() {
    return (/foo/).test("bar");
}
```



















