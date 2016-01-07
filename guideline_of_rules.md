/\*所有规则以官网文档为准，下面的guideline仅做简要解释，仅包括规则的主要作用*/

//在定义对象的时候，getter/setter需要同时出现

"accessor-pairs": 2, 

//箭头函数中的箭头前后需要留空格

"arrow-spacing": [2, { "before": true, "after": true }], 

//如果代码块是单行的时候，代码块内部前后需要留一个空格

"block-spacing": [2, "always"],

//大括号语法采用『1tbs』,允许单行样式

"brace-style": [2, "1tbs", { "allowSingleLine": true }],

//在定义对象或数组时，最后一项不能加逗号

"comma-dangle": [2, "never"],

//在写逗号时，逗号前面不需要加空格，而逗号后面需要添加空格

"comma-spacing": [2, { "before": false, "after": true }],

//如果逗号可以放在行首或行尾时，那么请放在行尾

"comma-style": [2, "last"],

//在constructor函数中，如果classes是继承其他class，那么请使用super。否者不适用super

"constructor-super": 2,

//在if-else语句中，如果if或else语句后面是多行，那么必须加大括号。如果是单行就应该省略大括号。

"curly": [2, "multi-line"],

//该规则规定了`.`应该放置的位置，

"dot-location": [2, "property"],

//

"eol-last": 2,

//

"eqeqeq": [2, "allow-null"],

//

"generator-star-spacing": [2, { "before": true, "after": true }],

//

"handle-callback-err": [2, "^(err|error)$" ],

//

"indent": [2, "tab", { "SwitchCase": 1 }],

//

"key-spacing": [2, { "beforeColon": false, "afterColon": true }],

//

"new-cap": [2, { "newIsCap": true, "capIsNew": false }],

//

"new-parens": 2,

//

"no-array-constructor": 2,

//

"no-caller": 2,

//

"no-class-assign": 2,

//

"no-cond-assign": 2,

//

"no-const-assign": 2,

//

"no-control-regex": 2,

//

"no-debugger": 2,

//

"no-delete-var": 2,

//

"no-dupe-args": 2,

//

"no-dupe-class-members": 2,

//

"no-dupe-keys": 2,

//

"no-duplicate-case": 2,

//

"no-empty-character-class": 2,

//

"no-empty-label": 2,

//

"no-eval": 2,

//

"no-ex-assign": 2,

//

"no-extend-native": 2,

//

"no-extra-bind": 2,

//

"no-extra-boolean-cast": 2,

//

"no-extra-parens": [2, "functions"],

//

"no-fallthrough": 2,

//

"no-floating-decimal": 2,

//

"no-func-assign": 2,

//

"no-implied-eval": 2,

//

"no-inner-declarations": [2, "functions"],

//

"no-invalid-regexp": 2,

//

"no-irregular-whitespace": 2,

//

"no-iterator": 2,

//

"no-label-var": 2,

//

"no-labels": 2,

//

"no-lone-blocks": 2,

//

"no-mixed-spaces-and-tabs": 2,

//

"no-multi-spaces": 2,

//

"no-multi-str": 2,

//

"no-multiple-empty-lines": [2, { "max": 1 }],

//

"no-native-reassign": 2,

//

"no-negated-in-lhs": 2,

//

"no-new": 2,

//

"no-new-func": 2,

//

"no-new-object": 2,

//

"no-new-require": 2,

//

"no-new-wrappers": 2,

//

"no-obj-calls": 2,

//

"no-octal": 2,

//

"no-octal-escape": 2,

//

"no-proto": 2,

//

"no-redeclare": 2,

//

"no-regex-spaces": 2,

//

"no-return-assign": 2,

//

"no-self-compare": 2,

//

"no-sequences": 2,

//

"no-shadow-restricted-names": 2,

//

"no-spaced-func": 2,

//

"no-sparse-arrays": 2,

//

"no-this-before-super": 2,

//

"no-throw-literal": 2,

//

"no-trailing-spaces": 2,

//

"no-undef": 2,

//

"no-undef-init": 2,

//

"no-unexpected-multiline": 2,

//

"no-unneeded-ternary": [2, { "defaultAssignment": false }],

//

"no-unreachable": 2,

//

"no-unused-vars": [2, { "vars": "all", "args": "none" }],

//

"no-useless-call": 2,

//

"no-with": 2,

//

"one-var": [2, { "initialized": "never" }],

//

"operator-linebreak": [2, "after", { "overrides": { "?": "before", ":": "before" } }],

//

"padded-blocks": [2, "never"],

//

"quotes": [2, "single", "avoid-escape"],

//

"radix": 2,

//

"semi": [2, "never"],

//

"semi-spacing": [2, { "before": false, "after": true }],

//

"space-after-keywords": [2, "always"],

//

"space-before-blocks": [2, "always"],

//

"space-before-function-paren": [2, "always"],

//

"space-before-keywords": [2, "always"],

//

"space-in-parens": [2, "always"],

//

"space-infix-ops": 2,

//

"space-return-throw-case": 2,

//

"space-unary-ops": [2, { "words": true, "nonwords": false }],

//

"spaced-comment": [2, "always", { "markers": ["global", "globals", "eslint", "eslint-disable", 

"*package", "!", ","] }],

//

"use-isnan": 2,

//

"valid-typeof": 2,

//

"wrap-iife": [2, "any"],

//

"yoda": [2, "never"],

//

"standard/object-curly-even-spacing": [2, "either"],

//

"standard/array-bracket-even-spacing": [2, "either"],

//

"standard/computed-property-even-spacing": [2, "even"]