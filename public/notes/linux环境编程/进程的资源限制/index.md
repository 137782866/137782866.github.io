# 进程的资源限制


每个进程都有一组资源限制，其中一些可以用getrlimit和setrlimit函数获取和修改。

`getrlimit`和`setrlimit`函数是Unix系统中用于管理系统资源限制的重要工具。通过这两个函数，可以查询和设置当前进程对CPU时间、文件大小、打开文件数等资源的限制，从而更有效地控制进程的行为和避免资源耗尽。

函数定义：

```c
#include &lt;sys/resource.h&gt;  
int getrlimit(int resource, struct rlimit *rlp);
int setrlimit(int resource, const struct rlimit *rlp);

```

参数：

`resource`：指定要获取的资源类型。常见的资源类型包括（不限于）：

- `RLIMIT_CORE`：core文件的最大字节数。
- `RLIMIT_CPU`：CPU时间的最大量值（秒）。
- `RLIMIT_DATA`：进程的数据段最大字节长度。
- `RLIMIT_FSIZE`：可以创建的文件的最大字节长度。
- `RLIMIT_NOFILE`：每个进程能打开的最多文件数。
- `RLIMIT_STACK`：栈的最大字节长度。
- `RLIMIT_AS`：进程可用内存最大字节数。
- `RLIMIT_SWAP`：用户可以消耗的交换空间最大数。
- ...（其他类型）

`rlimit`结构体：

```c
struct rlimit {  
    rlim_t rlim_cur;  /* 当前（软）限制 */  
    rlim_t rlim_max;  /* 最大（硬）限制 */  
};
```

返回值：成功返回0，失败返回-1。

注意：

- 任何一个进程都可将一个软限制值更改为小于或等于其硬限制值。
- 任何一个进程都可隆低其硬限制值，但它必须大于或等于其软限制值。这种降低，对普通用户而言是不可逆的（除非进程具有超级用户权限）。
- 只有超级用户进程可以提高硬限制值。

实例代码：

```c
#include &lt;stdio.h&gt;  
#include &lt;sys/resource.h&gt;  
#include &lt;errno.h&gt;  
#include &lt;string.h&gt;  
  
int main() {  
    struct rlimit rl;  
  
    // 先获取当前限制  
    if (getrlimit(RLIMIT_NOFILE, &amp;rl) == -1) {  
        perror(&#34;getrlimit&#34;);  
        return 1;  
    }  
  
    // 假设我们只是想要增加软限制到硬限制的值（这通常不需要root权限）  
    rl.rlim_cur = rl.rlim_max; // 注意：这里我们假设硬限制足够大以容纳我们想要的软限制  
  
    // 设置新的限制  
    if (setrlimit(RLIMIT_NOFILE, &amp;rl) == -1) {  
        perror(&#34;setrlimit&#34;);  
        return 1;  
    }  
  
    // 再次获取限制以确认更改  
    if (getrlimit(RLIMIT_NOFILE, &amp;rl) == -1) {  
        perror(&#34;getrlimit&#34;);  
        return 1;  
    }  
  
    printf(&#34;New soft limit: %ld\n&#34;, rl.rlim_cur);  
    printf(&#34;Hard limit remains: %ld\n&#34;, rl.rlim_max);  
  
    return 0;  
}
```

运行结果：

```
$ ./a.out 
New soft limit: 1048576
Hard limit remains: 1048576
```


---

> 作者: WindSun  
> URL: http://localhost:1313/notes/linux%E7%8E%AF%E5%A2%83%E7%BC%96%E7%A8%8B/%E8%BF%9B%E7%A8%8B%E7%9A%84%E8%B5%84%E6%BA%90%E9%99%90%E5%88%B6/  

