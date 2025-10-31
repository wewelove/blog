---
title: Markdown 基本语法
abbrlink: '7894e390'
series: development-standards
categories:
  - - 笔记
tags:
  - markdown
date: 2023-08-20 10:27:22
---

## 语法

### 段落和换行

段落： 前后必须保留一个或多个的空行。  
段落内换行：要在行尾追加两个以上的空格然后回车。

```markdown
春晓

春眠不觉晓，处处闻啼鸟。  
夜来风雨声，花落知多少。
```

春晓

春眠不觉晓，处处闻啼鸟。  
夜来风雨声，花落知多少。

### 标题

在行首插入 1 到 6 个 `#` ，对应标题 `<h1>` 到 `<h6>`。

```markdown
# 标题 h1
## 标题 h2
### 标题 h3
#### 标题 h4
##### 标题 h5
###### 标题 h6
```

### 引用

在段落或其他内容前使用 `>` 符号，就可以将这段内容标记为 '引用' 的内容 `<blockquote>`。

```markdown
> 引用内容。

多行引用：

> 多行引用，第一行;  
> 多行引用，第二行;  
> 多行引用，第三行。

嵌套引用：

> 嵌套引用，第一层。
>
> > 嵌套引用，第二层。
> >
> > > 嵌套引用，第三层。
```

> 引用内容。

多行引用：

> 多行引用，第一行;  
> 多行引用，第二行;  
> 多行引用，第三行。

嵌套引用：

> 嵌套引用，第一层。
>
> > 嵌套引用，第二层。
> >
> > > 嵌套引用，第三层。

### 列表

无序列表项 `<ul>` 以 `*` `+` 或 `-` 加空格空格开始:

```markdown
* 无序列表 1
    + 嵌套子列表 11
    + 嵌套子列表 12
+ 无序列表 2
- 无序列表 3
```

- 无序列表 1
  - 嵌套子列表 11
  - 嵌套子列表 12

- 无序列表 2

- 无序列表 3

有序列表项 `<ol>` 以 `任意数字` 加 `.` 再加 `空格` 开始:

```markdown
1. 有序列表 1
   + 子列表 11
   + 子列表 12
1. 有序列表 2
1. 有序列表 3
```

1. 有序列表 1
   - 子列表 11
   - 子列表 12
2. 有序列表 2
3. 有序列表 3

列表项可以包含其它内容，内容必须以 4 个空格或 1 个制表符缩进:

```markdown
1. 项目中包含多个段落

    段落内容，  
    段落内容。
```

1. 项目中包含多个段落

    段落内容，  
    段落内容。

### 代码

````markdwon
```js
function fn () {
  var str = 'Hello World';
}
```
````

```js
function fn () {
  var str = 'Hello World';
}
```

行内代码:

```markdown
行内代码如 `<title></title>` ，` 是 Tab 键上方、数字 1 键左侧的按键。
```

行内代码如 `<title></title>` ，` 是 Tab 键上方、数字 1 键左侧的按键。

### 分隔线

使用三个或更多的 `*` 来添加分隔线 `<hr>`:

```markdown
***
```

***

### 链接

行内式格式为 `[链接名称](链接地址 "标题文本")` :

```markdown
[百度](https://www.baidu.com "百度")  
[Google](https://www.google.com.hk "谷歌")  
[Readme](README.md "相对路径")
```

[百度](https://www.baidu.com "百度")
[谷歌](https://www.google.com.hk "谷歌")
[说明](README.md "相对路径")

参考式链接的写法相当于行内式拆分成两部分，通过 *标识符* 来连接两部分:

```markdown
[链接名称][标识符]

[标识符]: 链接地址 "标题文本"
```

- 标识符可以是字母、数字、空白或标点符号;
- 参考式便于集中管理链接内容。

```markdown
[百度][baidu] [谷歌][google] [百度][1] [谷歌][2]

[baidu]: https://www.baidu.com "百度"
[google]: https://www.google.com.hk "谷歌"
[1]: https://www.baidu.com "百度"  
[2]: https://www.google.com.hk "谷歌"
```

[百度][baidu] [谷歌][google] [百度][1] [谷歌][2]

[baidu]: https://www.baidu.com "百度"
[google]: https://www.google.com.hk "谷歌"
[1]: https://www.baidu.com "百度"
[2]: https://www.google.com.hk "谷歌"

自动链接使用 `<>` 包括的链接地址或邮箱地址会被自动转换为超链接：

```markdown
<https://www.baidu.com>  
<123@email.com>
```

<https://www.baidu.com>  
<123@email.com>

### 图片

插入图片的语法和插入超链接的语法基本一致，只是在最前面多一个 `!`。

行内式的图片语法:

```markdown
![图片名称](图片地址)  
![图片名称](图片地址 "标题文本")
```

参考式的图片语法:

```markdown
![图片名称][标识符]

[标识符]: 图片地址  "标题文本"
```

```markdown
![It's Markdown!](http://7xlhbk.com1.z0.glb.clouddn.com/markdown.jpg "It's Markdown!")

![It's Markdown!][Markdown]

[Markdown]: http://7xlhbk.com1.z0.glb.clouddn.com/markdown.jpg  "It's Markdown!"

```

### 强调

使用 `*文本*` 或 `_文本_` 实现 `<em>文本</em>` ，通常表现为斜体:

```markdown
强调 *演示* _文本_
```

强调 *演示* *文本*

使用 `**文本**` 或 `__文本__` 实现 `<strong>文本</strong>`，通常表现为加粗:

```markdown
强调 **演示** __文本__
```

强调 **演示** **文本**

### 字符转义

反斜线 `\` 转义特殊字符:

```markdown
\ ` * _ {} [] () # + - . !
```

```markdown
- 2 * 1 = 2

\- 2 * 1 = 2
```

- 2 * 1 = 2

\- (2 * 1) = - 2

### 删除线

```markdown
~~删除线~~
```

~~删除线~~

### 表格

使用 `|` 分隔单元格，使用 `-` 分隔表头和其他行：

```markdown
| 表头 1 | 表头 2 |
| --- | --- |
| 单元格 11 | 单元格 21 |
| 单元格 12 | 单元格 22 |
```

| 表头 1    | 表头 2    |
| --------- | --------- |
| 单元格 11 | 单元格 21 |
| 单元格 12 | 单元格 22 |

> 建议 `|` 和 `-` 两侧保留空格，这样内容更加清晰。

使用 `:` 实现单元格左右或居中对齐：

```markdown
| 默认 | 左对齐 | 居中对齐 | 右对齐 |
| --- | :--- | :---: | ---: |
| 默认左对齐 | 此列文本左对齐 | 此列文本居中对齐 | 此列文本右对齐 |
| 左对齐 | 左对齐 | 居中对齐 | 右对齐 |
```

| 默认       | 左对齐         |     居中对齐     |         右对齐 |
| ---------- | :------------- | :--------------: | -------------: |
| 默认左对齐 | 此列文本左对齐 | 此列文本居中对齐 | 此列文本右对齐 |
| 左对齐     | 左对齐         |     居中对齐     |         右对齐 |

语法嵌套:

```markdown
| 强调 | 代码 | 链接 |
| --- | --- | --- |
| _百度_ | `Baidu` | [百度](https://www.baidu.com) |
| __谷歌__ | `Google` | [谷歌](https://www.google.com.hk) |
```

| 强调     | 代码     | 链接                              |
| -------- | -------- | --------------------------------- |
| *百度*   | `Baidu`  | [百度](https://www.baidu.com)     |
| **谷歌** | `Google` | [谷歌](https://www.google.com.hk) |

### 兼容 HTML 语法

可以直接引用 `HTML` 元素，元素的开始标签前和结束标签后各保留一个空行:

```markdown
<table class="table" style="color:red;">
  <tr><th>#</th><th>代码</th><th>输出</th></tr>
  <tr><td>1</td><td>date('Y-m-d')</td><td>2016-07-15</td></tr>
  <tr><td>2</td><td>date('Y-m-t')</td><td>2016-07-31</td></tr>
</table>
```

## 参考

- [Markdown 语法说明 - 中文](http://wowubuntu.com/markdown/)
- [Markdown 语法说明 - 英文](http://wowubuntu.com/markdown/)
- [Markdown 入门参考](https://github.com/wewelove/Learning-Markdown)
