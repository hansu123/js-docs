<h4>ejs模板引擎</h4>

一.安装和配置

1.安装npm install ejs --save
或者下好放入node_modules中

2.引入ejs

const ejs=require("ejs");

创建app.js和views文件夹，在views下新建show.ejs(不是.html文件)

在app.js中设置

```
ejs.set("view engine",ejs);//主流的有jade,handlebars,ejs,这里使用ejs引擎

ejs.set("View","./views")//设置路径，默认是在views文件夹下，如果默认的话可以不用设置
```

在show.ejs中随便写啥都可以，只要后缀改成ejs就可以


二.基本用法

1.用法：
在app.js路由中写入：
res.render("模板文件名",{数据json})
注意：
res.render不是render
只写模板文件名，不用写后缀和路径


2.ejs语法：


2.1 js常用语法——针对ejs页面

<% 变量名="" %> 定义变量
<%= 变量%>输出变量
<% for(){}%> 循环打印
<% if(){}%> 条件判断

ex:

```
<% content="hello lili" %>
<h1><%=content%></h1>
//输出变量名
```
```
app.js中
server.get("/show",(req,res)=>{
res.send("show",{name:'lili'})
});

show.ejs中
<%=name%>
//输出后台传入的变量名
```

```
app.js中

server.get("/show",(req,res)=>{
var sql="select * from user";
connection.query(sql,(err,result)=>{
if(err){throw err;}
res.render("show",{data:result})
});
});

show.ejs中

<table>
<% for(i in data){%>
<tr>
<td>data[i].user_name</td>
<td>data[i].user_phone</td>
<td>data[i].user_email</td>
</tr>
<%}%>
//循环打印数据库查询的值

```

2.2 模板嵌套

新建header.ejs

```
<ul><li>首页<li><li>新闻<li><li>电影<li></ul>
```
在show.ejs引入

语法：<% include 路径 %>

```
<%include header.ejs%>
```
