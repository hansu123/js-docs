# 数组API

####    1.concat——数组连接

```js
arr1.concat(arr2);
```
####    2.join——将数组以固定字符连接输出字符串。
```js
arr.join('|');//表示将数组以|连接，并输出为字符串
```
####    3.slice——分割数组中的元素并返回分割字符。

> 用法： slice(起始下标，结束下标 //<font color="red">不包含</font>)，负数表示倒数第n个<br>

```js
arr.slice(2);//表示分割从str[2]开始一直到结束
arr.slice(2,5);//表示从str[2]开始一直到str[5]结束,但不包含str[5]
```
####    4.splice——删除数组中的元素，并返回分割字符。

> 用法： splice (起始下标，删除的数量，替换的值1，替换的值2...）负数表示倒数第n个，如果只有一个替换值，代表只替换第一个删除元素的值

```js
arr.splice(1,2,"y","x");//从str[0]开始删除2个元素，并用值进行替换
```
####    5.sort——数组排序默认按照unicode码<br>
> 从小到大:<br>

```js
arr.sort(function(a,b){a-b;});
```
> 从大到小:<br>

```js
arr.sort(function(a,b){b-a;});
```
####    6.reverse——将数组倒置，及逆序排列，用法：<br>
```js
arr.reverse();
```
####    7.toString——将数组转化为字符串并以 ',' 分割。<br>

> 返回的是以逗号隔开的字符串

```js
arr.toString();
```
####    8.push——向数组的末尾添加一个元素。<br>

> 返回的是添加后的长度

```js
arr.push();
```
####    9.pop——删除数组末尾的一个元素。<br>

> 返回的是删除元素的值。

```js
arr.pop();
```
####    10.unshift——向数组的开头添加一个元素。<br>

> 返回的是添加后的长度。

```js
arr.unshift();
```
####    11.shift——删除数组开头第一个元素。<br>

> 返回的是删除的元素。

```js
arr.shift();
```
