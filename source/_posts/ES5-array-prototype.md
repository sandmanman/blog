---
title: '[转载]数组Array.prototype方法的使用'
date: 2017-07-20 23:19:57
categories: javaScript日常笔记
tags: array
---

熟悉数组Array.prototype方法的使用

> 转载：[你还在用for循环大法麽](https://shimo.im/doc/VXqv2bxTlOUiJJqO/)  [常用数组方法](https://segmentfault.com/a/1190000002913698)


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

## 数组的增加、删除和修改

### push()

向数组末尾增加新的内容，返回的是添加后新数组的长度,原有的数组改变了

```javascript
var arr = [10,11,12,13,14,15];
var res = arr.push(16,17);
console.log(res);    //8
console.log(arr);    //[10,11,12,13,14,15,16,17]
```


### unshift()

向数组的开头增加新的内容，返回的是添加后新数组的长度，原来的数组也改变

```javascript
var res = arr.unshift(16,17);
console.log(res);   //8
```

### splice(n,m,x)

把原有数组中的某些项进行替换。（先删除，然后用x替换）。从索引n（包含n）开始，向后删除m个元素，用x替换，返回删除的数组

原有数组改变规律：

`splice(0,0,x)` 相当于unshift，`splice(arr.length,0,x)` 相当于push，`splice(n,0,x)` 向数组中间某个位置添加新的内容，从索引n开始，删除0个内容，把新增的内容x放在索引n的前面。返回的是一个空数组，原有数组改变n开始的索引 `splice(n,m)` 删除数组指定项，从所以n（包含n）开始，向后删除m个元素 ，把删除的内容当做新数组返回，原有数组改变。

```javascript
var res = arr.splice(2,0,"michael");  //在12后面添加“michael"
console.log(arr);
```

### pop()

删除数组最后一个，返回的是删除的那一项，原有数组改变。

### shift() 

删除数组第一个，返回的是删除的那一项，原有数组改变。


## 数组的查询和复制

### slice(n,m)

从索引 n（包含n）开始找到索引 m (不包含m)处。把找到的内容作为一个新的数组返回，原有数组是不改变的。

```javascript
var arr = [10,11,12,13,14,15];
var res = arr.slice(1,4);
console.log(res);   //[11, 12, 13]
console.log(arr);   //[10, 11, 12, 13, 14, 15]
slice(n)  // 从索引n（包含n）开始找到末尾
slice(0)  // slice()  将原来数组原封不动的复制一份，数组clone
```

### concat()

本意是实现数组的拼接的 `arr1.concat(arr2)` 将数组 arr2 和 arr1 合并成新的数组，原来的数组也不变。

`concat` 也可以是数组的克隆，原来的数组也改变（相当于 `slice(0)`)。

```javascript
var arr1=[10,11,12,13,14,15];
var arr2=[16,17];
var res=arr1.concat(arr2);
console.log(arr1);  //[10, 11, 12, 13, 14, 15]
console.log(res);   //[10, 11, 12, 13, 14, 15, 16, 17]
```

## 将数组转化为需要的字符串

### toString()

把数组中的每一项拿出来，用逗号隔开，组成字符串，原有数组不变。

```javascript
var arr = ["name","michael","age","24"];
var res = arr.toString();
console.log(res);   //name,michael,age,24
console.log(arr);   //["name", "michael", "age", "24"]
```

### join()

把数组中的每一项拿出来，用指定的分隔符隔开，原有数组不变。

```javascript
var arr = ["name","michael","age","24"];
var res = arr.join("|");
console.log(res);           //"name|michael|age|24"
console.log(res.length);    //19
console.log(arr);           //["name", "michael", "age", "24"]
console.log(arr.length);    //4
```

实现数组中数字的求和：

```javascript
var arr = [10,11,12,13,14,15];
var str = arr.join("+");     //"10+11+12+13+14+15"
var total = eval(str);       //eval 将指定的字符串变成真正的额表达式执行
console.log(total);          //75
console.log(arr);            //[10, 11, 12, 13, 14, 15]
```

## 排列和排序

### reverse() 

数组倒过来排列，原有数组改变。

```javascript
var arr = [10,11,12,13,14,15];
console.log(arr);           //[10, 11, 12, 13, 14, 15]
var res = arr.reverse();    //"10+11+12+13+14+15"
console.log(res);           //[15, 14, 13, 12, 11, 10]
```

### sort()

数组的排序，可以实现由大到小（由小到大），原有的数组也变。

直接写 `sort` 只能处理10以内的数字排序,处理10以上的我们需要传递一个参数，这个参数必须是函数。

```javascript
var arr = [10,12,11,19,13,15,6];
console.log(arr);           //[10, 12, 11, 19, 13, 15,6]
var res = arr.sort();
console.log(res);           //[10, 11, 12, 13, 15, 19, 6]
```

改进：

```javascript
var arr = [10,12,11,19,13,15,6];
console.log(arr);           //[10, 12, 11, 19, 13, 15,6]
var res1 = arr.sort(function(a,b){return a-b;});   //实现由小到大
console.log(res1);
var res2 = arr.sort(function(a,b){return b-a;})   //实现由大到小
console.log(res2);          //[10, 11, 12, 13, 15, 19, 6]
var arr = [10,12,11,19,13,6];
console.log(arr);           //[10, 12, 11, 19, 13, 15,6]

var res1 = arr.sort(function(a,b){
//a表示每一次循环的时候的当前项，b是后面的项
//return a-b;当前项减去后一项，如果大于0，代表前面的比后面的大，这样的话就交换位置
//冒泡排序：sort实现排序，就是遵循冒泡排序的思想实现的
console.log(a+"<====>"+b);
return a-b;});   //实现由小到大
```


