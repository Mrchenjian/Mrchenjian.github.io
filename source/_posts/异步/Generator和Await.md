---
title: Generator和Await
top: false
cover: false
toc: true
mathjax: true
date: 2020-03-26 19:00:42
password:
summary:
tags:
categories:
- 异步
---
 
 
 
  # ES7中的Generator、Await

    Await 是基于Gennerator 的语法糖

   Gennerator 是生成器   `普通函数名前 加*  叫生成器函数`

     ````js
      function*gen(){ 
      console.1og(1)；
      yield 1;//=>每一个yield控制一个状态节点
      yield 2； yield3；
        const inter=gen()； 
        console.log(inter.next())；(value:1,done:false) 
        console.log(inter.next())；
        //{value:2,done:false} console.log(inter.next())；
        //{value:3,done:fasle} console.log(inter.next())；
        //{value:undefined,done:true)
      ````

  **await**

    ````js
    function yieldPromise (generator) { 
    Let interator = generator();
    recursFunc.call(interator); 
    function recursFunc (value) {
    let interator = this，
    result = interator.next(value) ; 
    if(result.done) return; 
    Promise.resolve (result,value).then(
      value=>{recursFunc.call(interator, value)  
      });
    } 
    yieldPromise(function*(){ 
    Let vI = yfeld Promise.resolve(100); 
    console. Log(v1);
    let V2 = yteLd Promise.resolve(200); 
    console. Log(v2); 
    });
    ````

    

   Generator函数执行：不会让的数立即执行，返回结果是Interator选代器（ES6:for of循环）

    yield本身没有返回值，或者说返回值为undefined，next方法带参数，
    参数会被当作上一条vield返回值，所以第一个yield的返回值一直是undefined

    第一次 不能赋值

    

  