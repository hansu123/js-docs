# JS函数基本概要



### 一.函数

#### 1.简述变量声明提前

函数声明时会将所有变量提升到当前作用域最顶端，将变量的初始换留在原来位置。

#### 2.简述函数的调用

* 作为普通函数调用 fn()
* 作为构造函数调用 new Fn()
* 作为对象的方法调用 obj.fn()
* call和apply调用 fn.call(...)

函数作用域取决于创建时，而不是调用时

#### 3.简述this的四种用法

* 作为普通函数 this->window
* 在构造函数中指代实例对象 this.name=name
* 在对象中的方法要访问对象属性时 sing:function(){console.log(this.name)}
* call和apply调用时 call(this,.....)

this的指向取决于调用时，而不是创建时

#### 5.函数的命名空间

其实就是匿名函数的自调用,目的是不污染全局变量

```javascript
(function(){  
}())
```



#### 6.3.3 模块化封装

```javascript
function count(){
var num=1;
return {
addNum:function(){console.log(num++)},
redNum:function(){console.log(num--)}
}
}
let fn1=count().addNum;
let fn2=count().redNum;
fn1(); //1
fn2(); //1
```

这个题目也很经典，其实还是上面所说的，函数调用时都会自己形成一个新的作用域链。彼此不会产生影响。

#### 6.3.4谈谈闭包的缺点-内存泄漏

内存垃圾回收的策略是看看每个值的引用计数是否为0.

所以当A对象包含B,B包含A时会发生循环引用也就是常说的内存泄漏

比如


