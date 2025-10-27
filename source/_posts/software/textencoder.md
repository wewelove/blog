---
title: TextEncoder 文件编码转换神器
date: 2025-10-23 15:56:28
categories:
  - [软件, 效率工具]
tags:
  - TextEncoder
  - 编码
  - 结束符
---

## TextEncoder

TextEncoder 专门用于更改文件的编码：程序读取和写入格式 ASCII、ANSI、Latin-1 （ISO 8859-1）、Latin-2 （ISO 8859-2）、Win-1250（中欧）、Win-1251（西里尔字母）、Win-1252（西欧）、Win-1253（希腊语）、CP437、 UTF-7、UTF-8、UTF-16 大端、UTF-16 小端、UTF-32 大端和 UTF-32 小端。这些格式中的每一种都可以任意转换为任何其他格式。您可以指定是否应将字节顺序标记写入文件。

TextEncoder 还可以更改所用换行符的类型。支持换行符 CRLF（Windows、DOS、OS/2、CP/M、TOS）、LF（Unix、Linux、macOS、Mac OS X、AmigaOS）、CR（经典 Mac OS、Apple II、Commodore）和 NL（AIX OS、IBM 大型机系统、OS/390）以及换行符 FF、NEL、LS、PS、VT 和 TAB 的各种 Unicode 字符。此外，任何自定义字符或字符串也可以用作读写的换行符。这些自定义字符可以选择直接定义为文本或以其代码点的形式定义。读取文件时，还可以在多个不同换行符的列表中换行。

![封面](/images/textencoder-basic.png)

::: center
{% btn https://www.sttmedia.com/textencoder, 官方网站, fas fa-home, blue outline %}
{% btn https://www.sttmedia.com/textencoder-help, 使用手册, fas fa-book, green outline %}
:::

## 功能特性

- **Unicode 格式转换：** 使用文本编码器，可以将各种文本文件从一种 [Unicode 格式](https://www.sttmedia.com/unicode-fileformats) 转换为另一种格式，或者转换为 ANSI。[支持的格式](https://www.sttmedia.com/textencoder-formats)有 ASCII、[ANSI](https://www.sttmedia.com/unicode-ansi)、[UTF-7](https://www.sttmedia.com/unicode-utf7)、[UTF-8](https://www.sttmedia.com/unicode-utf8)、[UTF-16](https://www.sttmedia.com/unicode-utf16) 大端、UTF-16 小端、[UTF-32](https://www.sttmedia.com/unicode-utf32) 大端、UTF-32 小端、拉丁文 1 （ISO 8859-1）、拉丁文 2 （ISO 8859-2）、CP437、Win-1250（中欧）、Win-1251（西里尔文）、Win-1252（西欧）和 Win-1253（希腊文）。这些格式中的每一种都可以转换为任何其他格式。

- **换行符类型的更改：** 此外，可以使用 TextEncoder 更改文本文件所用[换行符](https://www.sttmedia.com/newline)的类型。TextEncoder 支持换行类型 CR LF （Windows、DOS、OS/2、CP/M、TOS）、LF （Unix、Linux、MacOS、Mac OS X、AmigaOS）、CR （Classic Mac OS、Apple II、Commodore） 和 NL （AIX OS、IBM Mainframe Systems、OS/390） 以及通过预设换行符 FF、NEL、LS、PS、VT 和 TAB 的各种 [Unicode](https://www.sttmedia.com/unicode) 字符。此外，任何用户定义的字符或字符串都可以直接定义为文本或以代码点的形式定义以用作换行符。

- **固定线长：** 除了由换行符定义的换行符外，TextEncoder 还支持由每行固定数量的字符定义的换行符。具有此类行限制且没有特定换行符号的文件可以通过 TextEncoder 转换为任何其他基于字符的格式。另一个方向，因此可以使用 TextEncoder 从文本文件中删除换行符。

- **批处理：**它可以同时处理任意数量的文本文件。通过将文件从计算机的任何文件夹拖到文件列表中，可以轻松地将文件添加到程序中。添加文件并选择编码和/或换行符类型后，您只需单击"转换"即可将所有更改应用于所有文件。

- **字节顺序标记（BOM）：** 该程序可用于从文件中删除 [字节顺序标记（BOM）](https://www.sttmedia.com/unicode-byteordermark)或将字节顺序标记添加到文件中）。此外，当将文件从一种格式转换为另一种格式时，您可以根据需要写入带有或不带有字节顺序标记的文件。

- **多个字符的换行符：** 通常，在文件中，只有一个字符或一个字符串用作换行符。但是，TextEncoder 还支持使用任意数量的字符或字符串的列表，在所有这些字符或字符串中，行都是断行的。此列表可以定义为文本或代码点的形式。例如，如果文件中混合了多种括号类型，则此功能非常有用。在这种情况下，TextEncoder 可以通过将混合换行符转换为任何统一换行符类型来修复此类文件。

- **完整的 Unicode 支持：** 这些页面上列出的所有功能也可以使用 [Unicode 字符](https://www.sttmedia.com/unicode) 执行。无论您是想使用 Unicode 文件名，还是文件是否包含中文、西里尔文、希腊文、希伯来文或其他特殊字符，都无关紧要。

- **对不受支持的字符进行验证：** 在以特定编码保存文件之前，TextEncoder 会自动检查文件中包含的所有字符是否也可以在所选编码中表示。如果没有，则会显示相应的警告消息，其中包含受影响的字符，您可以决定是否要继续。如果通过命令行或脚本调用 TextEncoder，则不会显示警告消息。

- **脚本控制：** 在批 [处理文本编码器](https://www.sttmedia.com/textencoder-batch-version) 版本中，您可以使用批处理脚本执行文本编码器的所有功能。因此，您可以完全自动化转换过程。如果您在没有参数的情况下启动批处理版本，它可以像带有图形用户界面的普通文本编码器一样使用。

- **无需安装：** 该软件无需安装即可运行。这可以节省您的注册表，您可以立即使用该程序。

## 系列文章

{% series TextEncoder %}

## 下载地址

::: download
{% btn https://www.sttmedia.com/textencoder/download, 官网下载, iconfont icon-home, blue outline %}
:::

## 软件授权

[免费软件, 如果喜欢请支持本项目](https://www.sttmedia.com/donate){.btn-beautify .green}
