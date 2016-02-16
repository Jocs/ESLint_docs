# Migrating to v2.0.0

ESLint v2.0.0 是ESLint 发布的第二个主要版本，作为版本升级结果，ESLint规则在v2.0.0和以前0.x和1.x有了一些显著的变化，这些变化主要来源于ESLint社区的使用者对ESLint的反馈信息，一些反馈意见由于升级轨迹暂未考虑在内。这次升级是必要的，我们相信这次版本升级使得ESLint能够更好的工作，同时我们也希望这次升级所带来的好处能够弥补升级变化带给你的苦恼。

**注意：** 如果你是从0.x升级到v2.0.0，请首先参考 [Migrating to 1.0.0](http://eslint.org/docs/user-guide/migrating-to-1.0.0)。

## 规则模式的变化

Due to a quirk in the way rule schemas worked, it was possible that you'd need to account for the rule severity (0, 1, or 2) in a rule schema if the options were sufficiently complex. That would result in a schema such as:

``` 
module.exports = {
    "type": "array",
    "items": [
        {
            "enum": [0, 1, 2]
        },
        {
            "enum": ["always", "never"]
        }
    ],
    "minItems": 1,
    "maxItems": 2
};
```

This was confusing to rule developers as it seemed that rules shouldn't be in charge of validating their own severity. In 2.0.0, rules no longer need to check their own severity.

**To address:** If you are exporting a rule schema that checks severity, you need to make several changes:

1. Remove the severity from the schema
2. Adjust `minItems` from 1 to 0
3. Adjust `maxItems` by subtracting 1

Here's what the schema from above looks like when properly converted:

``` 
module.exports = {
    "type": "array",
    "items": [
        {
            "enum": ["always", "never"]
        }
    ],
    "minItems": 0,
    "maxItems": 1
};

```

## 移除的规则

下面的规则已经由一些新的规则所代替，下面罗列出了被移除的规则及其替代规则：

- [no-arrow-condition](http://eslint.org/docs/rules/no-arrow-condition) 的作用被 [no-confusing-arrow](http://eslint.org/docs/rules/no-confusing-arrow) 和 [no-constant-condition](http://eslint.org/docs/rules/no-constant-condition) 联合使用所替代，只要把这两个规则都打开，其效果和之前的[no-arrow-condition](http://eslint.org/docs/rules/no-arrow-condition) 一样。
- [no-empty-label](http://eslint.org/docs/rules/no-empty-label) is replaced by [no-labels](http://eslint.org/docs/rules/no-labels) with `{"allowLoop": true, "allowSwitch": true}` option.
- [space-after-keywords](http://eslint.org/docs/rules/space-after-keywords) is replaced by [keyword-spacing](http://eslint.org/docs/rules/keyword-spacing).
- [space-before-keywords](http://eslint.org/docs/rules/space-before-keywords) is replaced by [keyword-spacing](http://eslint.org/docs/rules/keyword-spacing).
- [space-return-throw-case](http://eslint.org/docs/rules/space-return-throw-case) is replaced by [keyword-spacing](http://eslint.org/docs/rules/keyword-spacing).

**解决方案：** 你需要手动的对ESLint配置文件中的规则进行升级，ESLint v2.0.0在你依然使用被移除规则时会发出警告并给出替换的规则，在你升级的过程中你会发现一些惊喜之处。

## 层叠的配置文件效果上的变化

在2.0.0之前，如果项目里既包含`.eslintrc` 文件，同时也包含 `package.son` 文件，这两个文件都包含配置信息，那么ESLint的效果是这两个文件合并后的效果。在v2.0.0版本中，如果同时存在以上两个文件，那么只有`.eslintrc`文件生效，`package.json`文件内的配置信息将被忽略，除非在不存在`.eslintrc`文件时，`package.json` 文件对ESLint的配置才会生效。

**解决方案：** 如果在相同的目录下同时包含`.eslintrc.*`和`package.son` 文件来对ESLint进行配置，手动合并这两个文件为一个文件。

## 内建全局变量

在2.0.0版本之前，ES6中的一些新的全局变量，比如`Promise`, `Map`, `Set`, 和`Symbol` 包含在了内建的全局环境中，这可能会导致一些潜在的问题出现，比如：即使在ES5环境下，Promise还不能使用时，`no-undef` 也允许了Promise 构造函数的使用，并且不会报错。在2.0.0版本，内建环境只包含了ES5的所有全局变量，而ES6新增的全局变量迁移到了`es6` 环境中。

**解决方案**： 如果你正在使用ES6的语法编写代码，那么如果你还没有添加`es6`环境，那么尽快添加它。

``` 
// In your .eslintrc
{
    env: {
        es6: true
    }
}

// Or in a configuration comment
/*eslint-env es6*/

```

## 语言配置项

在2.0.0版本之前，启动语言配置项是通过在配置文件中添加`ecmaFeatures`配置项来实现的。而在2.0.0版本，发生了如下变化：

- `ecmaFeatures` 配置项现从属于`parserOptions`配置属性下面。
- 所有的ECMAScript 6 标记的`ecmaFeatures` 都被移除，并被一个`parserOptions` 下面的`ecmaVersion`配置项所替代，该配置项可以被设置为如下值：3， 5（默认值）或者 6.
- `ecmaFeatures.modules`标记被`parserOptions`配置项下面的`sourceType`属性所替代，该属性可能的取值包括`"script"`(默认值)和`"module"` 用于启动ES6中的modules特性。

**解决方案：** 如果你在`ecmaFeatures`中使用了ECMAScript 6 特性标签，那么你需要使用`ecmaVersion: 6` 来替换之前配置，ECMAScript 6特性标签列表如下：

- `arrowFunctions` - enable [arrow functions](https://leanpub.com/understandinges6/read#leanpub-auto-arrow-functions)
- `binaryLiterals` - enable [binary literals](https://leanpub.com/understandinges6/read#leanpub-auto-octal-and-binary-literals)
- `blockBindings` - enable `let` and `const` (aka [block bindings](https://leanpub.com/understandinges6/read#leanpub-auto-block-bindings))
- `classes` - enable classes
- `defaultParams` - enable [default function parameters](https://leanpub.com/understandinges6/read/#leanpub-auto-default-parameters)
- `destructuring` - enable [destructuring](https://leanpub.com/understandinges6/read#leanpub-auto-destructuring-assignment)
- `forOf` - enable [`for-of` loops](https://leanpub.com/understandinges6/read#leanpub-auto-iterables-and-for-of)
- `generators` - enable [generators](https://leanpub.com/understandinges6/read#leanpub-auto-generators)
- `modules` - enable modules and global strict mode
- `objectLiteralComputedProperties` - enable [computed object literal property names](https://leanpub.com/understandinges6/read#leanpub-auto-computed-property-names)
- `objectLiteralDuplicateProperties` - enable [duplicate object literal properties](https://leanpub.com/understandinges6/read#leanpub-auto-duplicate-object-literal-properties) in strict mode
- `objectLiteralShorthandMethods` - enable [object literal shorthand methods](https://leanpub.com/understandinges6/read#leanpub-auto-method-initializer-shorthand)
- `objectLiteralShorthandProperties` - enable [object literal shorthand properties](https://leanpub.com/understandinges6/read#leanpub-auto-property-initializer-shorthand)
- `octalLiterals` - enable [octal literals](https://leanpub.com/understandinges6/read#leanpub-auto-octal-and-binary-literals)
- `regexUFlag` - enable the [regular expression `u` flag](https://leanpub.com/understandinges6/read#leanpub-auto-the-regular-expression-u-flag)
- `regexYFlag` - enable the [regular expression `y` flag](https://leanpub.com/understandinges6/read#leanpub-auto-the-regular-expression-y-flag)
- `restParams` - enable the [rest parameters](https://leanpub.com/understandinges6/read#leanpub-auto-rest-parameters)
- `spread` - enable the [spread operator](https://leanpub.com/understandinges6/read#leanpub-auto-the-spread-operator) for arrays
- `superInFunctions` - enable `super` references inside of functions
- `templateStrings` - enable [template strings](https://leanpub.com/understandinges6/read/#leanpub-auto-template-strings)
- `unicodeCodePointEscapes` - enable [code point escapes](https://leanpub.com/understandinges6/read/#leanpub-auto-escaping-non-bmp-characters)

如果你使用了以上标签，如下：

``` 
{
    ecmaFeatures: {
        arrowFunctions: true
    }
}
```

那么为了使用ES 6,你需要设置 `ecmaVersion`:

``` 
{
    parserOptions: {
        ecmaVersion: 6
    }
}
```

如果你在`ecmaFeatures`中使用了非ES6标签，那么你需要把这些标签迁移到`parserOptions`配置项中，例如：

``` 
{
    ecmaFeatures: {
        jsx: true
    }
}

```

然后你需要把 `ecmaFeatures` 迁移到 `parserOptions`配置项之下:

``` 
{
    parserOptions: {
        ecmaFeatures: {
            jsx: true
        }
    }
}
```

若果你使用了 `ecmaFeatures.modules` 来启动ES6 module，那么需要做如下修改：

``` 
{
    ecmaFeatures: {
        modules: true
    }
}

{
    parserOptions: {
        sourceType: "module"
    }
}
```

除此之外，如果你在规则中使用了 `context.ecmaFeatures` ，那么你需要对你的代码做如下升级：

1. If you're using an ES6 feature flag such as `context.ecmaFeatures.blockBindings`, rewrite to check for`context.parserOptions.ecmaVersion > 5`.
2. If you're using `context.ecmaFeatures.modules`, rewrite to check that the `sourceType` property of the Program node is `"module"`.
3. If you're using a non-ES6 feature flag such as `context.ecmaFeatures.jsx`, rewrite to check for `context.parserOptions.ecmaFeatures.jsx`.

If you're not using `ecmaFeatures` in your configuration, then no change is needed.

##  `"eslint:recommended"`中新增规则

``` 
{
    "extends": "eslint:recommended"
}
```

在2.0.0中， `"eslint:recommended"`新增11条规则。

- [constructor-super](http://eslint.org/docs/rules/constructor-super)
- [no-case-declarations](http://eslint.org/docs/rules/no-case-declarations)
- [no-class-assign](http://eslint.org/docs/rules/no-class-assign)
- [no-const-assign](http://eslint.org/docs/rules/no-const-assign)
- [no-dupe-class-members](http://eslint.org/docs/rules/no-dupe-class-members)
- [no-empty-pattern](http://eslint.org/docs/rules/no-empty-pattern)
- [no-new-symbol](http://eslint.org/docs/rules/no-new-symbol)
- [no-self-assign](http://eslint.org/docs/rules/no-self-assign)
- [no-this-before-super](http://eslint.org/docs/rules/no-this-before-super)
- [no-unexpected-multiline](http://eslint.org/docs/rules/no-unexpected-multiline)
- [no-unused-labels](http://eslint.org/docs/rules/no-unused-labels)

解决方案：如果你不想使用这些规则，只需要将这些规则警告级别设置为0，如下：

``` 
{
    "extends": "eslint:recommended",
    "rules": {
        "no-case-declarations": 0,
        "no-class-assign": 0,
        "no-const-assign": 0,
        "no-dupe-class-members": 0,
        "no-empty-pattern": 0,
        "no-new-symbol": 0,
        "no-self-assign": 0,
        "no-this-before-super": 0,
        "no-unexpected-multiline": 0,
        "no-unused-labels": 0,
        "constructor-super": 0
    }
}

```

## 作用域解析的变化

We found some bugs in our scope analysis that needed to be addressed. Specifically, we were not properly accounting for global variables in all the ways they are defined.

Originally, `Variable` objects and `Reference` objects refer each other:

- `Variable#references` property is an array of `Reference` objects which are referencing the variable.
- `Reference#resolved` property is a `Variable` object which are referenced.

But until 1.x, the following variables and references had the wrong value (empty) in those properties:

- `var` declarations in the global.
- `function` declarations in the global.
- Variables defined in config files.
- Variables defined in `/* global */` comments.

Now, those variables and references have correct values in these properties.

`Scope#through` property has references where `Reference#resolved` is `null`. So as a result of this change, the value of `Scope#through` property was changed also.

**To address:** If you are using `Scope#through` to find references of a built-in global variable, you need to make several changes.

For example, this is how you might locate the `window` global variable in 1.x:

``` 
var globalScope = context.getScope();
globalScope.through.forEach(function(reference) {
    if (reference.identifier.name === "window") {
        checkForWindow(reference);
    }
});

```

This was a roundabout way to find the variable because it was added after the fact by ESLint. The `window` variable was in `Scope#through` because the definition couldn't be found.

In 2.0.0, `window` is no longer located in `Scope#through` because we have added back the correct declaration. That means you can reference the `window`object (or any other global object) directly. So the previous example would change to this:

``` 
var globalScope = context.getScope();
var variable = globalScope.set.get("window");
if (variable) {
    variable.references.forEach(checkForWindow);
}

```

Further Reading: http://estools.github.io/escope/

## Default Changes When Using `eslint:recommended`

This will affect you if you are extending from `eslint:recommended`, and are enabling [`no-multiple-empty-lines`](http://eslint.org/docs/rules/no-multiple-empty-lines) or [`func-style`](http://eslint.org/docs/rules/func-style) with only a severity, such as:

``` 
{
    "extends": "eslint:recommended",
    "rules": {
        "no-multiple-empty-lines": 2,
        "func-style": 2
    }
}

```

The rule `no-multiple-empty-lines` has no default exceptions, but in ESLint `1.x`, a default from `eslint:recommended` was applied such that a maximum of two empty lines would be permitted.

The rule `func-style` has a default configuration of `"expression"`, but in ESLint `1.x`, `eslint:recommended` defaulted it to `"declaration"`.

ESLint 2.0.0 removes these conflicting defaults, and so you may begin seeing linting errors related to these rules.

**To address:** If you would like to maintain the previous behavior, update your configuration for `no-multiple-empty-lines` by adding `{"max": 2}`, and change `func-style` to `"declaration"`. For example:

``` 
{
    "extends": "eslint:recommended",
    "rules": {
        "no-multiple-empty-lines": [2, {"max": 2}],
        "func-style": [2, "declaration"]
    }
}

```

## SourceCode constructor (Node API) changes

`SourceCode` constructor got to handle Unicode BOM. If the first argument `text` has BOM, `SourceCode` constructor sets `true` to `this.hasBOM` and strips BOM from the text.

``` 
var SourceCode = require("eslint").SourceCode;

var code = new SourceCode("\uFEFFvar foo = bar;", ast);

assert(code.hasBOM === true);
assert(code.text === "var foo = bar;");

```

So the second argument `ast` also should be parsed from stripped text.

**To address:** If you are using `SourceCode` constructor in your code, please parse the source code after it stripped BOM:

``` 
var ast = yourParser.parse(text.replace(/^\uFEFF/, ""), options);
var sourceCode = new SourceCode(text, ast);

```

## Rule Changes

- [`strict`](http://eslint.org/docs/rules/strict) - defaults to `"safe"` (previous default was `"function"`)

## Plugins No Longer Have Default Configurations

Prior to v2.0.0, plugins could specify a `rulesConfig` for the plugin. The `rulesConfig` would automatically be applied whenever someone uses the plugin, which is the opposite of what ESLint does in every other situation (where nothing is on by default). To bring plugins behavior in line, we have removed support for`rulesConfig` in plugins.

**To address:** If you are using a plugin in your configuration file, you will need to manually enable the plugin rules in the configuration file.

