## JavaScript函数声明与函数表达式
### 如何定义一个函数
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在JavaScript里有两种定义函数的方法

 1. 函数声明
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;function 函数名称 (参数：可选){ 函数体 }
 2. 函数表达式
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;function 函数名称（可选）(参数：可选){ 函数体 }

### 常见的函数定义以及所属的定义方法
- function foo(){} 函数声明
- var bar = function foo(){}; 函数表达式
- new function bar(){}; 函数表达式
- function foo(){ function bar(){} 函数声明}
- (function(){})() 函数表达式
- +function(){}() 函数表达式
- !function(){}() 函数表达式
- ;(function(){})() 函数表达式，分号反正前面没加分号，解析错误

### 函数声明与函数表达式的一些细微的不同
在JavaScript里函数声明会有一个hoist的过程，也就是说在函数执行的之前，函数体就已经被解析了。一个典型的例子
```javascript
if (true) {
  function foo() {
    return 'first';
  }
}
else {
  function foo() {
    return 'second';
  }
}
foo();
```
正常情况下，得到的结果是 second 
而 
```javascript
var foo;
if (true) {
  foo = function() {
    return 'first';
  };
}
else {
  foo = function() {
    return 'second';
  };
}
foo();
```
我们能得到想要的结果

> http://www.nowamagic.net/librarys/veda/detail/1630