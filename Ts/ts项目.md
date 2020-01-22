# 注意事项

## 一. vue的用法

### 1.this报错

```js
this.$refs[formName]
```
会报错要改成

```js
(this as any).$refs[formName] 
```

### 2.引入报错
```js
import dejwt from 'jwt-decode'
```
会报错，因为第三方类库并没有ts的.d.ts 类型的声明文件
安装@types/jwt-decode即可

### 3.每个组件中必须得加

```js
import { Component, Vue } from "vue-property-decorator";
@Component({
  components: {}
})
```

不然不会报错，但是会有莫名其妙的bug，比如elem表单无法输入

### 4.!:的用法

```js
@Prop(Boolean) isCollapse:boolean
```

通常这么写会报错，`has no initializer and is not definitely assigned in the constructor`
意思是没有初始化，此时我们可以

```
@Prop(Boolean) isCollapse!:boolean
```

意思就是你不用管我有没有初始化



## 二. vuex用法

### 1.有命名空间

```js
import { Action, namespace } from "vuex-class";
const admintor = namespace("admintor");
export default class signIn extends Vue {
  @admintor.Action("checkLogin") checkLogin;
  @someModule.Getter('foo') moduleGetterFoo
  handleLogin(){
      this.checkLogin()
  }
}
```

### 2.无命名空间

```js
import { State,Getter,Action,Mutation, namespace } from "vuex-class";
const admintor = namespace("admintor");
export default class signIn extends Vue {
  @State('userName') userName
  @Getter('getUserName') getUserName
  @Action('foo') actionFoo
  @Mutation('setUserName') setUserName
  mounted(){
      console.log(this.userName)
  }
}
```

[vuex-class（npm）地址](<https://www.npmjs.com/package/vuex-class>)



### 3.拦截器的写法

```js
import axios,{AxiosResponse,AxiosRequestConfig} from "axios"
request.interceptors.request.use((config:AxiosRequestConfig)=>{
  console.log(config)
  return config
})

request.interceptors.response.use((response:AxiosResponse) =>{
  console.log(response.status)
  return response
},(err)=>{
  console.log(err.response.status)
})
```

