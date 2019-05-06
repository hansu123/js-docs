# css之选择器

### 一.元素选择器

#### 1.通配符*

匹配所有

```css
*{margin:0;padding:0}
```

#### 2.ID选择器:唯一标识

```css
#container{background:red;}//匹配id为container元素的背景颜色为红色
```

#### 3.类选择器：可以定义多个

```css
.active{background:red}//匹配class为active元素的背景颜色为红色
```

多类选择符:

```css
.title{color:#000;padding-left:10px;}
.hide{display:none;}
<p class="title hide"></p>  //会同时拥有两个类选择器的属性
```

#### 4.类型选择符

```css
p{color:#FFF;}//匹配所有p元素文字颜色为白色
```

```css
h1,p{color:#FFF;}//匹配所有h1元素和p元素文字颜色为白色
```

### 二.关系选择器

#### 1.包含选择符  ：匹配子代所有指定元素(子子孙孙)

```css
div.container p{background:red;}
<div class="container">
<p>我是lilei</p> //背景颜色为红色
<div>
<p>我是hanmeimei</p> //背景颜色为红色
</div>
</div>
```

#### 2.子代选择符 >：匹配直接子元素中的指定元素(子代)

```css
div.container>p{background:red;}
<div class="container">
<p>我是lilei</p> //背景颜色为红色
<div>
<p>我是hanmeimei</p> //无背景颜色
</div>
</div>
```

#### 3.相邻选择器+：匹配下一个相邻兄弟元素

```css
div.header{background:red;}
div.header+div{background:yellow;}
<div class="header"></div> //背景颜色为红色
<div class="content"></div> //背景颜色为黄色
<div class="footer"></div> 
```

#### 4.兄弟选择器~：匹配之后所有兄弟元素

```css
div.header{background:red;}
div.header~div{background:yellow;}
<div class="header"></div> //背景颜色为红色
<div class="content"></div> //背景颜色为黄色
<div class="footer"></div> //背景颜色为黄色
```

#### 5.排除选择器E:not(selector) 匹配不是selector的元素

```css
//去掉所有li的右边框，但是最后一个保留
div ul li:not(:last-child){border-right:0px;}

<div>
<ul>
<li>1</li>
<li>2</li>
<li>3</li>
</ul>
</div>

给除了class为active的元素背景都换成红色

div ul li:not(.active){background:red;}
```



### 三.属性选择符

#### 1.(E)[attr]：选择具有attr属性的E元素

```css
[name]{border:1px solid red;}//设置所有具有name属性的元素的边框为红色
```

#### 2.(E)[attr=value]：选择属性attr的属性值为value的E元素

```css
input[name="username"]{border:1px solid red;}//设置所有具有name属性，且属性值为username的元素的边框为红色
```

#### 3.(E)[attr^=value]：选择属性attr的属性值开头为value的E元素

```css
input[name^="user"]{border:1px solid red;}//设置所有具有name属性且属性值开头为user的元素的边框为红色
```

#### 4.(E)[attr$=value]：选择属性attr的属性值结尾为value的E元素

```css
input[name$="d"]{border:1px solid red;}//设置所有具有name属性且属性值结尾为d的元素的边框为红色
```

#### 5.(E)[attr~=value]：选择属性attr的属性值包含value的E元素

```css
input[name~="user"]{border:1px solid red;}//设置所有具有name属性且属性值包含user的元素的边框为红色
<input type="text" name="user a">
```

#### 6.(E)[attr*=value]：选择属性attr的属性值包含value的E元素···

```css
input[name*="user"]{border:1px solid red;}//设置所有具有name属性且属性值包含user的元素的边框为红色
<input type="text" name="username">
```

#### 7.(E)[attr|=value]：选择属性attr的属性值包含value的E元素

```css
input[name|="user"]{border:1px solid red;}//设置所有具有name属性且属性值包含user的元素的边框为红色
<input type="text" name="user-a">
```

**几个容易混淆的选择器:**

|        ~=        |  *=  |      \|=      |   =    |
| :--------------: | :--: | :-----------: | :----: |
| 包含字符必须是独立单词,空格连接 | 包含即可 | 开头必须是独立单词，—连接 | 开头包含即可 |

### 四.伪类选择器

#### 1.链接伪类 

* :link 访问之前
* :visited 访问之后 

```css
a:link{color:red} //访问之前是红色
a:visited{color:pink} //访问之后是粉色
```

#### 2.动态伪类 :active被激活后  :hover悬停时  :focus获取焦点时

* :active被激活后
* :hover悬停时
* :focus获取焦点

```css
a:active{color:green} //按住不放激活时为绿色
a:hover{color:blue}//鼠标悬停为蓝色

input:focus{border:2px solid blue;}//文本框获取焦点时边框变为2px蓝色
```

#### 3.状态伪类：

- :disabled 禁用

```css
input[type="submit"]:disabled{
background:#eee;
}
```

- :enabled 可用

```css
input[type="submit"]:enabled{
background:pink;
}
```

- :checked 被选中

```css
input[type="checkbox"]:checked{
background:red;
}
```

- :target 被链接的锚点

url后链接锚点，指向文档中的某个元素，这个元素为目标元素，E:target用来选取这个目标元素

```css
#demo1:target{background:red;} //点击a后，背景为红素
#demo2:target{background:yellow;} //点击a后，背景为黄色
<a href="#demo1"></a>
<a href="#demo2"></a>
<div id="demo1"></div>
<div id="demo2></div>
```

#### 4.后代选择器

##### 4.1模糊查找选择器

* E:first-child 匹配父元素下的第一个子元素E

```css
ul li:first-child{background:red}
<ul>
<li>li1</li>//背景红色
<li>li2</li>
</ul>
```

**注意：如果父元素子代不全是E,first-child会匹配几个连续E元素中的第一个**

```css
ul li:first-child{background:red}
<ul>
<p>p1</p>
<li>li1</li>
<p>p2</p>
<li>li2</li>//背景红色
<li>li3</li>
</ul>
```

* E:last-child  匹配父元素下的最后一个子元素E
* E:only-child 匹配子元素E是父元素唯一子元素的E元素

```css
div>p:only-child{background:red;}
<div>
<p>p1</p> //红色，必须只有一个子元素p才会生效
</div>
<div>
<p>p2</p>
<p>p3</p>
</div>
```

* E:nth-child(n) 匹配父元素下第n个直接子元素，如果第n个子元素不是E元素，则无效

```css
nth-child(2n)/nth-child(even) 匹配所有序号为偶数的直接子元素
nth-child(2n-1)/nth-child(odd) 匹配所有序号为奇数的直接子元素
```

* E:nth-last-child(n) 匹配父元素倒数第n个直接子元素，如果第n个子元素不是E元素，则无效

```css
nth-child(2n)/nth-child(even) 匹配所有序号为偶数的直接子元素
nth-child(2n-1)/nth-child(odd) 匹配所有序号为奇数的直接子元素
```

4.1精确查找选择器

* E:first-of-type 匹配父元素下第一个E元素
* E:last-of-type 匹配父元素下最后一个E元素
* E:only-of-type 匹配子元素E是父元素的子元素下唯一E元素的E元素


* E:nth-of-type(n) 匹配父元素下第n个直接子元素E


* E:nth-last-of-type(n)  匹配父元素下倒数第n个直接子元素E


**两者的区别**

| E:first-child | E:first-of-type |
| :-----------: | :-------------: |
|  必须是连续E中的第一个  |     就是第一个E      |



|   E:nth-child(n)   | E:nth-of-type(n) |
| :----------------: | :--------------: |
| 如果子元素第n个是E会匹配，否则无效 |      就是第n个E      |

```css
 div>p:nth-child(2) {background: red;}
  div>p:nth-child(3) {background: yellow;}
 <div>
        <span>ss</span>
        <p>p1</p> //背景变红
        <span>ss</span>
        <p>p2</p> //无效，因为他是第四个子元素
        <p>p3</p>
 </div>
```

```css
 div>p:nth-child(2) {background: red;}
 div>p:nth-child(3) {background: yellow;}
 <div>
        <span>ss</span>
        <p>p1</p> 
        <span>ss</span>
        <p>p2</p> //背景变红,第二个p
        <p>p3</p> //背景变红,第三个p
 </div>
```

| E:only-child  | E:only-of-type  |
| :-----------: | :-------------: |
| E必须是父元素的唯一子元素 | E必须是父元素子元素下唯一的E |

### 五.伪对象选择器

#### 1.E:first-letter: 匹配E第一个首字母

#### 2.E:first-line: 匹配E第一行

#### 3.E::before    匹配E元素前

#### 4.E::after 匹配E元素后

#### 5.E::placeholder  匹配表单E输入框未输入之前里文本内容

#### 6.E::selection 匹配表单E下拉框选中时






















