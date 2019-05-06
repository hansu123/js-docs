# 手写一个promise

今天无聊参考了掘金一些大牛博客，手动写了写promise，觉得受益匪浅

先来说概念：Promise 对象用于表示一个异步操作的最终状态（完成或失败），以及该异步操作的结果值。

具体流程如下:

![](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/%E9%9D%A2%E8%AF%95/promises.png)

了解这个图对下面理解会更深刻

### 一.基本逻辑

1.promise是一个构造函数

2.实例化的时候需要传递一个函数，这个函数带有两个形参

3.这两个形参是两个函数，可以理解为promise的静态方法。分别代表请求成功和错误。请求成功触发resolve()，请求失败触发reject().

4.如何判断成功触发，当状态从pending转变为fulfilled的时候就意味着可以触发resolve(),失败反之。

根据以上先实现基本逻辑。

```javascript
class Promise{

constructor(fn){
this.status="pending";
this.data=null;
this.reason=reason;
let _self=this;
function resolve(data){
if(_self.status=="pending"){
_self.status=="resolve";  
}
};

function reject(reason){
if(_self.status=="pending")
{console.log(_self.reason);}
}
try{
fn(resolve,reject);  
}
catch(err)(
this.reason=err;
)  
}}

let promise1=new Promise((resolve,reject)=>{
console.log("ok");
})
```

以上就是基本逻辑实现，很简单，主要有几个注意点：

* promise的状态从pending->resolve或者reject之后就无法更改了，所以在执行resolve和reject时要先判断当前的状态
* 实例化传入的是一个函数，两个形参分别对应两个静态方法，传入后在promise中立即执行
* 实例化对象传入的函数中的函数体先执行（同步任务）

### 二.实现then

经常使用我们都知道then是promise实例化对象的方法。也就是自身实例方法，进一步讲就是原型对象上的方法。明白这个就很好实现了。(以下是es6语法添加到类中即可)

```javascript
then(fn){
if(this.status=="resolve"){
fn(this.data);  
}}
```

以上就是基本逻辑实现，很简单，主要有几个注意点：

* then是请求成功时才执行，所以要判断当前状态。
* then中的函数一般会传递resolve抛出的参数，这里用了设置好的一个实例属性模拟数据。

### 三.模拟异步

我们知道通常情况下，promise是解决异步函数的比如：

```javascript
class Promise{

constructor(fn){
this.status="pending";
this.data=null;
this.reason=reason;
let _self=this;
function resolve(data){
if(_self.status=="pending")
{ 
_self.status=="resolve";
_self.data=data;
}
};

function reject(reason){
if(_self.status=="pending")
{
  console.log(_self.reason);}
}
try{
fn(resolve,reject);  
}
catch(err)(
this.reason=err;
)  
}
then(fn){
if(this.status=="resolve"){
fn(this.data);  
}}
}

let promise1=new Promise((resolve,reject)=>{
setTimeout(()=>{
resolve("data")
},1000); //异步定时器
})

promise1.then((result)=>{
console.log(result);  
})
```

以上执行顺序是：

实例化对象的时候执行函数->触发resolve->遇到异步任务暂时先不管扔到一边->执行then,这个时候then懵逼了，then心里想的是我草状态是pending你让我执行啥，于是无功而返->计时器时间到了，再执行resolve，此时状态才从pending改为resolve,但为时已晚，then已经离我而去。

解决方案：既然异步执行到then它是pending状态，那我们可以将其先保存起来，等到计时器时间到了我们再在resolve中执行不就ok了吗？

```javascript
class Promise{

constructor(fn){
this.status="pending";
this.data=null;
this.reason=reason;
this.resolveFuns=[];
this.rejectFuns=[];
let _self=this;
function resolve(data){
if(_self.status=="pending")
{ 
_self.status=="resolve";
_self.data=data;
if(!_self.resolveFuns){
_self.resolveFuns.forEach((fun)=>{fun(_self.data)})  
}
}
};

function reject(reason){
if(_self.status=="pending")
{
  console.log(_self.reason);}
}
try{
fn(resolve,reject);  
}
catch(err)(
this.reason=err;
)  
}
then(fn){
if(this.status=="resolve"){
fn(this.data);  
}}
if(this.status=="pending"){
this.resolveFuns.push(fn);
}}  
}

let promise1=new Promise((resolve,reject)=>{
setTimeout(()=>{
resolve("data")
},1000); //异步定时器
})

promise1.then((result)=>{
console.log(result);  
})
```

以上执行顺序是：

实例化对象的时候执行函数->触发resolve->遇到异步任务暂时先不管扔到一边->执行then,这个时候发现状态是pending就把它先放到异步函数数组中->计时器时间到了，再执行resolve，此时状态才从pending改为resolve,判断异步函数数组是否为空，不为空就挨个执行。完事

这里大家可能会有点懵逼，其实只要记得一点就是，按照promise制定的规则来。

还有就是我建议大家可以将自己以前写的promise拿来做对比。这样效果会更好。亲测有效。

### 四.链式调用

.then().then().......

