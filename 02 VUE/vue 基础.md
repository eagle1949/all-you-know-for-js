<!-- TOC -->

- [0.1. vue 基础](#01-vue-基础)
    - [0.1.1. data 数据监控](#011-data-数据监控)
    - [0.1.2. vue 数据劫持 Object.defineProperty](#012-vue-数据劫持-objectdefineproperty)
    - [0.1.3. vue实例上的方法](#013-vue实例上的方法)
        - [0.1.3.1. vm.$el](#0131-vmel)
        - [0.1.3.2. vm.$options](#0132-vmoptions)
        - [0.1.3.3. vm.$nextTick](#0133-vmnexttick)
        - [0.1.3.4. vm.$watch](#0134-vmwatch)
        - [0.1.3.5. vm.$set](#0135-vmset)
    - [0.1.4. 模板template](#014-模板template)
    - [0.1.5. directive](#015-directive)
        - [0.1.5.1. v-html v-once v-if v-show](#0151-v-html-v-once-v-if-v-show)
        - [0.1.5.2. v-for](#0152-v-for)
        - [0.1.5.3. v-model](#0153-v-model)
        - [0.1.5.4. select](#0154-select)
        - [0.1.5.5. radio](#0155-radio)
        - [0.1.5.6. checkbox](#0156-checkbox)
        - [0.1.5.7. 修饰符 .number  .trim](#0157-修饰符-number--trim)
        - [0.1.5.8. 修饰符 键盘修饰符 鼠标的修饰符](#0158-修饰符-键盘修饰符-鼠标的修饰符)
        - [0.1.5.9. 属性绑定 :  v-bind:  绑定样式 class style  对象 数组（多个） 数组里可以放对象](#0159-属性绑定---v-bind--绑定样式-class-style--对象-数组多个-数组里可以放对象)
    - [0.1.6. computed](#016-computed)
        - [0.1.6.1. computed 和 watch 区别  watch可以支持异步](#0161-computed-和-watch-区别--watch可以支持异步)
        - [0.1.6.2. computed 全选 反选](#0162-computed-全选-反选)
    - [0.1.7. 生命周期](#017-生命周期)
    - [0.1.8. 组件](#018-组件)
        - [0.1.8.1. 组件开发的优点](#0181-组件开发的优点)
        - [0.1.8.2. 组件的定义方式](#0182-组件的定义方式)
        - [0.1.8.3. 组件的数据](#0183-组件的数据)
        - [0.1.8.4. 组件的属性应用以及校验](#0184-组件的属性应用以及校验)
            - [0.1.8.4.1. 属性应用](#01841-属性应用)
            - [0.1.8.4.2. 属性校验](#01842-属性校验)
            - [0.1.8.4.3. 批量传入数据](#01843-批量传入数据)
        - [0.1.8.5. 事件应用](#0185-事件应用)
        - [0.1.8.6. $parent $children](#0186-parent-children)
        - [0.1.8.7. Provide inject](#0187-provide-inject)
        - [0.1.8.8. ref特性](#0188-ref特性)
        - [0.1.8.9. slot](#0189-slot)
            - [0.1.8.9.1. 具名插槽](#01891-具名插槽)
            - [0.1.8.9.2. 作用域插槽](#01892-作用域插槽)
        - [0.1.8.10. 组件间通信](#01810-组件间通信)
        - [0.1.8.11. envetBus 事件车](#01811-envetbus-事件车)

<!-- /TOC -->
## 0.1. vue 基础
### 0.1.1. data 数据监控
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
### 0.1.2. vue 数据劫持 Object.defineProperty
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


### 0.1.3. vue实例上的方法
#### 0.1.3.1. vm.$el
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
#### 0.1.3.2. vm.$options
对象参数
#### 0.1.3.3. vm.$nextTick
- 数据变化后更新视图操作是异步执行的
- 在同一事件循环中的数据变化后，DOM完成更新，立即执行nextTick(callback)内的回调。
```
vm.$nextTick(()=>{
    console.log(vm.$el.innerHTML);
    console.log(vm.$options);
})
```
#### 0.1.3.4. vm.$watch
watch是观察一个特定的值，当该值变化时执行特定的函数
```
vm.$watch('info.xxx', function(newValue, oldValue){
    console.log(newValue, oldValue);
})
```
#### 0.1.3.5. vm.$set
把数据都代理给了vm
```
vm.$set(vm.info,'address','zf');
vm.arr = [1,3,4]
```

### 0.1.4. 模板template
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
### 0.1.5. directive
#### 0.1.5.1. v-html v-once v-if v-show
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
#### 0.1.5.2. v-for
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
#### 0.1.5.3. v-model
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
#### 0.1.5.4. select
```
 <select v-model="selectValue" multiple>
    <option value="0" disabled>请选择</option>
    <option v-for="(list,key) in lists" :value="list.id">{{list.value}}</option>
</select>
{{selectValue}}
```
#### 0.1.5.5. radio
```
<!--  radio  可以根据v-model来进行分组-->
男：<input type="radio" v-model="radioValue" value="男">
女：<input type="radio" v-model="radioValue" value="女">
{{radioValue}}
```
#### 0.1.5.6. checkbox
```
 <!-- 全选 多选 true/ false -->
<input type="checkbox" v-model="checkValues"> {{checkValue}}
<!-- 爱好  --> 
<br>
游泳：<input type="checkbox" v-model="checkValues" value="游泳">
健身<input type="checkbox" v-model="checkValues" value="健身">
{{checkValues}}
```
#### 0.1.5.7. 修饰符 .number  .trim
```
<input type="text" v-model.number.trim="val"> {{val}}
```
#### 0.1.5.8. 修饰符 键盘修饰符 鼠标的修饰符
```
<input type="text" @keyup.ctrl.enter="fn">
```
#### 0.1.5.9. 属性绑定 :  v-bind:  绑定样式 class style  对象 数组（多个） 数组里可以放对象
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
### 0.1.6. computed
#### 0.1.6.1. computed 和 watch 区别  watch可以支持异步
```
computed : {
    // fullName(){
    //     return this.firstname + this.lastname
    // }
}
```
#### 0.1.6.2. computed 全选 反选
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
### 0.1.7. 生命周期
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
### 0.1.8. 组件
#### 0.1.8.1. 组件开发的优点
- 开发的优点 
- 方便协作 
- 方便维护  
- 复用 （数据是根据传入的数据展示）
- 
#### 0.1.8.2. 组件的定义方式
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
#### 0.1.8.3. 组件的数据
组件的数据必须是函数形式
```
'MyButton':{
    data(){
        return {msg:'点我啊'}
    },
    template:`<button>{{msg}}</button>`
}
```
#### 0.1.8.4. 组件的属性应用以及校验
##### 0.1.8.4.1. 属性应用
```
<my-button :msg="content" a="1" b="2"></my-button>
let vm = new Vue({
    el: '#app',
    data : {
        content : '点我啊'
    },
    components : {
        'MyButton' : {
            props : ['msg', 'a'],
            template : `<div> my-button {{msg}} {{a}}</div>`
        }
    }
})
```
组件是驼峰，写成html要变成横杠

##### 0.1.8.4.2. 属性校验
```
 <my-button :msg="content" :a="1" b="2"></my-button>
let vm = new Vue({
    el: '#app',
    data : {
        content : '点我啊'
    },
    components : {
        'MyButton' : {
            //props : ['msg', 'a'],
            props : {
                msg : {
                    type : String,
                    default : '111'
                },
                a :{
                    type : Number,
                    default : '111'
                }
            },
            template : `<div> my-button {{msg}} {{a}}</div>`
        }
    }
})
```
##### 0.1.8.4.3. 批量传入数据
```
let vm = new Vue({
    el: '#app',
    data : {
        content : '点我啊'
    },
    components : {
      'MyButton' : {
        mounted(){
            console.log(this.$attrs);
        },
        inheritAttrs: false,
        template : `<div> my-button {{this.$attrs.msg}}my v-bind="$attrs"></my></div>`,
        components : {
            'my' : {
                props : ['a', 'b'],
                template : `<span>{{a}} {{b}}</span>`
            }
        }
    }
}
```
#### 0.1.8.5. 事件应用
儿子要调用父亲的方法  有三种方式
```
<my-button @click="change" @mouseup="change"></my-button>
let vm = new Vue({ 
    el:'#app',
    data:{ 
        content:'点我啊'
    },
    methods:{
        change(){
            alert(1);
        }
    },
    components:{
        'MyButton':{ // v-bind=$attrs  v-on=$listeners 绑定所有的方法
            template:`<div>
                <button @click="$listeners.click()">点我啊</button>
                <button @click="$emit('click')">点我啊</button>
                <button v-on="$listeners">点我啊</button>
            </div>`
        }
    }
});
```
#### 0.1.8.6. $parent $children
```
<collapse>
    <collapse-item title="react">内容1</collapse-item> 
    <collapse-item title="vue">内容2</collapse-item>
    <collapse-item title="node">内容3</collapse-item>
</collapse>
// 平级通信  $parent 获取父组件的实例 $children获取所有的儿子
Vue.component('Collapse',{
    mounted(){
        console.log(this._uid);
    },
    methods:{
        cut(childId){ // 只要儿子点击了 我就要知道当前点击的是谁
            this.$children.forEach(child => {
                if(child._uid !== childId){
                    child.show = false;
                }
            });
        }
    },
    template:`<div class="wrap">
        <slot></slot>
    </div>`
});
Vue.component('CollapseItem',{
    props:['title'],
    data(){
        return {show:false}
    },
    methods:{
        change(){
            this.$parent.cut(this._uid);
            this.show = !this.show;
        }
    },  
    template:`<div>
        <div class="title" @click="change">{{title}}</div> 
        <div v-show="show">
            <slot></slot>
        </div>   
    </div>`
})
// 手风琴效果 折叠菜单效果
let vm = new Vue({
    el:'#app',
    mounted(){
        console.log(this._uid)
    }
})
```
#### 0.1.8.7. Provide inject
```
Vue.component('my',{
    inject:['a'],
    template:"<div>{{a}}</div>"
})
let vm = new Vue({
    el:'#app',
    provide:{ // 在根上提供了一个a属性 ，所有的子组件 都可以获取
        a:1
    },
    mounted(){
        console.log(this._uid)
    }
})
```
#### 0.1.8.8. ref特性
ref不能给多个元素设置相同的ref 只识别一个
```
let vm = new Vue({
    el:'#app',
    mounted(){
        console.log(this.$refs.my);
        console.log(this.$refs.com.show());
    },
    components:{
        'myComponent':{
            methods:{
                show(){
                    alert(1);
                }
            },
            template:`<div>my-component</div>`
        }
    }
});
```
#### 0.1.8.9. slot
##### 0.1.8.9.1. 具名插槽 
```
<div id="app">
    <child>
        <header slot="header">header</header>
        <footer slot="footer">footer</footer>
    </child>

</div>
<script src="node_modules/vue/dist/vue.js"></script>
<!-- 只要模板中使用了数据，必须在实例上声明 -->
<script>
    let vm = new Vue({
        el: '#app',
        components:{
            'child':{
                data(){
                    return {arr:[1,2,3]}
                },
                template:`<div>
                        <slot name="header">default header</slot>
                        <div>content</div>
                        <slot name="footer">default footer</slot>
                    </div>`
             }
        }
    })
    // 把数据都代理给了vm
</script>
```
##### 0.1.8.9.2. 作用域插槽
```
 <child>
    <template slot-scope="props"><!--定义一个插槽，该插槽必须放在template标签内-->
        <li>{{props.value}}</li><--!定义使用渲染方式-->
    </template>
</child>
<child>
    <template slot-scope="props">
        <h1>{{props.value}}</h1><!--定义不同的渲染方式-->
    </template>
</child>
```
```
Vue.component('child',{
    data: function(){
        return {
            list:[1,2,3,4]
        }
    },
    template: `<div>
                    <ul>
                        <slot v-for="value in list" :value=value>//使用slot占位
                        </slot>
                    </ul>
                </div>`
})
var vm=new Vue({
    el: '#root'
})
原文：https://blog.csdn.net/willard_cui/article/details/82469114 

```
#### 0.1.8.10. 组件间通信
- 1.props和emit父亲组件向子组件传递数据通过prop传递，子组件传递数据给父组件是通过emit触发事件来做的
- 2.attrs和listenners
- 3.parent，children
- 4.$refs获取实例
- 5.父组件通过provider提供变量，然后在子组件通过inject来注入变量
- 6.envetBus平级组件数据传递，这种情况可以使用中央事件总线方式
- 7.vuex状态管理


#### 0.1.8.11. envetBus 事件车
> 在vue中传递数据都是通过属性传递（父子关系），父子通信是通过emit来触发父级事件，如果遇到平级组件可以通过共同的父级进行传递数据。但开发当中，我们经常遇到平级组件和跨组件数据交互就可以通过一个共同的实例进行数据传递。

通过事件来共享数据 （发布订阅）

创建bus实例
```
//lib/bus.js
import Vue from 'vue';
let $bus = new Vue();
Vue.prototype.$bus = $bus;
```
```
import './lib/bus';
```
```
//Boy组件，发射dinner事件
<template>
    <div>
        男孩 <button @click="sayToGirl()">对女孩说</button>
    </div>
</template>

<script>
    export default {
        methods : {
            sayToGirl(){
                this.$bus.$emit('dinner', 'are you hungry')
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>
```
```
//girl组件监听dinner事件
<template>
    <div>
        女孩 <span>{{message}}</span>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                message : ''
            }
        },
        mounted(){
            this.$bus.$on('dinner', (data)=>{
                this.message = data;
            })
        }
    }
</script>

<style lang="scss" scoped>

</style>
```
