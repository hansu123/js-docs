# VUE-TS

### 1. @Component

```js
import {componentA,componentB} from '@/components';

export default{
    components:{
        componentA,
        componentB,
    },
    directives: {
        focus: {
            // 指令的定义
            inserted: function (el) {
                el.focus()
            }
        }
    }
}
```

ts写法

```js
import {Component,Vue} from 'vue-property-decorator';
import {componentA,componentB} from '@/components';

 @Component({
    components:{
        componentA,
        componentB,
    },
    directives: {
        focus: {
            // 指令的定义
            inserted: function (el) {
                el.focus()
            }
        }
    }
})
export default class YourCompoent extends Vue{
   
}
```

### 2. @Prop 父子组件之间值的传递

js写法

```js
export default{
    props:{
        propA:String, // propA:Number
        propB:[String,Number],
        propC:{
            type:Array,
            default:()=>{
                return ['a','b']
            },
            required: true,
            validator:(value) => {
                return [
                    'a',
                    'b'
                 ].indexOf(value) !== -1
        }
    }
}
}
```

ts写法

```js
import {Component,Vue,Prop} from vue-property-decorator;

@Component
export default class YourComponent extends Vue {
    @Prop(String)
    propA:string;
    
    @Prop([String,Number])
    propB:string|number;
    
    @Prop({
     type: String, // type: [String , Number]
     default: 'default value', // 一般为String或Number
      //如果是对象或数组的话。默认值从一个工厂函数中返回
      // defatult: () => {
      //     return ['a','b']
      // }
     required: true,
     validator: (value) => {
        return [
          'InProcess',
          'Settled'
        ].indexOf(value) !== -1
     }
    })
    propC:string;   
}
```

### 3. @Watch

js写法

```js
export default {
  watch: {
    'person': {
      handler: (val, oldVal) { },
      immediate: true,
      deep: true
    }
  }
}
```

ts写法

```js
import {Vue, Component, Watch} from 'vue-property-decorator';

@Component
export default class YourComponent extends Vue{
    @Watch('person', { immediate: true, deep: true })
    onPersonChanged(val: Person, oldVal: Person) { }
}
```

### 4. @Emit

> 由@Emit $emit 定义的函数发出它们的返回值，后跟它们的原始参数。 如果返回值是promise，则在发出之前将其解析。

> 如果事件的名称未通过事件参数提供，则使用函数名称。 在这种情况下，camelCase名称将转换为kebab-case。

js写法

```js
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    addToCount(n) {
      this.count += n
      this.$emit('addToCount', n)
    }
  }
}
```

ts写法

```js
import { Vue, Component, Emit } from 'vue-property-decorator'

@Component
export default class YourComponent extends Vue {
  count = 0

  @Emit()
  addToCount(n: number) {
    this.count += n
    return n//传参
  }
}
```

注意：

父组件中子组件的方法名不能写成驼峰的形式，要拆成小写的形式

`@addToCount=""=>@add-to-count=""`

### 5. @Provide  提供  /  @Inject  注入

> 注：父组件不便于向子组件传递数据，就把数据通过Provide传递下去，然后子组件通过Inject来获取

js写法

```js
const symbol = Symbol('baz')

export const MyComponent = Vue.extend({

  inject: {
    foo: 'foo',
    bar: 'bar',
    'optional': { from: 'optional', default: 'default' },
    [symbol]: symbol
  },
  data () {
    return {
      foo: 'foo',
      baz: 'bar'
    }
  },
  provide () {
    return {
      foo: this.foo,
      bar: this.baz
    }
  }
})
```

ts写法

```js
import {Vue,Component,Inject,Provide} from 'vue-property-decorator';

const symbol = Symbol('baz')

@Component
export defalut class MyComponent extends Vue{
    @Inject()
    foo!: string;
    
    @Inject('bar')
    bar!: string;
    
    @Inject({
        from:'optional',
        default:'default'
    })
    optional!: string;
    
    @Inject(symbol)
    baz!: string;
    
    @Provide()
    foo = 'foo'
    
    @Provide('bar')
    baz = 'bar'
}
```

### 6.data

js写法

```js
export default {
  data() {
    return {
      count: 0
    }
  }
}
```

ts写法

```tsx
import { Vue, Component, Emit } from 'vue-property-decorator'

@Component
export default class YourComponent extends Vue {
  count:number = 0
}
```

### 7.计算属性

js写法

```js
export default {
  computed:{
      money(){
        return `${this.price}*${this.num}元`  
      }
  }
}
```

ts写法

```js
import { Vue, Component, Emit } from 'vue-property-decorator'

@Component
export default class YourComponent extends Vue {
    get money(){
        return `${this.price}*${this.num}元` 
    }
  
}
```





[github地址](<https://github.com/kaorun343/vue-property-decorator>)

[vue项目基础上使用ts](<https://juejin.im/post/5ba75b355188255c5e66e4d3>)



装饰器的本质就是把内部所有的东西转化成vue的参数从而转为为vue的组件的构造函数