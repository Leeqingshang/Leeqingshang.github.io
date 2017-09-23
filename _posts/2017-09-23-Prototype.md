---
layout: post
title: 对象的原型
category: blog
description: 对象的原型
---

每个对象（null除外）都与另外一个对象相关联，而被关联的另外一个对象。“另一个”对象就是我们熟知的原型，每一个对象都从原型继承属性。————注1

在上文的定义中，我们所说的原型是值原型对象，而不是原型属性。每个对象都有自己的原型对象，但只有构造函数才自带有`prototype`属性，并且这个属性指向构造函数的原型对象。

不同方式创建的对象所拥有的原型对象也各自不同。

* 通过*直接量创建*,也叫*字面量创建*的对象。这种方式创建的对象的原型都是`Object.prototype`。也就是说Object构造函数的原型对象。而通过这种方式创建的对象的原型是对`Object`的原型对象的引用。

        let a = {x:0,y:1};
        console.log(Object.getPrototypeOf(a) === Object.prototype); //true
        //Object.getPrototypeOf()是用来访问一个对象的原型对象的方法
        
* 通过关键字`new`来创建的对象。这种方式创建的对象的原型对象，其实是构造函数的`prototype`属性的值。而第一种方式创建对象的原型对象，其实等同于`new Object()`创建的对象的原型对象，因为都指向`Object.prototype`。

        let b = new Object();
        console.log(Object.getPrototypeOf(b) === Object.prototype); //true
        let c = new Array();
        console.log(Object.getPrototypeOf(c) === Array.prototype);//true
        
        function F(){
            //code
        }
        
        let f = new F();
        
        console.log(Object.getPrototypeOf(f) === F.prototype);//true
        
* 通过`Object.create()`创建的对象。这个方法接受两个参数，其中第一个参数是一个对象，而这个对象就是新创建的对象的原型对象。也就是说可以用这个方法实现原型的继承。

        let o = Object.create({},{x:{value:1,writable:true,enumerable:false,configurable:true}});
        console.log(o);//{x:1}
        
        let s = Object.create(o);
        
        console.log(s.x); //继承自原型对象o
        


到这里我们知道除了第三种方法创建的对象，其他对象的原型基本上据说其构造函数的`prototype`属性的值。所以，是先有了构造函数的“原型”属性，才有的后面的实例对象的原型对象，这个对象其实就是对构造函数的原型属性的值的引用。那么，在最开始，构造函数的原型对象是什么呢？答案是`ƒ () { [native code] }`也就是最开始的构造函数（全局Function）的原型对象，也就是`Function.prototype`，所有其他的构造函数都是初始函数的实例。

可能你会问那么Object是不是最开始也是一个构造函数，话是没错的，但在js里一切皆对象。何况我还有直接量可以创建对象。

****Function继承自Function.prototype，并且Function.prototype不允许修改。****————注2


注1：引用《JavaScript权威指南》6.1.3

注2：[详情查看](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/prototype)