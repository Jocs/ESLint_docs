### 八、ECMAScript 2015

8.1[arrow-body-style](http://eslint.org/docs/rules/arrow-body-style) - require braces in arrow function body

箭头函数当函数体只有一句代码时可以省略大括号。这条规则强制代码中始终统一大括号的使用。除此之外，这条规则也会警告一些可能的编码错误，如下：

``` 
/*eslint-env es6*/
// Bad
var foo = () => {};

// Good
var foo = () => ({});
```

如上BAD例子，开发者本意可能是希望返回一个空对象，但是JavaScript 解析引擎确认为大括号是一个代码块，该箭头函数返回undefined。

该规则接受一个可选配置，可以是`always` 或者`as-needed`.

- `"always"` enforces braces around the function body
- `"as-needed"` enforces no braces where they can be omitted (default)

举个栗子，下面代码将被视为错误：

``` javascript
/*eslint arrow-body-style: [2, "always"]*/
/*eslint-env es6*/
let foo = () => 0;
```

而下面代码将不被认为错误：

``` javascript
let foo = () => {
    return 0;
};
let foo = (retv, name) => {
    retv[name] = true;
    return retv;
};

```

当配置`as-needed`的时候，下面代码会报错：

``` javascript
/*eslint arrow-body-style: [2, "as-needed"]*/
/*eslint-env es6*/

let foo = () => {
    return 0;
};

let foo = () => {};
```

而下面代码不会报错：

``` javascript
/*eslint arrow-body-style: [2, "as-needed"]*/
/*eslint-env es6*/

let foo = () => 0;
let foo = (retv, name) => {
    retv[name] = true;
    return retv;
};
let foo = () => { bar(); };
let foo = () => { /* do nothing */ };
let foo = () => {
    // do nothing.
};
```



8.2[arrow-parens](http://eslint.org/docs/rules/arrow-parens) - require parens in arrow function arguments

该规则规定了箭头函数参数是否需要圆括号包围，接受两个配置参数，`always` 和`as-needed`.

详见官网。



8.3[arrow-spacing](http://eslint.org/docs/rules/arrow-spacing) - require space before/after arrow function's arrow (fixable)

该规则规定了箭头函数的箭头前后是否加空格。默认配置如下：

``` javascript
 { "before": true, "after": true }
```

也就是说，默认配置要求箭头前后加空格。

举个栗子，下面的代码将会报错。

``` javascript
/*eslint arrow-spacing: 2*/
/*eslint-env es6*/

()=> {};     /*error Missing space before =>*/
() =>{};     /*error Missing space after =>*/
(a)=> {};    /*error Missing space before =>*/
(a) =>{};    /*error Missing space after =>*/
a =>a;       /*error Missing space after =>*/
a=> a;       /*error Missing space before =>*/
()=> {'\n'}; /*error Missing space before =>*/
() =>{'\n'}; /*error Missing space after =>*/
```

而下面代码将不会报错。

``` javascript
/*eslint arrow-spacing: 2*/
/*eslint-env es6*/

() => {};
(a) => {};
a => a;
() => {'\n'};
```

当配置为`{ "before": false, "after": false }`.时。下面代码不会报错。

``` javascript
/*eslint arrow-spacing: [2, { "before": false, "after": false }]*/
/*eslint-env es6*/

()=>{};
(a)=>{};
a=>a;
()=>{'\n'};
```



8.4[constructor-super](http://eslint.org/docs/rules/constructor-super) - verify calls of `super()` in constructors

该规则用来保证constructor函数中super()应该正确出现，比如在继承的classes中必须要super（）。而非继承的classes中不需要super（）。



8.5[generator-star-spacing](http://eslint.org/docs/rules/generator-star-spacing) - enforce spacing around the `*` in generator functions (fixable)

该规则用来规定generator函数中星号前后的空白，接受四个配置：

- `{"before": true, "after": false}` → `"before"`
- `{"before": false, "after": true}` → `"after"`
- `{"before": true, "after": true}` → `"both"`
- `{"before": false, "after": false}` → `"neither"`



8.6[no-arrow-condition](http://eslint.org/docs/rules/no-arrow-condition) - disallow arrow functions where a condition is expected

箭头函数的箭头很容易和比较操作符相混淆（`>`,`<`, `<+` ,`>=`）这条规则规定了，在条件语句中禁止使用箭头函数。即使箭头函数参数被圆括号包围，但是该规则依然会警告。举个栗子，下面代码将会被警告：

``` javascript
/*eslint no-arrow-condition: 2*/
/*eslint-env es6*/

if (a => 1) {}
while (a => 1) {}
for (var a = 1; a => 10; a++) {}
a => 1 ? 2 : 3
(a => 1) ? 2 : 3
var x = a => 1 ? 2 : 3
var x = (a) => 1 ? 2 : 3
```



8.7[no-class-assign](http://eslint.org/docs/rules/no-class-assign) - disallow modifying variables of class declarations

该规则禁止重命名classes名。



8.8[no-const-assign](http://eslint.org/docs/rules/no-const-assign) - disallow modifying variables that are declared using `const`

通过const关键字申明的变量不能够修改，修改后会导致`runtime error` .但是在一些非ES2015环境下，可能会忽略这点，因此这条规则就是为了加强这一特性而定。



8.9[no-dupe-class-members](http://eslint.org/docs/rules/no-dupe-class-members) - disallow duplicate name in class members

class中的成员不允许有相同的名字。举个栗子，下面将报错：

``` javascript
/*eslint no-dupe-class-members: 2*/
/*eslint-env es6*/

class Foo {
  bar() { }
  bar() { }          /*error Duplicate name "bar".*/
}

class Foo {
  bar() { }
  get bar() { }      /*error Duplicate name "bar".*/
}

class Foo {
  static bar() { }
  static bar() { }   /*error Duplicate name "bar".*/
}
```





8.10[no-this-before-super](http://eslint.org/docs/rules/no-this-before-super) - disallow use of `this`/`super` before calling `super()` in constructors.

在constructors中，不能够在super（）前面调用`this/super`.

8.11[no-var](http://eslint.org/docs/rules/no-var) - require `let` or `const` instead of `var`

使用const和let替代var。



8.12[object-shorthand](http://eslint.org/docs/rules/object-shorthand) - require method and property shorthand syntax for object literals

在对象中，要求使用方法和属性的简写形式。

该方法并没有禁止在对象字面量中使用箭头函数，因此可以在对象字面量中使用箭头函数。举个栗子：

``` javascript
/*eslint object-shorthand: 2*/
/*eslint-env es6*/

var foo = {
    x: (y) => y
};
```

该规则接受四个配置项：

1. `"always"` expects that the shorthand will be used whenever possible.
2. `"methods"` ensures the method shorthand is used (also applies to generators).
3. `"properties` ensures the property shorthand is used (where the key and variable name match).
4. `"never"` ensures that no property or method shorthand is used in any object literal.



8.13[prefer-arrow-callback](http://eslint.org/docs/rules/prefer-arrow-callback) - suggest using arrow functions as callbacks

该规则要求在回调函数需是箭头函数，也就是说函数作为函数的参数传入时，传入的函数需要时箭头函数。

举个栗子：

``` javascript
/*eslint prefer-arrow-callback: 2*/
/*eslint-env es6*/

foo(a => a);
foo(function*() { yield; });

// this is not a callback.
var foo = function foo(a) { return a; };

// using `this` without `.bind(this)`.
foo( () => this.a; );

// recursively.
foo(function bar(n) { return n && n + bar(n - 1); });
```

箭头函数非常适合做回调函数：

* 箭头函数中的this对象直接绑定到了其外面包围的函数的this对象。
* 箭头函数往往比函数表达式更简洁。



8.14[prefer-const](http://eslint.org/docs/rules/prefer-const) - suggest using `const` declaration for variables that are never modified after declared

如果一个变量申明后就不再被修改，那么使用const来申明该变量。

下面的代码将被视为错误：

``` javascript
/*eslint prefer-const: 2*/
/*eslint-env es6*/

let a = 3;               /*error `a` is never modified, use `const` instead.*/
console.log(a);

// `i` is re-defined (not modified) on each loop step.
for (let i in [1,2,3]) {  /*error `i` is never modified, use `const` instead.*/
    console.log(i);
}

// `a` is re-defined (not modified) on each loop step.
for (let a of [1,2,3]) { /*error `a` is never modified, use `const` instead.*/
    console.log(a);
}
```

而下面的代码将被视为正确：

``` javascript
/*eslint prefer-const: 2*/
/*eslint-env es6*/

let a; // there is no initialization.
console.log(a);

// `end` is never modified, but we cannot separate the declarations without modifying the scope.
for (let i = 0, end = 10; i < end; ++i) {
    console.log(a);
}

// suggest to use `no-var` rule.
var b = 3;
console.log(b);
```



8.15[prefer-reflect](http://eslint.org/docs/rules/prefer-reflect) - suggest using Reflect methods where applicable

该规则推荐使用Reflect上的方法替代以前老方法。

- [`Reflect.apply`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-reflect.apply) effectively deprecates [`Function.prototype.apply`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-function.prototype.apply) and [`Function.prototype.call`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-function.prototype.call)
- [`Reflect.deleteProperty`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-reflect.deleteproperty) effectively deprecates the [`delete` keyword](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-delete-operator-runtime-semantics-evaluation)
- [`Reflect.getOwnPropertyDescriptor`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-reflect.getownpropertydescriptor) effectively deprecates [`Object.getOwnPropertyDescriptor`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-object.getownpropertydescriptor)
- [`Reflect.getPrototypeOf`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-reflect.getprototypeof) effectively deprecates [`Object.getPrototypeOf`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-object.getprototypeof)
- [`Reflect.setPrototypeOf`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-reflect.setprototypeof) effectively deprecates [`Object.setPrototypeOf`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-object.setprototypeof)
- [`Reflect.preventExtensions`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-reflect.preventextensions) effectively deprecates [`Object.preventExtensions`](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-object.preventextensions)

详见官网。



8.16[prefer-spread](http://eslint.org/docs/rules/prefer-spread) - suggest using the spread operator instead of `.apply()`.

推荐使用扩展符替代`apply()`方法。举个栗子

``` javascript
var args = [1, 2, 3, 4];
//以前这样写
Math.max.apply(Math, args);
//现在这样写
Math.max(...args);
```



8.17[prefer-template](http://eslint.org/docs/rules/prefer-template) - suggest using template literals instead of strings concatenation

推荐使用模板代替以前的字符串拼接。举个栗子：

``` javascript
var str = "Hello, " + name + "!";
//现在这样写
var str = `Hello, ${name}!`;
```



8.18[require-yield](http://eslint.org/docs/rules/require-yield) - disallow generator functions that do not have `yield`

这条规则检测generates函数中有没有yield关键字，如果没有就会报错。

举个栗子，下面的代码将会报错：

``` javascript
/*eslint require-yield: 2*/
/*eslint-env es6*/

function* foo() { /*error This generator function does not have `yield`.*/
  return 10;
}
```

而下面的代码将不会报错。

``` javascript
/*eslint require-yield: 2*/
/*eslint-env es6*/

function* foo() {
  yield 5;
  return 10;
}

function foo() {
  return 10;
}

// This rule does not warn on empty generator functions.
function* foo() { }
```

