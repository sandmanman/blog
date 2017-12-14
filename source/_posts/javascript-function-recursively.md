---
title: '有关 javaScript 递归的笔记'
date: 2017-12-14 16:01:33
categories: javaScript日常笔记
tags: 递归
---

### 写在前面
近期面试过程中，深知自己的JS比较弱。这里记录下面试中提到的递归问题，希望慢慢的能够真正理解。

### 一些文字和例子


>所谓递归，是当一个函数调用自身，并且该调用做了同样的事情，这个循环持续到基本条件满足时，调用循环返回。

【转载】[js递归？通俗的描述递归，下面这个例子如何理解](https://segmentfault.com/q/1010000004963360)：

```javascript
function rec(x){
    if(x!==1){
        console.log("test1:", x); 
        rec(x-1); 
        console.log("test2", x);
     } else {
         console.log("test", x);
     }
}
rec(5);
```

题主执行下这段代码，就能看到执行顺序了，当x>1时，满足if的判断，打印test1，然后递归调用rec(x-1)，进入下一次循环，这里不会打印test2，直到x=1，打印test。然后开始执行rec(x-1)后面的部分的代码。
题主不嫌麻烦可以看如下的代码，基本上递归就是用一种优雅的方式做了下面代码做的事情。

```javascript
function rec(x){
    if(x!==1){
        console.log("test1:", x); 
        //我把调用自身函数直接写进来
        var a = x - 1;
        if(a!==1){
            console.log("test1:", a);
            //我把调用自身函数直接写进来 
            var b = a - 1;
            if(b!==1) {
              ...
              ...
              ...
            }
            console.log("test2", a)
        }
        console.log("test2", x);
     } else {
         console.log("test", x);
     }
}
rec(5);
```

【转载】[递归方法无限级菜单](http://blog.csdn.net/zy1281539626/article/details/72545957)：

```javascript
var arr = [{
    "id": 1,
    "name": "一级菜单1",
    "children":[
        {
            "id": 2,
            "name": "1-1二级菜单",
            "children":[
                {
                    "id":3,
                    "name":"1-1-1三级菜单",
                    "children":[]
                },
                {
                    "id":4,
                    "name":"1-1-2三级菜单",
                    "children":[
                        {
                            "id":5,
                            "name":"1-1-2-1四级菜单",
                            "children":[]
                        }
                    ]
                }
            ]
        }
    ]
},{
    "id": 6,
    "name": "一级菜单2",
    "children":[
        {
            "id": 7,
            "name": "2-1二级菜单",
            "children":[
                {
                    "id":8,
                    "name":"2-1-1三级菜单",
                    "children":[]
                },
                {
                    "id":9,
                    "name":"2-1-2三级菜单",
                    "children":[
                        {
                            "id":10,
                            "name":"2-1-2-1四级菜单",
                            "children":[]
                        }
                    ]
                }
            ]
        }
    ]
}];

// 解析函数
function parseJson(arr){
    if(arr.length!=0){
        function pp(arr){
            for(var i=0;i<arr.length;i++){
                if(arr[i].children && arr[i].children.length!=0){
                    console.log(arr[i].name);//这里可以写菜单的样式结构...
                    pp(arr[i].children);
                }else{
                    console.log(arr[i].name);//这里可以写菜单的样式结构...
                }
            }
        }
        pp(arr);
    }
}
parseJson(arr);
```

还有两篇看不懂的翻译文章：

[翻译连载 | 第 9 章：递归（上）－《JavaScript轻量级函数式编程》 |《你不知道的JS》姊妹篇](https://juejin.im/post/59c1d91d6fb9a00a53275f79)
[翻译连载 | 第 9 章：递归（下）－《JavaScript轻量级函数式编程》 |《你不知道的JS》姊妹篇](https://juejin.im/post/59c87fb46fb9a00a437b1a2e)


