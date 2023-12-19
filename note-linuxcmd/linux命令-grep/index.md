# Linux命令-grep


## 关于grep

grep（global search regular expression(RE) and print out the line，全面搜索正则表达式并把行打印出来），**是一个强大的文本搜索工具**，能使用正则表达式搜索并把匹配的行打印出来。

Unix 的grep家族包括`grep`、`egrep`和`fgrep`。

egrep和fgrep的命令只跟grep有很小不同。egrep是grep的扩展，支持更多的re元字符。

fgrep就是fixed grep或fast grep，它们把所有的字母都看作单词，也就是说，正则表达式中的元字符表示其自身的字面意义，不再特殊。

而Linux使用GNU版本的grep。它功能更强，可以通过-G、-E、-F命令行选项来使用egrep和fgrep的功能。

## grep命令格式选项

grep的命令格式：

```shell
grep [OPTION]... PATTERNS [FILE]...
```

匹配模式选择:

```shell
-E, --extended-regexp     扩展正则表达式egrep
-F, --fixed-strings       一个换行符分隔的字符串的集合fgrep
-G, --basic-regexp        基本正则
-P, --perl-regexp         调用的perl正则
-e, --regexp=PATTERN      后面根正则模式，默认无
-f, --file=FILE           从文件中获得匹配模式
-i, --ignore-case         不区分大小写
-w, --word-regexp         匹配整个单词
-x, --line-regexp         匹配整行
-z, --null-data           一个 0 字节的数据行，但不是空行
```

杂项：

```shell
-s, --no-messages         不显示错误信息
-v, --invert-match        显示不匹配的行
-V, --version             显示版本号
--help                    显示帮助信息
--mmap                	  使用mmap输入
```

输入控制:

```shell
-m, --max-count=NUM       匹配的最大数
-b, --byte-offset         打印匹配行前面打印该行所在的块号码。
-n, --line-number         显示的加上匹配所在的行号
--line-buffered           刷新输出每一行
-H, --with-filename       当搜索多个文件时，显示匹配文件名前缀
-h, --no-filename         当搜索多个文件时，不显示匹配文件名前缀
--label=LABEL             print LABEL as filename for standard input
-o, --only-matching       只显示一行中匹配PATTERN 的部分
-q, --quiet, --silent     不显示任何东西
--binary-files=TYPE       假定二进制文件的TYPE 类型；TYPE 可以是`binary', `text', 或`without-match'
-a, --text                匹配二进制的东西
-I                        不匹配二进制的东西
-d, --directories=ACTION  目录操作，读取，递归，跳过
-D, --devices=ACTION      设置对设备，FIFO,管道的操作，读取，跳过
-R, -r, --recursive       递归调用
--include=PATTERN         只查找匹配FILE_PATTERN 的文件
--exclude=PATTERN         跳过匹配FILE_PATTERN 的文件和目录
--exclude-from=FILE       跳过所有除FILE 以外的文件
-L, --files-without-match 匹配多个文件时，显示不匹配的文件名
-l, --files-with-matches  匹配多个文件时，显示匹配的文件名
-c, --count               显示匹配的行数
-Z, --null                在FILE 文件最后打印空字符
```

文件控制：

```shell
-B, --before-context=NUM  打印匹配本身以及前面的几个行由NUM控制
-A, --after-context=NUM   打印匹配本身以及随后的几个行由NUM控制
-C, --context=NUM         打印匹配本身以及随后，前面的几个行由NUM控制
-NUM                      根-C的用法一样的
--color[=WHEN],
--colour[=WHEN]           使用标志高亮匹配字串；

-U, --binary              使用标志高亮匹配字串；
-u, --unix-byte-offsets   当CR 字符不存在，报告字节偏移(MSDOS 模式)
```

规则表达式：

```shell
^    # 锚定行的开始 如：'^grep'匹配所有以grep开头的行。    
$    # 锚定行的结束 如：'grep
```

## 常见用法

在文件中搜索一个单词，命令会返回一个包含 **“match_pattern”** 的文本行：

```shell
grep match_pattern file_name
grep "match_pattern" file_name
```

在多个文件中查找：

```shell
grep "match_pattern" file_1 file_2 file_3 ...
```

输出除之外的所有行 **-v** 选项：

```shell
grep -v "match_pattern" file_name
```

标记匹配颜色 **--color=auto** 选项：

```shell
grep "match_pattern" file_name --color=auto
```

使用正则表达式 **-E** 选项：

```shell
grep -E "[1-9]+"
# 或
egrep "[1-9]+"
```

使用正则表达式 **-P** 选项：

```shell
grep -P "(\d{3}\-){2}\d{4}" file_name
```

只输出文件中匹配到的部分 **-o** 选项：

```shell
echo this is a test line. | grep -o -E "[a-z]+\."
line.

echo this is a test line. | egrep -o "[a-z]+\."
line.
```

统计文件或者文本中包含匹配字符串的行数 **-c** 选项：

```shell
grep -c "text" file_name
```

输出包含匹配字符串的行数 **-n** 选项：

```shell
grep "text" -n file_name
# 或
cat file_name | grep "text" -n

#多个文件
grep "text" -n file_1 file_2
```

打印样式匹配所位于的字符或字节偏移：

```shell
echo gun is not unix | grep -b -o "not"
7:not
```

注：一行中字符串的字符偏移是从该行的第一个字符开始计算，起始值为0。选项 -b -o 一般总是配合使用。

搜索多个文件并查找匹配文本在哪些文件中：

```shell
grep -l "text" file1 file2 file3...
```

**grep递归搜索文件**

在多级目录中对文本进行递归搜索：

```shell
grep -r -n "text" .
# .表示当前目录。
```

忽略匹配样式中的字符大小写：

```shell
echo "hello world" | grep -i "HELLO"
# hello
```

选项 **-e** 指定多个匹配样式：

```shell
echo this is a text line | grep -e "is" -e "line" -o
is
line

#也可以使用 -f 选项来匹配多个样式，在样式文件中逐行写出需要匹配的字符。
cat patfile
aaa
bbb

echo aaa bbb ccc ddd eee | grep -f patfile -o
```

在grep搜索结果中包括或者排除指定文件：

```shell
# 只在目录中所有的.php和.html文件中递归搜索字符"main()"
grep "main()" . -r --include *.{php,html}

# 在搜索结果中排除所有README文件
grep "main()" . -r --exclude "README"

# 在搜索结果中排除filelist文件列表里的文件
grep "main()" . -r --exclude-from filelist
```

使用0值字节后缀的grep与xargs：

```shell
# 测试文件：
echo "aaa" > file1
echo "bbb" > file2
echo "aaa" > file3

grep "aaa" file* -lZ | xargs -0 rm

# 执行后会删除file1和file3，grep输出用-Z选项来指定以0值字节作为终结符文件名（\0），xargs -0 读取输入并用0值字节终结符分隔文件名，然后删除匹配文件，-Z通常和-l结合使用。
```

grep静默输出：

```shell
grep -q "test" filename
# 不会输出任何信息，如果命令运行成功返回0，失败则返回非0值。一般用于条件测试。
```

打印出匹配文本之前或者之后的行：

```shell
# 显示匹配某个结果之后的3行，使用 -A 选项：
seq 10 | grep "5" -A 3
5
6
7
8

# 显示匹配某个结果之前的3行，使用 -B 选项：
seq 10 | grep "5" -B 3
2
3
4
5

# 显示匹配某个结果的前三行和后三行，使用 -C 选项：
seq 10 | grep "5" -C 3
2
3
4
5
6
7
8

# 如果匹配结果有多个，会用“--”作为各匹配结果之间的分隔符：
echo -e "a\nb\nc\na\nb\nc" | grep a -A 1
a
b
--
a
b
```



---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note-linuxcmd/linux%E5%91%BD%E4%BB%A4-grep/  

