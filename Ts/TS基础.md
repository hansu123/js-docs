# TS基础

### 数组

>  类型+[]

```js
let numbers:number[]=[1,2,3,4] //数组中只允许出现数字
let arr:any[]=[1,"lilei"] // 数组可以出现任意类型
```

> Array<类型>

```js
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```



### 函数

> 参数类型，返回值类型

函数声明

```js
function sum(x: number, y: number): number {
    return x + y;
}
sum(1, 2);
```

函数表达式

```js
let sum:(x:number,y:number)=>number=(x: number, y: number):number=>{
    return x+y
}
```

#### 1. 默认值

```js
function sum(x: number=1, y: number): number {
    return x + y;
}
sum(1, 2);
```

#### 2. 可选值（必须在最后）

```js
function sum(x=1: number, y?: number): number {
    return x + y;
}
sum(1, 2);
```

#### 3. 剩余参数

```js
function sum(x:number=1, ...args:any[]): number {
  let sum=args.reduce((sum,num)=>{
      return sum+=num
  },x)
  return sum
}
console.log(sum(1, 2, 3));
```



### 类

#### 1. 普通写法

js写法

```js
class Person{
    constructor(name,age){
        this.name=name
        this.age=age
    }
    say(){
        console.log(this.name)
    }
}
```

ts写法

```js
class Person{
    //必须事先定义实例属性的类型
    name:string
    age:number
    constructor(name,age){
        this.name=name
        this.age=age
    }
    say():void{
        console.log(this.name)
    }
}
```

#### 2.修饰符

- `public`公开的，可以供自己、子类以及其它类访问
- `protected`受保护的，可以供自己、子类访问，但是其他就访问不了
- `private`私有的，只有自己访问，而子类、其他都访问不了

```js
class Person{
    //必须事先定义实例属性的类型
    protected name:string
    protected age:number
    constructor(name:string,age:number){
        this.name=name
        this.age=age
    }
    public userInfo():Object{
        return {name:this.name,age:this.age}
    }
    private say():void{
        console.log(this.name)
    }
}

let lilei=new Person("lilei",22)
console.log(lilei.name)//报错
console.log(lilei.age)//报错
console.log(lilei.userInfo())//打印结果
lilei.say() //报错
```

#### 3.静态属性和方法

> static 属性 ；static 方法

```js
class Person{
    static country:string="china"
    //必须事先定义实例属性的类型
    protected name:string
    protected age:number
    constructor(name:string,age:number){
        this.name=name
        this.age=age
    }
    static sing():void{
        console.log("起来，不愿...")
    }
    public userInfo():Object{
        return {name:this.name,age:this.age}
    }
    private say():void{
        console.log(this.name)
    }
}

let lilei=new Person("lilei",22)
console.log(lilei.country) //报错
lilei.sing() //报错
console.log(Person.country) //china
Person.sing() //起来..
```



#### 4.继承

```js
class Person{
    //必须事先定义实例属性的类型
    name:string
    age:number
    constructor(name,age){
        this.name=name
        this.age=age
    }
    say():void{
        console.log(this.name)
    }
}
class Student extends Person{
    klass:string
    constructor(name,age,klass){
        super(name,age)
        this.klass=klass
    }
}
```





### 联合类型

#### 1. 普通赋值

```js
let sex:number|boolean=1
sex=false
console.log(sex)
```

#### 2. 函数

```js
function getLength(something: string | number): number {
    return something.length;
}
```

`length` 不是 `string` 和 `number` 的共有属性，所以会报错。



### 类型断言

> 不知道类型的情况下，需要提前判断

```js
function getLength(something: string | number): number {
    if(<string>something.length){
        return something.length;
    }
}
```

**必须是初始参数类型或者是联合类型中的某一个**

另一种写法(as)

```js
function getLength(something: string | number): number {
    if((something as string).length){
        return something.length;
    }
}
```



### 类型别名

```js
type Han=string | number
function getLength(something:Han ): number {
    if(<string>something.length){
        return something.length;
    }
}
```

自定义范围

```js
type Method='get'|'GET'|'post'|'POST'|'put'|'PUT'|'patch'
interface RequestValue{
url:string,
params?:any,
method?:Method,（只能选择范围里的值）
data?:any,
}
```



### 接口

####1.demo

```js
interface fullName {
  firstName: String;
  secondName: string;
}
let userName:fullName={
   firstName:"James",
   secondName:"Harden"
}
```

#### 2.可选属性

```js
interface fullName {
  firstName: String;
  secondName: string;
  age?: number;//可选属性
}
let getFullName=function(name: fullName): string {
  return name.firstName + name.secondName
}
console.log(getFullName({ firstName: "James", secondName: "Harden" }))
```

#### 3.任意属性

使用`[propName:string]:any`，第一个string表示属性名的类型，一般都是字符串，第二个表示属性值的类型，而且已经确定了或者不确定属性的值都必须是任意属性的子集

```js
//多一些属性，少一些属性都不行
//任意属性
interface Person {
  name: string;
  age?: number;
  [propName: string]: any;
}

let tom: Person = {
  name: 'Tom',
  gender: 'male',
  sex:1,
  salary:1000
};
console.log(tom)
```

**string和number是any的子集**

#### 4.数组接口

```js 
let StringArray{
    [index:number]:string
}
```

#### 5.函数接口









### 泛型

```js
interface DataFunc{
  <T>(a:T):T
}
let getData:DataFunc=function<T>(a:T):T{
return a
}
console.log(getData<number>(12))
console.log(getData<string>("lilei"))
```



[TS文档](<https://ts.xcatliu.com/advanced/generics>)