---
title: "hugo安装部署"
date: 2024-07-01T12:00:00+08:00
lastmod: 2024-07-01T12:00:00+08:00
draft: false
tags: ["建站"]
categories: ["建站"]
collections: ["Hugo建站日志"]
featuredImage: ""
featuredImagePreview: ""
---

所有的操作都是在Windows平台。

## 安装git

地址：[Git - Downloads](https://git-scm.com/downloads "Git - Downloads")，下载 `64-bit Git for Windows Setup` 版本，然后安装即可。

安装时需要勾选必要的选项，成功安装后，鼠标右键菜单中会出现如下两个选项：

![](/image/image_Ov1sRufoOb.png)

## 安装hugo

git 仓库下载地址：[gohugoio/hugo](https://github.com/gohugoio/hugo/tags "gohugoio/hugo")，选择最新的 release 版本，然后直接下载 `hugo extended` 版本（extended是hugo的复杂版本，功能更多），比如现在最新的是 `hugo_extended_0.127.0_windows-amd64.zip`。

下载后，解压到某文件目录下，比如：`D:\Hugo\bin`，然后开始配置环境变量：

1. 按 Win+R 键，输入 `sysdm.cpl`  进入“系统属性”，然后点击“高级”。
2. 点击“环境变量”，然后找到“系统变量”。
3. 在“系统变量”部分，找到名为“Path”的变量，点击“编辑”。
4. 然后点击“新建”，将上面的路径 `D:\Hugo\bin` 输入，然后“确定”。
5. 打开cmd，输入 `hugo version` 回车，若出现 hugo 版本，则表示 hugo 正常安装。

## hugo 生成站点

先到对应的文件目录下，右键使用 Git Bash（也可以用 Cmd 或者 PowerShell 打开），生成一个自己的站点名 SiteName：

```纯文本
hugo new site SiteName
```

这样就在当前目录下生成一个 SiteName 文件夹，然后进入文件中，目录结构如下：

```纯文本
archetypes/
assets/
content/
data/
i18n/
layouts/
static/
themes/
hugo.toml
```

- **创建文章**

  创建一个 about.md 页面：
  ```纯文本
  hugo new about.md
  ```
  about.md 生成到了 `content/about.md` 下面，打开 about.md 内容如下：
  ```纯文本
  +++
  title = 'About'
  date = 2024-06-25T21:55:16+08:00
  draft = true
  +++

  正文内容...

  ```
  内容是 `Markdown` 格式，如果不了解 Markdown 语法，需要先熟悉一下。

  `+++` 之间的内容是 TOML 格式的，根据喜好也可以换成 YAML 格式（使用 `---` 标记）或者 JSON 格式。

  因为 about.md 界面大多是用于显示我关于栏的内容，是一个单独的界面。

  创建文章时，一般会放到 `post` 目录，方便之后生成聚合页面。可以编辑文章，先将 draft 属性改为  false 或者删掉，还可以添加其他属性，这里先不赘述。
  ```纯文本
  hugo new post/first.md
  ```

## hugo安装主题

可以去 [hugo主题商店](https://themes.gohugo.io/ "hugo主题商店") 找到自己喜欢的主题。

这里我选择的是 `FixIt` 主题，作者是国人所以对中文支持的很好，并且比较现代化，支持的格式比较多。

- 主题 github 地址：[hugo-fixit/FixIt](https://github.com/hugo-fixit/FixIt/releases "hugo-fixit/FixIt")
- 主题 wiki 地址：[FixIt](https://fixit.lruihao.cn/zh-cn/ "FixIt")
- 作者博客：[菠菜眾長 - 李瑞豪的博客](https://lruihao.cn/ "菠菜眾長 - 李瑞豪的博客")。

下载最新的 release 版本，然后在站点 `SiteName\theme` 目录下创建主题文件夹 `FixIt`，将下载的 release 版 zip 压缩包里的文件全部解压到 `SiteName\theme\FixIt` 下。

然后在站点的根目录执行 hugo 命令：

```纯文本
hugo server --theme=FixIt --buildDrafts
```

如果正常的话，会出现一个 web server 地址：`http://localhost:1313`。

然后在浏览器里打开这个地址（注意：为了避免浏览器缓存，可以使用 InPrivate 隐身模式打开），然后看皮肤是否运行正常，这里先不说 hugo.toml 主题相关的其他配置，只是测试主题是否正常。
