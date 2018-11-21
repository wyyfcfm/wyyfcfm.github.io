---
layout:     post   				
title:      ES6-第十五节-Iterator和for...of循环			
subtitle:   ES6入门学习总结  
date:       2018-10-24 			
author:     BY wyy						
header-img: img/post-sample-image.jpg 	
catalog: true 					
tags:					
    - ES6
---

## 1.概念
Iterator（遍历器）的概念    
JavaScript 原有的表示“集合”的数据结构，主要是数组（Array）和对象（Object），ES6 又添加了Map和Set。  
这样就有了四种数据集合，用户还可以组合使用它们，定义自己的数据结构，比如数组的成员是Map，Map的成员是对象。   
这样就需要一种统一的接口机制，来处理所有不同的数据结构。   
遍历器（Iterator）就是这样一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。  
任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。  
Iterator 的作用有三个：  
- 1.是为各种数据结构，提供一个统一的、简便的访问接口；  
- 2.是使得数据结构的成员能够按某种次序排列；  
- 3.是 ES6 创造了一种新的遍历命令for...of循环，Iterator 接口主要供for...of消费。  
Iterator 的遍历过程是这样的。  
（1）创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。  
（2）第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员。  
（3）第二次调用指针对象的next方法，指针就指向数据结构的第二个成员。  
（4）不断调用指针对象的next方法，直到它指向数据结构的结束位置。  
默认 Iterator 接口  
Iterator 接口的目的，就是为所有数据结构，提供了一种统一的访问机制，即for...of循环（详见下文）。当使用for...of循环遍历某种数据结构时，该循环会自动去寻找 Iterator 接口。  
一种数据结构只要部署了 Iterator 接口，我们就称这种数据结构是“可遍历的”（iterable）。  
## 2.原生具备iterator接口
原生具备 Iterator 接口的数据结构如下：  
`Array`   
`Map`  
`Set`  
`String`  
`TypedArray`  
`函数的 arguments 对象`  
`NodeList 对象`  
## 3.调用 Iterator 接口的场合
- 解构赋值   
- 扩展运算符    
- yield*    
- for...of  
- Array.from()  
- Map(), Set(), WeakMap(), WeakSet()（比如new Map([['a',1],['b',2]])）  
- Promise.all()  
- Promise.race()  
- 字符串的 Iterator 接口  
字符串是一个类似数组的对象，也原生具有 Iterator 接口。
