---
layout:     post
title:      ES6-第十六节-Generator函数的语法
subtitle:   ES6入门学习总结
date:       2019-02-28
author:     BY wyy
header-img: img/post-sample-image.jpg
catalog: true
tags:
    - ES6
---
## 一、概念
  Generator 函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同。
  Generator 函数有多种理解角度。语法上，首先可以把它理解成，Generator 函数是一个状态机，封装了多个内部状态。
  执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。

## 二、用法
```
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();

hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }
```
    调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针。以后，每次调用遍历器对象的next方法，就会返回一个有着value和done两个属性的对象。value属性表示当前的内部状态的值，是yield表达式后面那个表达式的值；done属性是一个布尔值，表示是否遍历结束。

## 三、yield表达式
由于 Generator 函数返回的遍历器对象，只有调用next方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。yield表达式就是暂停标志。

遍历器对象的next方法的运行逻辑如下。

（1）遇到yield表达式，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。

（2）下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式。

（3）如果没有再遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。

（4）如果该函数没有return语句，则返回的对象的value属性值为undefined。

需要注意的是，yield表达式后面的表达式，只有当调用next方法、内部指针指向该语句时才会执行，因此等于为 JavaScript 提供了手动的“惰性求值”（Lazy Evaluation）的语法功能。
```
function* gen() {
  yield  123 + 456;
}
```
上面代码中，yield后面的表达式123 + 456，不会立即求值，只会在next方法将指针移到这一句时，才会求值。
``next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值。``

## 四、next()、throw()、return() 的共同点
     next()、throw()、return()这三个方法本质上是同一件事，可以放在一起理解。它们的作用都是让 Generator 函数恢复执行，并且使用不同的语句替换yield表达式。

 next()是将yield表达式替换成一个值。
 ```
 const g = function* (x, y) {
   let result = yield x + y;
   return result;
 };

 const gen = g(1, 2);
 gen.next(); // Object {value: 3, done: false}

 gen.next(1); // Object {value: 1, done: true}
 // 相当于将 let result = yield x + y
 // 替换成 let result = 1;
 上面代码中，第二个next(1)方法就相当于将yield表达式替换成一个值1。如果next方法没有参数，就相当于替换成undefined。
 ```
 throw()是将yield表达式替换成一个throw语句。
 ```
 gen.throw(new Error('出错了')); // Uncaught Error: 出错了
 // 相当于将 let result = yield x + y
 // 替换成 let result = throw(new Error('出错了'));
 ```
 return()是将yield表达式替换成一个return语句。
 ```
 gen.return(2); // Object {value: 2, done: true}
 // 相当于将 let result = yield x + y
 // 替换成 let result = return 2;
```

## 五、yield*表达式
用来在一个 Generator 函数里面执行另一个 Generator 函数。
```
function* bar() {
  yield 'x';
  yield* foo();
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  yield 'a';
  yield 'b';
  yield 'y';
}
```
yield*后面的 Generator 函数（没有return语句时），等同于在 Generator 函数内部，部署一个for...of循环。
```
function* concat(iter1, iter2) {
  yield* iter1;
  yield* iter2;
}

// 等同于

function* concat(iter1, iter2) {
  for (var value of iter1) {
    yield value;
  }
  for (var value of iter2) {
    yield value;
  }
}
```

## 六、yield*应用
yield*命令可以很方便地取出嵌套数组的所有成员。
```
function* iterTree(tree) {
  if (Array.isArray(tree)) {
    for(let i=0; i < tree.length; i++) {
      yield* iterTree(tree[i]);
    }
  } else {
    yield tree;
  }
}

const tree = [ 'a', ['b', 'c'], ['d', 'e'] ];

for(let x of iterTree(tree)) {
  console.log(x);
}
// a
// b
// c
// d
// e
由于扩展运算符...默认调用 Iterator 接口，所以上面这个函数也可以用于嵌套数组的平铺。

[...iterTree(tree)] // ["a", "b", "c", "d", "e"]
```

## 七、Generator 与状态机
 Generator 是实现状态机的最佳结构。比如，下面的clock函数就是一个状态机。
 ```
 var ticking = true;
 var clock = function() {
   if (ticking)
     console.log('Tick!');
   else
     console.log('Tock!');
   ticking = !ticking;
 }
 上面代码的clock函数一共有两种状态（Tick和Tock），每运行一次，就改变一次状态。这个函数如果用 Generator 实现，就是下面这样。

 var clock = function* () {
   while (true) {
     console.log('Tick!');
     yield;
     console.log('Tock!');
     yield;
   }
 };
 ```
 上面的 Generator 实现与 ES5 实现对比，可以看到少了用来保存状态的外部变量ticking，这样就更简洁，更安全（状态不会被非法篡改）、更符合函数式编程的思想，在写法上也更优雅。Generator 之所以可以不用外部变量保存状态，是因为它本身就包含了一个状态信息，即目前是否处于暂停态。
