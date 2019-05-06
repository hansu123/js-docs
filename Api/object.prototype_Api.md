# Object原型对象的APi

### 1.hasOwnProperty():判断属性是不是该对象的一个属性或者对象,只能是自身定义的属性或方法,无法判断其原型链是否具有该属性和方法。

>用法:Object.hasOwnProperty("proName")

```javascript
function Student(){
  this.name="hansu",
  this.age=20,
}

Student.prototype.sing=function(){console.log(this.name);};
var hansu=new Student();

console.log(Student.hasOwnProperty("age"));//false,Student不具有该属性
console.log(hansu.hasOwnProperty("age"));//true

console.log(hansu.hasOwnProperty("sing"));//false原型链上的方法

hansu.sing=function(){console.log("1");}
console.log(hansu.hasOwnProperty("sing"));//true自身的方法
```
(补充)in:判断的是对象的所有属性和方法，包括对象实例和继承来的属性和方法

>用法: "proName" in Object

```javascript
function Student(){
  this.name="hansu",
  this.age=20
}
Student.prototype.sing=function(){console.log(this.name);};
var hansu=new Student();
console.log("age" in Student);//false,Student不具有该属性
console.log("age" in hansu);//true自身属性

console.log("sing" in hansu);//true原型链上的方法
hansu.sing=function(){console.log("1");}
console.log("sing" in hansu);//true自身的方法
```

### 2.isPrototypeOf():判断一个对象是否在另一个对象的原型链上

用法:ObjectA.prototype.isPrototypeOf(ObjectB)

```
//参照上面的例子
console.log(Student.prototype.isPrototypeOf(hansu));//true
```

(补充)instanceof:判断对象是否是构造函数的实例

用法:Object instanceof constructor;

```javascript
//参照上面的例子
console.log(hansu instanceof Student);//true

console.log(hansu.__proto__ instanceof Object);//true
console.log(Student.prototype instanceof Object);//true

console.log(Student instanceof Function);//true
console.log(hansu instanceof Function);//false
```

### 3.valueOf():返回一个对象的原始值

```javascript
var x="hello";
console.log(x.valueOf());//hello
console.log([1,2,3].valueOf());//[1,2,3]

var users={
    name:"lilei",
    sex:"female"
}
console.log(users.valueOf());//{name:"lilei",sex:"female"}

console.log(function(){console.log("hello")}.valueOf());//function(){...}

```
