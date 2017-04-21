---
title: '[转载]总结伪类与伪元素'
date: 2017-04-21 14:10:58
categories: CSS3
tags: 伪类,伪元素
---


## 伪类与伪元素

>CSS introduces the concepts of pseudo-elements and pseudo-classes  to permit formatting based on information that lies outside the document tree.

直译过来就是：css引入伪类和伪元素概念是为了格式化文档树以外的信息。也就是说，伪类和伪元素是用来修饰不在文档树中的部分，比如，一句话中的第一个字母，或者是列表中的第一个元素。下面分别对伪类和伪元素进行解释：

**伪类**用于当已有元素处于的某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化的。比如说，当用户悬停在指定的元素时，我们可以通过:hover来描述这个元素的状态。虽然它和普通的css类相似，可以为已有的元素添加样式，但是它只有处于dom树无法描述的状态下才能为元素添加样式，所以将其称为伪类。

**伪元素**用于创建一些不在文档树中的元素，并为其添加样式。比如说，我们可以通过:before来在一个元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在文档树中。

## 伪类与伪元素的区别

这里通过两个例子来说明两者的区别。

下面是一个简单的html列表片段：

```html
<ul>
    <li>我是第一个</li>
    <li>我是第二个</li>
</ul>
```

如果想要给第一项添加样式，可以在为第一个 `li` 添加一个类，并在该类中定义对应样式：

**HTML**

```html
<ul>
    <li class="first-item">我是第一个</li>
    <li>我是第二个</li>
</ul>
```

**CSS**

```css
li.first-item {
    color: orange
}
```

如果不用添加类的方法，我们可以通过给设置第一个 `li` 的 `:first-child` 伪类来为其添加样式。这个时候，被修饰的 `li` 元素依然处于文档树中。

**HTML**

```html
<ul>
    <li>我是第一个</li>
    <li>我是第二个</li>
</ul>
```

**CSS**
```css
li:first-child {
    color: orange
}
```

另一个简单的html段落片段：

```html
<p>Hello World, and wish you have a good day!</p>
```

想要给该段落的第一个字母添加样式，可以通过设置 `p` 的 `:first-letter` 伪元素来为其添加样式

```css
p:first-letter {
    font-size: 5em;
}
```

从上述例子中可以看出，伪类的操作对象是文档树中已有的元素，而伪元素则创建了一个文档数外的元素。因此，伪类与伪元素的区别在于：有没有创建一个文档树之外的元素。


## 伪元素是使用单冒号还是双冒号？

CSS3规范中的要求使用双冒号 `::` 表示伪元素，以此来区分伪元素和伪类，比如 `::before` 和 `::after` 等伪元素使用双冒号 `::`，`:hover` 和 `:active` 等伪类使用单冒号 `:`。除了一些低于IE8版本的浏览器外，大部分浏览器都支持伪元素的双冒号(::)表示方法。
然而，除了少部分伪元素，如::backdrop必须使用双冒号，大部分伪元素都支持单冒号和双冒号的写法，比如::after，写成:after也可以正确运行。

> 转载自AlloyTeam：http://www.alloyteam.com/2016/05/summary-of-pseudo-classes-and-pseudo-elements/

