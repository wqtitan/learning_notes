## 闭包

- 三行a, b, c的输出分别是什么？

```javascript
function fun(n,o){
    console.log(o);
    return {
        fun:function(m){
            return fun(m,n);
        }
    };
}
 
var a = fun(0);a.fun(1);a.fun(2);a.fun(3);
var b = fun(0).fun(1).fun(2).fun(3);
var c = fun(0).fun(1);c.fun(2);c.fun(3);
```

`fun(n,o)`返回一个对象，此对象中有一个fun方法，这个fun(m, n)中的n参数从外部fun(n,o)获取n值。

答案：

undefined 0 0 0 

undefined 0 1 2

undefined 0 1 1



- 下面的代码会输出什么？

```JavaScript
for (var i = 1; i <= 5; i++) {
   setTimeout( function timer() {
      console.log(i);
   }, 1000);
}

// 1秒后输出5个6
```

```javascript
for (var i = 1; i <= 5; i++) {
   setTimeout(function timer() {
        console.log(i);
   }, i * 1000);
}
// 每隔1秒输出1个6，共输出5个6
```

`setTimeout()`的作用是，将指定的代码移出本轮事件循环，等到下一轮事件循环，再检查是否到了指定时间。如果到了，就执行对应的代码；如果不到，就继续等待。

此for循环将5个setTimeout()事件添加到队列，循环执行完毕，i = 6，然后到下一轮事件循环，执行setTimeout()中的回调函数。

如何修改代码使控制台输出1，2，3，4，5

```JavaScript
//用闭包来解决：通过一个立即执行函数，为每次循环创建一个单独的作用域
for (var i = 1; i <= 5; i++) {
    (function (j) {
        setTimeout(function timer() {
            console.log(j);
        }, j * 1000);
    })(i);
}

//把 var 改为 let，let 每次都会创建一个块级作用域
for (let i = 1; i <= 5; i++) {
    setTimeout(function timer() {
        console.log(i);
    }, i * 1000);
}

//类似于方法二，使用setTimeout的第三个参数直接传参
for (var i = 1; i <= 5; i++) {
    setTimeout(function timer(i) {
        console.log(i);
    }, i * 1000, i);
}
```

```javascript
let i;  
for (i = 0; i < 3; i++) {  
  const log = () => {  
    console.log(i);  
  }  
  setTimeout(log, 100);  
}
// 3  3  3
// 等价于
for (var i = 0; i < 3; i++) {   
  setTimeout(function log() {  
        console.log(i);  
  } , 100);  
}
```

- 1

```javascript
function Foo() {
    getName = function () { alert (1); };
    return this;
}
Foo.getName = function () { alert (2);};
Foo.prototype.getName = function () { alert (3);};
var getName = function () { alert (4);};
function getName() { alert (5);}
 
//请写出以下输出结果：
Foo.getName();
getName();
Foo().getName();
getName();
new Foo.getName();
new Foo().getName();
new new Foo().getName();
```

