### 三. bind的作用

#### 1. 永久绑定

```javascript
function Score(math,English,chinese){
console.log(`${this.name}总得分为:${math+English+chinese}分`);
};

let myScore=Score.bind({name:"lilei"});
myScore(80,90,100);//lilei总得分为270
myScore(70,90,80);//lilei总得分为240
```

#### 2. 拷贝构造函数

因为bind绑定返回的是一个函数，call和apply是直接调用函数，所以可以实现拷贝函数的功能。

```javascript
function Klass(name,age){
    this.name=name;
    this.age=age;
}
var Student=Kclass.bind({});
var hanmeimei=new Student("hanmeimei");
console.log(hanmeimei.name);//hanmeimei
console.log(hanmeimei instanceof Klass);//true
```

这里的Student成为bound Klass

#### 3. 修改匿名函数this的指向

ex1:

```javascript
Array.prototype.multiply=function(){  
this.forEach(function(elem,index){//this->arr
this.push(elem*elem);  
}.bind(this));//绑定this->arr,如果不绑定会指向window
return this;
};
var arr=[1,2,3,4];
console.log(arr.multiply());//[1,2,3,4,1,4,9,16]
```

ex2:

```javascript
var user = {
  name: 'hansu',
  sing: function(){
    console.log(this.name);
    return function(){console.log(this.name);
  }.bind(this);
}}
var f=user.sing();
f();//hansu hansu
```

当我们想要永久修改一个函数的this指向，且该函数还未调用时可以用bind

### 四. 区别

|          |      call       |    apply     |      bind       |
| :------: | :-------------: | :----------: | :-------------: |
| 传入参数 | (对象,单个参数) | (对象,数组)  | (对象,单个参数) |
| 绑定时效 |      一次       |     一次     |      永久       |
|  返回值  |  函数执行结果   | 函数执行结果 |      函数       |
|   用法   |   函数.call()   | 函数.apply() |   函数.bind()   |

### 五. 原理

bind返回新函数

> 普通绑定：返回新函数，新函数内部是一个call调用
>
> 构造函数：返回新函数，新函数内部通过判断this的原型来决定调用call的参数，最后还要调整this的原型使其指向

#### 1. 普通绑定

```js
{
  let fn = function (name) {
    console.log(name)
    console.log(this.age)
  }
  let obj = {
    age: 11
  }
  fn.bind(obj)
  fn("lilei")
}

//bind会返回一个函数

{
  Function.prototype.HCall=function(context,...args){
    context=context?context:window
    context.fn=this
    var result=context.fn(...args)
    delete context.fn
    return result
  }
  Function.prototype.HBind = function (context,...args1) {
    let _this=this
    return function(...args2){
      _this.HCall(context,...args1.concat(args2))
    }
  }
  let fn = function (name,sex) {
    console.log(name)
    console.log(sex===1?"男":"女")
    console.log(this.age)
  }
  let obj = {
    age: 11
  }
  fn.HBind(obj,"lilei")(1)
}
```

#### 2.bind构造函数

```js
{
  let fn = function () {
    console.log(this.age)
  }
  let obj = {
    age: 11
  }
  let fn2=fn.bind(obj)
  let lilei=new fn2() //undefined
  console.log(lilei instanceof fn) //true
}
```

```js
//但是按照我们写的方式
Function.prototype.HCall=function(context,...args){
    context=context?context:window
    context.fn=this //this指代调用的fn
    var result=context.fn(...args)
    delete context.fn
    return result
}
Function.prototype.HBind = function (context) {
    let _this=this
    return function(){
      _this.HCall(context)
    }
}
let fn = function () {
    console.log(this.age)
}
let obj = {
    age: 11
}
let fn2=fn.HBind(obj)
let lilei=new fn2() //11
console.log(lilei instanceof fn) //false
//所以很明显错了，因为new的同时this的指向应该发生改变，应该指向lilei
```

> 1.0

```js
Function.prototype.HCall=function(context,...args){
    context=context?context:window
    context.fn=this //this指代调用的fn
    var result=context.fn(...args)
    delete context.fn
    return result
}

Function.prototype.HBind = function (context) {
    let _this=this
    return function(){
      console.log(this) //此时打印的是{}，指的是lilei这个实例，如果是普通调用这里的this指向window
      console.log(this instanceof fn2) //true
      _this.HCall(context)
    }
}
let fn = function () {
    console.log(this.age)
}
let obj = {
    age: 11
}
let fn2=fn.HBind(obj)
let lilei=new fn2() //11
console.log(lilei instanceof fn) //false

//到这里我们应该一目了然了，所以我们传入的context应该是lilei这个实例，而不是obj
```

> 2.0

```js
Function.prototype.HCall=function(context,...args){
    context=context?context:window
    context.fn=this //this指代调用的fn
    var result=context.fn(...args)
    delete context.fn
    return result
}
Function.prototype.HBind = function (context,...args1) {
    let _this=this
    let fn3=function (...args2){
      //这里的判断我们无法得知外部的函数名，所以只好在内部设置一个变量，这个变量和下文的fn2是等价的，所以只要判断this是否是fn3的实例就可以知道是不是new操作
      _this.HCall(this instanceof fn3?this:context,...args1.concat(args2))
    }
    return fn3
}
let fn = function () {
    this.name="lilei"
    console.log(this.age)
}
let obj = {
    age: 11
}
let fn2=fn.HBind(obj)
let lilei=new fn2() //11
```

> 3.0

最后new有一个很重要的步骤就是lilei的`_proto_`需要指向fn的prototype，另外还需要判断调用bind是否是一个函数

```js
Function.prototype.HCall=function(context,...args){
    context=context?context:window
    context.fn=this //this指代调用的fn
    var result=context.fn(...args)
    delete context.fn
    return result
}

Function.prototype.HBind = function (context,...args1) {
    if (typeof this !== 'function') {
    throw new TypeError('Error')
    } //不是函数报错
    let _this=this
    let fn3=function (...args2){
      //这里的判断我们无法得知外部的函数名，所以只好在内部设置一个变量，这个变量和下文的fn2是等价的，所以只要判断this是否是fn3的实例就可以知道是不是new操作
      _this.HCall(this instanceof fn3?this:context,...args1.concat(args2))
    }
    fn3.prototype=new this() //实现继承
    //fn3.prototype=new this()
    return fn3
}

let fn = function () {
    this.name="lilei"
    console.log(this.age)
}
let obj = {
    age: 11
}
let fn2=fn.HBind(obj)
let lilei=new fn2() //11
```

