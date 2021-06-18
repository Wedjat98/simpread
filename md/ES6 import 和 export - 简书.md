> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.jianshu.com](https://www.jianshu.com/p/d9be7c410e10)

export
======

在 es6 中一个文件可以默认为一个模块，模块通过 export 向外暴露接口，实现模块间交互等功能

##### 1. export 相关语法

exportDemo.js 文件

```
export var m = 1;
// 等价于
var m = 1;
export { m }

// 导出多个
var a = 1;
var b = 2;
var c = 3;
export { a, b, c }
// 导出对象
export const student = {
  name: 'Megan',
  age: 18
}
// 导出函数
export function add(a, b) {
  return a + b;
}
```

错误写法 1

```
var k = 1;
export k;
```

对应正确写法

```
export { k }
```

错误写法 2

```
const obj = {
  id: 1,
  value: 'lalala'
};
export obj;
```

对应正确写法

```
export { obj };
```

错误写法 3

```
function sum(a, b){
  return a + b;
}
export sum;
```

对应正确写法

```
export { sum };
```

导出接口别名

```
const person = {
  name: '张呆',
  age: 18,
  gender: "male"
}
export { person as boy }
```

##### 2. export default

exportDemo.js

```
//一个文件即模块中只能存在一个export default语句，导出一个当前模块的默认对外接口
export default var i = 0;
```

##### 3. import

以上述 exportDemo.js 为例，在 importDemo.js 中利用 import 进行导入

```
import variable from './exportDemo';
console.log(variable); // 0;
```

代码解释：上述 import 语句仅仅导入了 exportDemo.js 文件中定义为默认的接口即`i = 0`, 所以打印输出 0

```
import variable, { sum, boy } from './exportDemo';
console.log(variable); // 0;
console.log(sum);// function(a, b) { return a + b; }
console.log(boy); // { name: '张呆', age: 18, gender: male}
```

##### 4. 相关注意

> import 和 export 只能出现在模块的最外层（顶层）结构中，否则报错

> 由于 es6 模块是静态加载的，因此 import 和 export 不能出现在判断等动态语句中

##### 5. ES6 Module

**_es6 module 中模块加载方式是静态加载，因此 import 和 export 不能出现在判断等动态语句_**  
**_采用 import 获取的是模块接口的引用，当模块内部发生改变是，import 出的接口也会对应改变_**【与 commonjs 规范不同，commonjs 中获得的是接口运行结果的缓存】  
**_es6 module 内部自动采用严格模式_**  
对于 es6 module 的其他详细内容参考阮一峰老师的　[ES6 Module](http://es6.ruanyifeng.com/#docs/module)

##### 6. commonjs 规范

commonJS 规范规定，每一个模块内部，module 变量代表当前模块，这个变量是一个对象，它的 exports 属性（即 module.exports）是对外的接口，加载某个模块其实就是加载 module.exports 属性

```
// test.js
var x = 5;
var add = function(a, b){
  return a + b;
}
module.exports = {x, add}
```

在另一个文件中加载 test.js

```
var test = require('./test.js');
console.log(test.x); // 5
console.log(test.add(1, 2)); // 3
```

**使用 import 语法加载 commonjs 模块**

```
// commonjsDemo.js
const textConst = {
  COPY: "复制",
  RUN: "运行"
}
module.exports textConst
```

```
// importCommonjsDemo.js
import { COPY, RUN } from './commonjsDemo'
console.log(COPY); // 复制
console.log(RUN); // 运行
```

###### 6.1 module 对象

Node 中提供了一个 Module 构建函数，所有模块都是 Module 的实例

```
function Module(...){
  this.id = id;
 this.exports = {};
 this.parent = parent;
 // ....
}
```

每个模块内部，都有一个 module 对象，表示当前模块，具有以下属性

> module.id　模块的识别符，通常是带有绝对路径的模块文件名  
> module.filename 模块的文件名，带有绝对路径  
> module.loaded 返回一个布尔值，表示模块是否已经加载完成  
> module.parent　返回一个对象，表示调用该模块的模块  
> module.children 返回一个数组，表示该模块要用到的其他模块  
> module.exports　表示模块对外输出的值

**_module.exports_**  
　　该属性表示当前模块对外的接口，其他模块加载时，实际上是读取 module.exports 变量  
**_exports_**  
　为了方便，node 为每个模块提供了一个 exports 变量，指向 module.exports。这等同于在每个模块的头部，存在这样一行命令  
`var exports = module.exports;`  
　这样做的结果就是在输出接口的同时，可以向 exports 对象上添加方法；需要注意的是不要将 exports 重新指向别的值，否则将会切断 exports 和 module.exports 之间的联系

###### 6.2 require 命令

基本功能是读入并执行一个 javascript 文件，并返回该模块的 exports 对象，如果没有指定模块路径，则报错  
**_require 的加载规则_**  
后缀默认为. js  
当 nodejs 中遇到 require(X) 时，按照下面的顺序进行处理

> 如果 X 为内置模块，返回该模块，不再继续执行  
> 如果 X 以 "./" 或者 "/" 或者 "../" 开头  
> 　根据Ｘ所在父模块确定Ｘ的绝对路径  
> 　将Ｘ当成文件，依次查找下面的问题件，只要其中一个存在，则返回存在的文件，不再继续执行  
> 　　X  
> 　　X.js

X.json  
　　X.node  
　将Ｘ当成目录，依次查找下面文件，只要其中一个存在，就返回该文件，不再继续执行  
　　X/package.json  
　　X/index.js  
　　X/index.json  
　　X/index.node  
如果 X 不带路径  
　根据Ｘ所在的父模块，确定Ｘ可能的安装目录  
　依次在每个目录中，将Ｘ当做文件名或者目录名加载  
抛出 "not found"

参考：[http://www.ruanyifeng.com/blog/2015/05/require.html](http://www.ruanyifeng.com/blog/2015/05/require.html)  
参考：[http://javascript.ruanyifeng.com/nodejs/module.html](http://javascript.ruanyifeng.com/nodejs/module.html)  
**本文目的仅仅是为了个人查找阅读等提供方便**