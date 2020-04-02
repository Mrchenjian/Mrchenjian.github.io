---
title: Promise 进阶
top: false
cover: false
toc: true
mathjax: true
date: 2019-03-26 19:00:42
password:
summary:
tags:
categories:
- 异步
---


# Promiss 进阶

##  Promiss.all()  

**功能：** 批量执行

**用法**：1、Promise.all([p1,p2,p3...J])用于将多个Promise实例，包装成一个新的promiss实例

2、它接受一个数组作为参数。 

3、数组里可以是Promise对象，也可以是别的值，只有Promise会等待状态改变。

4、当所有子Promise都完成，该Promise完成，返回值是全部值的数组

5、有任何一个失败，该Promise失败，返回值是第一个失败的子Promise的结果。

````js
/ 使用`Promise.all()`包装多个Promise实例

console.log('here we go');
Promise.all([1, 2, 3])
    .then( all => {
        console.log('1：', all);
        return Promise.all([ function () {
            console.log('ooxx');
        }, 'xxoo', false]);
    })
    .then( all => {
        console.log('2：', all);
        let p1 = new Promise( resolve => {
            setTimeout(() => {
                resolve('I\'m P1');
            }, 1500);
        });
        let p2 = new Promise( (resolve, reject) => {
            setTimeout(() => {
                resolve('I\'m P2');
            }, 1000);
        });
        let p3 = new Promise( (resolve, reject) => {
            setTimeout(() => {
                resolve('I\'m P3');
            }, 3000);
        });
        return Promise.all([p1, p2, p3]);
    })
    .then( all => {
        console.log('all', all);
    })
    .catch( err => {
        console.log('Catch：', err);
    });
````



## Promise.all 最常见就是和.map()连用。

```js
// 遍历目录，找出最大的一个文件-通过Promise.all()和.map()
// https://www.imooc.com/video/16622


const fs = require('fs');
const path = require('path');
const FileSystem = require('./FileSystem');

function findLargest(dir) {
    return FileSystem.readDir(dir, 'utf-8')
        .then( files => {
            return Promise.all( files.map( file => {
                return new Promise (resolve => {
                    fs.stat(path.join(dir, file), (err, stat) => {
                        if (err) throw err;
                        if (stat.isDirectory()) {
                            return resolve({
                                size: 0
                            });
                        }
                        stat.file = file;
                        resolve(stat);
                    });
                });
            }));
        })
        .then( stats => {
            let biggest = stats.reduce( (memo, stat) => {
                if(memo.size < stat.size) {
                    return stat;
                }
                return memo;
            });
            return biggest.file;
        })
}
```

## Promiss.resolve()

**返回值：**返回一个fulfilled的Promise实例，或原始Promise实例。

**参数：** 1、参数为空，返回一个状态为fulfilled的Promise 实例

2、参数是一个跟Promise无关的值，同上，不过fulfuilled响应函数会得到这个参数

3、参数为Promise实例，则返回该实例，不做任何修改

4、参数为thenable，立刻执行它的.then0

```js
// Promise.resolve()
// https://www.imooc.com/video/16625


console.log('start');

Promise.resolve()
    .then( () => {
        console.log('Step 1');
        return Promise.resolve('Hello');
    })
    .then( value => {
        console.log(value, 'World');
        return Promise.resolve(new Promise( resolve => {
            setTimeout(() => {
                resolve('Good');
            }, 2000);
        }));
    })
    .then( value => {
        console.log(value, ' evening');
        return Promise.resolve({
            then() {
                console.log(', everyone');
            }
        })
    })
```



## Promiss.reject()

**返回值：** 返回一个rejected的Promise实例。

Promise.reject)不认thenable

```js
let promise = Promise.reject('something wrong');

promise
    .then( () => {
        console.log('it\'s ok');
    })
    .catch( () => {
        console.log('no, it\'s not ok');

        return Promise.reject({
            then() {
                console.log('it will be ok');
            },
            catch() {
                console.log('not yet');
            }
        });
    });
```

## Promise.race()

类似Promise.all()，区别在于它有任意一个完成就算完成。

   **常见用法：**  把异步操作和定时器放在一起  如果定时器先触发，就认为超时，告知用户

```js

console.log('start');

let p1 = new Promise(resolve => {
    // 这是一个长时间的调用
    setTimeout(() => {
        resolve('I\'m P1');
    }, 10000);
});
let p2 = new Promise(resolve => {
    // 这是个稍短的调用
    setTimeout(() => {
        resolve('I\'m P2');
    }, 2000)
});
Promise.race([p1, p2])
    .then(value => {
        console.log(value);
    });
```



## 把回调包装成 Promise

把回调包装成Promise最为常见。它有两个显而易见的好处

* 可读性更好

* 返回的结果可以加入任何Promise队列

```js
const fs = require('./FileSystem');

fs.readFile('../README.md', 'utf-8')
    .then(content => {
        console.log(content);
    });
```

## 把任何异步操作包装成Promise

**假设需求：**

1、用户点击按钮，弹出确认窗体。

2、用户确认和取消有不同的处理。
3、样式问题不能使用window.confirm0

```js
// 弹出窗体
let confirm = popupManager.confirm('您确定么？');
confirm.promise
    .then(() => {
        // do confirm staff
    })
    .catch(() => {
        // do cancel staff
    });

// 窗体的构造函数
class Confirm {
    constructor() {
        this.promise = new Promise( (resolve, reject) => {
            this.confirmButton.onClick = resolve;
            this.cancelButton.onClick = reject;
        })
    }
}
```

## 实现队列

有时候我们不希望所有动作一起发生，而是按照一定顺序，逐个进行。

```js
let promise=doSomething()；
promise=promise.then(doSomethingElse)；
promise=promise.then(doSomethingElse2)；
promise=promise.then(doSomethingElse3);

//使用 forEach 实现
// 易错  没有把.then0产生的新Promise 实例赋给 promise，没有生成队列。
function queue(things){ 
    1et promise=Promise.resolve()； 
    things.forEach( 
        thing=>{ promise=promise.then(()=>{
            return new Promise(resolve=>{ 
                doThing(thing，（）=>{ resolve()；
                                }）； 
                        }）； 
                               }）；
                                      }）；
        return promise；
        }

//使用.reduce()
//易错 Promise 实例创建之后，会立刻运行执行器代码，所以这个也无法达成队列的效果。
function queue(things){
    return things.reduce((promise,thing)=>{ 
        return promise.then(()=>{
            return new Promise( resolve=>{ 
                doThing(thing,()=>{ 
                    resolve()； 
                }）； 
                        }）；
                               }）；
                            }，Promise.resolve())；
}
queue(['lots','of','things',..J)；

```





## 补充

jQuery 已经实现了Promise。

如果你需要在IE中使用Promise，有两个选择：

* 只想实现异步队列：jQuery.defered

* 需要兼容所有平台：Bluebird Promise polyfill

### Fetch API

Fetch API是XMLHttpRequest的现代化替代方案

* 更强大，也更友好。
* 直接返回一个Promise实例



```js
fetch('some.json')
    .then( response => {
        return response.json();
    })
    .then( json => {
        // do something with the json
    })
    .catch( err => {
        console.log(err);
    });
```

##  异步函数 async/await

* 赋予JavaScript以顺序手法编写异步脚本的能力
* 既保留异步运算的无阻塞特性，还继续使用同步写法。



**那我为啥还要学Promise?**

async/await 仍然需要 Promise

```js

function resolveAfter2Seconds(x) {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve(x);
        }, 2000);
    });
}

async function f1() {
    var x = await resolveAfter2Seconds(10);
    console.log(x); // 10
}
f1();
```

