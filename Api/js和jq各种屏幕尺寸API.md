# js和jq各种屏幕尺寸API

### 一.js

#### 1.window(window可省略)

* window.innerWidth 屏幕可视区的宽度，相对于浏览器的窗口(窗口大小变化而变化,但包含滚动条)
* window.innerHeight 屏幕可视区的高度，相对于浏览器的窗口(窗口大小变化而变化)

#### 2.screen

* screen.width()屏幕分辨率的宽度，相对于计算机(不变)
* screen.height()屏幕分辨率的高度，相对于计算机(不变)

#### 3.document

* document.body.clientWidth整个文档的宽度(padding+content)
* document.body.clientHeight 整个文档的高度(padding+content)
* document.body.offsetWidth 整个文档的宽度(padding+border+content)
* document.body.offsetHeight 整个文档的高度(padding+border+content)
* document.body.scrollWidth 整个文档正文的宽度(有横向滚动条的情况不包括border)
* document.body.scrollHeight 整个文档正文的高度(有纵向滚动条的情况不包括border)
* document.body.scrollTop 
* document.body.scrollLeft

### 四.element

* elem.offsetLeft 元素相对于父元素左边的偏移量
* elem.offsetTop 元素相对于父元素顶部的偏移量
* elem.getBoundingClientRect();
  * top/y 元素距离窗口顶部距离
  * right 元素距离窗口右侧距离
  * bottom 元素距离窗口低部距离
  * left/x 元素距离窗口左侧距离
  * width 元素宽度
  * height 元素高度

### 二.jq

#### 1.$(window)

* $(window).width() 当前窗口可视区宽度
* $(window).height()  当前窗口可视区高度
* $(window).scrollTop()  当前窗口下滑距离
* $(window).scrollLeft()  当前窗口右滑距离

#### 2.$(document.body)

* $(document.body).innerWidth()  当前窗口body的总宽度 (padding+content)
* $(document.body).innerHeight()  当前窗口body的总高度 (padding+content)
* $(document.body).outerWidth()  当前窗口body的总宽度 (margin+padding+border+content)
* $(document.body).outerHeight()  当前窗口body的总高度 (margin+padding+border+content)

### 三.event

- eventscreenX 距离左边屏幕的距离
- event.screenY 距离上边屏幕的距离
- event.clientX  距离左边浏览器窗口的距离
- eevent.clientY 距离上边浏览器窗口的距离
- event.offsetX  距离父对象左边的距离
- event.offsetY 距离父对象上边的距离

### 四.element

- $(elem).offset().Left 元素相对于父元素左边的偏移量
- $(elem).offset().Top 元素相对于父元素顶部的偏移量
- $(elem)[0].getBoundingClientRect()
  - top/y 元素距离窗口顶部距离
  - right 元素距离窗口右侧距离
  - bottom 元素距离窗口低部距离
  - left/x 元素距离窗口左侧距离
  - width 元素宽度
  - height 元素高度

![](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/docs/js/Sizeapi.jpg)



