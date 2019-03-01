<!-- TOC -->autoauto- [es6划重点](#es6划重点)auto    - [1.作用域变量](#1作用域变量)auto        - [1.1.`let`和`var`对比](#11let和var对比)auto            - [1.变量不提升](#1变量不提升)auto            - [2.不能重复定义](#2不能重复定义)auto            - [3.暂存死区](#3暂存死区)auto            - [4.闭包新写法](#4闭包新写法)auto    - [2.const(常量)](#2const常量)auto    - [3.解构](#3解构)auto        - [1.数组解构](#1数组解构)auto        - [2.对象解构](#2对象解构)auto        - [3.混合解构](#3混合解构)auto        - [4.传递参数，结果解构](#4传递参数结果解构)auto    - [4.拷贝](#4拷贝)auto        - [4.1.浅拷贝](#41浅拷贝)auto        - [4.2.对象合并](#42对象合并)auto        - [4.3.JSON.parse(JSON.stringify())](#43jsonparsejsonstringify)auto        - [4.4.深拷贝（递归拷贝）](#44深拷贝递归拷贝)auto        - [4.5.展开运算符](#45展开运算符)auto    - [5. 代理proxy](#5-代理proxy)auto        - [5.1.普通函数（defineProperty）](#51普通函数defineproperty)auto        - [5.2.mvvm](#52mvvm)auto        - [5.3 普通代理](#53-普通代理)auto        - [5.4 多层代理](#54-多层代理)auto    - [6.箭头函数](#6箭头函数)auto        - [6.1 普通剪头函数](#61-普通剪头函数)auto        - [6.2 类数组](#62-类数组)auto        - [6.3 this指向问题](#63-this指向问题)auto        - [6.4 let不会将变量放在window上](#64-let不会将变量放在window上)auto    - [7.arr(数组新方法)](#7arr数组新方法)auto        - [7.1 reduce(收敛)](#71-reduce收敛)auto        - [7.2 filter(过滤)](#72-filter过滤)auto        - [7.3 map](#73-map)auto        - [7.3 every](#73-every)auto        - [7.4 findIndex](#74-findindex)auto        - [7.5 Array.from](#75-arrayfrom)auto        - [7.6 Array.of()](#76-arrayof)auto        - [7.7 copyWithin](#77-copywithin)auto        - [7.8 Object.keys](#78-objectkeys)auto    - [8.Symbol](#8symbol)auto    - [9.template](#9template)auto        - [9.1 模板字符串](#91-模板字符串)auto        - [9.2 模板字符串实现原理](#92-模板字符串实现原理)auto    - [10.集合](#10集合)auto        - [10.1 Set](#101-set)auto        - [10.2 Map](#102-map)auto    - [11 class](#11-class)auto        - [11.1 es5 实现的类](#111-es5-实现的类)auto        - [11.2 es6 写法](#112-es6-写法)auto        - [11.3 get 与 set](#113-get-与-set)auto    - [参考文档](#参考文档)autoauto<!-- /TOC -->
# es6划重点
## 1.作用域变量
### 1.1.`let`和`var`对比
#### 1.变量不提升
`var` 可能会造成变量提升<br/>
这里变量提升了,先声明`a`然后打印再赋值,结果是`undefined`
```
console.log(a);//undefined
var a = 1;

//相当于
var a;
console.log(a);
a = 1;
```
`let`的话，变量不会提升，打印的时候，会`报错`，因为还没声明
```
console.log(a);//a is not defined
let a = 1;
```
#### 2.不能重复定义
`var` 可能会被重新赋值, `let`不能重复声明一个变量
```
var a = 1;
var a = 2;
console.log(a);//2
```
```
let a = 1;
let a = 2;//Identifier 'a' has already been declared 这里是说它已经被声明了，不能重复声明
console.log(a);
```

#### 3.暂存死区
var的作用域问题 (函数作用域 全局作用域) (let 暂存死区)<br />
只要块级作用域内存在`let`命令，它所声明的变量就“绑定”（`binding`）这个区域，不再受外部的影响。
```
{
    let a = 1;
}
console.log(a);//a is not defined
```
```
{
    var a = 1;
}
console.log(a)//1
```
#### 4.闭包新写法
以前
```
;(function () {

})();
```
现在
```
{}
```

## 2.const(常量)
`const`声明一个只读的常量。一旦声明，常量的值就不能改变。
<br>
`const`命令声明的`常量`也是`不提升`，同样存在`暂时性死区`，只能在`声明的位置后面使用`。
```
const PI = 3.141593
PI > 3.0
```
es5写法
```
Object.defineProperty(typeof global === "object" ? global : window, "PI", {
    value:        3.141593,
    enumerable:   true,//对象属性是否可通过for-in循环，flase为不可循环，默认值为true
    writable:     false,//对象属性是否可修改,flase为不可修改，默认值为true
    configurable: false//能否使用delete、能否需改属性特性、或能否修改访问器属性
})
PI > 3.0;
```


## 3.解构
### 1.数组解构
```
let [,b,c,d=100] = [1,2,3];
console.log(b,d);
```
### 2.对象解构
```
let obj = {name:'cjw',age:18};
//这里重新命名了
let {name:Name,age,address="默认"} = obj;
console.log(Name, age, address)
```
### 3.混合解构
```
let [{name}] =  [{name:'cjw'}];
```
### 4.传递参数，结果解构
```
Promise.all(['cjw','9']).then(([name,age])=>{
  console.log(name, age);
});
```

## 4.拷贝
### 4.1.浅拷贝
```
let arr1 = [1,2,3,[1,2,3]];
let arr2 = [1,2,3];
let arr = [...arr1,...arr2];
console.log(arr)
arr1[3][0] = 100;
```

### 4.2.对象合并
```
let school = {name:'zfpx',a:{a:1}};
let my  = {age:18};
let newObj = {...school,a:{...school.a},...my}; 
console.log(newObj)
```

### 4.3.JSON.parse(JSON.stringify())
这个只能拷贝普通对象，new Date之类不能拷贝
```
let school = { name: 'zfpx', a: { a: 1 } ,date:new Date(),reg:new RegExp(/\d+/),fn:function(){}};
let s = JSON.parse(JSON.stringify(school));
```

### 4.4.深拷贝（递归拷贝）
```
function deepClone(obj) { // 递归拷贝 深拷贝
  if(obj == null) return null;
  if (obj instanceof Date) return new Date(obj);
  if(obj instanceof RegExp) return new RegExp(obj);
  if(typeof obj !== 'object') return obj;
  let t = new obj.constructor
  for(let key in obj ){
    t[key] = deepClone(obj[key])
  }
  return t;
}
let o = { a: [1, 2, 3] }
let r = deepClone(o);
o.a[1] = 1000
```
### 4.5.展开运算符
```
// 剩余运算符只能用在最后一个参数
function test(a, b,...c) { // c = [5,6,7]
  // 将类数组转化成数组
  let d = Array.prototype.slice.call(arguments,2)
  // a,b,...c
  let e =  Array.from(arguments).slice(2);
  let arr = [...arguments].slice(2);
  console.log(e);
}
test(1,2,3,5,6,7);
```
把多个对象的属性复制到一个对象中,第一个参数是复制的对象,从第二个参数开始往后,都是复制的源对象
```
// Object.assign  {...} 
let name ={name:'zfpx'}
let age = {age:9}
let obj = Object.assign(name,age); // {...}
console.log(obj);
```

## 5. 代理proxy
### 5.1.普通函数（defineProperty）
```
Object.defineProperty(obj, 'PI', {
    enumerable: true,
    configurable: false,
    get(){
        console.log('get');
    },
    set(){
        console.log('set');
        val = v;
    }
})
obj.PI = 3.15;
```
### 5.2.mvvm
```
let obj = {name: {name: 'cjw'}, age: 18};
function observer(obj){
    if(typeof obj != 'object') return obj;
    for(let key in obj){
        defineReactive(obj, key, obj[key]);
    }
}
function defineReactive(obj, key, value){
    observer(value);
    Object.defineProperty(obj, key, {
        get(){
            return value;
        },
        set(){
            console.log('update');
        }
    })
}
observer(obj);
obj.name.name = 'cjw';
```

### 5.3 普通代理
```
let proxy = new Proxy(obj, {
    set(target, key, value){
        if(key === 'length') return true;
        console.log('update');
        return Reflect.set(target, key, value);
    },
    get(target, key){
        return Reflect.get(target, key);
    }
})
proxy.push('123');
console.log(proxy.length);
```

### 5.4 多层代理
```
let obj = {name: {name: 'cjw'}, age : 18};
function set(obj, callback){
    let proxy = new Proxy(obj, {
        set(target, key ,value){
            if(key === 'length') return true;
            console.log('更新');
            return Reflect.set(target, key, value);
        },
        get(target, key){
            return Reflect.get(target, key);
        }
    })
    callback(proxy);
}
set(obj, function(proxy){
    proxy.age = '100';
    console.log(proxy);
})
```

## 6.箭头函数
`this` 指向 <br />
去掉`function` 关键字 <br />
去掉`return` 和 `{}` <br />
### 6.1 普通剪头函数
```
function a(a) {
  return function (b) {
    return a + b;
  }
}
let a = a => b => a + b;
console.log(a(1)(2));
```

### 6.2 类数组
```
let a = (...arguments) => {
  console.log(arguments)
}
a(1, 2, 3);
```

### 6.3 this指向问题
```
// this指向问题
let obj = {
  a: 1,
  b() { // obj = this
    console.log(this);
    setTimeout(() => { // 箭头函数中没有this指向 从而解决了this的问题
      this.a = 100;
    }, 1000);
  }
}
obj.b();
setTimeout(() => {
  console.log(obj.a)
}, 2000);
```

### 6.4 let不会将变量放在window上
```
let a = 1000; // let不会将变量放在window上
let obj = {
  a: 1,
  b: () => {
    this.a = 100; // window
  }
}
obj.b();
console.log(obj.a,a);
```

## 7.arr(数组新方法)
filter过滤 forEach 循环 map 映射 reduce 收敛 some every 反义
### 7.1 reduce(收敛)
原生写法
```
let arr = [1,2,3,4,5];
Array.prototype.myReduce = function (callback,prev) {
  for(let i = 0 ; i<this.length;i++){
    if(!prev){
      // 0 1
      prev = callback(this[i],this[i+1],i+1,this);
      i++;
    }else{
      prev = callback(prev,this[i],i+1,this);
    }
  }
  return prev
}
let r = arr.myReduce((prev,next,currentIndex,arr)=>{
  return prev+next
},100)
```

### 7.2 filter(过滤)
```
let arr = [1,2,3]
let arr1 = arr.filter(item=>item<=2);
console.log(arr1);
```

### 7.3 map
```
let arr =[1,2,3];
let arr1 = arr.map(item=>item*2);
```

### 7.3 every
```
let arr = [1,2,3];
let flag = arr.every(item=>item==3);
console.log(arr.includes(3)); //es7
```

### 7.4 findIndex
```
let arr = [1, 2, 3];
let item = arr.find(item => item == 4);
console.log(item); //es7
```
### 7.5 Array.from
将一个数组或者类数组变成数组,会复制一份
```
let newArr = Array.from(oldArr);
```

### 7.6 Array.of()
of是为了将一组数值,转换为数组
```
console.log(Array(3), Array(3).length);
console.log(Array.of(3), Array.of(3).length);
```

### 7.7 copyWithin
Array.prototype.copyWithin(target, start = 0, end = this.length) 覆盖目标的下标 开始的下标 结束的后一个的下标
```
[1, 2, 3, 4, 5].copyWithin(0, 1, 2)//[ 2, 2, 3, 4, 5 ]
```

### 7.8 Object.keys
Object.keys可以把对象取出来key组成数组 for of 可以迭代数组
```
for (var a of Object.values({ name: 'cjw', age: 9 }) ){ // forEach不能return 
  console.log(a);
}
```

## 8.Symbol
```
let s = Symbol();
let q = Symbol();

console.log(s === q);//false

let s = Symbol.for('cjw');
let q = Symbol.for('cjw');
console.log(s);//Symbol(cjw)
console.log(q);//Symbol(cjw)

console.log(Symbol.keyFor(q));
console.log(s === q);//ture
```

## 9.template
### 9.1 模板字符串
```
let name = 'cjw';
let age = 9;

let str = `${name}今年${age}`;
console.log(str);
```
### 9.2 模板字符串实现原理
```
let newStr = str.replace(/\$\{([\s\S])\}/g, function(){
    return eval(arguments);
})
console.log(newStr);
```

## 10.集合
### 10.1 Set
set可以做去重 set不能放重复的
```
let set = new Set([1,2,3,3,2,1]);
console.log([...set]);
```

### 10.2 Map
```
let map = new Map();
map.set('js','123');
map.set('node','456');
map.forEach(item=>{
  console.log(item);
});
```


## 11 class
### 11.1 es5 实现的类
// call 构造函数里面的属性方法复制
// Object.crate 复制原型里面的属性和方法
```
function Animal(type) {
  this.type = { t: type};
}
Animal.prototype.eat = function () {
  console.log('eat');
}
function Cat(type) {
   Animal.call(this,type); // 让父类在子类中执行，并且this指向子类
}
// 原型上还有一个属性
// 4.继承实例上和原型上的方法
function create(proto) {
  let Fn = function () { }
  Fn.prototype = proto;
  return new Fn();
}
Cat.prototype = Object.create(Animal.prototype,{constructor:{value:Cat}});
let cat = new Cat('哺乳类')
console.log(cat.type);
cat.eat();
console.log(cat.constructor);
```

### 11.2 es6 写法
```
class Animal {
    constructor(type){
        this.type = type;
    }
    static flag(){
        return 'animal';
    }
    eat(){
        console.log('eat');
    }
}
class Cat extends Animal{
    constructor(type){
        super(type);
    }
}

let cat = new Cat('哺乳类');
console.log(cat.type);
cat.eat();
console.log(Cat.flag);
```

### 11.3 get 与 set
getter可以用来得获取属性，setter可以去设置属性
```
class Person {
    constructor(){
        this.hobbies = [];
    }
    set hobby(hobby){
        this.hobbies.push(hobby);
    }
    get hobby(){
        return this.hobbies;
    }
}
let person = new Person();
person.hobby = 'basketball';
person.hobby = 'football';
console.log(person.hobby);
```

## 参考文档
[ECMAScript 6 入门 let 和 const 命令--阮一峰](http://es6.ruanyifeng.com/#docs/let)

[es6-features.org](http://es6-features.org/#Constants)