---
title: 分类
date: 日期
type: "categories"
tags: 
  - js基础
categories: 
  - 前端
comments: false

---

# js基础-day-04

# 小娜V1.0

## 需求

- 输入q：完全退出；
- 输入1：得到求和功能，我们输入"数字,数字,数字"的格式，帮我们计算出我们的输入的和；
- 输入2：获得当前时间；
- 输入3：随机给我讲一个笑话；
- 不q：一直循环提示 可以 输入1/2/3、q这些功能点；

## 功能

### 获取时间

- 需求：输入2，获取当前时间；
- 在js中，要获取系统的当前日期和时间，需要用到一个js自带的**Date对象** ，
- **现在先不用管什么是对象，先学习如何使用，对象能给我们带来什么**

```js
// 创建Date对象
var date = new Date();
console.log(data); // 系统时间不同，输出的结果也会不同，但是都是输出当前系统的时间

// 获取时间对象的各个部分，对象.方法();

// 获取年份
var year = date.getFullYear();
console.log(year);

// 获取月份 ， 得到的月份是从0开始的 ，使用 0-11 表示 1-12 月
var month = date.getMonth();
console.log(month);

// 获取天
var day = date.getDate();
console.log(day);

// 周几：getDay() 返回值是 0（周日） 到 6（周六） 之间的一个整数。

// 获取小时
var hour = date.getHours();
console.log(hour);

// 获取分钟
var minute = date.getMinutes();
console.log(minute);

// 获取秒数
var second = date.getSeconds();
console.log(second);
```

- 为了格式上的好看：单位数，补位成双位数；

```js
if(day < 10){
    day = '0' + day;
}
```

* 字符串拼接：

```js
      // es5 字符串拼接
      // year + "-" + month + "-" + date + " " + hour + ":" + minute + ":" + second;

      // es6 字符串拼接；``
      var str = `${year}-${month}-${date} ${hour}:${minute}:${second}`;
```



### 随机笑话

- 需求：输入3，随机给我讲个笑话；
- 随机值：每次给你的值，一般情况下是不一样的。
- 有给随机值的一个对象Math；

```js
// 获取随机数
var r = Math.random();

// 输出一个在 [0,1) 之间的浮点数，可以得到0，但是无法得到1
console.log(r); 

```

- 如果想要得到一个随机整数，需要把整机浮点数  乘以 一个 倍数，再取整，

```js
// 获取 [0,10) 之间的随机浮点数，随机的一个值，
var r = Math.random() * 10;

```

- 取整：向下取整，向数轴的左边获取最近的一个整数

```js
// 向下取整
var a = Math.floor(1.12);
console.log(a); // 输出1
var b = Math.floor(1.99);
console.log(b); // 输出1
var c = Math.floor(-3.2);
console.log(c); // 输出 -4
var d = Math.floor(3.9);
console.log(d); // 输出 -4

```

- 获取一个随机整数

```js
// 获取一个 [0,10] 之间的随机整数
var r = Math.random();
r = r * 11 ;// 因为 Math.random得到的是不能得到1的浮点数，我们等下要向下取整，就得不到10了， * 11 向下取整才能得到10
r = Math.floor(r);
console.log(r); // 得到一个在 [0,10] 之间的整数

```

- 分析：随机来个笑话：
  - 笑话是个数字，有很多效果，我不知道要哪个；
  - 随机给我来一个，随机的下标就可以；
  - 随机的下标：随机的整数；看整数的范围：数组的长度；

```js
var index = Math.random();  // [0,1)
index *= list.length ; // [0,5)

// 最后一条永远拿不到，让最后一个笑话为空；
index = Math.floor(index); // [0,4]

```



# 函数

* 假设我们要输出一个故事

```javascript
console.log("从前有座山，山里有座庙");
console.log("庙里有个在给小和尚讲故事");
console.log("讲的是什么呢？");
console.log("老和尚对小和尚说：");
```

* 如果我想**随时随地的给别人讲**，代码就会很多；不友好；

```javascript
// 1次
console.log("从前有座山，山里有座庙");
console.log("庙里有个在给小和尚讲故事");
console.log("讲的是什么呢？");
console.log("老和尚对小和尚说：");


// 2次
console.log("从前有座山，山里有座庙");
// ......
```

## 介绍

* 函数：我们**把一段相对独立的具有特定功能的代码块封装起来**，形成一个**独立实体**，起个名字（函数名），在后续开发中可以**随时反复调用**。方法
* 作用：**封装（包起来）一段代码，将来可以随时拿来使用。**
* **封装：功能要单一；**

## 语法

```js
// function 关键字 用于声明函数
// tellStroy 函数名
function tellStroy(){
  // 里面叫函数体：我们封装，我们想随时随地拿来使用的东西；
  console.log("从前有座山，山里有座庙");
  console.log("庙里有个在给小和尚讲故事");
  console.log("讲的是什么呢？");
  console.log("老和尚对小和尚说：");
}
```

* 调用：声明的函数，知识一段代码被包起来；需要被调用，才能执行当前的函数；

```js
tellStroy(); //此时在控制台中就会输出一个故事

// 如果想输出多次，就可以调用多次这个函数
tellStroy(); 
tellStroy(); 
tellStroy(); 
```

* 起名字重名，会覆盖；和变量一样；



## 参数

### 配置参数

* 参数：对于函数来说，函数 内部的变量；
* 为什么？给别人讲故事，得不同的情况套用不同的人名把；
* 参数：形式上的参数，为什么叫形式上的参数，它就是代替位置，相当于是变量在这占了个坑；至于这个参数真实背后代表什么值，我们现在还不知道；
* 语法：

```javascript
// 在小括号内的变量，对于函数来说，就是参数；
// 参数就是函数 内部的变量；
function 函数名(参数){
  // 函数体
}

function tellStroy(name){
  console.log("从前有座山，山里有座庙");
  console.log("庙里有个老和尚在给小和尚讲故事");
  console.log("讲的是什么呢？");
  console.log("老和尚对"+ name +"说：");
}
```

* 既然是**变量**，可以被改变存储值；如何改变？调用时传递一个值；

```javascript
// 调用函数的时候，传入参数；
tellStroy('清风');
tellStroy('明月');
```

* 需求：如果我们想在调用函数的时候，把老和尚的名字也明确一下，就把老和尚的名字作为变量改变下；

```javascript
// 声明函数，配置参数；
function tellStroy(name1,name2){
  console.log("从前有座山，山里有座庙");
  console.log("庙里有个老和尚在给小和尚讲故事");
  console.log("讲的是什么呢？");
  console.log(name1 "对"+ name2 +"说：");
}

// 调用函数
tellStroy('圆通','清风');
```

* 如何配置参数？根据业务需求；



### 参数不赋值

* 变量：函数内部的变量，没有赋值，默认为undefined；和我们的变量一模一样；


```js
function tellStroy(name){
  console.log("从前有座山，山里有座庙");
  console.log("庙里有个老和尚在给小和尚讲故事");
  console.log("讲的是什么呢？");
  console.log("老和尚对"+ name +"说：");   // 老和尚对undefined说；
}
```

* 解决：对参数进行判断，如果是undefined，给一个默认值；

```js
function tellStroy(name){
    
  // if 条件语句
  if(name==undefined) {
      name = '小和尚'
  }
  else {
      name = name;
  }
  
  // 三元表达式；
  name = name?name:"小和尚"；
  console.log("从前有座山，山里有座庙");
  console.log("庙里有个老和尚在给小和尚讲故事");
  console.log("讲的是什么呢？");
  console.log("老和尚对"+ name +"说：");
}
```



### 形参与实参

* 参数：
  * 可以是一个，也可是多个；
  * 函数内部的变量
* 名称：
  - 在定义函数时写的**占位**用的参数，我们称为**形参**（**形式上的参数**）；
  - 在调用函数，**实际参与函数执行的真实的参数**，我们称为**实参**（**实际上的参数**）；

### 相互不影响

```js
var a = 10;
var b = 20;

function fn(x,y){
  x = x + y;
  console.log(x,y);
}

// 传入实参a b，相当于是变量a的值，赋值给了函数内部变量x，
// x再以后的运算和a没有任何关系；
// 输出 30，20
fnm(a,b); 

// 输出 10,20 a的值并没有受到x的变化影响
console.log(a,b);
```



