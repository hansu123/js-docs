## call和apply和bind

### 1.call和apply

每个函数都会继承来自Function.prototype的方法call,apply和bind就是其中的方法。
可将一个函数的对象上下文从初始的上下文改变为第一个传入的实参指定的新对象,也就是改变this的指向。

call:call([thisObject[,arg1 [,arg2 [,...,argn]]]])
apply:apply([thisObj [,argArray] ]);

#### 1.1 几个注意点：

1.1.1 首先它是函数的方法，除了函数，别的对象无法调用此方法

1.2.2 我们来看看第一个参数可以传入什么？

```javascript
function Student(){
console.log(this);
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

*  空值：Student.call();//lilei，this会指向window,window.name="lilei";
*  this：Student.call(this);//lilei，this没有明确的对象还是会指向window,window.name="lilei";
*  对象：Student.call(hanmeimei);//hanmeimei,this指向对象hanmeimei
*  数字：Student.call(1)//this：对应的构造函数Number的实例
*  字符串：Student.call("abc")//this：对应的构造函数String的实例
*  布尔：Student.call(true);//this：对应的构造函数Bollean的实例
*  undefined：Student.call(undefined)//this指向window
*  null：Student.call(null)//this指向window

1.2.3 我们来看看其余参数可以传入什么？

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

### 2 call,apply的作用

#### 2.1用其他对象的方法

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

#### 2.2调用匿名函数

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

### 3 bind的作用

#### 3.1 永久绑定

```javascript
function Score(math,English,chinese){
console.log(`${this.name}总得分为:${math+English+chinese}分`);
};

Score.bind({name:"lilei"});
Score(80,90,100);//lilei总得分为270
Score(70,90,80);//lilei总得分为240

Score.bind({name:"lilei"}，90);//绑定math=90
Score(90,100);//lilei总得分为280
Score(90,80);//lilei总得分为260
```

#### 3.2 拷贝构造函数

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

#### 3.3 修改匿名函数this的指向

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

### 4 区别

|      |   call    |   apply    |   bind    |
| :--: | :-------: | :--------: | :-------: |
| 传入参数 | (对象,单个参数) |  (对象,数组)   | (对象,单个参数) |
| 绑定时效 |    一次     |     一次     |    永久     |
| 返回值  |  函数执行结果   |   函数执行结果   |    函数     |
|  用法  | 函数.call() | 函数.apply() | 函数.bind() |

