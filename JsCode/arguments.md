# arguments

> 函数体内的一个局部变量，包含传递给函数的每一个参数

先看几个例子

```js
function sum(a,b){
console.log(a+b);
};

function pay(a,b,c){
console.log(a+b+c);
};

pay(1,2);//NaN
pay(1,2,3);//6

```

可以看出js并没有函数重载功能，因为js是解释型语言，函数名称相同的后者将会覆盖前者。为了实现函数重载js引入了arguments。


1.arguments的属性：

![](img/arguments.jpg)

arguments是一个类数组对象，可以理解为是数组的亲兄弟。当我们调用函数时，函数内部自动把我们传入的实参放在arguments中,第一个参数属性名为'0',第二个参数属性名为'1'....依次类推。

callee:这个属性代表了函数自身和我们定义的函数一模一样，用的不多，可以用来进行递归运算。

length:类数组的长度

`__proto__`这个很熟悉了,由此可以看出他是Object的一个实例，不是数组的实例。

于是我们可以用arguments轻松完成重载：


2.arguments的方法：

arguments是类数组对象，除了length属性和索引元素之外没有任何Array属性。
我们可以用arguments.length表示对象长度，我们可以用arguments[index]来表示索引元素。

于是一开始的重载我们可以这么写：

```js
function sum(){
for(var i=0,sum=0;i<arguments.length;i++)
{sum+=i;}
console.log(sum);
}
sum(1,2);
sum(1,2,3);
```

如何使用数组方法：

arguments既然是类数组，那我们只要把它转化为数组就可以用数组的方法了。

一般我们会想到的方法：

```js
var arr=[];
for(var i=0;i<arguments.length;i++){
arr.push(arguments[i]);
}
//arr就是转化后的数组
```

我们再来看看其他方法：

var args = Array.prototype.slice.call(arguments);

var args = [].slice.call(arguments);

这个怎么理解呢？

我们先来理解第一个：

1.var args = Array.prototype.slice.call(arguments);

我们不妨自己模拟写一个slice方法

```js
Array.prototype.slice=function(s,e){
var arr2=[];
for(var i=s;i<e;i++)
{
arr2.push(this[i]);
}
return arr2;
};

```

怎么调用呢？

```js
以前我们都用这种方式：
var arr1=[1,2,3];
console.log(arr1.slice(0,1));

```

那跟我们讲的那两种方式有什么关系呢？

来，继续，我们再看一个call的基本用法

```js
function fn(){
console.log(this);
console.log(this.a);
};
var c={a:1};
fn.call(c);//1
```

这下明白了吧！

```js
fn=>Array.prototype.slice;
fn.call(c)=>Array.prototype.slice.call(arguments);
```

第二个怎么理解呢？

var args = [].slice.call(arguments);

slice的原理我们已经知道了。

我们再看一个call的用法：

```js
var lilei={name:"lilei",sing:function(){
console.log(this.name);}}
var hanmeimei={name:"hanmeimei"};
lilei.sing.call(hanmeimei);//hanmeimei
```

看到这里大家应该明白了吧！

```js
lilei=>[];
hanmeimei=>arguments;
lilei.sing.call(hanmeimei)=>[].slice.call(arguments);
```
另外es6也提供了两种方法：
*	Array.from(arguments) Array.from()
*	[...arguments] 扩展运算符

```js
function fn(){
    console.log(arguments);
    console.log(Array.from(arguments));
    console.log([...arguments]);
}
fn(10,20,40)
```


3.注意

*	箭头函数内部没有arguments
```js
try{
var fn=()=>{console.log(arguments.length)};
fn(1,2,3,4);}
catch(err){console.log("error")}//error
var fn2=function(){console.log(arguments.length)}
fn2(1,2,3,4);//4
```

*	arguments只在函数内部使用
```js
console.log(typeof arguments[0])
```

*	是否会跟踪参数的值
*	(非严格模式)跟踪
```js
function fn(a){
    console.log(arguments[0]);
    a=9;
    console.log(arguments[0]);
}
fn(10)//10,9
```
*	(严格模式)不跟踪
```js
"use strict";
function fn(a){
    console.log(arguments[0]);
    a=9;
    console.log(arguments[0]);
}
fn(10)//10,10
```