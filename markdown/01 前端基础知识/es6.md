<!-- TOC -->

- [1. es6划重点](#1-es6划重点)
    - [1.1. 作用域变量](#11-作用域变量)
        - [1.1.1. `let`和`var`对比](#111-let和var对比)
            - [1.1.1.1. 变量不提升](#1111-变量不提升)
            - [1.1.1.2. 不能重复定义](#1112-不能重复定义)
            - [1.1.1.3. 暂存死区](#1113-暂存死区)
            - [1.1.1.4. 闭包新写法](#1114-闭包新写法)
    - [1.2. const(常量)](#12-const常量)
    - [1.3. 解构](#13-解构)
        - [1.3.1. 数组解构](#131-数组解构)
        - [1.3.2. 对象解构](#132-对象解构)
        - [1.3.3. 混合解构](#133-混合解构)
        - [1.3.4. 传递参数，结果解构](#134-传递参数结果解构)
    - [1.4. 拷贝](#14-拷贝)
        - [1.4.1. 浅拷贝](#141-浅拷贝)
        - [1.4.2. 对象合并](#142-对象合并)
        - [1.4.3. JSON.parse(JSON.stringify())](#143-jsonparsejsonstringify)
        - [1.4.4. 深拷贝（递归拷贝）](#144-深拷贝递归拷贝)
        - [1.4.5. 展开运算符](#145-展开运算符)
    - [1.5. 代理proxy](#15-代理proxy)
        - [1.5.1. 普通函数（defineProperty）](#151-普通函数defineproperty)
        - [1.5.2. mvvm](#152-mvvm)
        - [1.5.3. 普通代理](#153-普通代理)
        - [1.5.4. 多层代理](#154-多层代理)
    - [1.6. 箭头函数](#16-箭头函数)
        - [1.6.1. 普通剪头函数](#161-普通剪头函数)
        - [1.6.2. 类数组](#162-类数组)
        - [1.6.3. this指向问题](#163-this指向问题)
        - [1.6.4. let不会将变量放在window上](#164-let不会将变量放在window上)
    - [1.7. arr(数组新方法)](#17-arr数组新方法)
        - [1.7.1. reduce(收敛)](#171-reduce收敛)
        - [1.7.2. filter(过滤)](#172-filter过滤)
        - [1.7.3. map](#173-map)
        - [1.7.4. every](#174-every)
        - [1.7.5. findIndex](#175-findindex)
        - [1.7.6. Array.from](#176-arrayfrom)
        - [1.7.7. Array.of()](#177-arrayof)
        - [1.7.8. copyWithin](#178-copywithin)
        - [1.7.9. Object.keys](#179-objectkeys)
    - [1.8. Symbol](#18-symbol)
    - [1.9. template](#19-template)
        - [1.9.1. 模板字符串](#191-模板字符串)
        - [1.9.2. 模板字符串实现原理](#192-模板字符串实现原理)
    - [1.10. 集合](#110-集合)
        - [1.10.1. Set](#1101-set)
        - [1.10.2. Map](#1102-map)
    - [1.11. class](#111-class)
        - [1.11.1. es5 实现的类](#1111-es5-实现的类)
        - [1.11.2. es6 写法](#1112-es6-写法)
        - [1.11.3. get 与 set](#1113-get-与-set)
    - [1.12. 参考文档](#112-参考文档)

<!-- /TOC -->
# 1. es6划重点
## 1.1. 作用域变量
### 1.1.1. `let`和`var`对比
#### 1.1.1.1. 变量不提升
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
#### 1.1.1.2. 不能重复定义
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

#### 1.1.1.3. 暂存死区
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
#### 1.1.1.4. 闭包新写法
以前
```
;(function () {

})();
```
现在
```
{}
```

## 1.2. const(常量)
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


## 1.3. 解构
### 1.3.1. 数组解构
```
let [,b,c,d=100] = [1,2,3];
console.log(b,d);
```
### 1.3.2. 对象解构
```
let obj = {name:'cjw',age:18};
//这里重新命名了
let {name:Name,age,address="默认"} = obj;
console.log(Name, age, address)
```
### 1.3.3. 混合解构
```
let [{name}] =  [{name:'cjw'}];
```
### 1.3.4. 传递参数，结果解构
```
Promise.all(['cjw','9']).then(([name,age])=>{
  console.log(name, age);
});
```

## 1.4. 拷贝
### 1.4.1. 浅拷贝
```
let arr1 = [1,2,3,[1,2,3]];
let arr2 = [1,2,3];
let arr = [...arr1,...arr2];
console.log(arr)
arr1[3][0] = 100;
```

### 1.4.2. 对象合并
```
let school = {name:'zfpx',a:{a:1}};
let my  = {age:18};
let newObj = {...school,a:{...school.a},...my}; 
console.log(newObj)
```

### 1.4.3. JSON.parse(JSON.stringify())
这个只能拷贝普通对象，new Date之类不能拷贝
```
let school = { name: 'zfpx', a: { a: 1 } ,date:new Date(),reg:new RegExp(/\d+/),fn:function(){}};
let s = JSON.parse(JSON.stringify(school));
```

### 1.4.4. 深拷贝（递归拷贝）
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
### 1.4.5. 展开运算符
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

## 1.5. 代理proxy
### 1.5.1. 普通函数（defineProperty）
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
### 1.5.2. mvvm
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

### 1.5.3. 普通代理
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

### 1.5.4. 多层代理
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

## 1.6. 箭头函数
`this` 指向 <br />
去掉`function` 关键字 <br />
去掉`return` 和 `{}` <br />
### 1.6.1. 普通剪头函数
```
function a(a) {
  return function (b) {
    return a + b;
  }
}
let a = a => b => a + b;
console.log(a(1)(2));
```

### 1.6.2. 类数组
```
let a = (...arguments) => {
  console.log(arguments)
}
a(1, 2, 3);
```

### 1.6.3. this指向问题
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

### 1.6.4. let不会将变量放在window上
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

## 1.7. arr(数组新方法)
filter过滤 forEach 循环 map 映射 reduce 收敛 some every 反义
### 1.7.1. reduce(收敛)
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

### 1.7.2. filter(过滤)
```
let arr = [1,2,3]
let arr1 = arr.filter(item=>item<=2);
console.log(arr1);
```

### 1.7.3. map
```
let arr =[1,2,3];
let arr1 = arr.map(item=>item*2);
```

### 1.7.4. every
```
let arr = [1,2,3];
let flag = arr.every(item=>item==3);
console.log(arr.includes(3)); //es7
```

### 1.7.5. findIndex
```
let arr = [1, 2, 3];
let item = arr.find(item => item == 4);
console.log(item); //es7
```
### 1.7.6. Array.from
将一个数组或者类数组变成数组,会复制一份
```
let newArr = Array.from(oldArr);
```

### 1.7.7. Array.of()
of是为了将一组数值,转换为数组
```
console.log(Array(3), Array(3).length);
console.log(Array.of(3), Array.of(3).length);
```

### 1.7.8. copyWithin
Array.prototype.copyWithin(target, start = 0, end = this.length) 覆盖目标的下标 开始的下标 结束的后一个的下标
```
[1, 2, 3, 4, 5].copyWithin(0, 1, 2)//[ 2, 2, 3, 4, 5 ]
```

### 1.7.9. Object.keys
Object.keys可以把对象取出来key组成数组 for of 可以迭代数组
```
for (var a of Object.values({ name: 'cjw', age: 9 }) ){ // forEach不能return 
  console.log(a);
}
```

## 1.8. Symbol
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

## 1.9. template
### 1.9.1. 模板字符串
```
let name = 'cjw';
let age = 9;

let str = `${name}今年${age}`;
console.log(str);
```
### 1.9.2. 模板字符串实现原理
```
let newStr = str.replace(/\$\{([\s\S])\}/g, function(){
    return eval(arguments);
})
console.log(newStr);
```

## 1.10. 集合
### 1.10.1. Set
set可以做去重 set不能放重复的
```
let set = new Set([1,2,3,3,2,1]);
console.log([...set]);
```

### 1.10.2. Map
```
let map = new Map();
map.set('js','123');
map.set('node','456');
map.forEach(item=>{
  console.log(item);
});
```


## 1.11. class
### 1.11.1. es5 实现的类
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

### 1.11.2. es6 写法
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

### 1.11.3. get 与 set
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

## 1.12. 参考文档
[ECMAScript 6 入门 let 和 const 命令--阮一峰](http://es6.ruanyifeng.com/#docs/let)

[es6-features.org](http://es6-features.org/#Constants)