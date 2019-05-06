# 弹性布局

###    一.父容器的属性

><h6>1.flex-direction</h6>

#####   规定主轴排列方向

*	row:默认值水平排列 
*	column:垂直方向排列
*	row-reverse:水平翻转排列
*	column-reverse:垂直翻转排列


><h6>2.flex-wrap</h6>

#####   规定是否换行

*	nowrap:默认值不换行
*	wrap:换行
*	wrap-reverse:换行，但是翻转，最后一个到最前面


><h6>3.flex-flow</h6>

(flex-direction和flex-wrap的简写)

*	row wrap:水平排列并且换行（这个可以自由组合）

><h6>4.justify-content</h6>

#####   规定主轴水平排列的对齐方式

*	flex-start:默认值，左对齐
*	flex-end:右对齐
*	center:中间对齐
*	space-between:两端对齐
*	space-around:均匀分布

><h6>5.align-items**(不要漏加s)**</h6>

#####   规定交叉轴对齐方式，当然一般在父容器高度大于子容器时才能看出效果

*	flex-start:顶端对齐
*	flex-end:底部对齐
*	center:中间对齐
*	baseline:第一行文字的底部对齐（容器中有文字的情况）
*	strech:未设置高度时，高度和父容器的高度一样


#####    ex：1.当主轴方向是水平排列的时候，交叉轴就是垂直的。也就是规定了垂直方向的排列方式

![](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/css-img/alignitem01.png)

#####    ex：2.当主轴方向为垂直排列的时候，交叉轴就是水平的。
flex-start就是左边顶端，flex-end就是右边顶端（相当于将水平排列的逆时针旋转来看）

![](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/css-img/alignitem02.png)

#####    ex：3.baseline就是有文字的话，根据文字底部对齐

![](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/css-img/baseline.png)


><h6>6.align-content</h6>

#####   规定了多行内容在交叉轴的排列方式（设置后，align-items将无效）,类似主轴垂直排列，然后设置主轴排列方向

*	flex-start:顶端对齐
*	flex-end:底部对齐
*	center:中间对齐
*	space-between:两端对齐
*	space-around:均匀分布
*	strech:根据它的flex-grow(后面讲)瓜分父容器高度

![](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/css-img/aligncontent.png)


### 二.flex-item（每个元素的属性）

>1.order

#####   规定元素排列顺序

这个很好理解，order的值越大排列顺序越靠前，默认值是0;

>2.flex-grow

#####   规定当父容器有剩余空间时，怎么分配，默认值是0，及不分配

假设有三个容器d1,d2,d3 如果值都是0，那么即使有剩余空间也不会被分配。

![](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/css-img/flexgrow.png)

好现在我们设定d1的flex-grow为1

![](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/css-img/flexgrow02.png)

接下来我们设定d2,d3的flex-grow分别为2,3

![](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/css-img/flexgrow03.png)

所以flex-grow的值就是瓜分所占剩余空间的比例。

这里有一个问题了，万一父容器没有剩余空间了怎么办？
如果不够的话，按元素宽度比均匀父容器空间。
比如的d1的宽度是200px,d2是100px,d3的宽度是300px,父容器是500px怎么办？
d1的宽度就是(200/600)*500px,以此类推

>3.flex-shrink

#####   规定当父容器空间不足时，元素的压缩比例。默认为1，自动压缩。

有时候我们会遇到这样的问题，父容器宽度不够了，但是我想让我的元素等比例缩小
比如的d1的宽度是200px,d2是100px,d3的宽度是300px，父容器是500px，默认是1:1:1
我就想让d1变得最小,d2变大一点，怎么办呢？
设置d1的flex-shrink为3，d3的flex-shrink为2看看会出现什么效果?

![](https://hansu-1253325863.cos.ap-shanghai.myqcloud.com/newblog/markdown/css-img/shrink.png)

至于压缩了多少空间？设压缩率为x
d1压缩的空间为:(500/(3+2+1))*3-200

>4.flex-basis 

#####   规定在主轴上占据的空间，默认值是auto，和width类似

#####   注意

*   4.1 光写flex-basis:0;无效
写flex-grow:1;flex-basis:0;表示剩余空间占满

*   4.2 简写flex:flex-grow,flex-shrink,flex-basis

>5.align-self

#####   元素自己在交叉轴的对齐方式

*	auto:父元素规定的align-content/align-items一致	
*	flex-start:顶端对齐
*	flex-end:底部对齐
*	center:中间对齐
*	baselin:第一行文字底部对齐
*	strech:根据它的flex-grow(后面讲)瓜分父容器高度



