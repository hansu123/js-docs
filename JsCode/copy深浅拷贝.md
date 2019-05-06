# 一.深浅拷贝

## 1.深拷贝与浅拷贝的区别

### 1.1 如何区分深拷贝与浅拷贝?

简单点来说，就是假设B复制了A，当修改A时，看B是否会发生变化，如果B也跟着变了，说明这是浅拷贝，如果B没变，那就是深拷贝。

先看两个简单的例子:

ex1:深拷贝
```javascript
var a=2;
b=a;
a++;
console.log(b);//2
```

ex2:浅拷贝
```javascript
var arr1=[1,2,3];
var arr2=arr1;
arr1[0]++;
console.log(arr2[0]);//2

var lilei={
  age:20,
  salary:10000
};
var hanmeimei=lilei
lilei.salary=11000;
console.log(hanmeimei.salary);//11000
```
### 1.2内存存储结构

#### 为什么会发生上述这两种情况呢？我们要深入了解下js内存存储结构:

> 深拷贝:值传递，a和b是相互独立的，互不影响

![copy1](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/js-img/copy1.png)

> 浅拷贝:值是地址，地址指向共同的开辟的一块存储空间

![copy2](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/js-img/copy2.png)

#### 注意:

基本数据类型在堆中开辟空间

引用类型在栈中开辟空间，并将地址留在堆内存中

```javascript
var a={n:1};
var b=a;
a={x:1};//内存中开辟了新空间，指向新地址
console.log(b);
```

![copy3](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/js-img/copy3.png)

* {n:1}在内存中开辟空间，改变a的地址指向，所以a和b相互独立


#### 一道经典的面试题:

```
var a={n:1};
var b=a;
a.x=a={x:1};
console.log(a.x);//1
console.log(b.x);//{x:1}
```

![copy5](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/js-img/copy5.png)

* `var b=a;` 完成浅拷贝
* `a.x=a={x:1};` 因为js存在变量提升，会把a.x提升到最前端，所以会在a和b共同指向的内存中再开辟一块内存空间为x
* `a={x:1};` 赋值留在原地，此时因为赋的值是引用类型所以又要开辟一块新的空间，并将a的地址指向该空间
* `a.x=a;` 赋值留在原地，此时a.x的意思不在是说a中的属性x,它指代的是一开始变量提升时在内存中开辟的那块空间，同样因为赋的值是引用类型所以将其指向该空间
* `console.log(a.x)` 此时a指向的是{x:1},所以a.x为1
* `console.log(b.x)` 此时b还是指向之前的内存空间，内存空间中的x和a又共同指向一个空间，所以b.x为{x:1}



### 1.3浅拷贝的其他方式

数组中有两个特殊的api,他们能在不改变原数组的情况下返回新数组，这样一来我们也可以变向的实现拷贝

slice:截取返回新数组

```javascript
var arr1=[1,2,3];
var arr2=arr1.slice();
arr1[0]++;
console.log(arr2[0]);//1
```

concat:连接返回新数组

```javascript
var arr1=[1,2,3];
var arr2=arr1.concat();
arr1[0]++;
console.log(arr2[0]);//1
```

有人会问，这不是深拷贝吗？arr1修改之后并没有影响arr2啊！恭喜你入坑了，我们再来看一个例子

```javascript
var arr1=[[1,2,3],2,3];
var arr2=arr1.slice();
arr1[0][0]=3;
console.log(arr2[0][0]);//3
```

为什么会这样呢？我们仔细看一下数组的存储结构就一目了然了。

![copy4](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/js-img/copy4.png)

我们发现通过slice或者concat只能达到第一层都是基本数据类型的情况下才能达到深拷贝。如果第一层中出现引用类型则无法实现深拷贝，因为引用类型的传值永远都是地址。

怎么解决呢？

### 1.4如何实现深层拷贝

深拷贝之所以生效，是因为我们在最后一层通过值传递的方式将变量拷贝到另一个变量中。这样就保证了两个变量之间互相独立，不再是指向共同的地址。

所以我们只要把引用类型转化成基本数据类型传值即可:

ex1:数组

```javascript
var arr1=[1,2,3];
var arr2=[];
arr1.forEach((elem,i,arr)=>{
  arr2[i]=elem;
});
arr1[0]++;
console.log(arr2[0]);//1
```

ex2:对象

```javascript
var lilei={
  age:20,
  salary:10000
};
var hanmeimei={};
for(var key in lilei){
  hanmeimei[key]=lilei[key];
}
lilei.salary=11000;
console.log(hanmeimei.salary);
```

那如何实现更深层次的拷贝呢？

```javascript
function deepCopy(Obj){
  var result;
  if(Object.prototype.toString.call(Obj) == '[object Array]'){
    result=[];
    for(var key in Obj){
      result[key]=deepCopy(Obj[key]);
    }
  }
  else if(Object.prototype.toString.call(Obj) == '[object Object]'){
   result={};
    for(var key in Obj){
      result[key]=deepCopy(Obj[key]);
    }
  }
  else{return Obj;}
  return result;
}
var arr1=[[1,2,3],1,2];
var arr2=deepCopy(arr1);
arr1[0][0]=3;
console.log(arr2[0][0]);//1
```

这里补充几个知识点:

* 判断变量是数组还是对象

  * 通过变量的原型链中的constructor可以实现这个目的

    ```javascript
    1 var a=[];
    2 var b={};
    3 console.log(a.prototype.constructor);//Array
    4 console.log(b.prototype.constructor);//Object
    ```

  * 利用一些数组中有的而对象中没有的属性，比如length（当为空array时长度也可能为0,object的长度为undefined）。

    ```javascript
    var a=[];
    var b={};
    typeof a === 'object' && !isNaN(a.length)//true
    typeof b === 'object' && !isNaN(b.length)//false

    //数组中有slice方法而对象中没有
    var a=[];
    var b={};
    a.__proto__.hasOwnProperty("slice")//true
    b.__proto__.hasOwnProperty("slice")//false
    ```

  * 调用toString( )方法试着将该变量转化为代表其类型的string。 

    ```javascript
    var a=[];
    var b={};
    Object.prototype.toString.call(a)  === '[object Array]'//true
    Object.prototype.toString.call(b)  === '[object Array]'//false
    ```

  * 利用对象构造函数的原型对象

    ```javascript
    1 var a=[];
    2 var b={};
    3 console.log(a instanceof Array);//true
    4 console.log(b instanceof Object);//true
    ```

    ​

    ​

#### 



### 