---
title: '[转载]ES5中数组Array.prototype方法的使用'
date: 2017-03-08 16:08:57
categories: javaScript日常笔记
tags: array
---

熟悉ES5中数组Array.prototype方法的使用

> 转载：[你还在用for循环大法麽？](https://shimo.im/doc/VXqv2bxTlOUiJJqO/)

```javascript
Array.prototype.indexOf
Array.prototype.lastIndexOf
Array.prototype.every
Array.prototype.some
Array.prototype.forEach
Array.prototype.map
Array.prototype.filter
Array.prototype.reduce
Array.prototype.reduceRight
```

## indexOf()

`indexOf()` 方法返回在该数组中第一个找到的元素位置，没有则返回 `-1`

**使用 for:**
```javascript
var arr = ['apple','orange','pear'],
    found = false;
for(var i= 0, l = arr.length; i< l; i++){
    if(arr[i] === 'orange'){
        found = true;
    }
}
console.log("found:",found);
```

**使用 indexOf:**
````javascript
var arr = ['apple','orange','pear'];  
console.log("found:", arr.indexOf("orange") != -1);
```

## lastindexOf()

`lastindexOf()` 方法返回在该数组中最后一个找到的元素位置，与 indexOf 相反

## every()

`every()` 可以监测数组中的每一项是否符合条件

**使用 for:**

```javascript
/* 
* 是否全部大于0
*/
var ary = [12,23,24,42,1];
var result = function(){
  for (var i = 0; i < ary.length; i++) {
    if(ary[i] < 0){
       return false;
    }
  }
  return true; //需全部满足
}
console.log(result()) //全部满足,返回true
```

**使用 every:**
```javascript
var ary = [12,23,24,42,1];
var result = ary.every(function(item, index){
  return item > 0
})
console.log(result)
```


## some()

`some()` 可以监测数组中的某一项是否符合条件

**使用 for:**
```javascript
/* 
* 是否存在小于0的项
*/
var ary = [12,23,-24,42,1];
var result = function(){
  for (var i = 0; i < ary.length; i++) {
    if(ary[i] < 0){
       return true;
    }
  }
  return false; //只需满足一个
}
console.log(result())  //有一项小于0，返回true
```

**使用 some:**
```javascript
var ary = [12,23,-24,42,1];
var result = ary.some(function(item, index){
  return item < 0
})
console.log(result)
```


## forEach()

`forEach()` 为每个元素执行对应的方法，是用来替换for循环的

**使用 for:**
```javascript
var arr = [1,2,3,4,5,6,7,8];

for(var i= 0, l = arr.length; i< l; i++){
  console.log(arr[i]);
}
```

**使用 forEach:**
```javascript
var arr = [1,2,3,4,5,6,7,8];

arr.forEach(function(item,index){
  console.log(item);
});
```

## map()

'map()' 对数组的每个元素进行一定操作（映射）后，会返回一个新的数组，**是处理服务器返回数据时是一个非常实用的函数**

**使用 for:**
```javascript
var oldArr = [{
   first_name:"Colin",last_name:"Toh"
 },
 {
   first_name:"Addy",last_name:"Osmani"
 },
 {
   first_name:"Yehuda",last_name:"Katz"
 }];

function getNewArr(){
  var newArr = [];
  for(var i= 0, l = oldArr.length; i< l; i++){
    var item = oldArr[i];
    item.full_name = [item.first_name,item.last_name].join(" ");
    newArr[i] = item;
  }
  return newArr;
}
console.log(getNewArr());
```

**使用 map:**
```javascript
var oldArr = [{
   first_name:"Colin",last_name:"Toh"
 },
 {
   first_name:"Addy",last_name:"Osmani"
 },
 {
   first_name:"Yehuda",last_name:"Katz"
 }];

function getNewArr(){
  return oldArr.map(function(item,index){
    item.full_name = [item.first_name,item.last_name].join(" ");
    return item;
  });

}
console.log(getNewArr());
```

------

## forEach 与 map 的区别

**语法：**

`forEach` 和 `map` 都支持2个参数：一个是回调函数（item,index,list）和 [上下文](http://note.youdao.com/noteshare?id=aa25aa404e4d92b9ed40ef79a46f91da)；

>forEach：用来遍历数组中的每一项；这个方法执行是没有返回值的，对原来数组也没有影响；数组中有几项，那么传递进去的匿名回调函数就需要执行几次；每一次执行匿名函数的时候，还给其传递了三个参数值：数组中的当前项item,当前项的索引index,原始数组list；**理论上这个方法是没有返回值的，仅仅是遍历数组中的每一项，不对原来数组进行修改；但是我们可以自己通过数组的索引来修改原来的数组**；

`forEach` 方法中的 `this` 是 `ary`,匿名回调函数中的 `this` 默认是 `window`；

```javascript
var ary = [12,23,24,42,1];
var res = ary.forEach(function (item,index,input) {
  input[index] = item*10;
})
console.log(res);//-->undefined;
console.log(ary);//-->会对原来的数组产生改变；
```

>map： 和 forEach 非常相似，都是用来遍历数组中的每一项值的，用来遍历数组中的每一项；
区别：map的回调函数中支持 return 返回值；return 的是啥，相当于把数组中的这一项变为啥（并不影响原来的数组，只是相当于把原数组克隆一份，把克隆的这一份的数组中的对应项改变了）；

不管是forEach还是map 都支持第二个参数值，第二个参数的意思是把匿名回调函数中的this进行修改。

```javascript
var ary = [12,23,24,42,1];
var res = ary.map(function (item,index,input) {
  return item*10;
})
console.log(res);//-->[120,230,240,420,10];
console.log(ary);//-->[12,23,24,42,1]；
```

------


## filter()

`filter()` 方法创建一个新的匹配过滤条件的数组

**使用 for:**

```javascript
var arr = [
  {"name":"apple", "count": 2},
  {"name":"orange", "count": 5},
  {"name":"pear", "count": 3},
  {"name":"orange", "count": 16},
];
var newArr = [];
for(var i= 0, l = arr.length; i< l; i++){
  if(arr[i].name === "orange" ){
    newArr.push(arr[i]);
  }
}
console.log("Filter results:",newArr);
```

**使用 filter:**

```javascript
var arr = [
  {"name":"apple", "count": 2},
  {"name":"orange", "count": 5},
  {"name":"pear", "count": 3},
  {"name":"orange", "count": 16},
];
var newArr = arr.filter(function(item){
  return item.name === "orange";
});
console.log("Filter results:",newArr);
```


## reduce()

我好像不理解这东西，原文就不放了......暂空

## isArray()

`isArray()` 是Array对象的一个静态函数，用来判断一个对象是不是数组

```javascript
var ary1 = [];
var res1 = Array.isArray(ary1);  // Output: true
console.log(res1)

var ary2 = new Array();
var res2 = Array.isArray(ary2);  // Output: true
console.log(res2)

var ary3 = [1, 2, 3];
var res3 = Array.isArray(ary3);  // Output: true
console.log(res3)

var ary4 = new Date();
var res4 = Array.isArray(ary4);  // Output: false
console.log(res4)
```