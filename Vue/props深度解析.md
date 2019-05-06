# props—组件间传参

### 在看下文之前我先声明几个概念：

`<template></template>` 中的内容我成为组件

`var componentName={}`我称为组件声明

### 1.首先我们来看两个经典错误(省略了一些Vue实例声明)

#### 1.1不能在父组件里的子组件标签里直接绑定子组件的属性和方法

```html
<template id="cTodo">
<div>
<h1>待办事项</h1>
<todo-list v-show="day=='Sunday'"></todo-list>  //错误的用法
</div>
</template>
```

有时我们想通过msg来控制子组件的显示，可是控制台会提示 `error:属性或方法"msg"不是在实例上定义的，而是在呈现过程中引用的。`

所以显然直接在父组件里绑定子组件的属性和方法是错误的，我们可以在子组件的模板中进行修改。

```html
<template id="cTodoList">
<ul v-show="day=='Sunday'">   //因为是子组件上的数据，所以在子组件上随意使用
  <li>逛街</li>
</ul>
</template>
```

或者我们在父组件中定义数据

```html
<script>
var TodoList={
template:"#cTodoList",
data(){return {}}}
Vue.component("Todo",{
template:"#cTodo",
data(){return {
tasks:["洗衣服","开会","取快递"],
day:"Sunday"}},//直接在父组件上定义day
components:{TodoList}})
</script>
</script>
<template id="cTodo">
<div><h1>待办事项</h1><todo-list v-show="day=='Sunday'"></todo-list></div> //可以使用
</template>
<template id="cTodoList">
<ul><li>逛街</li></ul>
</template>
```

#### 1.2直接使用父组件中的数据

```html
<script>
var TodoList={
template:"#cTodoList",
data(){return {}}
}
Vue.component("Todo",{
template:"#cTodo",
data(){return {tasks:["洗衣服","开会","取快递"]},
components:{TodoList}
})
</script>
<template id="cTodo">
<div>
<h1>待办事项</h1>
<todo-list  v-for="task of tasks"></todo-list> //错误用法
</div>
</template>
<template id="cTodoList">
<div><p>{{task}}</p></div> 
</template>
```

由于各组件的作用域是相互独立的所以子组件无法直接调用父组件的数据。如何解决呢？看完下面你就会啦。

### 2.父子通信

出现以上两种错误的根本原因就是数据无法达到直接的传递，也就是所谓的通信，我们希望能够有数据可以供我们随意调用就像全局变量一样写意。Vue中为我们提供了父子通信的方法—props.

props是桥接数据的工具，它有两种用法，静态传递和动态传递。

#### 2.1静态传递(我一直觉得没啥用)

> 直接在子组件上标签上给props中的变量名赋值，只能传递字符串

```html
<script>
var TodoList={
template:"#cTodoList",
props:["day"],
data(){return {}}
}
Vue.component("Todo",{
template:"#cTodo",
data(){return {tasks:["洗衣服","开会","取快递"]},
components:{TodoList}
})
</script>
<template id="cTodo">
<div>
<h1>待办事项</h1>
<todo-list day="Sunday"></todo-list>
</div>
</template>
<template id="cTodoList">
<ul><li>{{day}}逛街</li></ul>
</template>
```

输出：

```html
待办事项
Sunday 逛街
```

#### 2.2 动态方法

> 将一个对象所有属性作为prop传递，可以使用不带任何参数的v-bind，通常我们使用v-bind后面需要加属性这里不需要。通常情况下父中的数据会是一个对象，我们需要动态调用。

如`1.2`中，我们可以这样写

```html
<script>
var TodoList={
template:"#cTodoList",
props:["tasks","task"],
data(){return {}}
}
Vue.component("Todo",{
template:"#cTodo",
data(){return {tasks:["洗衣服","开会","取快递"]},
components:{TodoList}
})
</script>
<template id="cTodo">
<div>
<h1>待办事项</h1>
<todo-list :tasks="tasks" :task="task" v-for="task of tasks"></todo-list>
</div>
</template>
<template id="cTodoList">
<div><p>{{task}}}</p></div> 
</template>
```

其实我们可以将以上的代码变向的理解为

```html
<template id="cTodo">
<div>
<h1>待办事项</h1>
<todo-list :tasks="tasks"></todo-list>
</div>
</template>
<template id="cTodoList">
<div><p v-for="task of tasks">{{task}}</p></div> 
</template>
```



#### 总结：

1.`props`可以自定义变量名

2.`props["tasks"]`在通信过程中其实做了两件事情

* 在自己的组件标签上定义了一个tasks属性，用来接收数据
* 在自己的data中写入了数据名tasks

3.`:tasks="tasks"`也做了两件事

* 将tasks值传给了tasks属性
* 在data中将值传入了tasks

4.子组件只要用到的数据都需要props进行传递

5.子组件拿到的数据可以进行计算等后续操作



最后附上通信逻辑图：

![](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/vue/props1.png)



