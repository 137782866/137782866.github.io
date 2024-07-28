# 代码高亮测试


```js
function helloWorld () {
  alert(&#34;Hello, World!&#34;)
}
```

&lt;!--more--&gt;

```java
public class HelloWorld {
  public static void main(String[] args) {
    System.out.println(&#34;Hello, World!&#34;);
  }
}
```

```kotlin
package hello

fun main(args: Array&lt;String&gt;) {
  println(&#34;Hello World!&#34;)
}
```

```c
#include &lt;stdio.h&gt;

/* Hello */
int main(void){
  printf(&#34;Hello, World!&#34;);
  return 0;
}
```

```cpp
// &#39;Hello World!&#39; program 
 
#include &lt;iostream&gt;
 
int main(){
  std::cout &lt;&lt; &#34;Hello World!&#34; &lt;&lt; std::endl;
  return 0;
}
```

```cs
using System;
class HelloWorld{
  public static void Main(){ 
    System.Console.WriteLine(&#34;Hello, World!&#34;);
  }
}
```

```html
&lt;html&gt;
&lt;body&gt;
  Hello, World!
&lt;/body&gt;
&lt;/html&gt;
```

```go
package main
import fmt &#34;fmt&#34;

func main() 
{
   fmt.Printf(&#34;Hello, World!\n&#34;);
}
```

```scala
object HelloWorld with Application {
  Console.println(&#34;Hello, World!&#34;);
}
```

```php
&lt;?php
  echo &#39;Hello, World!&#39;;
?&gt;
```

```python
print(&#34;Hello, World!&#34;) 
```

```clojure
(defn hello-world
  &#34;A function print &#39;Hello world&#39;.&#34;
  []
  (prn &#34;Hello world&#34;))
```

```cpp
// Copyright 2010, Shuo Chen.  All rights reserved.
// http://code.google.com/p/muduo/
//
// Use of this source code is governed by a BSD-style license
// that can be found in the License file.

// Author: Shuo Chen (chenshuo at chenshuo dot com)

#include &#34;muduo/net/Acceptor.h&#34;

#include &#34;muduo/base/Logging.h&#34;
#include &#34;muduo/net/EventLoop.h&#34;
#include &#34;muduo/net/InetAddress.h&#34;
#include &#34;muduo/net/SocketsOps.h&#34;

#include &lt;errno.h&gt;
#include &lt;fcntl.h&gt;
//#include &lt;sys/types.h&gt;
//#include &lt;sys/stat.h&gt;
#include &lt;unistd.h&gt;

using namespace muduo;
using namespace muduo::net;

Acceptor::Acceptor(EventLoop *loop, const InetAddress &amp;listenAddr, bool reuseport)
    : loop_(loop),
      acceptSocket_(sockets::createNonblockingOrDie(listenAddr.family())), //这里创建非阻塞TCP socket套接字描述符，然后构造这个Socket对象 socket()
      acceptChannel_(loop, acceptSocket_.fd()),                            //此Acceptor的Channel中使用主函数中定义的loop，上面创建的套接字描述符也传给该Channel
      listening_(false),                                                   //服务器没有调用start()之前，不启动监听
      idleFd_(::open(&#34;/dev/null&#34;, O_RDONLY | O_CLOEXEC))                   //处理描述符用完的情况，需要断开连接
{
    assert(idleFd_ &gt;= 0);
    acceptSocket_.setReuseAddr(true);
    acceptSocket_.setReusePort(reuseport);
    acceptSocket_.bindAddress(listenAddr); //套接字和地址绑定sockets::bindOrDie
    acceptChannel_.setReadCallback(
        std::bind(&amp;Acceptor::handleRead, this));
}

Acceptor::~Acceptor()
{
    acceptChannel_.disableAll();
    acceptChannel_.remove();
    ::close(idleFd_);
}

void Acceptor::listen() //在TcpServer的start()函数开始运行
{
    loop_-&gt;assertInLoopThread();
    listening_ = true;
    acceptSocket_.listen();
    acceptChannel_.enableReading();
}

void Acceptor::handleRead()
{
    loop_-&gt;assertInLoopThread();
    InetAddress peerAddr;
    //FIXME loop until no more
    int connfd = acceptSocket_.accept(&amp;peerAddr);
    if (connfd &gt;= 0)
    {
        // string hostport = peerAddr.toIpPort();
        // LOG_TRACE &lt;&lt; &#34;Accepts of &#34; &lt;&lt; hostport;
        if (newConnectionCallback_) //TcpServer构造时绑定的函数是：TcpServer::newConnection
        {
            newConnectionCallback_(connfd, peerAddr);
        }
        else
        {
            sockets::close(connfd);
        }
    }
    else
    {
        LOG_SYSERR &lt;&lt; &#34;in Acceptor::handleRead&#34;;
        // Read the section named &#34;The special problem of
        // accept()ing when you can&#39;t&#34; in libev&#39;s doc.
        // By Marc Lehmann, author of libev.
        if (errno == EMFILE)
        {
            ::close(idleFd_);
            idleFd_ = ::accept(acceptSocket_.fd(), NULL, NULL);
            ::close(idleFd_);
            idleFd_ = ::open(&#34;/dev/null&#34;, O_RDONLY | O_CLOEXEC);
        }
    }
}

```


---

> 作者: WindSun  
> URL: http://localhost:1313/posts/%E4%BB%A3%E7%A0%81%E9%AB%98%E4%BA%AE%E6%B5%8B%E8%AF%95/  

