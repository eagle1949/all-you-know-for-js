<!-- TOC -->

- [1. css基础](#1-css基础)
    - [1.1. css实际上是如何工作的](#11-css实际上是如何工作的)
    - [1.2. 如何将你的css应用到html上](#12-如何将你的css应用到html上)
    - [1.3. 选择器](#13-选择器)
        - [1.3.1. 简单选择器 id class](#131-简单选择器-id-class)
        - [1.3.2. 属性选择器](#132-属性选择器)
        - [1.3.3. 伪类 ：hover](#133-伪类-hover)
        - [1.3.4. 伪元素 ：：after ::before ::first-letter](#134-伪元素-after-before-first-letter)
        - [1.3.5. 组合器](#135-组合器)
        - [1.3.6. 多重选择器](#136-多重选择器)
    - [1.4. css的单位和值](#14-css的单位和值)
    - [1.5. 层叠和继承](#15-层叠和继承)
    - [1.6. 框模型](#16-框模型)
    - [1.7. css框类型](#17-css框类型)
    - [1.8. 基本文本和字体样式](#18-基本文本和字体样式)
        - [1.8.1. 字体样式](#181-字体样式)
        - [1.8.2. 文本布局](#182-文本布局)
        - [1.8.3. 列表，特定样式](#183-列表特定样式)
        - [1.8.4. 样式区块化](#184-样式区块化)
        - [1.8.5. 使用css改变背景样式](#185-使用css改变背景样式)
        - [1.8.6. css边框](#186-css边框)
        - [1.8.7. 盒子阴影和文本阴影](#187-盒子阴影和文本阴影)
        - [1.8.8. filters 过滤器](#188-filters-过滤器)
    - [1.9. css布局](#19-css布局)
        - [1.9.1. 浮动布局](#191-浮动布局)
        - [1.9.2. 定位布局](#192-定位布局)
        - [1.9.3. css表格](#193-css表格)
        - [1.9.4. 弹性盒子](#194-弹性盒子)
        - [1.9.5. 网格](#195-网格)

<!-- /TOC -->
# 1. css基础
> css 是一种用于向用户指定文档如何呈现的语言

## 1.1. css实际上是如何工作的
- 浏览器将HTML和css转化为DOM，DOm在计算机内存中表示文档
- 浏览器显示DOM内容
```
HTML->parse HTML(LoadCss)->parse css ->create DomTree->display
```

## 1.2. 如何将你的css应用到html上
- 外部样式
- 内部样式
- 内联样式


- css 语法 （属性，属性值）
- css 声明 （`{}`开始结束，` ;`分割）
- css选择器和规则
css 定义了那些规则比其他规则更优先


## 1.3. 选择器
### 1.3.1. 简单选择器 id class
1. 类型选择器 span div
2. 类选择器 class
3. ID选择器 id
4. 通用选择器 *

### 1.3.2. 属性选择器
[attr][attr:val]
### 1.3.3. 伪类 ：hover
### 1.3.4. 伪元素 ：：after ::before ::first-letter
### 1.3.5. 组合器 
1. A,B 满足A或B
2. A B B是A的后代
3. A>B B是A的直接后代
4. A+B 相邻 B是A的下一个节点
5. A~B B是A之后的任意一个

### 1.3.6. 多重选择器


## 1.4. css的单位和值
1. 数值
长度尺寸： mm cm in pt pc em ex rem vw vh
2. 百分比
3. 颜色 rgb hsl
4. 坐标位置
5. 函数

## 1.5. 层叠和继承
- 最高位： ！important
- 千位: style
- 百位： ID
- 十位：类选择器，属性选择器，伪类
- 个位：元素选择器，伪元素


源代码次序
如果多个相互竞争的选择器具有相同的重要性和专业性，后面的规则战胜前面的规则
```
p{color: red}
p{color: blue} 
```

## 1.6. 框模型
weight height padding margin

- 背景裁剪
- background clip
- outline 轮廓
 

## 1.7. css框类型
- block 
- inline 
- inline-block


## 1.8. 基本文本和字体样式

### 1.8.1. 字体样式
- color: red; //颜色
- font-family: arial;  //字体种类
- font-size：14px; //字体大小
- font-style
- font-weight
- text-transform
- text-decoration
- text-shadow

### 1.8.2. 文本布局
- text-align: left; //文本对齐
- line-height：10px; //行高
- letter-spacing 
- word-spacing
- word-wrap //超出范围


### 1.8.3. 列表，特定样式
- list-style-type  //符号样式
- list-style-position //符号位置
- list-style-image  //自定义符号图片
- list-style

a:link
a:visited
a:focus
a:active

### 1.8.4. 样式区块化
- box-sizing 调整盒子模型
- 50% 包括边框和内边距

### 1.8.5. 使用css改变背景样式
- background-color
- background-image
- background-position
- background-repeat
- background:liner-gradient
- background-attachment  (scroll, fixed, local)
- background-size


### 1.8.6. css边框
- boder-top border-right
- border-radius
- border-iamge  //边框背景

### 1.8.7. 盒子阴影和文本阴影
- box-shadow （水平，垂直，模糊半径，颜色）
- text-shadow 


### 1.8.8. filters 过滤器
- drop-shadow
- blend modes 混合模式
- background-blend-mode
- min-blend-mode


## 1.9. css布局
### 1.9.1. 浮动布局 
```
float: left;
float: right;
float: none;  //清除浮动
float: inheri; //继承父亲的浮动属性
```
### 1.9.2. 定位布局
```
position: static //静态布局
position: relative //相对定位
position: absolute //绝对定位
position：fixed //固定定位
```
### 1.9.3. css表格
```
display: table
display: table-row
display: table-cell
display: table-caption
```

### 1.9.4. 弹性盒子
```
display:flex
```
- 在父内容里面垂直居中一个块内容
- 使容器的所有子项占用等量的宽度/高度，而不管有多少宽度，高度可用
- 使多列布局中的所有列采用相同的高度，即使它们包含的内容量不同

主轴： main axis
交叉轴：cross axis
flex container

- flex-direction: row column row-reverse column-reverse //行还是列
- flex-wrap: wrap
- flex: 200px
- flex-flow : row wrap //两个合并（flex-wrap flex-direction）
- align-item: center //交叉
- justify-item：space-around


```
align-item: stretch
align-item: center; //居中对齐
align-item：flex-start //开始对齐
align-item:flex-end //结束对齐
```

```
justify-content: flex-start
justify-content: flex-end
justify-content: center;
justify-content: space-around (均匀对齐)
justify-content: space-between (不留空间)
```

### 1.9.5. 网格