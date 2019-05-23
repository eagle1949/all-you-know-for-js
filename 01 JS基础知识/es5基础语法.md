<!-- TOC -->

- [1. javascript](#1-javascript)
    - [1.1. js基础](#11-js基础)
        - [1.1.1. 浏览器内核](#111-浏览器内核)
        - [1.1.2. javascript组成](#112-javascript组成)
        - [1.1.3. 变量](#113-变量)
            - [1.1.3.1. 创建变量](#1131-创建变量)
        - [1.1.4. 数据类型](#114-数据类型)
            - [1.1.4.1. 基本数据类型](#1141-基本数据类型)
            - [1.1.4.2. 引用数据类型](#1142-引用数据类型)
            - [1.1.4.3. Symbol 创建出来的是唯一的值](#1143-symbol-创建出来的是唯一的值)
            - [1.1.4.4. 如何输出结果](#1144-如何输出结果)
                - [1.1.4.4.1. alert](#11441-alert)
                - [1.1.4.4.2. confirm](#11442-confirm)
                - [1.1.4.4.3. prompt](#11443-prompt)
                - [1.1.4.4.4. console.log](#11444-consolelog)
                - [1.1.4.4.5. console.dir](#11445-consoledir)
                - [1.1.4.4.6. console.log 把json数据按照表格输出](#11446-consolelog-把json数据按照表格输出)
        - [1.1.5. 数据类型的详细剖析](#115-数据类型的详细剖析)
            - [1.1.5.1. Number 数据类型](#1151-number-数据类型)
                - [1.1.5.1.1. NaN isNaN()](#11511-nan-isnan)
                - [1.1.5.1.2. 把非数字类型转换为数字类型](#11512-把非数字类型转换为数字类型)
                - [1.1.5.1.3. parseInt/ParseFloat](#11513-parseintparsefloat)
            - [1.1.5.2. 布尔类型](#1152-布尔类型)
            - [1.1.5.3. null和undefined](#1153-null和undefined)
        - [1.1.6. Ojbect对象数据类型](#116-ojbect对象数据类型)
            - [1.1.6.1. 对象的操作，键值对的增删改查](#1161-对象的操作键值对的增删改查)
            - [1.1.6.2. 浅析js的运行机制](#1162-浅析js的运行机制)
        - [1.1.7. js中的判断操作语句](#117-js中的判断操作语句)
        - [1.1.8. 函数](#118-函数)
        - [1.1.9. 数组以及常规方法](#119-数组以及常规方法)
            - [1.1.9.1. push](#1191-push)
            - [1.1.9.2. pop](#1192-pop)
            - [1.1.9.3. unshift](#1193-unshift)
            - [1.1.9.4. shift](#1194-shift)
            - [1.1.9.5. splice](#1195-splice)
        - [1.1.10. 字符串以及常规方法](#1110-字符串以及常规方法)
        - [1.1.11. DOM 树](#1111-dom-树)
            - [1.1.11.1. 在js中获取dom元素的方法](#11111-在js中获取dom元素的方法)
                - [1.1.11.1.1. getElementById](#111111-getelementbyid)
                - [1.1.11.1.2. getElementsByTagName](#111112-getelementsbytagname)
                - [1.1.11.1.3. getElementsByClassName](#111113-getelementsbyclassname)
                - [1.1.11.1.4. getElementsByName](#111114-getelementsbyname)
                - [1.1.11.1.5. querySelector](#111115-queryselector)
                - [1.1.11.1.6. querySelectorAll  类似于jq  $('')](#111116-queryselectorall--类似于jq--)
                - [1.1.11.1.7. document.head head对象](#111117-documenthead-head对象)
                - [1.1.11.1.8. document.body body元素对象](#111118-documentbody-body元素对象)
                - [1.1.11.1.9. document.documentElement 获取html元素对象](#111119-documentdocumentelement-获取html元素对象)
            - [1.1.11.2. DOM常用方法](#11112-dom常用方法)
                - [1.1.11.2.1. 节点（node）](#111121-节点node)
                - [1.1.11.2.2. 描述节点之间关系的属性](#111122-描述节点之间关系的属性)
            - [1.1.11.3. DOM 的增删改查](#11113-dom-的增删改查)
                - [1.1.11.3.1. creteElement 创建一个元素标签](#111131-creteelement-创建一个元素标签)
                - [1.1.11.3.2. appendChild 把一个元素对象插入到指定容器末尾](#111132-appendchild-把一个元素对象插入到指定容器末尾)
                - [1.1.11.3.3. insertBefore  把一个元素对象插入到指定容器的某个标签之前](#111133-insertbefore--把一个元素对象插入到指定容器的某个标签之前)
                - [1.1.11.3.4. cloneNode 把某个节点进行克隆](#111134-clonenode-把某个节点进行克隆)
                - [1.1.11.3.5. removeChild 把容器中的某个节点删除](#111135-removechild-把容器中的某个节点删除)
                - [1.1.11.3.6. set/get/removeAttribute 设置、获取，删除当前某个元素自定义属性](#111136-setgetremoveattribute-设置获取删除当前某个元素自定义属性)

<!-- /TOC -->
# 1. javascript
## 1.1. js基础
### 1.1.1. 浏览器内核
- webkit 内核（v8引擎)
google chrome safari opera 大部分国产
- gecko内核
Mozilla Firefox 
- presto 内核
- Trident 排版引擎 （Internet Explorer）IE
- KHTMl排版引擎

> w3c 万维网联盟，开发者按照规范编写代码，浏览器开发商也会开发一套按规范吧代码通过GPU渲染成页面和图形等等。

>为啥会出现浏览器兼容
- 部分浏览器会提前开发一些更好的功能，后期这些功能被收录到w3c规范中，但是在收录之前，会存在兼容性问题
- 各个浏览器厂商为了突出自己的独特性，用其他方法实现w3c的功能

### 1.1.2. javascript组成
- ECMAScript (ES) js的核心语法
- DOM document object model 文档对象模型
- BOM brower object model 浏览器对象模型，提供api让js可以操作浏览器

- ecmascript 它是js的语法规范，js中的变量，数据类型，语法规范，操作语句，设计模式都是ES规定的。
1997 es1.0->
1998 es2.0->
1999 es3.0(最广泛)->
2000 es4（夭折）->
2015.6 es6

### 1.1.3. 变量
它不是具体的值，只是一个用来存储具体值的容器或者代名词。因为它存储的值可以改变，所以叫变量。

#### 1.1.3.1. 创建变量
- var(es3)
- function (es3) 创建函数
函数名也是变量，只不过存储的值是函数类型的而已
- let（es6）
- const （es6）
- import （es6）
- class (es6)
```
var [变量名] = 值; 
let [变量名] = 值;
const [变量名] = 值;
function [函数名](){

}
```
```
const m = 100; 
m = 200; //Assignment to constant variable 不能给一个常量重新赋值
```

### 1.1.4. 数据类型
数据值是一门编程语言进行生产的材料，Js中包含的值有一下这些类型
- 基本数据类型（值类型）
 + 数字（number）
 + 字符串 （string）
 + 布尔 （boolean）
 + null
 + undefined
 + symbol

- 引用数据类型
 + 对象Object
  + 普通对象
  + 数组对象
  + 正则类型
  + 日期类型
 + 函数 function

#### 1.1.4.1. 基本数据类型
- 数字
数字类型中有一个特殊的值 NaN（not a number）代表不是一个有效的数字，但是属于number类型
```
var n = 13; 
```
- 字符串
```
var str = '';
```
- boolean
```
var b = true;  //布尔类型只有两个值，true为真，false为假
```

#### 1.1.4.2. 引用数据类型
- 普通对象
```
var obj = {
	name: 'cjw',
	age : 18
}
```
- 数组
```
var arr = [11,12,13];
```
- 正则
```
var reg = /^$/
```
- 函数
```
function fn(){
	
}
```

#### 1.1.4.3. Symbol 创建出来的是唯一的值
```
var a = Symbol('zf');
var b = Symbol('zf');
a == b => false;

const a = Symbol('flag');
```

扩展： js代码如何被运行，以及运行后如何输出结果
- 浏览器内核渲染解析
- NODE也是一个基于V8引擎渲染和解析JS的工具

#### 1.1.4.4. 如何输出结果
##### 1.1.4.4.1. alert
> alert 在浏览器中通过弹框方式输出，（浏览器提示框）
```
var num = 12;
alert(num);  //=> window.alert

var str = 'zf';
alert(str);

alert(1+1); => 2 //基于alert出差的结果都会转换为字符串，把值（如果是表达式先计算结果，再通过toString转换为字符串，然后输出）

alert(true); => 'true'
alert([12, 13]); => '12,13'
alert({name:'cjw'}); => '[Object Object]
```
##### 1.1.4.4.2. confirm 
> 和alert的用法一致，只不过提示框有确定和取消两个按钮
```
var num = 12;
window.confirm(num);
var flag = confirm(num);
alert(flag);
```
##### 1.1.4.4.3. prompt
> 在confirm的基础上加上输出框

##### 1.1.4.4.4. console.log
> 在浏览器控制台输出日志（F12）
    + Elements: 当前页面总的元素和样式都可以在这里看到，还可以调试样式
    + console： 控制台，可以在js代码中通过log输出到这里，也可以直接写js
    + source：当前网站的源文件都在这里

##### 1.1.4.4.5. console.dir 
> 比log输出更想起，尤其是输出对象数据值的时候。

##### 1.1.4.4.6. console.log 把json数据按照表格输出

### 1.1.5. 数据类型的详细剖析
#### 1.1.5.1. Number 数据类型
##### 1.1.5.1.1. NaN isNaN()
- NaN: not a number 但它是数字类型
- isNaN: 检测当期的值是否不是有效数字返回，true代表不是数字，返回false是有效数字
语法：isNaN([value])
```
var num = 12;
isNaN(num); //=> false 
isNaN('12'); //=> false 先转换为数字
isNaN('zf'); //=> true
isNaN(true); => false
isNaN(false); => false
isNaN(null); => false
isNaN(undefined); => true
isNaN({age: 3}); => true
isNaN([12, 13]); => true
isNaN([12]); => false
isNaN(/^&/); => true
isNaN(function(){}); => true
```
isNaN检测机制
- 首先验证当前检测的值是否为数字类型，如果不是，浏览器会默认把值转换为数字类型
- 当检测的值已经是数字类型的是否，是有效数字返回false，不是返回true
- 数字类型只有NaN不是有效数字

##### 1.1.5.1.2. 把非数字类型转换为数字类型
- 其他数字类型的值转换为数字类型，用Number转换
[字符串转数字]
```
Number('13'); => 13
Number('13px'); => NaN 字符串中有一个非有效数字
Number(13.5); => 13.5 可以识别小数
```
[布尔转数字]
```
Number(true); => 1
Number(false); => 0
```
[其他]
```
Number(null); => 0
Number(undefined); => NaN
```
[把引用数据类型的值转换为数字]
> 先把引用值调取toString转换为字符串，然后再把字符串调用Number转换为数字
```
({}).toString() => '[Object Object]'
```
[数组]
```
[12, 13].toSting() => '12,13' => NaN
[12].toString() => '12' => 12
```
[正则]
```
/^&/.toString()=> '/^&/' => NaN
```

##### 1.1.5.1.3. parseInt/ParseFloat
> 等同于Number，也是为了把其他类型的值转换为数字类型
> 和Number的区别在于字符串转换分析上
> Number 出现任意非有效数字字符，结果就是NaN
parseInt 把一个字符串中的整数部分解析出来，parseFloat是把一个字符串中的小说（浮点数）部分解析出来
```
parseInt('12.5px'); => 12
parseFloat('12.5px'); => 12.5
parseInt('width: 12.5px'); => NaN
```
从字符串最左边开始查找有效数字，并且转换为数字，但是一旦遇到一个非有效数字字符查找结束

#### 1.1.5.2. 布尔类型
> 只有两个值： true/false
如何把其他数据类型转换为布尔类型
- Boolean
- !
- !!
null,undefined,'',NaN, 0 转换为false，其他为true

#### 1.1.5.3. null和undefined
都代表为空
- null 空对象指针
- undefined 未定义

[区别]
null：一般都是意料之中的没有（通俗解释：一般都是人为手动赋值为null， 后面的程序中我们会再次给他赋值
```
var num = null;=> null 手动赋值，预示后面我会吧num的值进行修改
num = 12
```

undefined：代表的没有一般都不是人为手动控制的，大部分都是浏览器自主为空（后面可以赋值，也可以不赋值）
```
var num; => 此时变量的值，浏览器给分配的就是undefined，后面可以赋值也可以不赋值
```

### 1.1.6. Ojbect对象数据类型
> 普通对象
> 1.由大括号包裹起来的
> 2.由0到多组属性和属性值（键值对）组成
- 属性：用来描述当前对象特性的
- 属性名：是当前具备的这个特性 （key）
- 属性值：是对这个特征的描述   （value）

```
var obj = {
    name : 'cjw',
    age : 18
}
```
#### 1.1.6.1. 对象的操作，键值对的增删改查
[读取]
```
obj.name
obj['name']
```
一般来说，对象的属性名都是字符串格式的，属性值不固定，任何格式都可以。

[增/删]
Js对象中，属性名是不允许重复的，唯一的
```
var obj = {name: 'xxx'}
var obj2 = {name: 'qqq'}

obj.name = 'cjw1'; //原有对象中有Name属性，此处修改属性
obj.sex = 'boy'; //原有对象中不存在sex，此处相当于给当前对象新增一个属性sex
```

[删]
彻底删除：对象中不存在这个属性了
```
delete obj['age']
```
假删除：并没有移除这个属性，只是让当前属性的值为空
obj.sex = null;

- 在获取属性值的时候，如果当前对象有这个属性名，则可以正常获取值（哪怕是null，如果没有就算undefined）
- 一个对象中的属性名，不仅仅是字符串格式的，还有可能是数字格式的。
```
var obj = {
    name : 'cjw'
    0 : 100
}
obj[0] = 100;
obj['0'] = 100
obj.0 => uncaught SyntaxError Unexpected number
```
- 当我们存储的属性名不是字符串也不是数字的时候，浏览器会把这个值转换为字符串（toString）然后进行存储
```
obj[{}] = 300; //先把[{}].toString 后的结果作为对象的属性名存储进来 obj['[object object]'] = 300

obj[{}] //获取的时候也是先把对象转换为字符串'[object object]' 然后获取之前存储的300
```

[数组对象] （对象由键值对组成的）
通过观察结果，我们发现数组对象的属性名为数字，（我们把数字属性名称为当前对象的索引）

#### 1.1.6.2. 浅析js的运行机制
- 1.当浏览器（它的内核引擎）渲染和解析js的时候，会提供一个js代码运行的环境，我们把这个环境称之为全局作用域（window）
- 2. 代码自上而下执行（之前还有一个变量提升阶段）

[基本数据类型]
基本数据类型的值会存储在当前作用域下  （栈内存）
```
var a = 12;
```
1. 首先开辟一个空间存储12
2. 在当前作用域中声明一个变量 a （var a）
3. 让声明的变量和存储的12 进行关联（把存储的12赋值给a，赋值操作叫定义）
基本数据类型（也叫值类型）是按照值操作的，把原有的值复制到新的空间或者位置上的同个空间

[引用类型]
引用类型的值不能直接存储到当前作用域下 （因为可能存储的内容过于复杂），我们需要先开辟一个新的空间（理解为仓库）把内容存储到这个空间  （堆内存）
```
var obj = {n: 100}
```
1. 首先开辟一个新的内存空间，把对象中的键值对依次存储起来（为了确保后面的可以找到这个空间，此空间有一个16进制的地址）
2. 声明一个变量
3. 让变量和空间地址关联在一起（把空间地址赋值给变量）
引用类型不是按照值操作的，它操作的是空间的引用地址，把原有空间地址的值赋值给新变量，但是原来的空间没有被克隆，还有一个空间，这样就会出现多个变量关联。


```
var obj = {
    n: 10,
    m: obj.n * 10
}
console.log(obj.m); //Uncaught TypeError： cannot read property ‘n’ of undefined
```
1. 形成一个全局作用域（栈内存）
2. 代码自上而下执行
首先开辟一个新的内存（AAFFF11），把键值对存储到内存 m:obj.n*10=> obj.n 此时堆内存信息还没存储完成，空间的地址还没给obj，此时obj是undefined，obj.n = undefined.n


```
var a = 12;
var ary1 = [3, 4];
var ary2 = ary1;
ary2[0] = 100;
ary2 = [4, 5]; //重新赋值，内存地址改变了
ary2[1] = 2;
ary1[1] = 0;

//[100,0],[4,2]
```

### 1.1.7. js中的判断操作语句
if/else if/else
typeof
switch case

### 1.1.8. 函数
1. 在JS中，函数就是一个方法（一个功能体），基于函数一般都是为了实现这个功能
2. 函数的诞生就是为了实现封装 （重复利用）
3. 函数为引用类型的一种，它也是安卓引用地址操作的
运行机制
- 堆内存 （存放字符串）
- 开辟的堆内存地址赋值给函数名
函数执行
- 私有作用域 栈内存
- 函数字符串复制过来，变成真正的js代码，在开辟的作用域中自上而下执行
[函数形参，实参]
参数是函数的入口，当我们在函数中封装一个功能，发现一些材料不正确，需要用户传递过来，此时我们就基于参数的机制提供出入口就行。


### 1.1.9. 数组以及常规方法
> 数组也是对象数据类型的，也是键值对组成的。
```
var ary = [12, 23, 34];
0:12 1:23 3:34
```
1. 以数组作为索引（属性名）索引从零开始递增
2. 有一个LENGTH属性存储的是数组长度
```
ary[0] 第一项 ary[length-1] 最后一项
```
数组中每一项的值都可以是任何数据类型

[数组中的常用方法]
#### 1.1.9.1. push
- 作用：向数组末尾追加新的内容
- 参数: 追加的内容（可以一个，也可以用多个）
- 返回值：新增后数组的长度
- 原数组改变
```
var ary = [12, 23, 34];
ary.push(100); => 返回4 [12, 23, 34, 100]
ary.push(100, {name:'xxx'}) => 6 [12, 23, 34, 100, 100, {...}]
```

#### 1.1.9.2. pop
- 作用：删除数组最后一项
- 参数: 无
- 返回值：被删除那一项内容
- 原数组改变
```
var ary = [12, 23, 34];
ary.pop(); =>返回34 [12, 23]
```

#### 1.1.9.3. unshift
- 作用：向数组开始追加新的内容
- 参数: 追加的内容（可以一个，也可以用多个）
- 返回值：新增后数组的长度
- 原数组改变
```
var ary = [12, 23, 34];
ary.unshift(100); => 返回4 [100 , 12, 23, 34]
```

#### 1.1.9.4. shift
- 作用：删除数组第一项
- 参数: 无
- 返回值：被删除那一项内容
- 原数组改变
```
var ary = [12, 23, 34];
ary.shift(); =>返回34 [23, 34]
```


#### 1.1.9.5. splice 
作用：（删除指定位置，指定位置加内容，修改指定位置信息）

- 删除 ary.splice(n, m)
参数：从索引n开始，删除m个内容，
返回：把删除的部门以一个新数组返回，
原来数组改变
```
var ary = [12,23,34,45,56];
ary.splice(2,3) =>返回[34,45,56] 原数组[12,23]改变
```

- 新增 ary.splice(n, 0, x ,...)
参数：从索引n开始，删除0个内容，插入数据
返回：[]
原来数组改变
```
var ary = [12,23,34, 45, 56];
ary.splice(4, 0, 100, 200); //=>[] 原数组[12,23,34,45,100,200,56]
```

- 修改 ary.splice(n, m, x ...)
参数：把原来内容删除，然后新增新的内容
返回：把删除的部门以一个新数组返回，
原来数组改变
```
var ary = var ary = [12,23,34, 45, 56];
ary.splice(2, 1, 100, 200); //=>[34] 原数组[12,23,100,200,45, 56]
```

### 1.1.10. 字符串以及常规方法


### 1.1.11. DOM 树
> DOM Tree
> 当浏览器加载html页面的时候，首先就是Dom结构的计算，计算出来的dom结构就是DOM tree
把页面中的HTML标签像树状结构一样分析出之间的层级关系
![](https://user-gold-cdn.xitu.io/2019/5/13/16ab1071f7ffa3b4?w=796&h=413&f=png&s=10429)
DOM 树描述了标签与标签之间的关系（节点间的关系，我们只需知道任意一个标签都可以根据Dom提供的属性和方法，获取到页面中任意一个标签和节点。

#### 1.1.11.1. 在js中获取dom元素的方法
- getElementById
- getElementsByTagName
- getElementsByClassName
- getElementsByName
- querySelector
- querySelectAll
- document.head
- document.body
- document.documentElement

##### 1.1.11.1.1. getElementById
> 通过元素的id获取指定的对象，使用都是document.getElementById(''),此外document是限定了获取元素的范围，我们把他称之为上下文。
- getElementById 的上下文是document，因为严格意思上，一个页面的id是不能重复的，浏览器在整个文档中，既可以获取这个唯一的ID
- 如果页面中的ID重复了，我们基于这个方法只能获取第一个元素，后面的元素无法获取
- 在IE6-7中表单元素的name会被当成ID使用（以后使用表单元素的时候，不要用name和和id有冲突的值）

##### 1.1.11.1.2. getElementsByTagName
- [context].getElementsByTagName 在指定上下文中根据标签名获取到一组元素集合。（HTMLCollection）
- 获取元素集合是一个类数组，（不能直接使用数组里面的方法）
- 它会把当前上下文中，子子孙孙（后代）层级内的标签都获取到，
- 基于这个方法获取到的结构永远是一个集合。

##### 1.1.11.1.3. getElementsByClassName
- [context].getElementsByClassName 在指定上下文中根据标签名获取到一组元素集合。（HTMLCollection）
- 真实项目中，我们经常基于样式来给元素设置样式，所以js中，我们也会经常基于样式获取元素 IE6-IE8不兼容

##### 1.1.11.1.4. getElementsByName
- 它的上下文也只能是document，在整个文档中，基于元素的name属性获取一组节点集合。
- 在IE6-IE8中，只对表单元素的name属性起作用。

##### 1.1.11.1.5. querySelector
- [context].querySelector 指定的上下文基于选择器，获取到指定的一个元素。（第一个）

##### 1.1.11.1.6. querySelectorAll  类似于jq  $('')
- 获取道选择器匹配到的所有元素，结果是一个节点集合（NodeList)

##### 1.1.11.1.7. document.head head对象
##### 1.1.11.1.8. document.body body元素对象
##### 1.1.11.1.9. document.documentElement 获取html元素对象
获取一屏幕的宽高
```
document.documentElement.clientWidth || document.body.clientWidth
```
获取当前页面中所有id为HAHA的（兼容所有浏览器）
1. 获取所有当前文档的html
2. 依次遍历检测谁的id等于HAHA
```
function getAllById(id){
    var nodeList = document.getElementsByTagName('*');
    var ary = [];
    for(var i=0; i<nodeList.length; i++){
        var item = nodeList[i];
        item.id === id ? ary.push(item) : null;
    }
    return ary;
}
console.log(HAHA); //在js中默认会把元素的id设置为变量，（不需要获取）
```

#### 1.1.11.2. DOM常用方法

##### 1.1.11.2.1. 节点（node）
> 在一个html中出现的所有东西都是节点
每个节点都会有一些属性区分自己的特性和特征
1. nodeType 节点类型
2. nodeName 节点名称
3. nodeValue 节点的值

- 元素节点（HTML）标签 1
```
1. nodeType 1
2. nodeName 大写签名
3. nodeValue null
```
- 文本节点（文字内容） 3
```
1. nodeType 3
2. nodeName "#text"
3. nodeValue 文本内容
```
- 注释节点 (注释内容) 8
```
1. nodeType 8
2. nodeName "#comment"
3. nodeValue 注释内容
```
- 文档节点 （document）9
```
1. nodeType 9
2. nodeName "#document"
3. nodeValue null
```

##### 1.1.11.2.2. 描述节点之间关系的属性
- parentNode 获取当前节点唯一的父亲节点
- childNodes 获取当前元素的子节点（儿子级的）包含所有元素节点，文本节点
- children 获取当前元素的所有元素子节点
- previousSibling 获取当前节点的上一个哥哥节点（元素或文本）
- previousElementSibling 上一个元素节点
- nextSibling 下一个弟弟节点（元素或文本）
- nextElementSibling 下一个元素节点
- firstChild 获取当前元素的第一个字节点 （元素或文本）
- firstElementChild
- lastChild 获取当前元素的最后一个字节点
- lastElementChild
- 
#### 1.1.11.3. DOM 的增删改查
##### 1.1.11.3.1. creteElement 创建一个元素标签
```
document.createElement([标签名])
```
##### 1.1.11.3.2. appendChild 把一个元素对象插入到指定容器末尾
```
[container].appendChild([newEle])
```
##### 1.1.11.3.3. insertBefore  把一个元素对象插入到指定容器的某个标签之前
```
[container].insertBefore([newEle], [oldEle])
```
##### 1.1.11.3.4. cloneNode 把某个节点进行克隆
```
[curEle].cloneNode() //浅克隆
[curEle].cloneNode(true) //深克隆，元素里面的内容也克隆
```
##### 1.1.11.3.5. removeChild 把容器中的某个节点删除
```
[container].removeChild([curEle])
```
##### 1.1.11.3.6. set/get/removeAttribute 设置、获取，删除当前某个元素自定义属性
```
oBox.myIndex = 10
oBox['myIndex']
delete oBox.myIndex
```
```
oBox.setAttribute('mycolor','blue');
oBox.getAttribute('mycolor');
oBox.removeAttribute('mycolor');
```

