一.对象的API

1.Object.create():创建对象

用法Object.create(obj,propertiesObject)

参数说明：
*	obj:是一个原型对象
*	propertiesObject:一组属性和值，属性就是对象中的属性，值是属性描述符。
	属性描述符：数据描述符和存取描述符

ex1：

```
 "use strict";
        var lilei = Object.create(Object.prototype, {
            _age: {
                value: 20,
                writable: false,
                enumerable: false,
                configurable: false
            },
            age: {
                get: function () { return this._age },
                set: function (value) { this._age = value; },
                enumerable: false,
                configurable: false
            }
        });
        console.log(lilei.age);//20
```

ex2:

```
"use strict";
        var hanmeimei = { name: "hanmeimei", age: 21 };
        var lilei = Object.create(hanmeimei);
        console.log(lilei.name);//hanmeimei
```

ex3:
```
"use strict";
        var lilei = Object.create(null);//创建空对象，没有Object.prototype的属性和方法
```

|new obj()|Object.create(obj)|
|--|--|
|\_\_proto\_\_指向构造函数的原型obj.prototype|\_\_proto\_\_指向obj|





2.Object.getOwnPropertyDescriptor(Obj,prop);返回对象中指定自有属性对应的属性描述符

3.Object.getOwnPropertyDescriptors(Obj);返回对象所有自有属性属性描述符

4.Object.getOwnPropertyNames(Obj);返回对象所有自有属性属性名组成的数组

5.Object.getPrototypeOf();返回该对象的原型对象

