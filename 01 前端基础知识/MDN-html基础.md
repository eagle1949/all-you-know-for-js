<!-- TOC -->

- [1. html 基础](#1-html-基础)
    - [1.1. HTML （HyperText Markup Language）](#11-html-hypertext-markup-language)
        - [1.1.1. html元素组成](#111-html元素组成)
        - [1.1.2. 嵌套元素](#112-嵌套元素)
        - [1.1.3. 块级元素和内联元素](#113-块级元素和内联元素)
        - [1.1.4. 空元素](#114-空元素)
        - [1.1.5. 元素属性](#115-元素属性)
        - [1.1.6. 分析html文档](#116-分析html文档)
            - [1.1.6.1. html中的空白](#1161-html中的空白)
            - [1.1.6.2. 实体引用](#1162-实体引用)
            - [1.1.6.3. html注释](#1163-html注释)
    - [1.2. html 头部](#12-html-头部)
        - [1.2.1. 元数据 meta元素](#121-元数据-meta元素)
    - [1.3. html基础](#13-html基础)
    - [1.4. 超链接](#14-超链接)
        - [1.4.1. 统一资源定位起（URI）](#141-统一资源定位起uri)
    - [1.5. 高阶文字排版](#15-高阶文字排版)
    - [1.6. 文档与网站架构](#16-文档与网站架构)
    - [1.7. 多媒体与嵌入](#17-多媒体与嵌入)
    - [1.8. 表格](#18-表格)
    - [1.9. html 表单](#19-html-表单)
        - [1.9.1. 通用属性](#191-通用属性)
        - [1.9.2. 文本输入域 input](#192-文本输入域-input)
        - [1.9.3. 多行文本域 textarea](#193-多行文本域-textarea)
        - [1.9.4. 下拉内容 select](#194-下拉内容-select)
        - [1.9.5. 按钮](#195-按钮)
        - [1.9.6. 高级表单组件](#196-高级表单组件)

<!-- /TOC -->
# 1. html 基础
## 1.1. HTML （HyperText Markup Language）
定义： 告知浏览器如何组织页面的标记语言
### 1.1.1. html元素组成
```
<p>我的猫咪脾气暴</p>
```
- 开始标签 (`<p>`)
- 结束标签 (`</p>`)
- 内容     (我的猫咪脾气暴)
- 元素    (`<p>我的猫咪脾气暴</p>`)

### 1.1.2. 嵌套元素
所有元素都要有正确的打开关闭
```
<p> 我的猫咪脾气<strong>暴</strong> </p> //right
<p> <strong>我的猫咪</p></strong> //wrong 
```

### 1.1.3. 块级元素和内联元素
- 块级元素在页面中以块的形式展示，相对于前面内容它会出现在新的一行，其内容在下一行。（div，p）
- 内联元素不会导致文本换行 （em，a，span，strong）

### 1.1.4. 空元素
没有开始标签和结束标签，通常此元素内部嵌入一些东西 （img）

### 1.1.5. 元素属性
```
<p class="edit">我的猫咪脾气暴</p>
```
- 布尔属性 disabled="disabled"
- 省略包围属性的引号 
```
<p class="edit" title=The Mozilla>我的猫咪脾气暴</p>  //只会读取the
```
建议始终加上引号，这样可以避免很多问题

- 单引号或者双引号都可以


### 1.1.6. 分析html文档
```
<!DOCYPE html>  //声明文档类型
<html>          //根元素
    <head>      //容器
        <meta charset="utf-8">  //文档utf-8
        <title>我的测试站点</title>  //标题
    </head>
    <body>             //包含了你访问页面显示在页面的内容，文本图片音频视频等
        <p>my page</p> 
    </body>
</html>
```
#### 1.1.6.1. html中的空白
```
<p> 狗   狗 很呆萌</p>
```
无论你用了多少空格，当渲染这些代码的时候，html解释器会将连续出现的空白字符减少为一个单独的空格。

#### 1.1.6.2. 实体引用
- < &lt
- > &gt
- " &quot
- ' &apos
- & &amp

#### 1.1.6.3. html注释
```
<!-- -->
```

## 1.2. html 头部
html头部是包含在head元素里的内容，不会在页面中显示

```
<head>
    <meta charset="utf-8" />
    <title> my test page</title>
</head>
```
### 1.2.1. 元数据 meta元素
元数据就是描述数据的数据
- 指定你的文档字符的编码
```
<meta charset="utf-8" />
```
- 添加作者和描述：
name （类型）
content （内容）

## 1.3. html基础
- p 段落
- h1 h2 ...h6 标题
- 为什么我们需要结构化
1. 快速定位 2.seo 3.css样式化
- 为什么我们需要语义？
1. 我们需要确保使用了正确的元素来给予内容正确的意思、作用以及外形

- 列表 lists  ul>li 无序 ol>li 有序
- strong 强调
- span 默认网格
- b 斜体
- i 粗体
- u 下划线


## 1.4. 超链接
```
<a title="my page" href="http://www.baidu.com">baidu</a>
```
### 1.4.1. 统一资源定位起（URI）
- 绝对链接
- 相对链接
- 电子邮件链接


## 1.5. 高阶文字排版
- dl>dt>dd 描述列表
- blockquote 引用，块引用
- q 行内应用
- site 引文
- abbr 缩略语
- address 联系人
- sup 上标
- sub 下标
- code 标记计算机通用代码
- pre 代码块
- var
- time 时间


## 1.6. 文档与网站架构
- header 标题位置
- nav 导航栏
- main 主内容
- aside 侧边栏
- footer 边角
- br hr 换行分割


## 1.7. 多媒体与嵌入
- img
- video
- audio
- iframe
- svg embed object


## 1.8. 表格
- table tr td th
- 为表格中的列提供相同样式
```
<colgroup>
    <col span="2">
    <col style="color:black">
</colgroup>
```
- caption 
- thead
- tfoot
- tbody
- scope 属性，用于帮助屏幕阅读设备更好理解表格

## 1.9. html 表单
### 1.9.1. 通用属性
- autofocus 允许你指定的当前页面加载时元素改自动输入焦点
- disabled 用户不能与元素交互
- name 元素名称，用户表单数据提交
- value 元素的初始值
- form 小部件与之相关联的表单元素

### 1.9.2. 文本输入域 input
- type="text" 单行文本域
- type="email" email
- type="password" 密码域
- type="search" 搜索域
- type="tel" 电话号码
- type="url" url域
- type="checkbox" 可选中项
- type="radio" 单选

### 1.9.3. 多行文本域 textarea
```
<textarea cols=30 rows=10></textarea>
```
- cols 文本控制可见宽度
- rows 控制可见文本行数
- wrap hard（换行） soft（不换行）

### 1.9.4. 下拉内容 select
```
<select><option><option></select>
```
- multiple 多选
- 自动补全输入框
```
<datalist>
    <option></option>
</datalist>
```

### 1.9.5. 按钮
- submit 
- reset
- Button

### 1.9.6. 高级表单组件
- type="number" 数字
- type=range 滑块
- type=datetime-local 本地时间
- type=month 月
- type=time 时间
- type=week 星期
- type=color 拾色器
- type=file 文件
- type=hidden 隐藏
- type=image 图像按钮
- progress 进度条