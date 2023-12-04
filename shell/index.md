# Shell基础命令


## shell 脚本第一行`#!`

`#!`始终出现在shell脚本的第一行的前两个字符，用于指示这是一个解释程序。语法格式如下：

```shell
#! BINEXECPATH [optional]
```

BINEXECPATH 是解释脚本的二进制可执行程序的路径，bash程序运行后，会指定这个解释程序代替其运行。并将脚本的路径作为第一个参数传递给解释程序。

例如：一个脚本程序路径是 path/to/script，而且它的内容以 #!/bin/bash 开头，那么程序加载器被指示用解释程序 /bin/bash 代替其运行，并将路径 path/to/script 作为第一个参数传递给解释程序 /bin/bash。

如果 shell 脚本中没有指定 #! 行，则默认以 /bin/sh 作为解释器。

## shell 注释

shell 脚本中的注释以 `#` 开头，这一行在 # 之后的所有内容都将被解释程序忽略掉，注释用于解释脚本相关语句和含义，让别人更容易都懂和理解。例如：

```shell
#!/bin/bash
# 2020-07-09
```

**多行注释**

开发过程中如果遇到大段的代码需要临时注释起来，过一会又要取消，每一行加一个 # 太费劲了，可以把一段要注释的代码用一对花括号括起来，定义成一个函数，没有地方调用这个函数，这块代码就不会被执行，达到了和注释一样的效果。

还可以用以下格式：

```shell
:<<EOF
注释内容...
注释内容...
注释内容...
EOF
```

EOF 也可以使用其他符号：

```shell
:<<!
注释内容...
注释内容...
注释内容...
!
```

## shell 设置权限和执行

编写一个脚本只是一个普通的文本，需要加上可执行权限次才可以执行，否则运行后只会出现 "Permission denied" 的错误信息。

给当前用户加上可执行权限：

```shell
$ chmod u+x ./test.sh
```

给所有用户执行此脚本权限：

```shell
$ chmod +x ./test.sh
```

执行 shell 脚本可以使用绝对路径，亦可以使用相对路径。

## shell 的 echo 命令

shell 的 echo 指令用于字符串的输出。命令格式：

```bash
echo string
```

可以使用echo实现更复杂的输出格式控制。

- **显示普通字符串**

```bash
echo "It is a test"
```

这里的双引号完全可以省略，以下命令与上面实例效果一致：

```bash
echo It is a test
```

- **显示转义字符**

```shell
echo "\"It is a test\""
```

结果是：

```bash
"It is a test"
```

同样，双引号也可以省略

- **显示变量**

read 命令从标准输入中读取一行，并把输入行的每个字段的值指定给 shell 变量

```bash
#!/bin/sh
read name 
echo "$name It is a test"
```

以上代码保存为 test.sh，name 接收标准输入的变量，结果将是:

```bash
[root@www ~]# sh test.sh
OK                     #标准输入
OK It is a test        #输出
```

- **显示换行**

```shell
echo -e "OK! \n" # -e 开启转义
echo "It is a test"
```

输出结果：

```bash
OK!

It is a test
```

- **显示不换行**

```bash
#!/bin/sh
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"
```

运行结果：

```
OK! It is a test
```

- **显示结果定向到文件**

```bash
echo "It is a test" > myfile
```

- **原样输出字符串，不进行转义或取变量(用单引号)**

```shell
echo '$name\"'
```

输出结果：

```bash
$name\"
```

- **显示命令执行结果**

```shell
echo `date`
```

**注意：** 这里使用的是反引号 **`**, 而不是单引号 **'**。

结果将显示当前日期：

```
Thu Jul 24 10:08:46 CST 2014
```

## shell printf命令

printf 命令模仿 C 程序库（library）里的 printf() 程序。

printf 由 POSIX 标准所定义，因此使用 printf 的脚本比使用 echo 移植性好。

printf 使用引用文本或空格分隔的参数，外面可以在 printf 中使用格式化字符串，还可以制定字符串的宽度、左右对齐方式等。默认 printf 不会像 echo 自动添加换行符，我们可以手动添加 \n。

printf 命令的语法：

```shell
printf  format-string  [arguments...]
```

参数说明：

- format-string：为格式控制字符串
- arguments：为参数列表。

实例：

```shell
$ echo "Hello, Shell"
Hello, Shell
$ printf "Hello, Shell\n"
Hello, Shell
```

接下来，用一个脚本来体现 printf 的强大功能：

```shell
#!/bin/bash
 
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg  
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876
```

执行脚本，输出结果如下所示：

```shell
姓名     性别   体重kg
郭靖     男      66.12
杨过     男      48.65
郭芙     女      47.99
```

`%s %c %d %f` 都是格式替代符，`％s` 输出一个字符串，`％d` 整型输出，`％c` 输出一个字符，`％f` 输出实数，以小数形式输出。

`%-10s` 指一个宽度为 10 个字符（`-` 表示左对齐，没有则表示右对齐），任何字符都会被显示在 10 个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。

`%-4.2f` 指格式化为小数，其中 `.2` 指保留2位小数。

实例：

```shell
#!/bin/bash

# format-string为双引号
printf "%d %s\n" 1 "abc"

# 单引号与双引号效果一样
printf '%d %s\n' 1 "abc"

# 没有引号也可以输出
printf %s abcdef

# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf %s abc def

printf "%s\n" abc def

printf "%s %s %s\n" a b c d e f g h i j

# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n"
```

运行结果：

```shell
1 abc
1 abc
abcdefabcdefabc
def
a b c
d e f
g h i
j  
 and 0
```

- **printf 的转义序列**

| 序列  | 说明                                                         |
| :---- | :----------------------------------------------------------- |
| \a    | 警告字符，通常为ASCII的BEL字符                               |
| \b    | 后退                                                         |
| \c    | 抑制（不显示）输出结果中任何结尾的换行字符（只在%b格式指示符控制下的参数字符串中有效），而且任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略 |
| \f    | 换页（formfeed）                                             |
| \n    | 换行                                                         |
| \r    | 回车（Carriage return）                                      |
| \t    | 水平制表符                                                   |
| \v    | 垂直制表符                                                   |
| \\    | 一个字面上的反斜杠字符                                       |
| \ddd  | 表示1到3位数八进制值的字符。仅在格式字符串中有效             |
| \0ddd | 表示1到3位的八进制值字符                                     |

实例：

```shell
$ printf "a string, no processing:<%s>\n" "A\nB"
a string, no processing:<A\nB>

$ printf "a string, no processing:<%b>\n" "A\nB"
a string, no processing:<A
B>

$ printf "www.runoob.com \a"
www.runoob.com $                  #不换行
```

## shell test命令

Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

### 数值测试

| 参数 | 说明           |
| :--- | :------------- |
| -eq  | 等于则为真     |
| -ne  | 不等于则为真   |
| -gt  | 大于则为真     |
| -ge  | 大于等于则为真 |
| -lt  | 小于则为真     |
| -le  | 小于等于则为真 |

实例：

```shell
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
```

输出结果：

```shell
两个数相等！
```

代码中的 `[]` 执行基本的算数运算，如：

```shell
#!/bin/bash

a=5
b=6

result=$[a+b] # 注意等号两边不能有空格
echo "result 为： $result"
```

结果为：

```shell
result 为： 11
```



































---

> 作者: WindSun  
> URL: https://example.org/shell/  

