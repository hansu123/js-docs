# new

### 一. new原理

**example**

```js
function Person(name){
    this.name=name
    console.log("Person实例")
}
let lilei = new Person("lilei")
```



#### new的内部做了以下步骤

1.创建一个空对象

```js
var obj = new Object();
```

2.让Person中的`this`指向obj，并执行Person的函数体

```
`var` `result = Person.call(obj);`
```

3、设置原型链，将obj的`__proto__`成员指向了Person函数对象的`prototype`成员对象

```
`obj.__proto__ = Person.prototype;`
```

4、判断Person的返回值类型，如果是值类型，返回obj。如果是引用类型，就返回这个引用类型的对象

```js
if (typeof(result) == "object") {person = result;}
else {person = obj;}
```

这个地方需要说明下

**example**

```js
function Person(name){
    this.name=name
    console.log("Person实例")
    return {
        name:"hanmeimei"，
        age:22
    }
}
let lilei = new Person("lilei")
lilei.name //hanmeimei
lilei.age //22
console.log(lilei instanceof Person) //false
```

以上情况，lilei就是构造函数返回的对象，并不是创建的实例

### 二. 实现

知道了原理，我们就手动来实现以下



```js
function Person(name){
    this.name=name
    console.log("Person实例")
}

function HNew(...args){
    //创建空对象
    let newObj={}
    //执行函数，并将this指向创建的对象
    let Fn=args.shift() //获取参数中的构造函数
    let res=Fn.call(newObj,...args)  //执行构造函数，并将剩下的参数传递
    //改变this的指向
    newObj.__proto__=Fn.prototype
    return typeof res==="object"?res:newObj
}
let lilei=HNew(Person,"lilei")
console.log(lilei instanceof Person) //true
```



[参考掘金博客](<https://juejin.im/post/590a99015c497d005852cf26>)