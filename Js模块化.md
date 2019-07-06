---
layout: post
title: js模块化
date: '2019-06-11'
---

## CommonJS
- 代表

Node.js（服务端）
Browserify（浏览器端）

- 模块定义
```
module.exports = xxx

或

exports.xxx1 = ...
exports.xxx2 = ...
```

- 模块引用
```
require('xxx');
```

## AMD
- 代表

Require.js

- 模块定义
```
define(function(){
  return ...
})

define(['module1','module2'], function(m1,m2){
  return ...
})
```

- 模块引用
```
require(['module1','module2'], function(m1,m2){
  ...
})
```

## CMD
- 代表

Sea.js

- 模块定义
```
define(function(require, exports, module){
  var module2 = require("./module2")
  exports.xxx = value;
  module.exports = value;
});
```

- 模块引用
```
require(function(require){
  var m1 = require('./module1')
})
```

## ES6 module
- 模块定义
```
export xxx1
export xxx2

或

export default xxx
```

- 模块引用
```
import { xxx1, xxx2 } from ... // xxx1，xxx2为导出的对象
import * as x from ... // x.xxx1，x.xxx2为导出的对象
import xxx from ... // xxx为export default导出的对象
```