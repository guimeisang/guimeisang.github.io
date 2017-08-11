---
title: require和import的理解
date: 2017-08-11 17:39:12
tags: ES6 require import 模块
---

**本文作者：guimeisang，未经同意，禁止转载**
 
> 以时间为维度，分为ES6之前和ES6之后。其实为了实现javascript的工程化，才引出模块化这个概念，require，import就是为实现这个而生。

<!-- more -->


## 在ES6之前
在ES6之前引入了模块的概念，主要是为了适应大型应用的。node的module遵守CommonJS规范。而在浏览器端，requirejs和seajs之类的工具包也出现了。所以说在node端，浏览器端。`require + module.exports `基本实现了所有模块化的编程。  

node的module遵循CommonJS规范，requirejs遵循AMD，seajs遵循CMD，虽然各有不同，但代码风格相似。 下面是模块导出端：   

```js
// demo1.js

// ---- node ----

module.exports = {
    a : function() {},
    b : 'foo'
}

// ----- 浏览器 AMD 和 CMD -----

define(function(require, exports, module){
    module.exports = {
        a : function() {},
        b : 'foo'
    };
});

// ------ UMD ------
define(function(){
    return {}; // return 的值就是导出的模块 
});
```
可以看出来，为了代码风格相似，**浏览器端的模块中要使用一个define函数来提供模块的闭包以外，其他代码可以完全一致。** 下面是导入模块：

```js
// demo2.js

// ---- node -----
var m = require('./demo1');
m.a();

// ---- AMD or CMD ----
defined(function(require, exports, module){
    var m = require('./a');
    m.a();
})
```

node发布的时候，就天生具备module，node相当于促进了js世界里面的模块化编程。  

注意：exports是module.exports的“别名”，即exports是module.exports地址的引用。


## ES6发布之后
ES6没有把require写入标准，所以其还是一个普通的函数，ES6使用import指令引入模块或模块中的部分接口。而导出的话，ES6标准中，用export指令，也就是说，module.exports只是node，requirejs等模块化库中的自定义变量，而非ES标准接口。

#### 模块导出
下面是导出模块的部分：  
```js
export function a(){};
export { name1, name2, ..., nameN };
export { variable1 as name1, variable2 as name2, ..., nameN };
export let name1, name2, ..., nameN;

export default expression;
export default function(...){...};

export * from ...;
export { name1, name2, ..., nameN } from ...;
export { import1 as name1, ..., nameN } from ...;

```
ES6里面，直接把要导出的变量，函数，对象，类等前面加上一个export关键词，错误演示：  

```js
// 错误演示
export 1; 
var a = 1;
export a;
function b() {};
export b;
```

这种直接内容，而不是导出变量绝对是错误的，或者说导出表达式也是错误的。导出接口仅限两种：
* 声明时导出；
* 以对象的形式导出（和解构联系起来）；


如果要导出某个变量，可以如下导出：  

```
var a = 1;
export {a}; // 解构，相当于{a: a}
function b(){};
export {b};
```

**敲黑板，划重点：** ES6最厉害的地方在于，可以在export变量之后，继续修改变量。

```
export cosnt a = 0;
a = 1;

// 在import之后，发现a的值为1，这个是CommonJS不能做到的。
```

#### 模块导入
> 和node之前的require不一样，require只能把模块放到一个变量中，而在ES6中，拥有对象解构赋值的能力，所以直接就把引入的模块接口赋值给变量了。内在机理也不同，require需要执行整个模块，就是将整个模块放到内存中（也就是我们说的在运行时），如果只是用到里面一个方法，性能上就差很多，而import...from，则只需要在编译的时候将需要的接口方法加载，其他的方法在程序启动之后根本触及不到，所以被称为在编译时，性能上有很大的改善。  

##### as 关键词

简单来说就是一个别名。export和import都可以用。  

```
// demo1.js

var a = function() {};  
export { a as fun }

// demo2.js

import { fun as a } from './demo1';
a();

```
上面代码中，demo1.js中，以fun代替a这个函数，所以在外面的模块中只能识别到fun，识别不到a。而在demo2.js中，引入fun方法，但是我们用a去代替它。（这有个好处就是，比如demo3.js也导出一个fun的接口，这个时候这个别名就很有用。可以解决这种接口重名的问题）


##### default 关键词

简单来说就是别名的语法糖。

```
// demo1.js

export default function() {}
// 等效于
function a() {}
export {a as default}

```
在导入的时候：

```
import a from './demo1';
// 等效于
import {default as a} from './d';
```
##### * 符号

简单来说，*代表所有，看下面的两个例子：  

```
import * as foo from '_';
```

意思是：将'_'模块中所有接口挂载在foo对象上，所以可以用underscore.each调用某个接口。

```
export * from '_';

// 等效于
import * as all from '_';
export all;
```


## 怎么用require和import这两兄弟？

总体感觉，既然ES6将import加入到标准了，这个时候一般还是乖乖的用import的吧。虽然说现在引擎还不能支持import，依然是用bable神器解析成reqiure，所以现在你会发现有些之前的CommonJS模块，依然可以用import引用。 

```
// demo1.js
module.exports = {};

// demo2.js
import a from './demo1';

```

这种混搭不是很好，但是可以用。

不过现在不是所有的环境都支持import，所以可以使用babel让node支持ES6，但是浏览器端，则毫无办法，可能暂时用require。但是很遗憾你ES6并不兼容require，所以到时候你必须要升级代码。   

import只能在文件开头使用，在import之前，你不能有其他的代码，这和其他语言是一样的。但是require则不同，它相当于node的一个定义在全局的函数，你可以在任意地方使用它，甚至使用变量表达式作为它的参数，这样有一个好处，就是可以在循环中加载模块。   

最后说一句，js引擎们很快就会实现ES6标准规定，如果时一个引擎标准都没办法实现，就会被淘汰，所以尽在去部署import，你未来可能改的代码越少。
