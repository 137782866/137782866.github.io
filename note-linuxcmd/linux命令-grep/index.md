# Linux命令-grep


## 关于grep

grep（global search regular expression(RE) and print out the line，全面搜索正则表达式并把行打印出来），**是一个强大的文本搜索工具**，能使用正则表达式搜索并把匹配的行打印出来。

Unix 的grep家族包括`grep`、`egrep`和`fgrep`。

egrep和fgrep的命令只跟grep有很小不同。egrep是grep的扩展，支持更多的re元字符。

fgrep就是fixed grep或fast grep，它们把所有的字母都看作单词，也就是说，正则表达式中的元字符表示其自身的字面意义，不再特殊。

而Linux使用GNU版本的grep。它功能更强，可以通过-G、-E、-F命令行选项来使用egrep和fgrep的功能。

grep的工作方式：

[grep linux 命令 在线中文手册 (51yip.com)](http://linux.51yip.com/search/grep)


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note-linuxcmd/linux%E5%91%BD%E4%BB%A4-grep/  

