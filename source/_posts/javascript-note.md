---
title: javaScript Evernote
date: 2016-12-07 16:38:36
categories: javaScript
tags: javaScript
comments: false
---

#### 异步请求
```javascript
function success(text) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = text;
}
function fail(code) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = 'Error code: ' + code;
}
// 新建XMLHttpRequest对象
var request;
if (window.XMLHttpRequest) {
    request = new XMLHttpRequest();
} else {
    request = new ActiveXObject('Microsoft.XMLHTTP');
}

request.onreadystatechange = function() { // 状态发生变化时，函数被回调
    if (request.readyState === 4) { // 成功完成
	    // 判断响应结果:
	    if (request.status === 200) {
		    // 成功，通过responseText拿到响应的文本:
		    return success(request.responseText);
	    } else {
		    // 失败，根据响应码判断失败原因:
		    return fail(request.status);
	    }
	} else {
	     // HTTP请求还在继续...
    }
}

// 发送请求:
request.open('GET', '/api/categories');
request.send();
```

#### 数组去重函数
```javascript
Array.prototype.delrep = function(fun) {
    if(!fun) {
        fun = function(d) {return d;};
    }
    var newArr = [];
    this.sort(function(a, b) {
        return fun(a) > fun(b) ? -1 : 1;
    });
    newArr.push(this[0]);
    this.forEach(function(d) {
        if(fun(d) != fun(newArr[0])) {
            newArr.unshift(d);
        }
    });
    return newArr;
};

// 对于基本类型数组
// 来源https://segmentfault.com/a/1190000000437452
[1,2,3,4,5,5,6,6,5].delrep(); //输出[1, 2, 3, 4, 5, 6]

// 对于对象数组
var data = [
    {
        name: "aaa",
        value: 123
    },
    {
        name: "bbb",
        value: 234
    },
    {
        name: "aaa",
        value: 789
    }
];
console.log(data2.delrep(function(d) {return d.name;}));
//输出
[
	{
	    name: "bbb",
	    value: 234
	},
	{
	    name: "aaa",
	    value: 789
	}
];
```


#### 验证码 倒计时60秒
```javascript
var time = 60;
function countDown(which){

  if(time == 0 ){
    $(which).text("重新获取");
    $(which).removeAttribute("disabled");
    time = 60;
  }else {
    $(which).text("等待"+time+"秒");
    $(which).attr("disabled",true);
    time--;
    setTimeout(function(){
      countDown(which)
    },
    1000)
  }
}
```


#### 字符串剪裁
```javascript
/**
 * 在拼接正则表达式字符串时，消除原字符串中特殊字符对正则表达式的干扰
 * author:meizz
 * version: 2010/12/16
 * param               {String}        str     被正则表达式字符串保护编码的字符串
 * return              {String}                被保护处理过后的字符串
*/
function escapeReg(str) {
        return str.replace(new RegExp("([.*+?^=!:\x24{}()|[\\]\/\\\\])", "g"), "\\\x241");
}  

/**
 * 删除URL字符串中指定的 Query
 * author:meizz
 * version:2010/12/16
 * param               {String}        url     URL字符串
 * param               {String}        key     被删除的Query名
 * return              {String}                被删除指定 query 后的URL字符串
*/

function delUrlQuery(url, key) {
        key = escapeReg(key);
        var reg = new RegExp("((\\?)("+ key +"=[^&]*&)+(?!"+ key +
  "=))|(((\\?|&)"+ key +"=[^&]*)+$)|(&"+ key +"=[^&]*)", "g");
        return url.replace(reg, "\x241")
}  

// 应用实例
var str = "http://www.xxx.com/?pn=0";   // 删除指定字符 pn=0
delUrlQuery(str, "pn");
```


#### 判断浏览器向上/下滚动
```javascript
var topf = 0;
window.onscroll = function() {
	tops = document.documentElement.scrollTop || document.body.scrollTop < topf ? alert("页面正在向上滚") : topf = document.documentElement.scrollTop || document.body.scrollTop;
}
```

------------------------------

#### 循环中的闭包
来源[javascript秘密花园](http://bonsaiden.github.io/JavaScript-Garden/zh/#function.closures)

一个常见的错误出现在循环中使用闭包，假设我们需要在每次循环中调用循环序号。

```javascript
for(var i = 0; i < 10; i++) {
    setTimeout(function() {
        console.log(i);  
    }, 1000);
}
```

上面的代码不会输出数字 `0` 到 `9`，而是会输出数字 `10` 十次。

当 `console.log` 被调用的时候，匿名函数保持对外部变量 `i` 的引用，此时 for循环已经结束， `i` 的值被修改成了 `10`。

为了得到想要的结果，需要在每次循环中创建变量 i 的拷贝。

**避免引用错误**

为了正确的获得循环序号，最好使用 匿名包装器（译者注：其实就是我们通常说的自执行匿名函数）。

```javascript
for(var i = 0; i < 10; i++) {
    (function(e) {
        setTimeout(function() {
            console.log(e);  
        }, 1000);
    })(i);
}
```

外部的匿名函数会立即执行，并把 `i` 作为它的参数，此时函数内 `e` 变量就拥有了 `i` 的一个拷贝。

当传递给 `setTimeout` 的匿名函数执行时，它就拥有了对 `e` 的引用，而这个值是不会被循环改变的。

有另一个方法完成同样的工作，那就是从匿名包装器中返回一个函数。这和上面的代码效果一样。

```javascript
for(var i = 0; i < 10; i++) {
    setTimeout((function(e) {
        return function() {
            console.log(e);
        }
    })(i), 1000)
}
```

---------------------------------
