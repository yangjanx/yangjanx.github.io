---
title: '初探Javascript函数式编程'
layout: post
tags:
    - functional programming
category: Javascript
---

<!--more-->
<!-- ![image](link) -->
## 函数式编程
> 函数式编程(Functionnal programming)起源于lambda(λ)演算，这是一种在20世纪30年代开发的用于研究可计算性，“Entscheidungs”问题，函数定义、应用和递归的系统。在计算机科学中，函数式编程是一种编程范式：将计算视为数学函数并避免改变状态和数据。它是一种声明式编程范例，在函数代码中，函数输出的值仅取决于传递给函数的参数。

## 纯函数 Pure Functions
**纯函数的三个原则**
* 变量都只在函数作用域内获取, 或作为的函数的参数传入
* 引用透明(Referentially transparent)
* 相同的输入保证相同的输出(same input -> same ouput)
```javascript
//纯函数
const add = (x,y) => x + y;

//反例
let name = 'Ryan';
const getName = () => name; //非纯函数
const setName = newName => {    //非纯函数
    name = newName;
};
const printUpperName = () => {  //非纯函数
    console.log(name.toUpperCase());
};
```
**纯函数的优点**
* 易于测试
* 因相同的输入总是得到相同的结果，故其结果可以缓存
* 不会产生副作用(Side effects), 不会改变被传入的数据或者其他数据

## 声明式编程 Declarative Style Programming
* 命令式(Imperative)编程：要实现某个功能，利用指令一步一步的要求程序执行，通过一步步修改它们的状态或值，以达到需要的结果。
```javascript
//输入一个数组，将每一项乘以2后输出
let arr = [1,2,3];
for(let i=0,len=arr.length;i<len;i++){
    arr[i] *= 2;
};
console.log(arr) //[2,4,6]
```
* 声明式(Declarative)编程：告诉程序我们需要什么结果，声明我们的用意，而无需提供具体的指令，所有的细节指令都将在函数内部完成。
```javascript
//输入一个数组，将每一项乘以2后输出
let arr = [1,2,3];
arr = arr.map(item => item * 2);
console.log(arr) //[2,4,6]
```

## 一等公民 First Class Functions
```javascript
const multiply = (x,y) => x * y;    //函数表达式

function add(x,y) {    //函数声明 --- 存在函数提升
    return x + y;
};

const addAlias = add;

const evens = [1,2,3].map(item => item * 2);
```

## 闭包 Closures
> 广义上说，闭包就是指一个变量在他自身作用域外被使用了，就叫发生了闭包
闭包允许我们封装某些需要的状态，特别在偏函数应用(partial application)，柯里化(currying)和组合函数(composing functions)中,这使得我们可以用非常小的模块来构建高阶模式。
```javascript
//一个简单的闭包
function fun() {
    var d = 1;
    return function(){
        console.log(d++)
    }
}
```

```javascript
//计数器例子
let makeCounter = function() {
    let privateCounter = 0;
    function changeBy(val) {
        privateCounter += val;
    }
    return {
        increment: function() {
            changeBy(1);
        },
        decrement: function() {
            changeBy(-1);
        },
        value: function() {
            return privateCounter;
        }
    }
};

let Counter1 = makeCounter();
let Counter2 = makeCounter();
console.log(Counter1.value());  //0
Counter1.increment();
Counter1.increment();
console.log(Counter1.value());  //2
Counter1.decrement();
console.log(Counter1.value());  //1
console.log(Counter2.value());  //0
```
每次调用其中一个计数器时，通过改变这个变量的值，会改变这个闭包的词法环境。然而在一个闭包内对变量的修改，不会影响到另外一个闭包中的变量。<br>
**以这种方式使用闭包，提供了许多与面向对象编程相关的好处 —— 特别是数据隐藏和封装。**

## 柯里化 Currying
> Currying(柯里化) 又称部分求值。一个curring的函数首先会接受一些参数，接受了这些参数之后，该函数不会立即求值，而是继续返回另一个函数，刚才传入的参数在函数形成的闭包中被保存起来，待到函数被真正需要求值的时候，之前传入的所有参数都会被一次性用于求值。
```javascript
//一个简单的柯里化函数
const createAdder = x => y => x + y;
const add3 = createAdder(3);
add3(2);    //5
add3(3);    //6
```

## 偏函数应用 Partial Application
> Partial Application(偏函数应用) 是指使用一个函数并将其应用一个或多个参数，但不是全部参数，在这个过程中创建一个新函数。

## 高阶函数

## 尾递归优化 Tail Call Optimization