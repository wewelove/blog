---
title: 为 Hexo 增加时序图解析功能
date: 2017-09-06 16:43:33
categories:
- 工具
tags:
- hexo
- uml
- sequence
---

## 安装

[hexo-filter-sequence](https://github.com/bubkoo/hexo-filter-sequence) 插件:

```sh
npm install --save hexo-filter-sequence
```

## 配置

站点配置文件 `_config.yml` 中增加如下配置:

```yml
sequence:
  raphael: https://cdn.bootcss.com/raphael/2.2.8/raphael.min.js
  webfont: https://cdn.bootcss.com/webfont/1.6.28/webfontloader.js
  snap: https://cdn.bootcss.com/snap.svg/0.5.1/snap.svg-min.js
  underscore: https://cdn.bootcss.com/underscore.js/1.9.1/underscore-min.js
  sequence: https://cdn.bootcss.com/js-sequence-diagrams/1.0.6/sequence-diagram-min.js
  # css: # optional, the url for css, such as hand drawn theme 
  options: 
    theme: simple
```

## 源码

源码修改后才能正常使用，进入插件目录作如下修改：

```js
// index.js
var assign = require('deep-assign');
var renderer = require('./lib/renderer');

hexo.config.sequence = assign({
  webfont: 'https://cdnjs.cloudflare.com/ajax/libs/webfont/1.6.27/webfontloader.js',
  // sequence-diagram 1.x 版本依赖 raphael, 2.x版本依赖 snap
  raphael: 'https://cdnjs.cloudflare.com/ajax/libs/raphael/2.2.7/raphael.min.js',
  snap: 'https://cdnjs.cloudflare.com/ajax/libs/snap.svg/0.4.1/snap.svg-min.js',
  underscore: 'https://cdnjs.cloudflare.com/ajax/libs/underscore.js/1.8.3/underscore-min.js',
  sequence: 'https://cdnjs.cloudflare.com/ajax/libs/js-sequence-diagrams/1.0.6/sequence-diagram-min.js',
  css: '',
  options: {
    theme: 'simple'
  }
}, hexo.config.sequence);

hexo.extend.filter.register('before_post_render', renderer.render, 9);
```

```js
// lib/renderer.js, 25 行
if (sequences.length) {
      var config = this.config.sequence;
      // resources
      data.content += '<script src="' + config.webfont + '"></script>';
      // sequence-diagram 1.x 版本依赖 raphael, 2.x版本依赖 snap
      data.content += '<script src="' + config.raphael + '"></script>';
      data.content += '<script src="' + config.snap + '"></script>';
      data.content += '<script src="' + config.underscore + '"></script>';
      data.content += '<script src="' + config.sequence + '"></script>';
      ......
}
```

## 示例

新建文章，增加如下内容：

```txt
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```
