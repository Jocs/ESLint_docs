### ESLint 的配置和使用

#### 一、什么是ESLint？

当`C语言`刚成为计算机语言的时候，有一些常见的错误不能够被原始的编译器捕捉，所以一个名为Lint的辅助程序被开发出来，用来扫面源文件中的错误。

在Douglas Rockford 的**\<JavaScript： The Good Parts>**也提到，JavaScript是一门「年轻的语言」，因此也就存在很多糟粕的地方，这些糟粕使得程序员在编写JavaScript代码的时候，容易出错，而不易被编辑器或程序员本身发现。于是Douglas Crockford亲自操刀，编写了JSLint代码规范检测工具，其认为JSLint是JavaScript的一个更加严格的子集，也就是他说的Good Parts，使用JSLint就能够检测我们在编程过程中无意得使用了一些「糟粕」的地方，避免了程序出错。

而ESLint是Nicholas C.Zakas编写的另外一份JavaScript代码规范检测工具，这当然不是重复得造轮子（大神不屑造轮子，大神一般都是重新发现轮子），在使用JSLint的时候，JSLint不是以插件的形式实现的，而是重新包装一个工具，可能Nicholas C.Zakas不喜欢这种方式，其认为代码规范检测工具（ESLint）应该是可插拔（使用插件），可配置的，于是自己便高高兴兴去写ESLint了。

正如ESLint官网这样定义ESLint：

> ESLint is designed to be completely configurable, meaning you can turn off every rule and run only with basic syntax validation, or mix and match the bundled rules and your custom rules to make ESLint perfect for your project. 

**ESLint 的设计思想就是高度可配置，在ESLint中你可以开关每一条规则，你可以使用最基本的语法检测，或者结合自定义的规则来使得ESLint完美运行在你的项目中。**

#### 二、怎么使用ESLint？

1. ESLint的配置
   
   * **ESLint配置的方法**
   
   ​	ESLint主要有两种配置方法
   
   1）Configuration Comment： 通过在所有的JS文件的JavaScript 注释中写ESLint配置信息。
   
   2）Configuration Files: 通过一份配置文件，来对ESLint进行基本配置，配置文件可以是JavaScript、JSON、YAML中任意一种格式。如果是用过`.eslintrc` 或者在`package.son` 文件中的eslintConfig属性内进行配置，那么ESLint就能够自动的检测到这两个文件中的配置信息，如果是其他文件或文件名，就需要在命令行中指定特定的配置文件。
   
   * **ESLint配置的主要内容**
   
   1）**Environments：** 定义我们脚本的运行环境，配置这项的一个主要原因就是不同的运行环境都会有预先定义的不同的global variables。
   
   2）**Globals**：配置脚本运行过程中附件的全局变量（global variables）；
   
   3）**Rules**： 在我们的项目中，需要配置的代码规则，并且指定每一条规则的警告级别（error level）。比如说`no-var: 2` 这条规则，ESLint在进行代码检测时，如果我们JavaScript代码中使用了`var`申明变量，那么就会报错，其中`2` 是一个警告等级，在后面会介绍到。
   
   * **下面是`.eslintrc` 配置文件中一些具体配置项的详细解释**
   
   **本文档实例，默认使用的是第二种配置方法，也就是通过配置文件来进行配置，并且使用的是`.eslintrc`文件。
   
   1）Specifying Language Options
   
   ESLint 允许使用者指定ESLint支持的的JavaScript 语法。在默认情况下，ESLint仅支持ECMAScript 5 的语法，同时在配置文件中，可以对特定的rule进行重写（或覆盖），来使ESLint支持ECMAScript 2016 或者是 JSX语法格式。
   
   这部分的配置是在`.eslintrc`文件中的ecmaFeatures 属性中进行配置。可配置项如下：
   
   `arrowFunctions` - enable [arrow functions](https://leanpub.com/understandinges6/read#leanpub-auto-arrow-functions)
   
   `binaryLiterals` - enable [binary literals](https://leanpub.com/understandinges6/read#leanpub-auto-octal-and-binary-literals)
   
   `blockBindings` - enable `let` and `const` (aka [block bindings](https://leanpub.com/understandinges6/read#leanpub-auto-block-bindings))
   
   `classes` - enable classes
   
   `defaultParams` - enable [default function parameters](https://leanpub.com/understandinges6/read/#leanpub-auto-default-parameters)
   
   `destructuring` - enable [destructuring](https://leanpub.com/understandinges6/read#leanpub-auto-destructuring-assignment)
   
   `forOf` - enable [`for-of` loops](https://leanpub.com/understandinges6/read#leanpub-auto-iterables-and-for-of)
   
   `generators` - enable [generators](https://leanpub.com/understandinges6/read#leanpub-auto-generators)
   
   `modules` - enable modules and global strict mode
   
   `objectLiteralComputedProperties` - enable [computed object literal property names](https://leanpub.com/understandinges6/read#leanpub-auto-computed-property-names)
   
   `objectLiteralDuplicateProperties` - enable [duplicate object literal properties](https://leanpub.com/understandinges6/read#leanpub-auto-duplicate-object-literal-properties) in strict mode
   
   `objectLiteralShorthandMethods` - enable [object literal shorthand methods](https://leanpub.com/understandinges6/read#leanpub-auto-method-initializer-shorthand)
   
   `objectLiteralShorthandProperties` - enable [object literal shorthand properties](https://leanpub.com/understandinges6/read#leanpub-auto-property-initializer-shorthand)
   
   `octalLiterals` - enable [octal literals](https://leanpub.com/understandinges6/read#leanpub-auto-octal-and-binary-literals)
   
   `regexUFlag` - enable the [regular expression `u` flag](https://leanpub.com/understandinges6/read#leanpub-auto-the-regular-expression-u-flag)
   
   `regexYFlag` - enable the [regular expression `y` flag](https://leanpub.com/understandinges6/read#leanpub-auto-the-regular-expression-y-flag)
   
   `restParams` - enable the [rest parameters](https://leanpub.com/understandinges6/read#leanpub-auto-rest-parameters)
   
   `spread` - enable the [spread operator](https://leanpub.com/understandinges6/read#leanpub-auto-the-spread-operator) for arrays
   
   `superInFunctions` - enable `super` references inside of functions
   
   `templateStrings` - enable [template strings](https://leanpub.com/understandinges6/read/#leanpub-auto-template-strings)
   
   `unicodeCodePointEscapes` - enable [code point escapes](https://leanpub.com/understandinges6/read/#leanpub-auto-escaping-non-bmp-characters)
   
   `globalReturn` - allow `return` statements in the global scope
   
   `jsx` - enable [JSX](http://facebook.github.io/jsx/)
   
   `experimentalObjectRestSpread` - enable support for the experimental [object rest/spread properties](https://github.com/sebmarkbage/ecmascript-rest-spread) 
   
   下面是一个在`.eslintrc`配置文件中的栗子：
   
   ``` javascript
   {
       "ecmaFeatures": {
           "blockBindings": true,
           "forOf": true,
           "jsx": true
       },
       "rules": {
           "semi": 2
       }
   }
   ```
   
   or (YAML格式)
   
   ``` yaml
   ecmaFeatures:
     blockBindings: true
     forOf: true
     jsx: true
   rules:
     semi: 2
   ```
   
   **上面的Language配置，基本都是ECMAScript2015的新语法，或者React的JSX.其实在配置Environment的时候，配置`es6: true` 其实就可以使用ECMAScript 2015的大部分新特性了，除了modules，也就是说，在ecmaFeatures的配置中，我们如果需要使用ECMAScript2015新特性，我们只需要如下配置：
   
   ``` yaml
   ecmaFeatures:
     modules: true
     experimentalObjectRestSpread: true
   ```
   
   2）解析器的配置（Specifying Parser）
   
   ESLint默认是采用的Espree解析器，你也可以自己指定其他的解析器被配置文件所用，但是自己指定的解析器需要满足如下要求：
   
   （1）Parser 必须通过npm 安装到本地，不需要全局安装。
   
   （2）Parser必须带有Esprima兼容的接口，也就是说该Parser需要暴露一个`parse()` 方法。
   
   （3）It must produce Esprima-compatible AST and token objects.
   
   指定Parser就是在`.eslintrc`文件中`parser`项进行配置，举个栗子：
   
   ``` javascript
   {
       "parser": "esprima",
       "rules": {
           "semi": 2
       }
   }
   ```
   
   下面列举一些ESLint兼容的Parsers。
   
   [Esprima](https://npmjs.com/package/esprima)
   
   [Esprima-FB](https://npmjs.com/package/esprima-fb) - Facebook's fork of Esprima that includes their proprietary syntax additions.
   
   [Babel-ESLint](https://npmjs.com/package/babel-eslint) - A wrapper around the [Babel](http://babeljs.io/) parser that makes it compatible with ESLint.
   
   **如果在项目中使用了Babel来对ES6代码进行编译，个人建议使用Babel-ESLint Parser。
   
   **当我们使用一个特定的Parser的时候，ecmaFeatures仍然是需要在配置文件中指定的，但是我们指定的Parser不一定需要使用到ecmaFeatures中定义使用的特性。
   
   3）ESLint运行环境的配置（Specifying Environments）
   
   `browser` - browser global variables.
   
   `node` - Node.js global variables and Node.js scoping.
   
   `commonjs` - CommonJS global variables and CommonJS scoping (use this for browser-only code that uses Browserify/WebPack).
   
   `worker` - web workers global variables.
   
   `amd` - defines `require()` and `define()` as global variables as per the [amd](https://github.com/amdjs/amdjs-api/wiki/AMD) spec.
   
   `mocha` - adds all of the Mocha testing global variables.
   
   `jasmine` - adds all of the Jasmine testing global variables for version 1.3 and 2.0.
   
   `jest` - Jest global variables.
   
   `phantomjs` - PhantomJS global variables.
   
   `protractor` - Protractor global variables.
   
   `qunit` - QUnit global variables.
   
   `jquery` - jQuery global variables.
   
   `prototypejs` - Prototype.js global variables.
   
   `shelljs` - ShellJS global variables.
   
   `meteor` - Meteor global variables.
   
   `mongo` - MongoDB global variables.
   
   `applescript` - AppleScript global variables.
   
   `nashorn` - Java 8 Nashorn global variables.
   
   `serviceworker` - Service Worker global variables.
   
   `embertest` - Ember test helper globals.
   
   `webextensions` - WebExtensions globals.
   
   `es6` - enable all ECMAScript 6 features except for modules.
   
   上面枚举了JavaScript可能运行到的一些环境，当然我们的项目不可能运行在每个环境中，因此不需要全部都写上，同时，JavaScript也不可能同时在多个环境中运行，因此，配置环境的时候可以配置多个使用环境，ESLint会更具具体代码运行环境来决定使用的哪种环境。
   
   下面举个栗子：
   
   \```

   ---

``` 
 env:
   browser: true
   node: true
   es6: true
```

``` 

   上面的例子说明，我们的代码可能会运行在浏览器环境、node环境或es6环境。

   4）指定额外全局变量（Specifying Globals）

   `no-undef`规则在我们使用没有在同一文件中定义的变量的时候会报错，当我们使用了全局变量的时候，我们就有必要对额外的全局变量进行配置，避免`no-undef`误会。我们可以通过代码注释或者配置文件来进行此项配置。举个栗子：

```

   ---

``` 
 globals:
   var1: true
   var2: false
```

``` 

   5) 引入插件（Specifying Plugins）

   ESLint一个优越的地方就是可以使用第三方插件，在使用插件之前，首先需要通过npm安装需要使用的插件。通过`plugins` 键名来指定插件，插件名前面的前缀`eslint-plugin-` 可以省略：举个栗子：

```

   ---

``` 
 plugins:
   - plugin1
   - eslint-plugin-plugin2
```

``` 

   **注意：全局安装的ESLint，插件也需要全局安装，本地安装的ESLint，插件既可以本地安装也可以全局安装。

   5）配置Rules （Configuring Rules）

   **重要的东西往往放在最后，配置Rules也就是整个配置文件的核心部分。

   在ESLint中拥有大量的rules（后面将不再对rules进行翻译），你可以通过在javascript文件中注释或者特定的配置文件来指定和修改项目中需要rules。在进行一条rule配置时，我们需要设置rule ID为如下值：

   0 - turn the rule off //关掉此条规则

   1 - turn the rule on as a warning (doesn't affect exit code) // 把这条规则指定为warning

   2 - turn the rule on as an error (exit code is 1 when triggered) //把这条rule指定为error

   同时对于某些rule，在制定上面警告级别的时候，还可以指定其他的选项。举个栗子：

```

   ---

``` 
 rules:
   eqeqeq: 0
   curly: 2
   quotes:
     - 2
     - "double"
```

``` 

   如果一条rule是通过plugin引入的，那么在配置此条rule的时候需要在rule ID前面加plugin 名和 `/` 举个栗子：

```

   ---

``` 
 plugins:
   - plugin1
 rules:
   eqeqeq: 0
   curly: 2
   quotes:
     - 2
     - "double"
   plugin1/rule1: 2
```

``` 

   **注意：在制定plugin中的rule时，我们需要把plugin的前缀`aslant-plugin-` 去掉。

   **所有的rules，其默认值设置为2.可以通过设置rule为1或者0来对其进行警告降级。

   **关于具体的每一条rule的解释和使用，我会另外整理一份详尽文档，这儿就不在赘述

   6）配置文件的格式问题

   ESLint 支持多种配置文件格式

   **JavaScript** - use `.eslintrc.js` and export an object containing your configuration.

   **YAML** - use `.eslintrc.yaml` or `.eslintrc.yml` to define the configuration structure.

   **JSON** - use `.eslintrc.json` to define the configuration structure. ESLint's JSON files also allow JavaScript-style comments.

   **package.json** - create an `eslintConfig` property in your `package.json` file and define your configuration there.

   **Deprecated** - use `.eslintrc`, which can be either JSON or YAML.

   **如果我们的项目中有多个配置文件，同时又有`.eslintrc` 文件，ESLint会只使用`.eslintrc`文件。配置文件的优先级如下（下面的优先级最高，个人建议就在`.eslintrc`中配置就可以了）：

   `.eslintrc.js`

   `.eslintrc.yaml`

   `.eslintrc.yml`

   `.eslintrc.json`

   `.eslintrc`

   7）配置文件的扩展（Extending Configuration Files）

   如果需要扩展一个特定的配置文件，只需要在配置文件中增加extends项就可以了，引用的路径既可以是相对路径也可以是绝对路径。举个栗子：

   ``` javascript
   {
       "extends": "./node_modules/coding-standard/.eslintrc",

       "rules": {
           // Override any settings from the "parent" configuration
           "eqeqeq": 1
       }
   }
```

   `extends` 也可以接受一个数组，数组后面的文件会重写数组前面文件中的rule，举个栗子：

``` javascript
   {
       "extends": [
           "./node_modules/coding-standard/eslintDefaults.js",
           // Override eslintDefaults.js
           "./node_modules/coding-standard/.eslintrc-es6",
           // Override .eslintrc-es6
           "./node_modules/coding-standard/.eslintrc-jsx",
       ],

       "rules": {
           // Override any settings from the "parent" configuration
           "eqeqeq": 1
       }
   }
```

   `extends` 亦可以用来引用一些在github上面共享的配置包，首先需要通过npm安装配置包文件，其次就可以像如下栗子引用了：

``` javascript
   {
       "extends": "eslint-config-myrules",

       "rules": {
           // Override any settings from the "parent" configuration
           "eqeqeq": 1
       }
   }
```

   **在扩展shareable的配置时，`aslant-config-` 也可以去掉，ESLint会自己帮我们加上的，如果想了解插件和Shareable Configs具体工作原理，请点击 [Shareable Configs](http://eslint.org/docs/developer-guide/shareable-configs) 。

   8）怎么在配置文件中写注释

   可以使用JavaScript或者YAML的代码注释风格，举个栗子：

``` 
   {
       "env": {
           "browser": true
       },
       "rules": {
           // Override our default settings just for this directory
           "eqeqeq": 1,
           "strict": 0
       }
   }
```

   9) 怎么制定ESLint忽略检测的文件或路径

   我们可以通过生成一个`.eslintignore`文件来告诉ESLint我们需要忽略检测哪些文件或者文件夹中的所有文件。在`.eslintignore`文件中，每一行就是表示需要ESLint忽略文件的路径，举个栗子：

``` 
   **/*.js
```

   当ESLint运行的时候，它会执行代码规范检测之前查找当前工作文件夹中的`.eslintignore`文件，当该文件找到后，ESLint运行时就会忽略该文件中的制定文件或文件夹，一个文件夹下只能够有一个`.eslintignore`文件。

   在`.eslintignore`文件中是使用`minimatch`来进行匹配的，因此一些如下特性可以使用：

   （1）以`#` 开始的一行会被视为注释，不会对忽略匹配产生影响。

   （2）以 `!` 开始的一行是negated patterns。也就是说在我们首先忽略了某个文件夹中的所有文件后，我们又想ESLint检测该文件夹中的某些文件时就可以使用该特性。举个栗子：

``` 
   # Ignore built files except build/index.js
   build/
   !build/index.js
```

   在上面的栗子中，我们依然会对build/index.js文件进行ESLint检测。

   （3）大括号语法可以在一个匹配中制定多个文件。举个栗子：

``` 
   # Ignore files compiled from TypeScript and CoffeeScript
   **/*.{ts,coffee}.js
```

   ​

1. ##### ESLint的使用方法

**我们配置好了文件，怎么去使用配置文件呢。也就是说how to use the Configuration Files？

有两种方法使用到配置文件，第一种是通过命令行来使用ESLint来检测我们的代码是否规范，第二种是在编辑器内实时检测代码规范。

1）命令行检测

如果我们不是使用的`.eslintrc`或`package.son`文件来配置ESLint，那么我们就需要在命令行中指定我们的配置文件，运行配置文件使用 `-c` 。举个栗子：

``` 
eslint -c myconfig.json myfiletotest.js
```

当然了，如果我们使用的是`.eslintrc`或`package.son`文件，那么在运行命令行的时候也就不用指定配置文件了，因为ESLint会自动查找项目中是否包含这两个文件，然后运行lint。这个有个和查找npm包一样的规则，首先会在当前文件夹中查找是否有`.eslintrc` 文件，如果没有找到，再向上一级文件夹中查找，知道root directory。这种查找`.eslintrc` 的方式也就给予了我们在一个项目中使用多份`.eslintrc`文件的可能，也就是在一个项目中我们可以使用多个代码规范。

当然我们还可以再简化下我们的工作，就是在`package.json`文件中，添加如下代码：

``` javascript
{
  "scripts":{
   	"lint": "eslint **/*.js" //其中**/*.js是我们需要检测的文件
}
}
```

这样这样我们就可以直接在命令行中执行：

> npm run lint

来对我们的代码进行代码规范检测了。

2）编辑器内实时检测代码规范

我现在只是在Sublime Text 3中进行了配置，由于大家使用的编辑器各不相同，这儿也就只简单说下在SubLime Text3中ESLint怎么配置。

（1）全局安装`aslant`和 `babel-eslint` ，后者是Parser

> ``` 
> npm install eslint babel-eslint -g
> ```

 (2) 在sublime Text3,通过package control安装aslant所需包。包括SublimeLinter和SublimeLinter-contrib-eslint 。

`command` + `shift` + `p` 打开包管理搜索框，输入Package Control：Install Package。等Install Package启动完毕，就可以搜索所需的包了，然后点击所需包进行安装。

（3）在项目中生成`.eslintrc`配置文件，配置env、rules等。里面的parser指令我们安装的`babel-eslint`.举个栗子：

``` yaml
env:
  browser: true
  es6: true

ecmaFeatures:
  modules: true

parser: "babel-eslint"
#never mind ，just example！！！
rules:
  no-empty: 2
  comma-dangle: 2
  no-native-reassign: 2
```

当上面所有步骤都完成后，就可以在SubLime Text中快乐的Coding了，并且如果代码规范和`.eslintrc`冲突的话，Sublime Text会自动用红圈高亮出错代码。

**若果上面步骤完成以后，还是不能够高亮出错代码，可以再在本地也安装eslint和babel-eslint。
