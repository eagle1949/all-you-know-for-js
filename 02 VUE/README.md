<!-- TOC -->

- [vue 基础](#vue-基础)
    - [1.data 数据监控](#1data-数据监控)
    - [2.vue 数据劫持 Object.defineProperty](#2vue-数据劫持-objectdefineproperty)
    - [3.vue实例上的方法](#3vue实例上的方法)
        - [1.vm.$el](#1vmel)
        - [2.vm.$options](#2vmoptions)
        - [3.vm.$nextTick](#3vmnexttick)
        - [4.vm.$watch](#4vmwatch)
        - [5.vm.$set](#5vmset)
    - [4.模板template](#4模板template)
    - [5.directive](#5directive)
        - [v-html v-once v-if v-show](#v-html-v-once-v-if-v-show)
        - [v-for](#v-for)
        - [v-model](#v-model)
        - [select](#select)
        - [radio](#radio)
        - [checkbox](#checkbox)
        - [修饰符 .number  .trim](#修饰符-number--trim)
        - [修饰符 键盘修饰符 鼠标的修饰符](#修饰符-键盘修饰符-鼠标的修饰符)
        - [属性绑定 :  v-bind:  绑定样式 class style  对象 数组（多个） 数组里可以放对象](#属性绑定---v-bind--绑定样式-class-style--对象-数组多个-数组里可以放对象)
    - [6.computed](#6computed)
        - [computed 和 watch 区别  watch可以支持异步](#computed-和-watch-区别--watch可以支持异步)
        - [computed 全选 反选](#computed-全选-反选)
    - [7. 生命周期](#7-生命周期)
    - [8. 组件](#8-组件)
        - [组件开发的优点](#组件开发的优点)
        - [8.1 组件的定义方式](#81-组件的定义方式)
        - [8.2 组件传递](#82-组件传递)

<!-- /TOC -->
## vue 基础
### 1.data 数据监控
```
<div id="app">
    {{info.address}}  {{arr}}
</div>
```
```
let vm = new Vue({
    el: '#app',
    data : { //存放数据
        msg : 'hello',
        info: {xxx:'xxx'},
        arr : [1, 2, 3, 4]
    }
})

```
### 2.vue 数据劫持 Object.defineProperty
- 如果属性不存在 默认后增加的内容 并不会刷新视图
- 数组调用push 是无效的    Object.defienProperty 不支持数组的
- 数组不能通过长度修改 也不能通过数组的索引进行更改

```
// 数据源
let obj = {
    name : 'come',
    age : {
        age : 19
    }
}

function observer(obj){
    if(typeof obj == 'object'){
        for(let key in obj){
            defineReactive(obj, key, obj[key]);
        }
    }
}

function defineReactive(obj, key, value){
    observer(value); //判断value是不是一个对象，如果是对象，继续监控
    Object.defineProperty(obj, key, {
        get(){
            return value;
        },
        set(val){
            observer(val);
            console.log('数据更新了');
            value = val;
        }
    })
}
observer(obj);
// obj.age = {name:1};
// vue 把 这个数组上的所有方法 都重写了
let arr = ['push','slice','shifit','unshift']
arr.forEach(method=>{
    let oldPush = Array.prototype[method];
    Array.prototype[method] = function(value){
        console.log('数据更新了111')
        oldPush.call(this,value);
    }
})
obj.arr = [];
obj.arr.push(6);//会更新数据
obj.arr.length--;//不会更新数据
```


### 3.vue实例上的方法
#### 1.vm.$el
```
console.log(vm.$el) 
//<div id="app">
//          [
//  5,
//  6,
//  7
//]
//    </div>
```
#### 2.vm.$options
对象参数
#### 3.vm.$nextTick
- 数据变化后更新视图操作是异步执行的
- 在同一事件循环中的数据变化后，DOM完成更新，立即执行nextTick(callback)内的回调。
```
vm.$nextTick(()=>{
    console.log(vm.$el.innerHTML);
    console.log(vm.$options);
})
```
#### 4.vm.$watch
watch是观察一个特定的值，当该值变化时执行特定的函数
```
vm.$watch('info.xxx', function(newValue, oldValue){
    console.log(newValue, oldValue);
})
```
#### 5.vm.$set
把数据都代理给了vm
```
vm.$set(vm.info,'address','zf');
vm.arr = [1,3,4]
```

### 4.模板template
取值表达式  可以 运算  , 取值 ， 做三元
```
<div id="app">
        <!-- 取值表达式  可以 运算  , 取值 ， 做三元 -->
    {{1+1}} 
    {{msg}}
    {{flag?'ok':'no'}}
    {{ {name:1} }}
    {{ 'msg' + 'hello'}}
</div>
```
```
let vm = new Vue({
    el:'#app',
    data:{
        msg:'hello',
        flag:true
    }
});
```
### 5.directive
#### v-html v-once v-if v-show
```
 <div id="app">
    <!-- 内部会进行缓存 以后使用的都是缓存里的结果 -->
   <div v-once>{{msg}}</div>
   <!-- v-html  innerHTML XSS攻击  不能将用户输入的内容展现出来 内容必须是可信任的-->
   <div v-html="d"></div>
    <!-- v-if 如果不成里 dom就会消失 -->
    <!-- v-if 控制的是 dom 有没有 v-show控制的是样式   -->
    <!-- v-show不支持template -->
    <template v-show="flag">
        <div>hello</div>
        <div >123</div>
    </template>
   <div>312</div>
</div>
```
```
<script>
    let vm = new Vue({
        el:'#app',
        data:{
            msg:'hello',
            d:'<h1>hello</h1>',
            flag:false
        }
    });
</script>
```
#### v-for
尽量不要使用index做为key
```
<div v-for="(fruit,index) in fruits" :key="index">
    <div v-if="index%2">{{fruit}}</div>
    <div v-else>{{index}}</div>
</div>
<div v-for="(value,key) in info">
    {{value}}
</div> 
```
```
let vm = new Vue({
    el:'#app',
    data:{ // for value of [1,2,3]
        fruits:['🍌','🍏','🍊'],
        info:{name:'珠峰',age:10,address:'回龙观'},
        flag:true
    }
});
```
#### v-model
```
let vm = new Vue({
data:{
    val:0,
    c:'d',
    msg:'hello',
    selectValue:[],
    radioValue:'男',
    checkValues:[],
    checkValue:true,
    lists:[{value:'菜单1',id:1},{value:'菜单2',id:2},{value:'菜单3',id:3}]
},
methods:{
    fn(e,a){
      alert(1)
    }
} 
}).$mount('#app');
```
```
<input type="text" :value="msg" @input="e=>msg=e.target.value">
<!-- v-model 是  @input + :value 的一个语法糖 -->
<input type="text" v-model="msg">
{{msg}}
```
#### select
```
 <select v-model="selectValue" multiple>
    <option value="0" disabled>请选择</option>
    <option v-for="(list,key) in lists" :value="list.id">{{list.value}}</option>
</select>
{{selectValue}}
```
#### radio
```
<!--  radio  可以根据v-model来进行分组-->
男：<input type="radio" v-model="radioValue" value="男">
女：<input type="radio" v-model="radioValue" value="女">
{{radioValue}}
```
#### checkbox
```
 <!-- 全选 多选 true/ false -->
<input type="checkbox" v-model="checkValues"> {{checkValue}}
<!-- 爱好  --> 
<br>
游泳：<input type="checkbox" v-model="checkValues" value="游泳">
健身<input type="checkbox" v-model="checkValues" value="健身">
{{checkValues}}
```
####  修饰符 .number  .trim
```
<input type="text" v-model.number.trim="val"> {{val}}
```
####  修饰符 键盘修饰符 鼠标的修饰符
```
<input type="text" @keyup.ctrl.enter="fn">
```
#### 属性绑定 :  v-bind:  绑定样式 class style  对象 数组（多个） 数组里可以放对象
```
<div class="abc" :class="{b:true}">
    你好
</div>
<div class="abc" :class="['a','b',c]">
        你好
 </div>
 <div style="color:red" :style="{background:'blue'}">xxx</div>
 <div style="color:red" :style="[{background:'red',color:'blue'}]">xxx</div>
```
### 6.computed
#### computed 和 watch 区别  watch可以支持异步
```
computed : {
    // fullName(){
    //     return this.firstname + this.lastname
    // }
}
```
#### computed 全选 反选
```
全选 <input type="checkbox" v-model="checkAll">
<input type="checkbox" v-for="(item,key) in checks" v-model="item.value" :key="key">
```
```
let vm = new Vue({
 el:'#app',
 data:{
     checks:[{value:true},{value:false},{value:true}],
 },
 computed:{
     checkAll:{
         get(){
            return this.checks.every(check=>check.value)
         },
         set(value){ // 双向绑定数据
            this.checks.forEach(check =>check.value = value);
         }
     }
 }
})
```
### 7. 生命周期
![image](https://cn.vuejs.org/images/lifecycle.png)
```
<div id="app">
    {{a}} {{b}}
</div>
```
```
let vm = new Vue({
    el:'#app',
    data:{
       a:1,
       b:1
    },
    beforeCreate(){ // 钩子函数  beforeXXX   xxxxed
        console.log(this,this.$data); // 初始化自己的生命周期 并且 绑定自己的事件
    },
    created(){ // 如果想调用ajax 
        console.log(this.$data); // 可以获取数据和调用方法
    },
    beforeMount(){ // 第一次调用渲染函数之前
        console.log('渲染前')
    },
    template:'',
    mounted(){ // 获取真实dom  因为页面已经渲染完了
        console.log('渲染后',this.$el.innerHTML);
        this.a = 100;
        this.timer = setInterval(()=>{

        })
    },
    beforeUpdate(){
        this.b = 200;
        console.log('更新前')
    },
    updated(){ // 一般不要操作数据 可能会导致死循环
        console.log('更新后');
    },
    beforeDestroy(){
        // 当前实例还可以用
        clearInterval(this.timer);
        console.log('销毁前')
    },
    destroyed(){
        // 实例上的方法 监听都被移除掉
        console.log('销毁后')
    }
 });  // 第一种  路由切换  手动销毁
 vm.$destroy();
```
### 8. 组件
#### 组件开发的优点
- 开发的优点 
- 方便协作 
- 方便维护  
- 复用 （数据是根据传入的数据展示）
- 
#### 8.1 组件的定义方式
> 全局组件  局部组件

全局组件
```
 Vue.component('my-button',{
    data(){ // 为了每个组件的数据 互相不影响
        return {msg:'点我啊'}
    },
    template:`<button>{{msg}}</button>`
});
```
局部组件
```
let vm = new Vue({ // 根实例
    el:'#app',
    components:{
        'MyButton':{
            data(){
                return {msg:'点我啊'}
            },
            template:`<button>{{msg}}</button>`
        }
    }
});
```
#### 8.2 组件传递
