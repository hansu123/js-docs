# VUE基础命令

### 1.模板语法

> {{}}

ex:

```
{{num}}//输出num
{{num+1}}//输出num+1的值
{{num<10?'0'+num:num}}//三目运算符
{{str1.split("").reverse().join("")}}//使用对象自身的API
```

### 2.v-bind(简写:)

> 用来绑定属性

ex:

```
<img :src="imgSrc" />
<div :class="" />
```

### 3.v-if

> 控制隐藏和显示

ex:

```html
<p v-if="seen"></p>   //seen为true显示，false隐藏
```

### 4.v-if-else

> 控制多个元素显示和隐藏

ex:

```html
<p v-if="seen"></p>   //seen为true显示，false隐藏
<p v-else>失败了</p>
```

### 5.v-show

> 控制隐藏和显示

用法和`v-if`类似，只是原理不同

* `v-for `  删除元素
* `v-show `  display:none

### 6.v-pre

> 使{{}}中的内容不会被编译

### v-once

> 在第一次DOM时进行渲染，渲染完成后视为静态内容，跳出以后的渲染过程。

### 7.v-cloak

> DOM渲染完成后显示,记得要写css的样式

```html
<P v-cloak>{{msg}}</p>
[v-cloak]{display:none}
```

### 8.v-text

> 输出纯文本

### 9.v-html

> 输出编译后的文本

### 10.v-for

> 循环输出

```
<ul>
<li v-for="elem of data">
</li>
</ul>
```

### 11.v-on(简写@)

> 绑定事件

```html
<button v-on="click">点我</button>
```

### 12.v-model

> 双向绑定数据

#### 1.文本框/多行文本域

```html
<input type="text" v-model="{{kword}}"> //自动获取value值
```

#### 2.复选框(绑定的是选中的value值，但是数据源必须是一个数组)

```html
<div> 
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames"> 
<label for="jack">Jack</label> 
<input type="checkbox" id="john" value="John" v-model="checkedNames"> 
<label for="john">John</label> 
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label> </div> 
<div>选中的值：{{checkedNames}}</div> 
// JavaScript var app = new Vue({
el: '#app',
data: { checkedNames: [] } 
})
```

#### 3.单选框(绑定的是选中的value值)

```html
<div> 
<input type="radio" id="jack" name="student" value="Jack" v-model="selectedName"> 
<label for="jack">Jack</label> 
<input type="radio" id="john" name="student" value="John" v-model="selectedName"> 
<label for="john">John</label> 
<input type="radio" id="mike" name="student" value="Mike" v-model="selectedName">
<label for="mike">Mike</label> </div> 
<div>选中的值：{{selectedName}}</div> 
// JavaScript var app = new Vue({
el: '#app',
data: { selectedName:"" } 
})
```

#### 4.下拉框

* 单项下拉框

```html
<select v-model="selectedcity">
<option value="beijing">北京</option>
<option value="shanghai">上海</option>
<option value="suzhou">苏州</option>
</select>
<div>选中的值：{{selectedcity}}</div> 
var app = new Vue({
el: '#app',
data: { selectedcity:"" } 
})
```

* 多项下拉框

```html
<select v-model="multipleSelected" multiple>
<option>CSS</option>
<option>HTML</option>
<option>JavaScript</option> 
<option>PHP</option> 
</select>
<span>请选择:{{multipleSelected}}</span>
var app = new Vue({
el: '#app',
data: { multipleSelected:[] } 
})
```

#### 5.修饰符

* 不带修饰符：修改`input`的值，`message`立马变同步`input`的输入值
* 带`.lazy`修饰符： 修改`input`的值，`message`并不会立马同步`input`的输入值，只有当`input`失去焦点时，`message`才会同步`input`的输入值
* 带`.number`修饰符： 当输入框的值，以数字加其他字符组合的内容，会自动去除其他的字符，只留数字；如果是其他字符加数字组合的内容，并不会删除其他字符，只留数字。一般带`.number`修饰符的`input`控制配合`type="number"`配合使用
* 带`.trim`修饰符： `input`输入框开始或末尾有空字符，将会自动删除空字符，如果空字符在其他字符中间，则不会删除空字符





