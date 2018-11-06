---
title: 'JS map()方法和parseInt函数'
layout: post
tags:
    - blog
    - git
category: Github
---

<!--more-->

偶然间看到一道 js题：  
```
[010,"10","10","3"].map(parseInt) = ?     // [8,NaN,2,NaN]
```
对于其中原理很感兴趣，故在搜集相关资料后写此文用以记录。

## map()函数
* 返回一个新的数组，且不会改变原数组
* 按照原始数组的顺序依次处理元素

**语法：** `array.map(function(currentValue,index,arr),thisValue)`  

**参数说明：**  
|参数|描述|
|:-|:-|
|currentValue|必须，当前元素的值|
|index|可选，当前元素的索引值|
|arr|可选，当前元素所属数组|
|thisValue|可选|
```
[1,3,5].map(function(item,index){
    return item + "(" + index + ")";
});  

// ["1(0)","3(1)","5(2)"]
```  

## parseInt()函数
* `parseInt(string,radix)`接收两个参数  

|参数|描述|
|:-|:-|
|string|需要被解析的字符串|  
|radix|要解析的字符串的基数，介于2~36之间|  
**注意：**  
* 若radix未设置或设置为0，根据string来判断基数
* 若radix不在范围内，返回NaN  
```
parseInt("010")             //10
parseInt(010)               //8
parseInt("10",2)            //2
parseInt("10",1)            //NaN
```  

## .map(parseInt)  
因为`parseInt`只接收2个参数，故map中的函数就只传递`(item,index)`给`parseInt`  

因此文章开头提到的题目`[010,"10","10","3"].map(parseInt)`实际上输出的是  
```
[parseInt(010,0),parseInt("10",1),parseInt("10",2),parseInt("3"),3]  
// [8,NaN,2,NaN]
```