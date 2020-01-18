# js面向对象—继承

定义：父对象的成员，子对象无需重复创建，就可直接使用

为什么要继承：其实我们可以发现构造函数也有一个弊端就是每次实例一个对象都要把一模一样的方法写进每个不同的实例中。这样其实也是一种重复，浪费内存空间。我们希望的是有一个专门供我们调取公共属性和方法的区域。但是由于js没有类的概念，所以需要通过构造函数来模拟类，然后通过原型来继承。

## 1.原型

当我们创建一个构造函数的时候，默认就会就会有一个属性叫做prototype。prototype属性本身指向一个对象，这个对象就是我们常说的原型对象。

为了便于理解直接上图：
```javascript
console.dir(Student);//打印构造函数的结构
```
![](img/prototype.jpg)


如何实现？

大家还记得new的第二步吗？`obj.__proto__ =  Student.prototype;`
其实我们每实例化一个对象，这个对象都有一个属性叫做`__proto__`,会自动帮我们把`__proto__`指向prototype,通过这种方法，每一个实例化的对象就都能访问原型对象中的属性和方法了。哇！看到这里发现祖师爷艾奇真的是奇才。

![](img/__proto__.jpg)

继承关系图：

![](img/oop1.png)

## 2.继承方式

### 2.1 通过改变原型继承

直接浅拷贝，使子类父类指向共同地址(不建议)，破坏了父级的原型对象

![inherit3](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/js_img/object/inherit3.png)

```javascript
function Person(name,age){
this.name=name;
this.age=age;
}
Person.prototype.sing=function(){console.log(this.name);}

function Student(sex){
this.sex=sex;
}
for(var key in Person.prototype)
{console.log(Person.prototype[key]);
Student.prototype[key]=Person.prototype[key];}//深拷贝Person.prototype
Student.prototype.name="lilei";//初始化值
Student.prototype.age=20;
let lilei=new Student("female");
console.log(lilei.sing());//lilei
```

改进深拷贝

![inherit4](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/js_img/object/inherit4.png)

```javascript
function Person(name,age){
this.name=name;
this.age=age;
}
Person.prototype.sing=function(){console.log(this.name);}

function Student(sex){
this.sex=sex;
}
for(var key in Person.prototype)
{console.log(Person.prototype[key]);
Student.prototype[key]=Person.prototype[key];}//深拷贝Person.prototype

Student.prototype.name="lilei";//初始化值
Student.prototype.age=20;
let lilei=new Student("female");
console.log(lilei.sing());//lilei
```



### 2.2 通过改变原型指向继承

子类构造函数想要继承父类构造函数，只要将子类构造函数的原型对象指向父类构造函数的原型对象即可实现继承。

![inherit1](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/js_img/object/inherit2.png)

ex:1

```javascript
function Person(name,age){
this.name=name;
this.age=age;
}
Person.prototype.sing=function(){console.log(this.name);}

function Student(sex){
this.sex=sex;
}
Student.prototype=new Person("lilei",20);//改变Student原型对象的指向并初始化值name和age
let lilei=new Student("female");
console.log(lilei.sing());//lilei
```

ex2:

这里用到一个API:Object.setPrototypeOf(obj, prototype);将prototype作为obj的原型

```javascript
function Person(name,age){
this.name=name;
this.age=age;
}
Person.prototype.sing=function(){console.log(this.name);}

function Student(sex){
this.sex=sex;
}
Object.setPrototypeOf(Student.prototype,Person.prototype);//改变Student原型对象的指向
Student.prototype.name="lilei";//初始化值
Student.prototype.age=20;//初始化值
let lilei=new Student("female");
console.log(lilei.sing());//lilei
```



坑:先实例化对象，再去改变原型指向

![inherit1](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/js_img/object/inherit1.png)

```javascript
function Person(name,age){
this.name=name;
this.age=age;
}
Person.prototype.sing=function(){console.log(this.name);}

function Student(sex){
this.sex=sex;
}
let lilei=new Student("female");
Student.prototype=new Person("lilei",20);//改变Student原型对象的指向
console.log(lilei.sing());//sing is not a function
```

### 2.3 构造函数继承

改变原型指向继承只可以继承父类构造函数原型对象中的属性和方法，而且需要写死实例化对象初始的属性，需要自己手动改变子类构造函数的实例化对象。

所以这个时候需要调用call和apply或者bind 实现继承

ex:

```javascript
function Person(name){
this.name=name;
this.sing=function(){console.log(this.name);}
};
function Student(name){
Person.call(this,name);
};

var lilei=new Student("lilei");
lilei.sing();
```

上面的这一过程其实就是new的原理

```javascript
function Student(name,age){
this.name=name;
this.age=age;
this.sing=function(){console.log(this.name);}
};
var lilei=Student("lilei",20);
=>
function Student(){
this.name=name;
this.age=age;
this.sing=function(){console.log(this.name);};
};

function new(name,age){
var obj={};
obj.__pro__=Student.prototype;
Student.call(obj,name,age);
return obj;
}

var lilei=new("lilei",20);
=>
function Student(){
this.name=name;
this.age=age;
this.sing=function(){console.log(this.name);};
};

function new(fun){
var obj={};
obj.__pro__=fun.prototype;
fun.call(obj,arguments[1],arguments[2]);
return obj;
}

var lilei=new(Student,"lilei",20);

然后不知道怎么就变成了var lilei=new Student("lilei",20);
```



### 2.4 组合继承

构造函数继承无法继承父类构造函数原型中的属性和方法，所以需要组合继承。

组合1:构造函数继承+原型继承( 也可以用Object.setPrototypeOf )

```javascript
function Person(name){
this.name=name;
};
Person.prototype.sing=function(){console.log(this.name);}
function Student(name){
Person.call(this,name);
};

Student.prototype=new Person();
Student.constructor=Student;
var lilei=new Student("lilei");
lilei.sing();
```

ex2:构造函数继承+深拷贝继承

```javascript
function Person(name,age){
this.name=name;
this.age=age;
}
Person.prototype.sing=function(){console.log(this.name);}
function Student(name,age,sex){
Person.call(this,name,age);
this.sex=sex;
}
for(var key in Person.prototype)
{console.log(Person.prototype[key]);
Student.prototype[key]=Person.prototype[key];}//深拷贝Person.prototype

let lilei=new Student("hansu",20,"female");
console.log(lilei.sing());//lilei
```



ex3:构造函数继承+浅拷贝继承



```javascript
function Person(name,age){
this.name=name;
this.age=age;
}
Person.prototype.sing=function(){console.log(this.name);}
function Student(name,age,sex){
Person.call(this,name,age);
this.sex=sex;
}

Student.prototype=Person.prototype;//浅拷贝Person.prototype

let lilei=new Student("hansu",20,"female");
console.log(lilei.sing());//lilei
```



综合:

构造函数继承+拷贝继承:因为是深拷贝所以修改父类原型对象中的属性和方法,子类无法动态获得

构造函数继承+原型继承:因为是一条原型链所以修改父类原型对象中的属性和方法,子类也动态获得




















