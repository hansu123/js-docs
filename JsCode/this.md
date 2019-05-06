# this

>this指向什么，完全取决于什么地方以什么方式调用，而不是创建时

### 1.默认绑定：一般函数

>默认指向全局对象,window或者global

##### ex1:

```js
a= 1;
function f1() {
  var a = 4;
  console.log(this.a);
}
f1();//结果为1
```

##### ex2:

```js
a=1;
function f2(){
this.a=2;//this代指window,this.a=>window.a及全局变量a;
console.log(a);
f2();//2
console.log(a);//2因为a是全局变量
```

##### ex3:

```js
function f3(a) {
  this.a = a;
  return this;
}
var a = f3(1);//=>window.a=1;a=window;
var b = f3(10);//=>window.a=10;b=window;
console.log(a.a);//10=>window.a=10
console.log(b.a);//10=>window.a=10
```

##### ex4:

```js
function f3(a) {
  this.a = a;
  return this;
}
a = f3(1); //=>window.a=1;window.a=window;所以window.a被重新赋值为window
b = f3(10); //=>window.a=10;window.b=window;
//=>window.a=10,window.b=window;
console.log(a.a); //undefined  a.a=>window.a.a=>10.a所以返回undefined
console.log(b.a); //10  b.a=>window.a=>因为window.a重新赋值为10，所以结果为10
```

### 2.隐性绑定：对象方法调用

>默认绑定为上下文对象：常用于对象中的方法的调用

##### ex1:

```js
var user = {
  name: 'hansu',
  sing: function() {
    console.log(this.name);//this指代user,this会去找上下文对象，如果找不到则成为window
  }
};
user.sing();
```

##### ex2:

```js
name = 1;
var user = {
  name: 'hansu',
  sing: function() {
    console.log(this.name); //this指代user,this会去找上下文对象，如果找不到则成为window
    return function() {
      console.log(this.name);
    }
  }
};
user.sing()();//1  此时的this上层只是个函数，所以会成为window,window.name=1,这种现象称为隐性丢失
```

### 3.显性绑定：call和apply调用

>也称硬绑定，就是通过call()和apply()方法,把函数的this绑定到对象中

```js
name=1;
function User(name) {
  console.log(this.name);
}
var user1={
name:'hansu'
}
User.apply(user1);//将User函数中的this绑定到user1对象中=>this.name===user1.name
User.apply();//1  如果没有参数，传递对象为window,this.name=window.name=1
User.call();//1
bind方法
function User(name) {
  console.log(this.name);
}
var user1={
name:'hansu'
}
var f=User.bind(user1);//将User函数中的this绑定到user1对象中=>this.name===user1.name,但只是返回绑定好的函数
f();//hansu//调用函数，执行结果
```

### 4.new绑定：构造函数调用

```js
function User={
this.name=name||User,
this.age=age,
this.sing=function(){
console.log(this.name);
}
}//=>创建构造函数
var user1=new User('hansu',20);//=>实例化一个对象
//这个具体在前一节构造函数中已经讲过
new 绑定>显示绑定>隐式绑定>默认绑定
```

### 5.箭头函数绑定

```js
var user = {
  name: 'hansu',
  sing: function(){
    console.log(this.name);
    return function(){console.log(this.name);//指向window
  }
}}
var f=user.sing();
f();//hansu window

var user = {
  name: 'hansu',
  sing: function(){
    console.log(this.name);
    return ()=>{console.log(this.name);//继承宿主对象user
  }
}}
var f=user.sing();
f();//hansu hansu
```

##### 注意

* 1.箭头函数中的this继承与它外面第一个不是箭头函数的函数的this指向一致。

* 2.箭头函数的this一旦绑定上下文，就不会被任何代码改变





