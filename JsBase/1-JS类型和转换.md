# JS类型和转换

#### 1.基本数据类型

number,boolean,string,null和undefined

##### 1.1 number

0开头表示八进制，0x开头表示十六进制

浮点型小数有计算误差，因为使用的是二进制浮点数进行计算

##### 1.2NaN

任何与NaN比较都为false

##### 1.3null和undefined

|      |        null         |          undefined          |
| :--: | :-----------------: | :-------------------------: |
|  异   | typeof(null)=object | typeof(undefined)=undefined |
|  同   | 都表示空缺值，if判断时都为false |         不包含任何属性和方法          |

何时会得到undefined

* 变量未初始化
* 未定义属性和数组中的值
* 函数没有返回值
* 对于有形参的函数未传入实参

#### 2.引用类型

其实就是对象，之所以称为引用类型，是因为它表示的是一个引用，引用类型可以无限制添加属性。

#### 3.typeof

```html
typeof(1)==number  //true
typeof(NaN)==number  //true 
typeof(true)==boolean //true
typeof('a')==string //true
typeof(null)==object //true
typeof(undefined)==undefined //true
```

这里注意的是`typeof(NaN)`的结果是`number`

#### 4.比较

- 基本数据类型是值比较
- 引用类型是比较的是地址，即是否引用同一个对象

```javascript
let a=[1,2,3],b=[1,2,3];
console.log(a==b); //false
let a=[1,2,3],b=[1,2,3];

let a=[1,2,3];
let b=a,c=a;
console.log(b==c); //true
```

#### 5.转换

##### 5.1隐式转换

* == 

  * null==undefined 为true
  * 数字和字符串比较，字符串转化为数字
  * 布尔值和数字值比较，布尔转化为数字
  * 对象通过valueof()和toString()转化为原始值

* if

  ```html
  if(undefined){...}
  if(null){...}
  if(""){}
  if(0){}
  if(false){...}  
  if(NaN){...}
  let a=1;
  if(a){...}
  ```

  null,undefined,"",0,false,NaN都会转化为false

  其余转为真

  **面试考点:**

  * [],{}也为真
  * undefined==false为false，是因为==并不会将undefined操作符转化为false

* 逻辑运算符

  * \+ 比如 字符串与数字相加，数字转化为字符串
  * ！比如 强制将后面的值转化为布尔类型
  * ...

##### 5.2显式转换

* Number(),String(),Boolean(),Object()

