# Hugo文章格式1


所有的格式需要参考：[FixIt内容管理](https://fixit.lruihao.cn/zh-cn/documentation/content-management/ &#34;FixIt内容管理&#34;)文档，这里只列出一些常用的格式，便于简单的日常写作。

## 前置参数

Hugo 允许你在文章内容前面添加 `yaml`, `toml` 或者 `json` 格式的前置参数

不是所有的以下前置参数都必须在你的每篇文章中设置。 很多只有在文章的参数和你的主题配置中的 `page` 部分不一致时才有必要这么做。参数如下：

```Markdown
title: 文章标题
subtitle: 文章副标题
date: 这篇文章创建的日期时间它通常是从文章的前置参数中的date字段获取的，但是也可以在主题配置中设置
lastmod: 上次修改内容的日期时间
draft: 如果设为true, 除非hugo命令使用了--buildDrafts/-D参数，这篇文章不会被渲染
author: 文章作者配置，和主题配置中的params.author部分相同
authorAvatar: 是否启用文章作者头像
description: 文章内容的描述
keywords: 文章内容的关键词
license: 这篇文章特殊的许可
images: 页面图片，用于Open Graph和Twitter Cards
tags: 文章的标签
categories: 文章所属的类别
collections: 文章所属的合集
featuredImage: 文章的特色图片
featuredImagePreview: 用在主页预览的文章特色图片
hiddenFromHomePage: 如果设为true, 这篇文章将不会显示在主页上
hiddenFromSearch: 如果设为true, 这篇文章将不会显示在搜索结果中
hiddenFromRss: 如果设为true, 这篇文章将不会显示在RSS中
hiddenFromRelated: 如果设为true, 这篇文章将不会显示在相关文章中
twemoji: 如果设为true, 这篇文章会使用twemoji
lightgallery: 和主题配置中的params.page.lightgallery部分相同
ruby: 如果设为true, 这篇文章会使用上标注释扩展语法
fraction: 如果设为true, 这篇文章会使用分数扩展语法
fontawesome: 如果设为true, 这篇文章会使用Font Awesome扩展语法
linkToMarkdown: 如果设为true, 内容的页脚将显示指向原始Markdown文件的链接
linkToSource: 如果设为true, 内容的页脚将显示指向源码的链接
linkToEdit: 如果设为true, 内容的页脚将显示指向编辑页面的链接
linkToReport: 如果设为true, 内容的页脚将显示指向报告问题的链接
rssFullText: 如果设为true, 在 RSS 中将会显示全文内容
pageStyle: 页面样式，详见 页面宽度
toc: 和主题配置中的params.page.toc部分相同
expirationReminder: 和主题配置中的params.page.expirationReminder部分相同
heading: 新增和主题配置中的params.page.heading部分相同
code: 和主题配置中的params.page.code部分相同
math: 和主题配置中的params.page.math部分相同
mapbox: 和主题配置中的params.page.mapbox部分相同
share: 和主题配置中的params.page.share部分相同
comment: 和主题配置中的params.page.comment部分相同
library: 和主题配置中的params.page.library部分相同
seo: 和主题配置中的params.page.seo部分相同
type: 页面渲染模板，详见页面模板
menu: 详见添加内容到菜单
password: 加密页面内容的密码，详见内容加密
message: 加密提示信息，详见内容加密
repost: 和主题配置中的params.page.repost部分相同
autoBookmark: 和主题配置中的params.page.autoBookmark部分相同
wordCount: 和主题配置中的params.page.wordCount部分相同
readingTime: 和主题配置中的params.page.readingTime部分相同
endFlag: 和主题配置中的params.page.endFlag部分相同
reward: 和主题配置中的params.page.reward部分相同
instantPage: 和主题配置中的params.page.instantPage部分相同

```

例如，一个常用的前置参数为：

```Markdown
---
title: &#34;第一篇文章测试&#34;
date: 2024-07-01T12:00:00&#43;08:00
lastmod: 2024-07-01T12:00:00&#43;08:00
draft: false
tags: [&#34;标签1&#34;, &#34;标签2&#34;]
categories: [&#34;分类1&#34;, &#34;分类2&#34;]
collections: [&#34;合集1&#34;, &#34;合集2&#34;]
featuredImage: &#34;/image/xxx.png&#34;
featuredImagePreview: &#34;/image/xxx.png&#34;
author:
  name: &#34;&#34; # 文章作者
  link: &#34;&#34; # 文章作者的链接
  email: &#34;&#34; # 文章作者的邮箱，用于设置Gravatar头像，优先于avatar
  avatar: &#34;&#34; # 文章作者的头像
---
```

## 内容摘要

FixIt使用内容摘要在主页中显示大致文章信息。默认情况下，Hugo自动将内容的前 70 个单词作为摘要。

你可以通过在网站配置中设置 `summaryLength` 来自定义摘要长度。

如果你要使用CJK中文/日语/韩语等语言创建内容，并且想使用 Hugo 的自动摘要拆分功能，请在网站配置中将 `hasCJKLanguage` 设置为 `true`。

### 手动拆分摘要

可以添加 `&lt;!--more--&gt;` 摘要分割符来拆分文章生成摘要，摘要分隔符之前的内容将用作该文章的摘要。

&gt; 注：小心输入 `&lt;!--more--&gt;` ，全部为小写且没有空格。

如果摘要不是文章开头的文字。可以在文章前置参数的 `summary` 变量中设置单独的摘要。

如果希望将文章前置参数中的 `description` 变量的内容作为摘要，仍然需要在文章开头添加 `&lt;!--more--&gt;` 摘要分割符。但需要将摘要分隔符之前的内容保留为空。

## 内容加密

### 全局加密

FixIt 主题提供了两个 front matter 用于全文加密。

`password`: \[必需] 加密页面内容的密码

`message`: \[可选] 加密提示信息

注意：

1. 每次输入正确密码后，会在用户本地缓存密码 hash 值，一天之内再次访问时，将自动解锁文章。
2. 文章最后提供有一个 “重新加密” 的按钮，点击即可立即忘记密码，并重新加密内容。
3. 加密文章已从搜索中隐藏。
4. 加密文章的 Markdown 输出已禁用。

### 部分加密

你可以使用 `fixit-encryptor` shortcode 来加密部分内容。加密层级支持无限级嵌套。`fixit-encryptor` shortcode具有以下命名参数：

`password=&#34;&#34;` \[必选]（第一个位置参数）部分加密内容的密码。

`message=&#34;&#34;`  \[可选]（第二个位置参数）解密输入框的提示信息。

实例1：

```Markdown```

## shortcodes

Hugo提供了多个内置的Shortcodes, 以方便作者保持Markdown内容的整洁。虽然可以使用HTML，但是正是因为它即使不经过渲染也可以轻松阅读。应该尽可能避免使用 HTML以保持内容简洁。

为了避免这种限制，Hugo创建了shortcodes。 shortcode是一个简单代码段，可以生成合理的 HTML 代码，并且符合 Markdown 的设计哲学。

Hugo 内置了一些预定义的 shortcodes，这里列出几个常用的：

### highlight （显示代码）

```text
{{&lt; highlight html &gt;}}
&lt;section id=&#34;main&#34;&gt;
    &lt;div&gt;
        &lt;h1 id=&#34;title&#34;&gt;{{ .Title }}&lt;/h1&gt;
        {{ range .Pages }}
            {{ .Render &#34;summary&#34;}}
        {{ end }}
    &lt;/div&gt;
&lt;/section&gt;
{{&lt; /highlight &gt;}}
```

显示效果：

```html
&lt;section id=&#34;main&#34;&gt;
    &lt;div&gt;
        &lt;h1 id=&#34;title&#34;&gt;{{ .Title }}&lt;/h1&gt;
        {{ range .Pages }}
            {{ .Render &#34;summary&#34;}}
        {{ end }}
    &lt;/div&gt;
&lt;/section&gt;
```

### youtube（嵌入视频）

```text
{{&lt; youtube w7Ft2ymGmfc &gt;}}
```

除此之外，FixIt扩展了一些shortcodes。

### ****link（链接功能）

link是 Markdown 链接语法的替代。link 可以提供一些其它的功能并且可以在代码块中使用。支持 本地资源引用 的完整用法。

其参数如下：

1. `href` \[必需]（第一个位置参数）链接的目标。
2. `content` \[可选]（第二个位置参数）链接的内容，默认值是 href 参数的值。支持 Markdown 或者 HTML 格式。
3. `title` \[可选]（第三个位置参数）HTML `a` 标签 的 `title` 属性，当悬停在链接上会显示的提示。
4. `card` \[可选]（第四个位置参数）是否显示为卡片式链接，默认值`false`。
5. `card-icon` \[可选]（第五个位置参数） 卡片式链接的图标，支持图片链接和 Font Awesome 图标。设置为`true`，自动从链接获取缩略图。
6. `download` \[可选] HTML`a`标签 的`download`属性。
7. `class` \[可选] HTML `a` 标签 的 `class` 属性。
8. `rel` \[可选] HTML `a` 标签 的 `rel` 补充属性。
9. `external-icon` \[可选] 是否自动显示外链图标。
10. `noreferrer` \[可选] `rel` 属性是否添加 `noreferrer`, 默认：`true`。

实例1：一个简单的超链接。

```Markdown
{{&lt; link href=&#34;https://assemble.io&#34; &gt;}}
{{&lt; link href=&#34;mailto:contact@revolunet.com&#34; &gt;}}
{{&lt; link href=&#34;https://assemble.io&#34; content=Assemble &gt;}}
```

实例4：可下载的卡片链接效果。

```Markdown
{{&lt; link href=&#34;/music/Wavelength.mp3&#34; content=&#34;Wavelength.mp3&#34; title=&#34;Download Wavelength.mp3&#34; download=&#34;Wavelength.mp3&#34; &gt;}}
{{&lt; link href=&#34;/music/Wavelength.mp3&#34; content=&#34;Wavelength.mp3&#34; title=&#34;Download Wavelength.mp3&#34; download=&#34;Wavelength.mp3&#34; card=true &gt;}}
```

### image（图片）

image用来插入图片，可以充分利用 [lightgallery](https://github.com/sachinchoolur/lightgallery &#34;lightgallery&#34;)，支持 本地资源引用 的完整用法。

其参数如下：

1. `src` \[必需]（第一个位置参数）图片的 URL。
2. `alt` \[可选]（第二个位置参数）图片无法显示时的替代文本，默认值是 src 参数的值。支持 Markdown 或者 HTML 格式。
3. `caption` \[可选]（第三个位置参数）图片标题。支持 Markdown 或者 HTML 格式。
4. `title` \[可选] 当悬停在图片上会显示的提示。
5. `class` \[可选] HTML `figure` 标签的 `class` 属性。
6. `src_s` \[可选] 图片缩略图的 URL, 用在画廊模式中，默认值是 src 参数的值。
7. `src_l` \[可选] 高清图片的 URL, 用在画廊模式中，默认值是 src 参数的值。
8. `height` \[可选] 图片的 `height` 属性。
9. `width` \[可选] 图片的 `width` 属性。
10. `linked` \[可选] 图片是否需要被链接，默认值是 `true`。
11. `rel` \[可选] HTML `a` 标签 的 `rel` 补充属性，仅在 linked 属性设置成 `true` 时有效。
12. `loading` \[可选] HTML `a` 标签 的 `loading` 补充属性，可选值：`eager`、`lazy`，默认值是 `lazy`。

示例1：显示一张图片。

```Markdown
{{&lt; image src=&#34;/images/lighthouse.jpg&#34; caption=&#34;Lighthouse&#34; src_s=&#34;/images/lighthouse-small.jpg&#34; src_l=&#34;/images/lighthouse-large.jpg&#34; &gt;}}
```

### admonition（提示横幅）

支持12种帮助你在页面中插入提示的横幅，支持 Markdown 或者 HTML 格式。

分别如下：

```Markdown
admonition 注意
abstract 
{{&amp;lt; /admonition &amp;gt;}}
```

```Markdown
{{&amp;lt; admonition  &amp;gt;}}
一个 **摘要** 横幅
{{&amp;lt; /admonition &amp;gt;}}
```

```Markdown
{{&amp;lt; admonition info &amp;gt;}}
一个 **信息** 横幅
{{&amp;lt; /admonition &amp;gt;}}
```

```Markdown
{{&amp;lt; admonition tip &amp;gt;}}
一个 **技巧** 横幅
{{&amp;lt; /admonition &amp;gt;}}
```

```Markdown
{{&amp;lt; admonition success &amp;gt;}}
一个 **成功** 横幅
{{&amp;lt; /admonition &amp;gt;}}
```

```Markdown
{{&amp;lt; admonition question &amp;gt;}}
一个 **问题** 横幅
{{&amp;lt; /admonition &amp;gt;}}
```

```Markdown
{{&amp;lt; admonition warning &amp;gt;}}
一个 **警告** 横幅
{{&amp;lt; /admonition &amp;gt;}}
```

```Markdown
{{&amp;lt; admonition failure &amp;gt;}}
一个 **失败** 横幅
{{&amp;lt; /admonition &amp;gt;}}
```

```Markdown
{{&amp;lt; admonition danger &amp;gt;}}
一个 **危险** 横幅
{{&amp;lt; /admonition &amp;gt;}}
```

```Markdown
{{&amp;lt; admonition bug &amp;gt;}}
一个 **Bug** 横幅
{{&amp;lt; /admonition &amp;gt;}}
```

```Markdown
{{&amp;lt; admonition example &amp;gt;}}
一个 **示例** 横幅
{{&amp;lt; /admonition &amp;gt;}}
```

```Markdown
{{&amp;lt; admonition quote &amp;gt;}}
一个 **引用** 横幅
{{&amp;lt; /admonition &amp;gt;}}
```

上面的相当于是自带的横幅，我们也可以自己定义横幅。

其参数如下：

1. `type` \[必需]（第一个位置参数）`admonition` 横幅的类型，默认值是 `note`。


2. `title` \[可选]（第二个位置参数）`admonition` 横幅的标题，默认值是 type 参数的值。（支持 markdown）。
3. `open` \[可选]（第三个位置参数）横幅内容是否默认展开，默认值是 `true`。

示例1：一个简单的自定义横幅。

```Markdown
{{&lt; admonition type=tip title=&#34;This is a tip&#34; open=false &gt;}}
一个 **技巧** 横幅
{{&lt; /admonition &gt;}}

```

```Markdown
{{&lt; admonition tip &#34;This is a tip&#34; false &gt;}}
一个 **技巧** 横幅
{{&lt; /admonition &gt;}}
```

### music（插入音乐）

music提供了一个内嵌的响应式音乐播放器。

#### 自定义音乐URL

支持本地资源引用的完整用法。

其参数如下：

1. `server` \[必需] 音乐的链接。
2. `type` \[可选] 音乐的名称。
3. `artist` \[可选] 音乐的创作者。
4. `cover` \[可选] 音乐的封面链接。

示例1：使用自定义音乐的URL。

```Markdown
{{&lt; music url=&#34;/music/Wavelength.mp3&#34; name=Wavelength artist=oldmanyoung cover=&#34;/images/Wavelength.jpg&#34; &gt;}}
```

#### 音乐平台URL自动识别

有一个命名参数来使用音乐平台 URL 的自动识别：

1. auto \[必需]（第一个位置参数）用来自动识别的音乐平台 URL，支持 `netease`，`tencent` 和 `xiami` 平台。

示例1：一个使用音乐平台 URL 的自动识别。


{{&lt; music auto=&#34;https://music.163.com/#/playlist?id=60198&#34; &gt;}}

#### 自定义音乐平台、类型和ID

有以下命名参数来使用自定义音乐平台：

1. `server` \[必需]（第一个位置参数）音乐平台 \[`netease`, `tencent`, `kugou`, `xiami`, `baidu`]。
2. `type` \[必需]（第二个位置参数）音乐类型 \[`song`, `playlist`, `album`, `search`, `artist`]。
3. `id` \[必需]（第三个位置参数）歌曲 ID，或者播放列表 ID，或者专辑 ID，或者搜索关键词，或者创作者 ID。

示例1：使用自定义音乐平台。

```Markdown
{{&lt; music server=&#34;netease&#34; type=&#34;song&#34; id=&#34;1868553&#34; &gt;}}

```

#### 其他参数

有一些可以应用于以上三种方式的其它命名参数：

1. `theme` \[可选] 音乐播放器的主题色，默认值是 `#448aff`。
2. `fixed` \[可选] 是否开启固定模式，默认值是 `false`。
3. `mini` \[可选] 是否开启迷你模式，默认值是 `false`。
4. `autoplay` \[可选] 是否自动播放音乐，默认值是 `false`。
5. `volume` \[可选] 第一次打开播放器时的默认音量，会被保存在浏览器缓存中，默认值是 `0.7`。
6. `mutex` \[可选] 是否自动暂停其它播放器，默认值是 `true`。

还有一些只适用于音乐列表方式的其它命名参数：

1. `loop` \[可选] \[`all`, `one`, `none`] 音乐列表的循环模式，默认值是 `none`。
2. `order` \[可选] \[`list`, `random`] 音乐列表的播放顺序，默认值是 `list`。
3. `list-folded` \[可选] 初次打开的时候音乐列表是否折叠，默认值是 `false`。
4. `list-max-height` \[可选] 音乐列表的最大高度，默认值是 `340px`。

### bilibili（嵌入视频）

提供了一个内嵌的用来播放 bilibili 视频的响应式播放器。

如果视频只有一个部分，则仅需要视频的 BV `id`。

示例1：一个bilibili视频链接。

```Markdown
https://www.bilibili.com/video/BV1Sx411T7QQ
```

```Markdown
{{&lt; bilibili id=BV1Sx411T7QQ &gt;}}
```

如果视频包含多个部分，则除了视频的 BV `id` 之外，还需要 `p`，默认值为 `1`。

例如：一个带有p参数的视频链接。

```Markdown
https://www.bilibili.com/video/BV1TJ411C7An?p=3
```

```Markdown
{{&lt; bilibili id=BV1TJ411C7An p=3 &gt;}}
```

**详细参数**

有以下命名参数：

1. `id` \[必需]（第一个位置参数）视频的 BV `id`。
2. `p` \[可选]（第二个位置参数）多 P 视频的集数。从 `1` 开始计数，默认值为 `1`。
3. `autoplay`  \[可选] 是否自动播放，默认值为 `false`。
4. `poster` \[可选] 是否展示封面，默认值为 `true`。
5. `muted` \[可选] 是否静音，默认值为 false。
6. `danmaku` \[可选] 是否开启弹幕，默认值为 `true`。
7. `t` \[可选] 跳转到媒体的初始时间点，默认值为 `0`，单位：秒。

### douyin（嵌入视频）

提供了一个内嵌的用来播放抖音视频的响应式播放器

视频 VideoID 可以通过 PC 端视频播放地址中获取。

示例1：播放抖音视频。

```markdown
https://www.douyin.com/video/7370344193077644584
```

```markdown
{{&lt; douyin id=7370344193077644584 &gt;}}
```

### typeit（打字动画）

typeit有以下命名参数：

1. `tag` \[可选]，内容容器的 HTML 标签。
2. `code` \[可选]，指定代码内容语言类型，可以实习语法高亮。
3. `code-link` \[可选]，是否解析代码内容中的 Markdown 链接，默认：`false`。
4. `group` \[可选]，内容分组，相同分组的内容将按顺序开始打字动画。
5. `loop` \[可选]，内容是否会在打字动画完成后继续循环。

注意：内容允许使用 `Markdown` 格式的简单内容，并且不包含富文本的块内容，例如图像等。

实例1：打印简单的带超链接的内容。

```text
{{&lt; typeit &gt;}}
这一个带有基于 [TypeIt](https://typeitjs.com/) 的打字动画的段落！
{{&lt; /typeit &gt;}}
```

实例2：打印代码内容

```text
{{&lt; typeit code=java &gt;}}
public class HelloWorld {
    public static void main(String []args) {
        System.out.println(&#34;Hello World&#34;);
    }
}
{{&lt; /typeit &gt;}}
```

注意：默认情况下，所有打字动画都是同时开始的。 但有时可能需要按顺序开始一组 `typeit` 内容的打字动画。一组具有相同 `group` 参数值的 `typeit` 内容将按顺序开始打字动画。

实例3：带有分组的顺序打字动画

```Markdown
{{&lt; typeit group=paragraph &gt;}}
首先, 这个段落开始
{{&lt; /typeit &gt;}}

{{&lt; typeit group=paragraph &gt;}}
接着, 这个段落开始
{{&lt; /typeit &gt;}}
```

注意：默认情况下，打字动画完成后将停止。若需要内容在打字动画完成后继续循环，可以使用 `loop` 参数。

实例4：循环打字动画

```markdown
{{&lt; typeit loop=true &gt;}}
这个段落将会循环
{{&lt; /typeit &gt;}}
```



---

> 作者: WindSun  
> URL: http://localhost:1313/notes/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/hugo%E6%96%87%E7%AB%A0%E6%A0%BC%E5%BC%8F/  

