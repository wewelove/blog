# 4Coder Hexo Blog

## Hexo

- [Hexo 官方](https://hexo.io/zh-cn/)
- [Hexo 官方文档](https://hexo.io/zh-cn/docs/)

## 常用命令

### new

```bash
hexo new [layout] <title>
```

新建一篇文章。如果没有设置 `layout` 的话，默认使用 `_config.yml` 中的 `default_layout` 参数代替。如果标题包含空格的话，请使用引号括起来。


### generate

```bash
hexo generate
# 简写
hexo g
```

生成静态文件。

| 选项 | 描述 |
| --- | --- |
| -d, --deploy | 文件生成后立即部署网站 |
| -w, --watch | 监视文件变动 |
| -b, --bail | 生成过程中如果发生任何未处理的异常则抛出异常 |
| -f, --force | 强制重新生成文件 |
| -c, --concurrency | 最大同时生成文件的数量，默认无限制 |

### server

```bash
hexo server
# 简写
hexo s
```

启动服务器。默认情况下，访问网址为： http://localhost:4000/。

| 选项 | 描述 |
| --- | --- |
| -p, --port | 重设端口 |
| -s, --static | 只使用静态文件 |
| -l, --log | 启动日记记录，使用覆盖记录格式 |

### deploy

```bash
hexo deploy
# 简写
hexo d
```

部署网站。

| 参数 | 描述 |
| --- | --- |
| -g, --generate | 部署之前预先生成静态文件 |

## Themes

- [Alex][01] - A very simple, elegant and responsive for Hexo.
- [awe][02] - AWE inspired by Awesome UI Kit which really looks beautiful and awesome.
- [biture][03] - A single column, widget-less minimalist theme for Hexo, based on Pure css framework.
- [crisp][04] - A minimalist theme inspired by a Ghost theme of the same name by Kathy Qian.
- [fexo][05] - A minimalist design theme for Hexo.
- [noderce][06] - Theme for Hexo.
- [next][07] - NexT is built for easily use with elegant appearance. First things first, always keep things simple.
- [typescript][08] - ypescript is a minimal theme for Hexo.
- [TKL][09] - TKL is a responsive design theme for Hexo. 
- [vno][10] - Inspired by a Ghost theme.
- [yilia][11] - The Ghost crisp theme ported to Hexo.

[01]: https://github.com/ppoffice/hexo-theme-alex
[02]: https://github.com/kywk/hexo-theme-awe
[03]: https://github.com/kywk/hexo-theme-biture
[04]: https://github.com/guolin/crisp-hexo-theme
[05]: https://github.com/forsigner/fexo
[06]: https://github.com/willerce/hexo-theme-noderce
[07]: https://github.com/iissnan/hexo-theme-next
[08]: https://github.com/artchen/hexo-theme-typescript
[09]: https://github.com/SuperKieran/TKL
[10]: https://github.com/lenbo-ma/hexo-theme-vno
[11]: https://github.com/litten/hexo-theme-yilia
