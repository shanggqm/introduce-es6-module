title: ECMAScript 6 Module
speaker: 郭美青
url: http://127.0.0.1:8888/
transition: fadeIn
files: css/main.css,css/demo.css

[slide style="background-image:url(/img/bg.jpg)"]
#ECMAScript 6 Module <span style="font-size:14px;">draft</span>
----
<br><br><br><br><br>
Meiqing Guo

[slide style="background-image:url(/img/mainBg.jpg)"]
##Guide
----
* ES5 模块化现状 
* ES6 模块化设计目标
* ES6 模块化现状
* ES6 模块化语法、优点
* ES6 模块化加载器
* ES6 模块化的构建

<!-- chapter 1 -->
[slide style="background-image:url(/img/mainBg.jpg)"]
###part 1 
##ES5 模 块 化 现 状
----
* CommonJs
* AMD


[slide style="background-image:url(/img/fw1.jpg)"]
##CommonJs
----
* Main use for `server` 
* `Synchronours` loading
* Compact syntax
<div style="margin-top:20px;"></div>

``` 
//server.js
var http = require("http");

function start(route, handlers){
	var onRequest = function(request, response){	
		//todo...
	};
	http.createServer(onRequest).listen(8888);
}

exports.start= start;
``` 
{:&.moveIn}
	

[slide style="background-image:url(/img/fw1.jpg)"]
##AMD
----
* Main use for `browser` 
* `Asynchronours` loading
* More complicated syntax
<div style="margin-top:20px;"></div>

``` 
define(function(require, exports, module){
	var calculate = require('calculate');
	var Work = function(){
		//calculate.add(1, 2);
	};
	
	exports = Work;
	module.exports.Work = Work;
	return {Work: Work};
	return  function(){};
});
``` 
{:&.moveIn}

<!-- chapter 2 -->
[slide style="background-image:url(/img/mainBg.jpg)"]
###part 2
##ES6 模 块 化 设 计 目 标
----

[slide style="background-image:url(/img/fw1.jpg)"]
<p style="font-family:'Georgia';font-size:30px;letter-spacing:0.05em;">
The `goal` for ECMAScript 6 modules was to create a format that both users of CommonJS and of AMD are `happy` with:
</p>
<br><br>
* 像CommonJs一样简单的语法, 甚至更简单 {:&.moveIn}
* 模块必须是静态的结构
* 支持模块的`异步加载`和`同步加载`, 能同时用在`server`和`client`端
* 支持模块加载的`灵活配置`
* 更好地支持模块之间的循环引用
* 拥有语言层面的支持，超越commonJs和AMD

<!-- chapter 3 --> 
[slide style="background-image:url(/img/mainBg.jpg)"]
###part 3
##ES6 草 案 模 块 化 的 现 状
----

[slide  style="background-image:url(/img/fw1.jpg)"]
##现状
----
<br><br>
* 2014年七月 TC39 敲定模块化语法的最后一些细节 {:&.moveIn}
<a href="https://github.com/rwaldron/tc39-notes/tree/master/es6/2014-07" target="_blank">TC39 metting</a> 
* 后续就是一些bug修复，命名规范的调整的工作  

<!-- chapter 4 -->
[slide style="background-image:url(/img/mainBg.jpg)"]
###part 4
##ES6 模 块 化 语 法、优 点

[slide  style="background-image:url(/img/fw1.jpg)"]
----
* 模块化`语法`（书写规范）
	* import {:&.moveIn}
	* export	
* `静态模块`设计的好处

[slide style="background-image:url(/img/fw1.jpg)"]
##模块定义
----
```javascript
// moduleA.js
var privateVar = 'I am private';

export var publicVar = 'I am public';
export function getPrivateVar(){
	return privateVar;
};
```
<br>
* 一个js文件就是一个模块 {:&.moveIn}
* 不需要AMD模块的define来营造一个闭包环境，js引擎原生支持

[slide style="background-image:url(/img/fw1.jpg)"]
##import
----
```java
import 'jquery';                        // import a module without any import bindings

import $ from 'jquery';                 // import the default export of a module

import { $ } from 'jquery';             // import a named export of a module

import { $ as jQuery } from 'jquery';   // import a named export to a different name

import * as crypto from 'crypto';    	// import an entire module instance object

```


[slide style="background-image:url(/img/fw1.jpg)"]
##named export
----
<br>
```javascript
export var myVar1 = ...;   				//普通变量
export let myVar2 = ...;   				//块级作用域变量
export const MY_CONST = ...;			//常量
	
export function myFunc() { 				//方法声明
    ...
}
export function* myGeneratorFunc() {
    ...
}
export class MyClass {					//类声明
    ...
}
```
<a href="http://127.0.0.1:8080/demo/index.html" target="_blank">Demo</a>
<a href="http://es6.ruanyifeng.com/#docs/generator" target="_blank">Generator function</a>

[slide style="background-image:url(/img/fw1.jpg)"]
##default export
----
<br>
```
export default 123;
export default function (x) {
    return x
};
export default x => x;
export default class {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
};
```

[slide style="background-image:url(/img/fw1.jpg)"]
##export once at the end
----
<br>
```javascript
const MY_CONST = ...;
function myFunc() {
    ...
}

export { MY_CONST, myFunc };
export { MY_CONST as THE_CONST, myFunc as theFunc };
```


[slide style="background-image:url(/img/fw1.jpg)"]
##Re-export
----
<br>

在当前模块中，导出其他依赖的模块的接口
<br><br>
```javascript
export * from 'src/other_module';
export { foo, bar } from 'src/other_module';

// Export other_module’s foo as myFoo
export { foo as myFoo, bar } from 'src/other_module';
```

[slide style="background-image:url(/img/fw1.jpg)"]
##两种export方式
----
* named export
* default export

<br><br>
> <br>推荐使用default export<br><br>
ECMAScript 6 favors the single/default export style, and gives the sweetest syntax to importing the default. Importing named exports can and even should be slightly less concise.<br><br>

[slide style="background-image:url(/img/fw1.jpg)"]
##Module meta data
```
import { url } from this module;
console.log(url);	
```
```
import * as metaData from this module;
console.log(metaData.url);
```
[slide style="background-image:url(/img/fw1.jpg)"]
##静态模块设计的优点 
----
* 可静态分析，无需执行代码就可以提取模块的依赖和暴露的接口 {:&.moveIn}
* 静态变量查找速度更快
* 静态语法检查
* 为未来可能添加的`宏`做准备
* 兼容未来的类型系统
* 跨语言的模块性

[slide style="background-image:url(/img/fw1.jpg)"]
##静态分析，无需执行代码
----

```
/*commonJS， 需要执行代码才能提取依赖*/
var mylib;
if (Math.random()) {
    mylib = require('foo');
} else {
    mylib = require('bar');
}
//需要执行
if (Math.random()) {
    exports.baz = ...;
}
```

[slide style="background-image:url(/img/fw1.jpg)"]
##更快的变量查找
----
commonJS
```javascript
var lib = require('lib');
lib.someFunc(); // property lookup
```

ECMAScript 6
```javascript
import * as lib from 'lib';
lib.someFunc(); // statically resolved
```

[slide style="background-image:url(/img/fw1.jpg)"]
##静态语法检查
----
全局变量、模块导入的变量、模块内部定义的变量都可以静态检查

[slide style="background-image:url(/img/fw1.jpg)"]
##对`宏(macro)`的支持
----
```javascript
macro class {
	rule {
		$className {
			constructor $cparams $cbody
			$($mname $mparams $mbody) ...
		}
	} => {
		function $className $cparams $cbody
		$($className.prototype.$mname = function $mname $mparams $mbody;) ...
	}
}

class Person {
	constructor(name){
		this.name = name;
	}
	say(msg){
		console.log(this.name + 'says: ' + msg);
	}
}
var bob = new Person("Bob");
bob.say('Macro are sweet!');
```
[slide style="background-image:url(/img/fw1.jpg)"]
##Micro
----
要支持在模块中import宏，模块必须是静态结构才行
<br><br>

```
//macroA
macro class .....

//app.js
import {class} from 'macroA';

class Person {
	//...
}
```

<ul style="font-size:16px;text-align:left;">
	<li><a href="http://sweetjs.org/" target="_blank" style="color:blue;text-decoration:underline;">[1].sweet.js</a></li>
	<li><a href="http://jlongster.com/Stop-Writing-JavaScript-Compilers--Make-Macros-Instead" target="_blank" style="color:blue;text-decoration:underline;">[2].stop writing javascript compiles, make macros instead</a></li>
</ul>
[slide style="background-image:url(/img/fw1.jpg)"]
##兼容未来的类型系统
----
javascript成为强类型语言？编译器要做类型检查？Oh my God！
<br>
<br>
* 强类型语言可以做类型检查，编译器可以更好地优化性能 {:&.moveIn}
* 只有静态导入才能进行静态检查

[slide style="background-image:url(/img/fw1.jpg)"]
##跨语言支持
----
* Javascript未来要支持程序员用不同语言写出来的代码，并编译为浏览器可执行的JS
* 借助macro和静态类型，可以实现上述描绘的场景
<br>
<br>
<ul style="font-size:16px;text-align:left;">
	<li><a href="https://github.com/clojure/clojurescript" target="_blank" style="color:blue;text-decoration:underline;">[1].clojurescript</a></li>
	<li><a href="http://roy.brianmckenna.org/" target="_blank" style="color:blue;text-decoration:underline;">[2].Roy语言</a></li>
	<li><a href="https://developers.google.com/closure/compiler/" target="_blank" style="color:blue;text-decoration:underline;">[3].Closure Compiler</a></li>
</ul>

<!-- chapter 5 -->
[slide style="background-image:url(/img/mainBg.jpg)"]
###part 5
##模 块 化 加 载 器

[slide style="background-image:url(/img/fw1.jpg)"]
##加载单个模块
----

```
System.import('some_module')
.then(some_module => {
    // Use some_module
})
.catch(error => {
    ...
});
```
<br>
* 可以在页面的script标签中使用
* 按条件加载不同的模块


[slide style="background-image:url(/img/fw1.jpg)"]
##加载多个模块
----
```
Promise.all(
    ['module1', 'module2', 'module3']
    .map(x => System.import(x)))
.then(([module1, module2, module3]) => {
    // Use module1, module2, module3
});
```

[slide style="background-image:url(/img/fw1.jpg)"]
##其他API
----
| API    | desc     |
| ------ | -------- |
| System.module(source, options?) |  evaluates the JavaScript code in source to a module (which is delivered asynchronously via a promise) |
| System.set(name, module) |  is for registering a module (e.g. one you have created via System.module()) |
|System.define(name, source, options) | both evaluates the module code in source and registers the result. |

[slide style="background-image:url(/img/fw1.jpg)"]
##加载器配置
----
* 尚未最终定稿
* 正在完成了第一个浏览器端加载器的实现和测试
* 允许用户配置加载的时候做Lint via （jsList、jsHint）
* 是否自动编译不同语言编写的模块（比如CoffeScript、TypeScript）
* 使用支持AMD、CommonJS
* ...

[slide style="background-image:url(/img/fw1.jpg)"]
##如何在HTML页面嵌入模块
----
* script标签天生同步，不支持模块语法
* 新增<module>标签支持嵌入式模块
```javascript
<module>
    import $ from 'lib/jquery';
    var x = 123;

    // The current scope is not global
    console.log('$' in window); // false
    console.log('x' in window); // false

    // `this` still refers to the global object
    console.log(this === window); // true
</module>
```
```html
<!-- 导入外部模块 -->
<module import="impl/main"><module>
```
```html
<!-- 旧的浏览器兼容写法 -->
<script type="module">
```

[slide style="background-image:url(/img/fw1.jpg)"]
##一些变化
----
* 模块化标准一统天下
* 浏览器内建API也会被当成模块，按照标准方式引入，不再以全局变量的方式出现
* Math、JSON、navigator等等


<!-- chapter 6 -->
[slide style="background-image:url(/img/mainBg.jpg)"]
###part 6
##模 块 化 构 建


[slide style="background-image:url(/img/fw1.jpg)"]
##构建问题
----
* ES6代码如何合并压缩？
* 压缩完如何寻址模块？
* ...


<div style="font-size:80px;color:red;font-weight:bold;padding:20px 0;">尚无定论</div>{:&.moveIn}

[slide]
##怎么体验ES6
----

```
git clone https://github.com/ModuleLoader/es6-module-loader.git
//安装依赖包
npm install

//其他库
//宏
npm install -g sweet.js 
//6到5的转换
npm install -g 6to5

//http server
npm install -g http-server
http-server e:/demo 

```

[slide style="background-image:url(/img/happy.jpg)"]


