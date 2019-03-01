
<!-- TOC -->
[toc]
#### 1.异步编程
> 在`JavaScript`的世界中，所有代码都是`单线`执行的。 由于这个“缺陷”，导致`JavaScript`的所有网络操作，浏览器事件，都必须是异步执行。异步执行可以用:
- 回调函数
- 发布订阅
- 观察者模式
- promise

---

##### 1.1.回调函数
```
function call(id, callback){
  return function(){
     callback(id+1);
  }
}
let fn = call(3, function(id){
  console.log(id);
})
fn();
```
lodash 里面的after函数实现方法
```
function after(times, callback){
  return function(){
    //次数一直减少
    if(--times == 0){
      callback();
    }
  }
}
let fn = after(3, function(){
  console.log('after 被调用了三次');
})
fn();
fn();
fn();
```
接下来就是常见的读取数据的问题，回调函数的话，我们只能一层一层往下读取，很容易就进入了`回调地狱`这个可怕的状态
```
let fs = require('fs');
let school = {}
fs.readFile('./age.txt', 'utf8', function (err, data) {
    school['name'] = data;
    fs.readFile('./name.txt', 'utf8', function (err, data) {
      school['age'] = data;//{ name: 'cjw', age: '18' }
    });
});

```
##### 1.2 发布订阅
`发布者和订阅者是没有依赖关系的`<br>
你可能对发布订阅有点陌生，其实只要在DOM节点上面绑定过事件函数，那就使用过发布—订阅模式。
```
document.body.addEventListener('click',function(){
  alert(2);
},false);
document.body.click();    //模拟用户点击
```
`实现原理`<br>
首先用一个数组arr保存回调函数，然后触发emit的时候，arr里面的回调函数一一执行
```
let fs = require('fs');

let dep = {
    arr: [],//保存回调函数
    on(callback){
        this.arr.push(callback);
    },
    emit(){
        this.arr.forEach(item=>{
            item();
        })
    }
}

let school = {};
//这里先加一个回调函数 (订阅)
dep.on(function(){
    if(Object.keys(school).length === 2){
        console.log(school);//{ name: 'cjw', age: '18' }
    }
})
//
fs.readFile('./age.txt', 'utf8', function(err, data){
    school['name'] = data;
    dep.emit();//发布,调用dep.arr 里面的回调函数一一执行
})
fs.readFile('./name.txt', 'utf8', function(err, data){
    school['age'] = data;
    dep.emit();//发布
})

```
##### 1.3 观察者模式<br>
`观察者模式 发布和订阅的 被观察者是依赖于观察者的`
```
//观察者
class Observer{
    constructor(){
        this.arr = [];
        this.val = 1;
    }
    updateVal(val){
        this.val = val;
        this.notify();
    }
    notify(){
        this.arr.forEach(s=>s.update());
    }
    save(s){//保存一个对象
        this.arr.push(s);
    }
}
// 被观察者，被观察者有一个更新的方法。
class Subject{
    update(){
        console.log('update')
    }
}
let s = new Subject();
let observer = new Observer();
observer.save(s);//保存一个对象
observer.save(s);
observer.updateVal(21);//更新值的时候，被观察者也执行一个更新的方法
```

##### 1.4 Promise
`promise`有以下两个特点:<br>
`1`.对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。<br>
`2`.一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`
```
let fs = require('fs');
function read(url){
    return new Promise((resolve, reject)=>{
        fs.readFile(url, 'utf8', (err, data)=>{
            if(err) reject(err);
            resolve(data);
        })
    })
}
let school = {};
read('./name.txt').then(data=>{
    school['name'] = data;
    return read('age.txt');
}).then(data=>{
    school['age'] = data;
    console.log(school);//{ name: 'cjw', age: '18' }
})
```


#### 2.promise用法与原理
##### 2.1 `Promise.prototype.then()`
`Promise` 实例具有`then`方法，也就是说，`then`方法是定义在原型对象
```
//let Promise = require('./promise.js');
let p = new Promise((resolve, reject)=>{
    setTimeout(function(){     
        reject('成功');
    },100)
    reject('3');
})
p.then((value)=>{
    console.log(value);//3,这里是3因为，只能从一个状态panding到另一个状态
}, (reason)=>{
    console.log(reason);
})
```
`基本概念`<br>

1.`new Promise`时需要传递一个`executor`执行器,执行器会立刻执行<br>
2.执行器中传递了两个参数 `resolve`成功的函数 他调用时可以传一个值 值可以是任何值 `reject`失败的函数 他调用时可以传一个值 值可以是任何值<br>
3.只能从`pending`态转到成功或者失败<br>
4.`promise`实例。每个实例都有一个`then`方法，这个方法传递两个参数，一个是成功另一个是失败<br>
5.如果调用`then`时 发现已经成功了会让成功函数执行并且把成功的内容当作参数传递到函数中<br>
6.`promise` 中可以同一个实例`then`多次,如果状态是`pengding`需要将函数存放起来 等待状态确定后 在依次将对应的函数执行 (发布订阅)<br>
7.如果类执行时出现了异常 那就变成失败态<br>

`Promise.prototype.then()`的实现
```
function Promise(executor){
    var self = this;
    self.status = 'pending';//从pending 转换为resolved rejected
    self.value = undefined;
    self.reason = undefined;
    self.onResolved = [];//专门存放成功的回调
    self.onRejected = [];//专门存放失败的回调
    //pending -> resolved
    function resolve(value){
        if(self.status === 'pending'){
            self.value = value;
            self.status = 'resolved';
            self.onResolved.forEach(fn=>fn());
        }
    }
     //pending -> rejected
    function reject(reason){
        if(self.status === 'pending'){
            self.reason = reason;
            self.status = 'rejected';
            self.onRejected.forEach(fn=>fn());
        }
    }
    try{
        executor(resolve, reject);
    }catch(e){
        reject(e);
    }
}
//then方法的实现
Promise.prototype.then = function(onfulfilled, onrejected){
   let self = this;
   if(self.status === 'resolved'){//判断状态，resolved时候，返回value
       onfulfilled(self.value);
   }
   if(self.status === 'rejected'){//判断状态，rejected时候，返回reason
       onrejected(self.reason);
   }

   if(self.status === 'pending'){
      self.onResolved.push(function(){
          onfulfilled(self.value);
      })
      self.onRejected.push(function(){
        onfulfilled(self.reason);
      })
   }
}
module.exports = Promise;
```
##### 2.2 `Promise.prototype.catch()`
`Promise.prototype.catch方法`是`.then(null, rejection)`的别名，用于指定发生错误时的回调函数。
```
let p = new Promise((resolve, reject)=>{
    resolve();
})
p.then(data=>{
    throw new Error();
}).then(null).catch(err=>{
    console.log('catch', err)
}).then(null, err=>{
    console.log('err', err);
})
```
`实现原理`
```
Promise.prototype.catch = function(onrejected){
    return this.then(null, onrejected);
}
```

##### 2.3 `Promise.all`
`Promise.all`方法用于将多个 Promise 实例，包装成一个新的 `Promise` 实例。
```
const p = Promise.all([p1, p2, p3]);
```
`实现原理`
```
Promise.all = function (promises) {
    return new Promise((resolve, reject) => {
        let results = []; let i = 0;
        function processData(index, data) {
        results[index] = data; // let arr = []  arr[2] = 100
        if (++i === promises.length) {
            resolve(results);
        }
        }
        for (let i = 0; i < promises.length; i++) {
        let p = promises[i];
        p.then((data) => { // 成功后把结果和当前索引 关联起来
            processData(i, data);
        }, reject);
        }
    })
}
```
##### 2.4 `Promise.race`
`Promise.race`方法同样是将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。
```
const p = Promise.race([p1, p2, p3]);
```
上面代码中，只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 `Promise` 实例的返回值，就传递给p的回调函数。
```
let Promise = require('./e2.promise');
let fs = require('mz/fs');

Promise.race([
    fs.readFile('./age.txt', 'utf8'),
    fs.readFile('./name.txt', 'utf8')
]).then(data=>{
    console.log(data);
})
```
`实现原理`
```
Promise.race = function(promises){
    return new Promise((resolve, reject)=>{
        for(let i=0; i< promises.length; i++){
            let p = promises[i];
            p.then(resolve, reject);
        }
    })
}
```

##### 2.5 `Promise.resolve`
Promise.resolve方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象。
```
const p = Promise.resolve();

p.then(function () {
  // ...
});
```
`实现原理`
```
Promise.resolve = function(value){
    return new Promise((resolve, reject)=>{
        resolve(value);
    })
}
```

##### 2.6 `Promise.reject`
Promise.reject方法允许调用时不带参数，直接返回一个rejected状态的 Promise 对象。
```
const p = Promise.reject();

p.then(function () {
  // ...
});
```
`实现原理`
```
Promise.reject = function(reason){
    return new Promise((resolve, reject)=>{
        reject(reason);
    })
}
```

##### 2.7 `promise`的一些扩展库
[bluebird](https://www.npmjs.com/package/bluebird)

[mz](https://www.npmjs.com/package/mz)

##### 2.8 应用 `async + await = generator + co`
`generator` 生产迭代器的<br>
生成器函数 `*  generator` 一般配合 `yield`
```
function * read() {
    yield 1;
    yield 2;
    yield 3;
    return 100
}
let it = read();
console.dir(it.next());
console.dir(it.next());
console.dir(it.next());
console.dir(it.next());
//结果：
{ value: 1, done: false }
{ value: 2, done: false }
{ value: 3, done: false }
{ value: 100, done: true }
```
`promise + generator`
```
let fs = require('mz/fs');
// let co = require('co');

function * read(){
    let age = yield fs.readFile('./age.txt', 'utf8');
    return age;
}

//co 原理
function co(it){
    return new Promise((resolve, reject)=>{
        function next(data){
            let { value, done } = it.next(data);
            if(!done){
                value.then(data=>{
                    next(data);
                }, reject)
            }else{
                resolve(value);
            }
        }
        next();
    })
}

co(read()).then(data=>{
    console.log(data);//18
}, err=>{
    console.log(err);
})
```
`async + await`是es7的语法
```
let fs = require('mz/fs');//这个mz库将nodejs里面的fs全部函数都promise化
// async 函数就是promise es7
// 回调的问题 不能try/catch 并发问题
async function read() { 
    let age = await fs.readFile('name.txt','utf8')
    return age
}
read().then(data=>{
  console.log(data);//cjw
})
```

#### 3.手写一个promise A+
[promise A+ 规范传送门](https://promisesaplus.com/)

测试代码是否符合a+ 规范 为了让其能测试
```
npm install promises-aplus-tests -g
promises-aplus-tests 文件名 可以测试
```
```
/*
 * @Author: caijw 
 * @Date: 2018-10-01 15:04:43 
 * @Last Modified by: caijw
 * @Last Modified time: 2018-10-08 22:41:06
 */
function Promise(executor){
    var self = this;
    self.status = 'pending';//从pending 转换为resolved rejected
    self.value = undefined;
    self.reason = undefined;
    self.onResolved = [];//专门存放成功的回调
    self.onRejected = [];//专门存放失败的回调
    //pending -> resolved
    function resolve(value){
        if(self.status === 'pending'){
            self.value = value;
            self.status = 'resolved';
            self.onResolved.forEach(fn=>fn());
        }
    }
     //pending -> rejected
    function reject(reason){
        if(self.status === 'pending'){
            self.reason = reason;
            self.status = 'rejected';
            self.onRejected.forEach(fn=>fn());
        }
    }
    try{
        executor(resolve, reject);
    }catch(e){
        reject(e);
    }
}

//这里主要就是递归循环，判断是否为promise，如果是promise就继续递归循环下去。
function resolvePromise(promise2, x, resolve, reject){
    if(promise2 === x){
        return reject(new TypeError('循环引用'));
    }

    let called;
    if(x!=null && (typeof x === 'object' || typeof x === 'function')){
        try{
            let then = x.then;
            //假设他是一个promise，then方法就是一个函数
            if(typeof then === 'function'){
                then.call(x, (y)=>{
                    if(called) return;
                    called = true;
                    // 递归解析 如果resolve的是一个promise 就要不停的让resolve的结果进行处理
                    resolvePromise(promise2, y, resolve, reject);
                },(e)=>{
                    if(called) return;
                    called = true;
                    reject(e);
                })
            }else{//不是就返回
                resolve(x);
            }
        }catch(e){
            if(called) return;
            called = true;
            reject(e);
        }
        
    }else{
        resolve(x);
    }
}





//至返回错误的 catch 就是不写成功的回调的then方法
Promise.prototype.catch = function(onrejected){
    return this.then(null, onrejected);
}

//1.解决输出的顺序的问题
// all方法的参数 是一个数组，会按照数组的结果放到成功的回调里(只有全成功才算成功)
// race方法参数也是一个数组。会同时发起并发，但是以返回最快的结果为结果


Promise.race = function(promises){
    return new Promise((resolve, reject)=>{
        for(let i=0; i< promises.length; i++){
            let p = promises[i];
            p.then(resolve, reject);
        }
    })
}


Promise.reject = function(reason){
    return new Promise((resolve, reject)=>{
        reject(reason);
    })
}

Promise.resolve = function(value){
    return new Promise((resolve, reject)=>{
        resolve(value);
    })
}

Promise.all = function (promises) {
    return new Promise((resolve, reject) => {
        let results = []; let i = 0;
        function processData(index, data) {
        results[index] = data; // let arr = []  arr[2] = 100
        if (++i === promises.length) {
            resolve(results);
        }
        }
        for (let i = 0; i < promises.length; i++) {
        let p = promises[i];
        p.then((data) => { // 成功后把结果和当前索引 关联起来
            processData(i, data);
        }, reject);
        }
    })
}



//回调函数
Promise.prototype.then = function(onfulfilled, onrejected){
    // onfulfilled / onrejected是一个可选的参数
    onfulfilled = typeof onfulfilled == 'function' ? onfulfilled :  val=>val;
    onrejected = typeof onrejected === 'function' ? onrejected :err => {
        throw err;
    }
   let self = this;
   
   let promise2;
   promise2 = new Promise((resolve, reject)=>{
        if(self.status === 'resolved'){
            setTimeout(()=>{
                try{
                    let x = onfulfilled(self.value);
                    resolvePromise(promise2, x, resolve, reject);
                }catch(e){
                    reject(e);
                }
            }, 0)
        }
        if(self.status === 'rejected'){
            setTimeout(()=>{
                try{
                    let x = onrejected(self.reason);
                    resolvePromise(promise2, x, resolve, reject);
                }catch(e){
                    reject(e);
                }
            }, 0)
            
        }
        if(self.status === 'pending'){
            self.onResolved.push(function(){
                setTimeout(()=>{
                    try{
                        let x = onfulfilled(self.value);
                        resolvePromise(promise2, x, resolve, reject);
                    }catch(e){
                        reject(e);
                    }
                }, 0)
            })
            self.onRejected.push(function(){
                setTimeout(()=>{
                    try{
                        let x = onrejected(self.reason);
                        resolvePromise(promise2, x, resolve, reject);
                    }catch(e){
                        reject(e);
                    }
                }, 0)
            })
        }
   })
   return promise2;
}
// 语法糖 简化问题 嵌套的问题 ，被废弃了
Promise.defer = Promise.deferred = function(){
    let dfd = {};
    dfd.promise = new Promise((resolve, reject)=>{
        dfd.resolve = resolve;
        dfd.reject = reject;
    })
    return dfd;
}

module.exports = Promise;
```
终于撸完promise了，小伙伴们看了，感觉有收获，请点个`赞`

#### 参考文档
[ECMAScript 6 入门 Promise--阮一峰](http://es6.ruanyifeng.com/#docs/promise)

[Hey, 你的Promise](https://juejin.im/post/5b58300b6fb9a04fea58a27e)

[ES6版Promise实现，给你不一样的体验](https://juejin.im/post/5b5b52dbe51d4534c34a4a6e)

[使用 Bluebird 开发异步的 JavaScript 程序](https://www.ibm.com/developerworks/cn/web/wa-lo-bluebird-develop-asynchronous-javascript/index.html)