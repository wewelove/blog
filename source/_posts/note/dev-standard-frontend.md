---
title: 前端开发规范
abbrlink: 28c50d86
series: development-standards
categories:
  - [笔记]
tags:
  - 规范
  - html
  - css
  - javascript
date: 2023-08-20 10:27:22
---

## HTML 规范

### HTML 类型

推荐使用 HTML5 的文档类型申明： <!DOCTYPE html>（建议使用 text/html 格式的 HTML。避免使用 XHTML。XHTML 以及它的属性，比如 application/xhtml+xml 在浏览器中的应用支持与优化空间都十分有限）。

- 规定字符编码；
- IE 兼容模式；
- 规定字符编码；
- doctype 大写；

正例：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="X-UA-Compatible" content="IE=Edge" />
    <meta charset="UTF-8" />
    <title>Page title</title>
  </head>
  <body>
    <img src="images/company-logo.png" alt="Company" />
  </body>
</html>
```

### 缩进

- 缩进使用 2 个空格；

- 嵌套的节点应该缩进；

### 分块注释

在每一个块状元素，列表元素和表格元素后，加上一对 HTML 注释。

`<!-- 英文 中文 start >`

`<!-- 英文 中文 end >`

 正例：

```html
<body>
  <!-- header 头部 start -->
  <header>
    <div class="container">
      <a href="#">
        <!-- 图片会把a标签给撑开，所以不用设置a标签的大小 -->
        <img src="images/header.jpg" />
      </a>
    </div>
  </header>
  <!-- header 头部 end -->
</body>
```

### 语义化标签

HTML5 中新增很多语义化标签，所以优先使用语义化标签，避免一个页面都是 div 或者 p 标签。

正例

```html
<header></header>
<footer></footer>
```

反例

```html
<div>
  <p></p>
</div>
```

### 引号

使用双引号(" ") 而不是单引号(' ') 。

正例： `<div class="news-div"></div>`

反例： `<div class='news-div'></div>`

## CSS 规范

### 命名

- 类名使用小写字母，以中划线分隔；
- id 采用驼峰式命名；
- scss 中的变量、函数、混合、placeholder 采用驼峰式命名；

- id 和 class 的名称总是使用可以反应元素目的和用途的名称，或其他通用的名称，代替表象和晦涩难懂的名称；

不推荐：

```css
.fw-800 {
  font-weight: 800;
}

.red {
  color: red;
}
```

推荐:

```css
.heavy {
  font-weight: 800;
}

.important {
  color: red;
}
```

### 选择器

css 选择器中避免使用标签名，总是考虑直接子选择器：

不推荐:

```css
.content .title {
  font-size: 2rem;
}
```

推荐:

```css
.content > .title {
  font-size: 2rem;
}
```

### 尽量使用缩写属性

不推荐：

```css
border-top-style: none;
font-family: palatino, georgia, serif;
font-size: 100%;
line-height: 1.6;
padding-bottom: 2em;
padding-left: 1em;
padding-right: 1em;
padding-top: 0;
```

推荐：

```css
border-top: 0;
font: 100%/1.6 palatino, georgia, serif;
padding: 0 1em 2em;
```

### 每个选择器及属性独占一行

不推荐：

```css
button, span {
  width:100px;height:50px;color:#fff;background:#00a0e9;
}
```

推荐：

```css
button,
span {
  width: 100px;
  height: 50px;
  color: #fff;
  background: #00a0e9;
}
```

### 省略 0 后面的单位

不推荐：

```css
div{
  padding-bottom: 0px;
  margin: 0em;
}
```

推荐：

```css
div{
  padding-bottom: 0;
  margin: 0;
}
```

### 避免使用 ID 选择器

避免使用 ID 选择器及全局标签选择器防止污染全局样式。

不推荐：

```css
#header{
  padding-bottom: 0px;
  margin: 0em;
}
```

推荐：

```css
.header{
  padding-bottom: 0px;
  margin: 0em;
}
```

## LESS 规范

### 代码组织

- 将公共 less 文件放置在 style/less/common 文件夹：

   `color.less, common.less`

- 按以下顺序组织：

```less
// 变量声明
@import "mixins/size.less";

// 样式声明
@default-text-color: #333;

.page {
  width: 960px;
  margin: 0 auto;
}
```

### 避免嵌套层级过多

将嵌套深度限制在 3 级，避免大量的嵌套规则：

不推荐：

```less
.main {
  .title {
    .name {
       color: #fff
    }
  }
}
```

推荐：

```less
.main-title {
   .name{
      color: #fff
   }
}
```

## Javascript 规范

### 命名

- 采用小写驼峰命名 lowerCamelCase，代码中的命名均不能以下划线，也不能以下划线或美元符号结束：

​  反例： `_name / name_ / name$`

- 方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格，必须遵从驼峰形式：

​  正例： `localValue / getHttpMessage() / inputUserId`

- 其中 method 方法命名必须是 **动词** 或者 **动词+名词** 形式：

​  正例：`saveShopCarData /openShopCarInfoDialog`

​  反例：`save / open / show / go`

​  **推荐，增、删、改、查、详情统一使用如下 5 个单词**

```text
add / delete / update / get / detail 
```

​  **函数方法常用的动词:**

```js
get 获取/set 设置,
add 增加/remove 删除
create 创建/destory 移除
start 启动/stop 停止
open 打开/close 关闭,
read 读取/write 写入
load 载入/save 保存,
create 创建/destroy 销毁
begin 开始/end 结束,
backup 备份/restore 恢复
import 导入/export 导出,
split 分割/merge 合并
inject 注入/extract 提取,
attach 附着/detach 脱离
bind 绑定/separate 分离,
view 查看/browse 浏览
edit 编辑/modify 修改,
select 选取/mark 标记
copy 复制/paste 粘贴,
undo 撤销/redo 重做
insert 插入/delete 移除,
add 加入/append 添加
clean 清理/clear 清除,
index 索引/sort 排序
find 查找/search 搜索,
increase 增加/decrease 减少
play 播放/pause 暂停,
launch 启动/run 运行
compile 编译/execute 执行,
debug 调试/trace 跟踪
observe 观察/listen 监听,
build 构建/publish 发布
input 输入/output 输出,
encode 编码/decode 解码
encrypt 加密/decrypt 解密,
compress 压缩/decompress 解压缩
pack 打包/unpack 解包,
parse 解析/emit 生成
connect 连接/disconnect 断开,
send 发送/receive 接收
download 下载/upload 上传,
refresh 刷新/synchronize 同步
update 更新/revert 复原,
lock 锁定/unlock 解锁
check out 签出/check in 签入,
submit 提交/commit 交付
push 推/pull 拉,
expand 展开/collapse 折叠
begin 起始/end 结束,
start 开始/finish 完成
enter 进入/exit 退出,
abort 放弃/quit 离开
obsolete 废弃/depreciate 废旧,
collect 收集/aggregate 聚集
```

- 常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长：

​  正例： `MAX_STOCK_COUNT`

​  反例： `MAX_COUNT`

### 代码格式

- 使用 2 个空格进行缩进：

​  正例：

```js
if (x < y) {
  x += 10;
} else {
  x += 1;
}
```

- 不同逻辑、不同语义、不同业务的代码之间插入一个空行分隔开来以提升可读性。任何情形，没有必要插入多个空行进行隔开。

### 字符串

统一使用单引号(‘)，不使用双引号(“)。这在创建 HTML 字符串非常有好处：

正例:

```js
let str = 'foo';
let testDiv = '<div id="test"></div>';
```

反例:

```js
let str = 'foo';
let testDiv = "<div id='test'></div>";
```

### 对象声明

- 使用字面值创建对象：

​  正例： `let user = {};`

​  反例： `let user = new Object();`

- 使用字面量来代替对象构造器：

​  正例：

```js
var user = {
  age: 0,
  name: 1,
  city: 3
};
```

​  反例：

```js
var user = new Object();
user.age = 0;
user.name = 0;
user.city = 0;
```

### 使用 ES6,7

必须优先使用 ES6,7 中新增的语法糖和函数。这将简化你的程序，并让你的代码更加灵活和可复用，比如箭头函数、await/async ， 解构， let ， for...of  等等。

### 括号

下列关键字后必须有大括号（即使代码块的内容只有一行）：if, else, for, while, do, switch, try, catch, finally, with。

正例：

```js
if (condition) {
  doSomething();
}
```

反例：

```js
if (condition) doSomething();
```

### undefined 判断

永远不要直接使用 undefined 进行变量判断；使用 typeof 和字符串 'undefined' 对变量进行判断。

正例：

```js
if (typeof person === 'undefined') {
    ...
}
```

反例：

```js
if (person === undefined) {
    ...
}
```

### 条件判断和循环最多三层

条件判断能使用三目运算符和逻辑运算符解决的，就不要使用条件判断，但是谨记不要写太长的三目运算符。如果超过 3 层请抽成函数，并写清楚注释。

### this 的转换命名

对上下文 this 的引用只能使用 'self' 来命名。

### 慎用 console.log

因 console.log 大量使用会有性能问题，所以在非 webpack 项目中谨慎使用 console.log 功能。
