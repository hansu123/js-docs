## call和apply

### 一.call和apply

每个函数都会继承来自Function.prototype的方法，call，apply和bind就是其中的方法。
可将一个函数的对象上下文从初始的上下文改变为第一个传入的实参指定的新对象,也就是改变this的指向。

> call:call([thisObject[,arg1 [,arg2 [,...,argn]]]])
> apply:apply([thisObj [,argArray] ]);

#### 几个注意点：

1. 首先它是函数的方法，除了函数，别的对象无法调用此方法
2. 我们来看看第一个参数可以传入什么？

```javascript
var name="lilei"
function Student(){
console.log(this.name);
};
var hanmeimei={
name:"hanmeimei";
}
Student.call();
Student.call(this);
Student.call(hanmeimei);
Student.call(1);
Student.call("abc");
Student.call(true);
Student.call(undefined);
Student.call(null);
```

* 空值：Student.call();//lilei，this会指向window,window.name="lilei";

*  this：Student.call(this);//lilei，this没有明确的对象还是会指向window，window.name="lilei";
*  对象：Student.call(hanmeimei);//hanmeimei,this指向对象hanmeimei
*  数字：Student.call(1)//this：对应的构造函数Number的实例
*  字符串：Student.call("abc")//this：对应的构造函数String的实例
*  布尔：Student.call(true);//this：对应的构造函数Bollean的实例
*  undefined：Student.call(undefined)//this指向window
*  null：Student.call(null)//this指向window

3. 我们来看看其余参数可以传入什么？

```javascript
function Student(age){
console.log(this.name,age);
};
var hanmeimei={
name:"hanmeimei";
}
```
其余参数就是实参传递。

`Student.call(hanmeimei,21);`就是把21赋值给Student中的形参age;

### 二. call,apply的作用

#### 1. 用其他对象的方法

```javascript
var lilei={
name:"lilei",
sing:function(){console.log(this.name);}
}
var hanmeimei={
name:"hanmeimei";
}
lilei.sing.call(hanmeimei);//hanmeimei
```

```
var arr=[1,343,42,12];
console.log(Math.max.apply(null,arr));
console.log(Math.max(11,2,89,100));
```

```javascript
Array.prototype.slice.call(arguments);//把传入的参数转化为数组，然后使用数组的api
```

#### 2. 调用匿名函数

```javascript
var lilei={
name:"lilei",
age:20
}
(()=>{console.log(this.name)}).call(lilei);//lilei
```

3.用来解耦，实现父类构造函数的继承

```javascript
function Class(klass,name){
this.klass=klass;
this.name=name;
}
function Student(klass,name){
Class.call(this,klass,name);//=>Class.klass=klass;Class.name=name;
this.sing=function(){console.log(我是${this.klass}班的${this.name})};
}
var lilei=new Student(11,"lilei");
lilei.sing();//我是11班lilei
```

解耦:相互独立，互不影响。传统的操作是循环遍历赋值，使用call和apply之后可以轻松解决。

### 三. 手写call和apply

> 原理：将执行的函数fn，放入到传入的对象中作为该对象的属性，然后去调用该对象中的刚才传入的fn，最后删除对象中的fn

#### 1.0

```js
{
  let fn=function (){
    console.log(this.age)
  }
  let obj={
    age:11
  }
  fn.call(obj)
}
//本质就是要改变fn中this的指向，相当于=>
{
  let obj={
    age:11,
    fn:function (){
      console.log(this.age)
    }
  }
  obj.fn()
}
```



#### 2.0

```js
//于是call其实本质就是在obj中放入fn，执行obj.fn，执行完后将其中的fn属性删除

{
  Function.prototype.HCall=function(context){
    context.fn=this
    context.fn()
    delete context.fn
  }

  let fn=function (){
    console.log(this.age)
  }
  let obj={
    age:11
  }
  fn.HCall(obj) //11
  console.log(obj) //{age:11}
}

//当然传入的第一个对象有很多值，可以是当前环境的this也可以是null
{
  Function.prototype.HCall=function(context){
    context=context?context:window
    context.fn=this
    context.fn()
    delete context.fn
  }
  var age=11
  var fn=function (){
    console.log(this.age)
  }
  fn.HCall(this) //11
  fn.HCall(null)
}

//最后实现传入参数
{
  Function.prototype.HCall=function(context,...args){
    context=context?context:window
    let fn=Symbol() //避免与已有属性发生冲突
    context[fn]=this
    var result=context[fn](...args)
    delete context[fn]
    return result
  }
  let fn=function (name,gender){
    console.log(name)
    console.log(gender===1?"男":"女")
    console.log(this.age)
  }
  let obj={
    age:11
  }
  fn.HCall(obj,"lilei",1) //11
  console.log(obj) //{age:11}
}
```



#### apply

```js
{
  Function.prototype.HApply=function(context,args){
    context=context?context:window
    let fn=Symbol() //避免与已有属性发生冲突
    context[fn]=this
    var result=context[fn](...args)
    delete context[fn]
    return result
  }
  let fn=function (name,gender){
    console.log(name)
    console.log(gender===1?"男":"女")
    console.log(this.age)
  }
  let obj={
    age:11
  }
  fn.HApply(obj,["lilei",1]) //11
  console.log(obj) //{age:11}
}
```

