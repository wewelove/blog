# Hexo 我的博客

- [Hexo 官方文档](https://hexo.io/docs/)
- [Butterfly 主题 - 快速开始](https://butterfly.js.org/posts/21cfbf15/)
- [Butterfly 主题 - 标签外挂](https://butterfly.js.org/posts/ceeb73f)
- [Butterfly 主题 - 示例](https://blog.uuanqin.top/)
- [Butterfly 主题 - 示例更多](https://butterfly.js.org/link/)
- [安知鱼 主题](https://docs.anheyu.com/)
- [安知鱼 主题 - 示例](https://blog.anheyu.com/)

## 快速开始

```sh
# 环境
node -v
v20.19.5

# 安装 hexo
npm install hexo-cli -g

# 安装依赖
yarn
# 启动服务
yarn server
# 构建
```

## 创建新文章

```bash
hexo new [layout] <title>
```

新建一篇文章。 如果没有设置 `layout` 的话，默认使用 `_config.yml` 中的 `default_layout` 参数代替。 使用布局 `draft` 来创建草稿。 如果标题包含空格的话，请使用引号括起来。

| 选项 | 描述 |
| --- | --- |
| `-p`, `--path` | 文章的路径。 自定义文章的路径。 |
| `-r`, `--replace` | 如果存在的话，替换当前的文章。 |
| `-s`, `--slug` | 文章别名。 自定义文章的 URL。 |

默认情况下，Hexo 会使用文章的标题来决定文章文件的路径。 对于独立页面来说，Hexo 会创建一个以标题为名字的目录，并在目录中放置一个 `index.md` 文件。 你可以使用 `--path` 参数来覆盖上述行为、自行决定文件的目录：

```bash
hexo new page --path about/me "About me"
```

以上命令会创建一个 `source/about/me.md` 文件，同时 Front Matter 中的 title 为 `"About me"`

注意！ title 是必须指定的！ 例如，这不会产生您可能期望的行为：

```bash
hexo new page --path about/me
```

此时 Hexo 会创建 `source/_posts/about/me.md`，同时 `me.md` 的 Front Matter 中的 title 为 `"page"`。 这是因为在上述命令中，hexo-cli 将 `page` 视为指定文章的标题、并采用默认的 `layout`。

```sh
hexo new post -p note/filename "文章标题"
```

## 部署

```sh
hexo clean && hexo generate && hexo deploy
```
