---
layout: post
title: 对象的属性
category: blog
description: Object's property
---

对象的属性的操作

对象属性的建立与赋值大多是一起完成的，比如

    var a = {x:1,y:2};
    var b = {};
    b.x = 3;
    b.y = 4;
    
只不过用关联数组形式来操作属性的建立有些时候会方便很多

    var a = {};
    a['x'] = 1;
    var b = 'name';
    a[b] = 'Jack';
    a.name;//Jack
    
删除属性
    
    delete a.name;
    a.name;//undefined
    
但delete关键字只是断开了属性与宿主对象的联系，并没有真正对属性进行删除操作

    var p = {a:{x:1}};
    var b = p.a;
    delete p.a;
    console.log(p.a);//undefined
    console.log(b.x);//1
    
**属性的检测** 

`in`

检测某个对象中是否包含某个属性，不区分自我属性或继承属性

    var o = {x:1};
    var s = Object.create(o);
    s.y = 2;
    'x' in s;//true
    'y' in s;//true
    
`hasOwnProperty()`

检测某个对象是否包含有某个自有属性

    s.hasOwnProperty('y');//true
    s.hasOwnProperty('x');//false
    
`propertyIsEnumerable()`

检测某个对象是否包含某个自有属性，且这个属性是可枚举的

    s.propertyIsEnumerable('y');//true
    s.propertyIsEnumerable('x');//false

**属性的特性**

属性有四大特性，可读（值）/value，可写/writable,可枚举/enumerable,可配置/configurable

在存取器属性中，读跟写改成get与set


`Object.getOwnPropertyDescriptor(obj,property)`

读取属性的特性

    Object.getOwnPropertyDescriptor(s,'y');
    //{value: 2, writable: true, enumerable: true, configurable: true}
    


    
`Object.defineProperty()`

设置或修改属性的特性 

    Object.defineProperty(s,'y',{value: 3, writable: false, enumerable: true, configurable: true})`
    s.y; //3
    s.y = 4;
    s.y;//3,因为y属性已经是不可写的 

    
`Object.defineProperties()`

设置或修改过个属性的特性

    var a = {x:1,y:2};    
    Object.defineProperties(a,{
       x:{value: 3, writable: false, enumerable: true, configurable: true},
       y:{value: 4, writable: false, enumerable: true, configurable: true}
    });
    
当然，实际上你大可不必把四个属性都写上去，只要写你想要修改的特性就好。 

**对象的扩展**

这也是一个重要的特性，只不过它属于对象而不是某个具体的属性。它规定了对象是否可以添加新的的属性。


`Object.isExtensible(obj)`

可扩展性的检测

    var person = {name:'jack'};
    Object.isExtensible(person);//true

`Object.preventExtensions(obj)`

把某个对象设置为不可扩展

    Object.preventExtensions(person);
    person.age = 20;
    person.age;//undefined

`Object.seal(obj)`

把某个对象设置为不可扩展并且把该对象所有属性设置为不可配置

可以用`Object.isSeal(obj)`方法来检测是否设置了该特性

`Object.freeze(obj)`

把某个对象设置为不可扩展并且把该对象所有属性设置为不可配置且不可写


属性特性影响规则：

1. 当对象设置为不可扩展，不允许新增属性

2. 当属性特性为不可配置，不允许对可枚举与可配置特性做出修改
3. 当属性特性为不可配置，可写特性可以从true改为false,但不能从false改为true
4. 当属性特性为不可配置时，不允许修改存储器的get与set方法
5. 当属性特性为不可配置时，不允许把普通属性改为存储器
6. 当属性特性为不可配置且不可写的时候，属性值才真正不允许修改；如果只设置了不可写但可配置的情况下，仍然可以使用修改特性value的方法对属性值做出修改。（实际上是先改为可写，在完成修改后，再改回来）



