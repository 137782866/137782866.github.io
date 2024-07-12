---
title: "Markdown 语法"
date: 2024-07-03T02:25:11+08:00
lastmod: 2024-07-03T02:25:11+08:00
draft: false
tags: ["建站"]
categories: ["建站"]
collections: ["Hugo建站日志"]
featuredImage: ""
featuredImagePreview: ""
---

内容来自FixIt：[Markdown 基本语法 | FixIt](https://fixit.lruihao.cn/zh-cn/documentation/content-management/markdown-syntax/basics/ "Markdown 基本语法 | FixIt")。本文只介绍Markdown基础部分语法，基本所有的Markdown编辑器都支持，关于扩充的Markdown语法，因为有些编辑器不支持，所以先不赘述。为什么选择Markdown？

**Markdown** 是一种更好的生成 **HTML** 内容的方式，其主要的一些好处是：

1. Markdown 简单易学，几乎没有多余的字符，因此编写内容也更快。
2. 用 Markdown 书写时出错的机会更少。
3. 可以产生有效的 XHTML 输出。
4. 将内容和视觉显示保持分开，这样就不会打乱网站的外观。
5. 可以在你喜欢的任何文本编辑器或 Markdown 应用程序中编写内容。
6. Markdown 使用起来很有趣！

## 标题

从 `h2` 到 `h6` 的标题在每个级别上都加上一个 `＃`:

```text
## h2 标题
### h3 标题
#### h4 标题
##### h5 标题
###### h6 标题
```

注意h1就是文章的标题，所以从Markdown都是从h2开始。

## 注释

注释是和 HTML 兼容的：

```纯文本
<!--
这是一段注释
-->
```

## 水平线

- `___`: 三个连续的下划线
- `---`: 三个连续的破折号
- `***`: 三个连续的星号

## 段落

按照纯文本的方式书写段落，如下段落：

```纯文本
Lorem ipsum dolor sit amet, graecis denique ei vel, at duo primis mandamus. Et legere ocurreret pri,
animal tacimates complectitur ad cum. Cu eum inermis inimicus efficiendi. Labore officiis his ex,
soluta officiis concludaturque ei qui, vide sensibus vim ad.
```

输出的 HTML 看起来像这样：

```纯文本
<p>Lorem ipsum dolor sit amet, graecis denique ei vel, at duo primis mandamus. Et legere ocurreret pri, animal tacimates complectitur ad cum. Cu eum inermis inimicus efficiendi. Labore officiis his ex, soluta officiis concludaturque ei qui, vide sensibus vim ad.</p>
```

可以使用一个空白行进行换行。

## 内联HTML元素

如果你需要某个 HTML 标签（带有一个类）, 则可以简单地像这样使用：

```纯文本
Markdown 格式的段落。

<div class="class">
    这是 <b>HTML</b>
</div>

Markdown 格式的段落。
```

## 强调

### 加粗

用于强调带有较粗字体的文本片段。

```纯文本
**渲染为粗体**
__渲染为粗体__
```

### 斜体

```纯文本
*渲染为斜体*
_渲染为斜体_
```

### 删除线

```纯文本
~~这段文本带有删除线。~~
```

### 组合

```纯文本
***加粗和斜体***
~~**删除线和加粗**~~
~~*删除线和斜体*~~
~~***加粗，斜体和删除线***~~
```

## 引用

用于在文档中引用其他来源的内容块。在要引用的任何文本之前添加 `>`:

```纯文本
> **Fusion Drive** combines a hard drive with a flash storage (solid-state drive) and presents it as a single logical volume with the space of both drives combined.
```

引用也可以嵌套：

```纯文本
> Donec massa lacus, ultricies a ullamcorper in, fermentum sed augue.
Nunc augue augue, aliquam non hendrerit ac, commodo vel nisi.
>> Sed adipiscing elit vitae augue consectetur a gravida nunc vehicula. Donec auctor
odio non est accumsan facilisis. Aliquam id turpis in dolor tincidunt mollis ac eu diam.
```

## 列表

### 无序列表

一系列项的列表，其中项的顺序没有明显关系。可以使用以下任何符号来表示无序列表中的项：

```纯文本
* 一项内容
- 一项内容
+ 一项内容
```

### 有序列表

一系列项的列表，其中项的顺序确实很重要。

```纯文本
1. Lorem ipsum dolor sit amet
2. Consectetur adipiscing elit
3. Integer molestie lorem at massa
4. Facilisis in pretium nisl aliquet
5. Nulla volutpat aliquam velit
6. Faucibus porta lacus fringilla vel
7. Aenean sit amet erat nunc
8. Eget porttitor lorem
```

如果你对每一项使用 `1.`, Markdown 将自动为每一项编号。

## 任务列表

任务列表使你可以创建带有复选框的列表。 要创建任务列表，请在任务列表项之前添加破折号 (`-`) 和带有空格的方括号 (`[ ]`)，要选择一个复选框，请在方括号之间添加 x (`[x]`)。

```纯文本
- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media
```

## 代码

### 行内代码

用 `` ` `` 包装行内代码段。

```纯文本
在这个例子中，`hello world` 会被包裹成 **代码**。
```

### 缩进代码

将几行代码缩进至少四个空格。

```纯文本
    // Some comments
    line 1 of code
    line 2 of code
    line 3 of code
```

### 围栏代码块

使用 “围栏” ` ``` ` 来生成一段带有语言属性的代码块。

````纯文本
```
Sample text here...
```
````

### 代码高亮

在代码 “围栏” 之后直接添加你要使用的语言的文件扩展名，如` ```js`，语法高亮显示将自动应用于渲染的 HTML 中。

````纯文本
```js
grunt.initConfig({
  assemble: {
    options: {
      assets: 'docs/assets',
      data: 'src/data/*.{json,yml}',
      helpers: 'src/custom-helpers.js',
      partials: ['src/partials/**/*.{hbs,md}']
    },
    pages: {
      options: {
        layout: 'default.hbs'
      },
      files: {
        './': ['src/templates/pages/index.hbs']
      }
    }
  }
};
```
````

## 表格

通过在每个单元格之间添加竖线作为分隔线，并在标题下添加一行破折号（也由竖线分隔）来创建表格。注意，竖线不需要垂直对齐。

```纯文本
| Option | Description |
| ------ | ----------- |
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |
```

注意：

在任何标题下方的破折号右侧添加冒号将使该列的文本右对齐。

在任何标题下方的破折号两边添加冒号将使该列的对齐文本居中。

```纯文本
| Option | Description |
|:------:| -----------:|
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |
```

## 链接

### 基本链接

```纯文本
<https://assemble.io>
<contact@revolunet.com>
[Assemble](https://assemble.io)
```

### 添加一个标题

```纯文本
[Upstage](https://github.com/upstage/ "Visit Upstage!")
```

### 引用式链接

引用式链接是一种特殊的链接，它使 URL 在 Markdown 中更易于显示和阅读。

引用式链接分为两部分：与文本保持内联的部分以及存储在文件中其他位置的部分，以使文本易于阅读。

```纯文本
[text][id]
//...
[id]: http://example.org/ "title"
```

例如：

```纯文本
[FixIt][fixit-repo]

[fixit-repo]: https://github.com/hugo-fixit/FixIt "A clean, elegant but advanced blog theme for Hugo"
```

## 定位标记

定位标记使你可以跳至同一页面上的指定锚点。

例如，每个章节：

```纯文本
## Table of Contents
  * [Chapter 1](#chapter-1)
  * [Chapter 2](#chapter-2)
  * [Chapter 3](#chapter-3)
```

将跳转到这些部分：

```纯文本
## Chapter 1 <a id="chapter-1"></a>
Content for chapter one.

## Chapter 2 <a id="chapter-2"></a>
Content for chapter one.

## Chapter 3 <a id="chapter-3"></a>
Content for chapter one.
```

定位标记的位置几乎是任意的。因为它们并不引人注目，所以它们通常被放在同一行了。

## 脚注

脚注使你可以添加注释和参考，而不会使文档正文混乱。 当你创建脚注时，会在添加脚注引用的位置出现带有链接的上标编号。 读者可以单击链接以跳至页面底部的脚注内容。

要创建脚注引用，请在方括号中添加插入符号和标识符 (`[^1]`)。 标识符可以是数字或单词，但不能包含空格或制表符。 标识符仅将脚注引用与脚注本身相关联 - 在脚注输出中，脚注按顺序编号。

在中括号内使用插入符号和数字以及用冒号和文本来添加脚注内容 (`[^1]：这是一段脚注`)。 你不一定要在文档末尾添加脚注。可以将它们放在除列表，引用和表格等元素之外的任何位置。

```纯文本
这是一个数字脚注 [^1]
这是一个带标签的脚注 [^label]

[^1]: 这是一个数字脚注
[^label]: 这是一个带标签的脚注
```

## 图片

图片的语法与链接相似，但包含一个在前面的感叹号。

```纯文本
![Minion](https://octodex.github.com/images/minion.png)
![Alt text](https://octodex.github.com/images/stormtroopocat.jpg "The Stormtroopocat")

```
