# 修改Typora编辑器的字体


Typora 是一个非常优秀的 Markdown 编辑器，但是由国外开发者开发，对于中文本地化支持不算特别完善，其中之一就是默认的字体是西方字体，在写作时，打的中文双引号根本无法分清楚是左双引号还是右双引号，所以需要自行修改一下配置，使用中文的字体，我们在写作时也会更方便直观。
对于老手来说，只需要修改两个文件即可，详细修改在后面说明：

```
C:\Users\XXXXXX\AppData\Roaming\Typora\conf\conf.user.json
C:\Users\XXXXXX\AppData\Roaming\Typora\themes\github.css
```

## 修改主题配置
首先打开 Typora，点击主题，可以查看你当前使用的是那款主题。目前我使用的是 Github。
其次依次打开：文件 =&gt; 偏好设置 =&gt; 外观 =&gt; 主题 =&gt; 打开主题文件夹，然后就会在文件管理器中打开你的主题目录，可以选择你使用的主题对应的 css 文件，比如 github.css，用代码编辑器打开后，搜索 `body` ，找到 body 中的 font-family 属性，然后添加自己想要的字体，比如中文的微软雅黑，以及英文代码常用的 Consolas：

```css
body {
    font-family: Consolas,&#34;Microsoft Yahei&#34;,&#34;Clear Sans&#34;, &#34;Helvetica Neue&#34;, Helvetica, Arial, &#39;Segoe UI Emoji&#39;, sans-serif;
    color: rgb(51, 51, 51);
    line-height: 1.6;
}
```
虽然添加了很多字体，但是只有第一个有效字体生效，上面的加了后，就是遇到英文，会使用 Consolas 字体显示，遇到中文，就会使用微软雅黑显示。
## 修改源码配置
源码模式下的字体，也可以修改一下，否则只会显示预览模式的。依次打开：文件 =&gt; 通用 =&gt; 打开高级设置。然后在文件管理器中打开 conf.user.json 文件，修改 defaultFontFamily 选项：

```json
  &#34;defaultFontFamily&#34;: {
    &#34;standard&#34;: &#34;Microsoft Yahei&#34;, //String - Defaults to &#34;Times New Roman&#34;.
    &#34;serif&#34;: &#34;Microsoft Yahei&#34;, // String - Defaults to &#34;Times New Roman&#34;.
    &#34;sansSerif&#34;: &#34;Consolas&#34;, // String - Defaults to &#34;Arial&#34;.
    &#34;monospace&#34;: &#34;Consolas&#34; // String - Defaults to &#34;Courier New&#34;.
  },
```

## 关于字体选择
这里总结了一下比较满意的中英文字体组合推荐，也可以自己设置几种喜欢的字体试试。
- mac 下：
	英文字体：Monaco
	中文字体：PingFangSC-Regular
- win 下：
英文字体：Consolas
中文字体：Microsoft Yahei

参考：
- [把TYPORA改为微软雅黑&#43;CONSOLAS](https://www.cnblogs.com/zhaobinyouth/p/12679513.html)
- [修改typora主题的字体](https://www.codenong.com/cs106134472/)


---

> 作者:   
> URL: http://localhost:1313/archives/%E4%BF%AE%E6%94%B9typora%E7%BC%96%E8%BE%91%E5%99%A8%E7%9A%84%E5%AD%97%E4%BD%93/  

