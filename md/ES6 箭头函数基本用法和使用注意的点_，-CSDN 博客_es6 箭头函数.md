> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_45466204/article/details/115004599)

> 箭头函数基本用法 ES6 允许使用 “箭头”（=>）定义函数。

### 基本用法

ES6 允许使用 “箭头”（`=>`）定义函数。

```
function fun(a){ return a; }
// ↓ 去掉 function 在 ( ) 和 { } 之间添加 => 
		var fun = (a) => { return a; }  
// ↓ 如果只有一个形参可省略 ( ) 如果一个形参都没有，必须加( )         
		var fun = a => { return a; }  
// ↓ 如果函数体只有一句话，可省略{ }，如果仅有的这句话还是 return， 则必须省略 return         
		var fun = a => a
```

### 使用注意的点

1.  箭头函数会改变函数内 `this` 的指向与上级作用域中的 this 指向保持一致。
2.  不可用当做构造函数，也就是说不能用`new`调用。
3.  不能在里边使用 `arguments` 对象，该对象在箭头函数中不存在，但还是可用使用剩余参数代替。（[剩余参数](https://blog.csdn.net/qq_45466204/article/details/115246614)）

**案例：**

```
var student = {
    sname : "LiMing",
    sage : 18,
    friends:["Jenny","Danny"],
    speak:function(){
        console.log(`${ this.sname} is ${this.sage} years old !`);
        this.friends.forEach(function(elem){
                            console.log(`${this.sname} and ${elem} are good friends ！`)
                            })
    }
}
student.speak();  
// LiMing is 18 years old !
// undefined and Jenny are good friends ！
// undefined and Danny are good friends ！
```

可以看到 `speak` 方法中 `this` 的指向没有问题，而 `forEach` 函数的回调函数中的 `this` 指向却不是我们所希望的，其实这个 `this` 指向的是全局对象 `window` 。因为回调函数是主函数在自己需要的时候自己调用的，调用时前边没有加任何 `对象.前缀` 所以 `this`指向的是 `window` （[js 中 this 的指向总结](https://blog.csdn.net/qq_45466204/article/details/108641549)）， `window` 中并没有 `sname` 属性所以是 `undefined`。

如果想要让回调函数中 `this` 指向正确，es6 之前的传统方法是使用 `bind()` 解决（[js 中 call，apply 和 bind 方法的区别和使用场景](https://blog.csdn.net/qq_45466204/article/details/111478930)）。

```
var student = {
    sname : "LiMing",
    sage : 18,
    friends:["Jenny","Danny"],
    speak:function(){
        console.log(`${ this.sname} is ${this.sage} years old !`);
        this.friends.forEach(function(elem){
                            console.log(`${this.sname} and ${elem} are good friends ！`)
                            }.bind(this))
    }
}
student.speak();  
// LiMing is 18 years old !
// LiMing and Jenny are good friends ！
// LiMing and Danny are good friends ！
```

es6 中因为箭头函数内外 `this` 指向相同，所以用箭头函数解决。

```
var student = {
    sname : "LiMing",
    sage : 18,
    friends:["Jenny","Danny"],
    speak:function(){
        console.log(`${ this.sname} is ${this.sage} years old !`);
        this.friends.forEach((elem) => {
                            console.log(`${this.sname} and ${elem} are good friends ！`)
                            })
    }
}
student.speak();  
// LiMing is 18 years old !
// LiMing and Jenny are good friends ！
// LiMing and Danny are good friends ！
```

其实以上对象中的方法 `speak` 理论也可以用箭头函数简化，但是我们并不希望这里的 `this` 于外边的相同，这里如果进行了简化请看以下代码

```
var student = {
    sname : "LiMing",
    sage : 18,
    friends:["Jenny","Danny"],
    speak:()=>{
        console.log(`${ this.sname} is ${this.sage} years old !`);
    }
}
student.speak();  
// undefined is undefined years old !
```

简化后 `speak` 方法中的 `this` 会指向全局 `window` ，而我们希望的是它指向调用时 `.` 前的对象，所以这里不能简化。