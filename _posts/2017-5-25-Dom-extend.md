---
layout: post
title: DOM Extend
category: blog
description: js-note 第11章
---

2008年由w3c设立统一标准，之前都是各家乱战。

### 标准扩展

**选择器扩展**

    someElement.querySelector();

获取符合选择条件的元素集合的第一个元素，传入参数为筛选条件，可以是各种选择符，相当强大。可以由document或者eLement调用    
    
    someElement.querySelectorAll();
    
获取符合条件的元素集合，本质上是一个附带元素属性方法的NodeList,但在实现上却不是跟`document.getElentsByTagName()`返回类似的动态集合，而是一个集合当前情况的快照。

IE里定义了一个`matchesSelector()`方法，用户检测一个元素是否会被`querySelector()`与`querySelectorAll()`方法返回，具体见[内置文档](https://github.com/Lijiazhen/Js-note/blob/gh-pages/dom.js)

**元素遍历**

因为IE9之前与其他浏览器下进行元素遍历时对文本节点的实现（IE不会返回文本节点）不一致,所以重新定义了一组属性

    someNode.firstElementChild //返回第一个子元素
    someNode.lastElementChild //返回最后一个子元素
    someNode.nextElementSibling //返回下一个兄弟元素
    someNode.previousElementSibling //返回上一个兄弟元素
    someNode.childElementCount  //返回子元素的个数
    
**classList**

在DOM1级中，我们要对元素的class属性进行操作,只有使用`getAttribute()`方法或者使用`className`属性获得元素的class属性值后再进行字符串操作，但这样太麻烦，所以在DOM EXTEND里面，重新定义了classList属性及附带方法

    someElement.classList.add(class);//添加新的类名
    someElement.classList.remove(class);//移除类名
    someElement.classList.toggle(class);//有则移除，无则添加
    someElement.classList.contains(class);//是否包含有给定的类名
    
classList IE9+支持

**getElementsByClassName()**

可用document或者element来调用，传入类名不分先后

IE9+支持

**焦点管理**

    document.activeElement//获取当前焦点所在元素,默认为body
    someElement.focus()//使元素获得焦点
    document.hansFocus();//判断文档是否获得焦点，可以此判断用户是否在与页面进行交互
    
    
**HTMLDocument的变化**

    document.readyState//文档的加载状态

有两个取值`loading`正在加载，文档与`complete`已经加载完毕

    document.compatMode//文档模式
    
返回`BackCompat`为混合模式，返回`css1Compat`为标准模式

    document.charset//文档当前使用的字符集
    document.defaultCharset//默认的字符集
    
通过这两个属性可以得到文档使用的字符编码的具体信息，也能对字符编码进行准确的控制。

>HTML5扩展中规定，自定义属性要加上data前缀


**innerHTML标记插入**

在DOM1级的时候,我们添加元素使用`appendChild()`方法，但构建元素结构的过程有些繁琐。所以有了innerHTML属性.

    element.innerHTML

在读模式下，这个属性会返回调用元素的所有子元素标签，包括元素，文本，注释

    element.innerHTML="<div>this is a childElement.</div>"

在写模式下，这个属性会重写调用元素的节点树。

值得注意的一点是，*不是所有浏览器对这个属性的读模式实现是相同的*,另外，IE8下在使用`innerHTML`属性进行写模式时，判定`<script></script>`与`<style></style>`为无作用域元素（也就是在页面上不可见的元素），要插入这类元素有三种方法：

1. 必须在其前面添加有意义的文本或者符号，过后再删除
2. 用一个容器包裹起来，并且这个容器本身要有内容
3. 使用`<input type="hidden"  />`来包裹这些无作用域元素。

因为前两种方法过后还用想办法清除前置文本字符或包裹元素所以推荐的是第三种方法

>并不是所有元素都支持这个属性

在使用`innerHTML`进行标记插入时最好要检查插入标记的正确性，IE8为此提供了`window.toStaticHTML()`,这个文档传入一个HTML字符串，返回一个处理后符合标准的版本。

**outerHTML属性**

    element.outerHTML
    
同`innerHTML`类似，获取元素的集合，但outerHTML是返回包括调用本身在内的集合

    element.outerHTML="<div>this is div.</div>"
    
如果用 `outerHTML`进行写模式的话，则相当于会把调用元素替换掉

**insertAdjacentHTML()方法**

这是最后一个插入标记的方法，记住，这是方法.
有两个参数，插入位置与要插入的html文本。插入位置必须是以下四个值之一。

1. “beforebegin”,在当前元素之前插入一个紧邻的同辈元素。
2. “afterbegin”,在当前元素的第一个子元素之前插入一个新的元素
3. “beforeend”,在当前元素的最后一个子元素之后插入一个新的元素
4. “afterend”,在当前元素之后插入一个紧邻的同辈元素

需要注意的是这个参数都是小写

三个插入文档的属性/方法都已经介绍，**在插入标记前最好是检查元素的正确性，而写模式下最好先手动删除要被替换的元素的所有处理事件**


**scrollIntoView()方法**

可以在所有HTML元素上调用，参数为`true`或`false`.当参数为true时，会尽量让元素与视窗顶部平齐。如果参数是false，则会让元素完全显示出来。见[scrollIntoView]()

### 专有扩展

还有一些非标准的扩展

**文档模式**

前面我们说标准扩展里有一个兼容模式`document.compatMode`.而IE里获取文档模式可以用以下属性

    document.documentMode;
    
**children属性**

    var childCount=element.children.length;
    var firstChild=element.children[0]

children这个属性现在基本已经可以拿来做通用，只不过IE8及之前版本中会的返回会包括注释节点，而IE9及以后的就只返回元素节点

**contains()方法**

IE最先引入的方法，用于检测传入元素是否是调用元素的子节点，是返回ture,不是返回false;DOM level 3中的 `compareDocumentPositin()`也能确定节点间的关系。通用方法见[内置文档](https://github.com/Lijiazhen/Js-note/blob/gh-pages/dom.js)

**插入文本**

    element.innerText
    element.outerText
    
基本跟innerTHML的概念差不多，只不过火狐不支持这个属性,所以还是写个通用，见[内置文档](https://github.com/Lijiazhen/Js-note/blob/gh-pages/dom.js)    


    
    
