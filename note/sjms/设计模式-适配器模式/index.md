# 设计模式-适配器模式


## 模式动机

在软件开发中采用类似于电源适配器的设计和编码技巧被称为适配器模式。

通常情况下，客户端可以通过目标类的接口访问它所提供的服务。有时，现有的类可以满足客户类的功能需要，但是它所提供的接口不一定是客户类所期望的，这可能是因为现有类中方法名与目标类中定义的方法名不一致等原因所导致的。在这种情况下，现有的接口需要转化为客户类期望的接口，这样保证了对现有类的重用。如果不进行这样的转化，客户类就不能利用现有类所提供的功能，适配器模式可以完成这样的转化。

在适配器模式中可以定义一个包装类，包装不兼容接口的对象，这个包装类指的就是适配器(Adapter)，它所包装的对象就是适配者(Adaptee)，即被适配的类。

适配器提供客户类需要的接口，适配器的实现就是把客户类的请求转化为对适配者的相应接口的调用。也就是说：当客户类调用适配器的方法时，在适配器类的内部将调用适配者类的方法，而这个过程对客户类是透明的，客户类并不直接访问适配者类。因此，适配器可以使由于接口不兼容而不能交互的类可以一起工作。这就是适配器模式的模式动机。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_042108.png)

## 模式定义

适配器模式(Adapter Pattern) ：将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作，其别名为包装器(Wrapper)。适配器模式既可以作为类结构型模式，也可以作为对象结构型模式。

## 模式结构

**类适配器**

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_042129.png)



**对象适配器**

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_042150.png)

适配器模式包含如下角色：

- **Target**：目标抽象类
- **Adapter**：适配器类
- **Adaptee**：适配者类
- **Client**：客户类

## 模式分析

典型的类适配器代码：

```java
public class Adapter extends Adaptee implements Target
{
    public void request()
    {
        specificRequest();
    }
}
```

典型的对象适配器代码：

```java
public class Adapter extends Target
{
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee)
    {
        this.adaptee=adaptee;
    }

    public void request()
    {
        adaptee.specificRequest();
    }
}
```

## 模式实例

### 实例1：仿生机器人

现需要设计一个可以模拟各种动物行为的机器人，在机器人中定义了一系列方法，如机器人叫喊方法cry()、机器人移动方法move()等。如果希望在不修改已有代码的基础上使得机器人能够像狗一样叫，像狗一样跑，使用适配器模式进行系统设计。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_042213.png)

实例代码：

```java
public interface Robot
{
    public void cry();
    public void move();
}

public class Bird
{
    public void tweedle()
    {
        System.out.println("鸟儿叽叽叫！");
    }

    public void fly()
    {
        System.out.println("鸟儿快快飞！");
    }
}

public class BirdAdapter extends Bird implements Robot
{
    public void cry()
    {
        System.out.print("机器人模仿：");
        super.tweedle();
    }

    public void move()
    {
        System.out.print("机器人模仿：");
        super.fly();
    }
}

public class Dog
{
    public void wang()
    {
        System.out.println("狗汪汪叫！");
    }

    public void run()
    {
        System.out.println("狗快快跑！");
    }
}

public class DogAdapter extends Dog implements Robot
{
    public void cry()
    {
        System.out.print("机器人模仿：");
        super.wang();
    }

    public void move()
    {
        System.out.print("机器人模仿：");
        super.run();
    }
}

//配置文件config.xml
<?xml version="1.0"?>
<config>
    <className>BirdAdapter</className>
</config>

//通过反射得到具体的适配器类
import javax.xml.parsers.*;
import org.w3c.dom.*;
import org.xml.sax.SAXException;
import java.io.*;
public class XMLUtil
{
//该方法用于从XML配置文件中提取具体类类名，并返回一个实例对象
    public static Object getBean()
    {
        try
        {
            //创建文档对象
            DocumentBuilderFactory dFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = dFactory.newDocumentBuilder();
            Document doc;
            doc = builder.parse(new File("config.xml"));

            //获取包含类名的文本节点
            NodeList nl = doc.getElementsByTagName("className");
            Node classNode=nl.item(0).getFirstChild();
            String cName=classNode.getNodeValue();

            //通过类名生成实例对象并将其返回
            Class c=Class.forName(cName);
              Object obj=c.newInstance();
            return obj;
           }
               catch(Exception e)
               {
                   e.printStackTrace();
                   return null;
               }
        }
}

//客户端
public class Client
{
    public static void main(String args[])
    {
        Robot robot=(Robot)XMLUtil.getBean();
        robot.cry();
        robot.move();
    }
}
```

### 实例2：加密适配器

某系统需要提供一个加密模块，将用户信息（如密码等机密信息）加密之后再存储在数据库中，系统已经定义好了数据库操作类。为了提高开发效率，现需要重用已有的加密算法，这些算法封装在一些由第三方提供的类中，有些甚至没有源代码。使用适配器模式设计该加密模块，实现在不修改现有类的基础上重用第三方加密方法。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_042231.png)

实例代码：

```java
//目标抽象类
public abstract class DataOperation
{
    private String password;

    public void setPassword(String password)
    {
        this.password=password;
    }

    public String getPassword()
    {
        return this.password;
    }

    public abstract String doEncrypt(int key,String ps);
}

//适配者类
public final class Caesar
{
    public String doEncrypt(int key,String ps)
    {
        String es="";
        for(int i=0;i<ps.length();i++)
        {
            char c=ps.charAt(i);
            if(c>='a'&&c<='z')
            {
                c+=key%26;
            if(c>'z') c-=26;
            if(c<'a') c+=26;
            }
            if(c>='A'&&c<='Z')
            {
                c+=key%26;
            if(c>'Z') c-=26;
            if(c<'A') c+=26;
            }
            es+=c;
        }
        return es;
    }
}

//适配器类
public class CipherAdapter extends DataOperation
{
    private Caesar cipher;

    public CipherAdapter()
    {
        cipher=new Caesar();
    }

    public String doEncrypt(int key,String ps)
    {
        return cipher.doEncrypt(key,ps);
    }
}

//新适配者类
public final class NewCipher
{
    public String doEncrypt(int key,String ps)
    {
        String es="";
        for(int i=0;i<ps.length();i++)
        {
            String c=String.valueOf(ps.charAt(i)%key);
            es+=c;
        }
        return es;
    }
}

//新适配器类
public class NewCipherAdapter extends DataOperation
{
    private NewCipher cipher;

    public NewCipherAdapter()
    {
        cipher=new NewCipher();
    }

    public String doEncrypt(int key,String ps)
    {
        return cipher.doEncrypt(key,ps);
    }
}

//配置文件 config.xml
<?xml version="1.0"?>
<config>
    <className>NewCipherAdapter</className>
</config>

//使用JAVA反射得到配置文件中的适配器类实例
import javax.xml.parsers.*;
import org.w3c.dom.*;
import org.xml.sax.SAXException;
import java.io.*;
public class XMLUtil
{
//该方法用于从XML配置文件中提取具体类类名，并返回一个实例对象
    public static Object getBean()
    {
        try
        {
            //创建文档对象
            DocumentBuilderFactory dFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = dFactory.newDocumentBuilder();
            Document doc;
            doc = builder.parse(new File("config.xml"));

            //获取包含类名的文本节点
            NodeList nl = doc.getElementsByTagName("className");
            Node classNode=nl.item(0).getFirstChild();
            String cName=classNode.getNodeValue();

            //通过类名生成实例对象并将其返回
            Class c=Class.forName(cName);
              Object obj=c.newInstance();
            return obj;
           }
               catch(Exception e)
               {
                   e.printStackTrace();
                   return null;
               }
        }
}

//客户端
public class Client
{
    public static void main(String args[])
    {
        DataOperation dao=(DataOperation)XMLUtil.getBean();
        dao.setPassword("sunnyLiu");
        String ps=dao.getPassword();
        String es=dao.doEncrypt(6,ps);
        System.out.println("明文为：" + ps);
        System.out.println("密文为：" + es);
    }
}
```

## 模式优缺点

**优点**如下：

- 将目标类和适配者类解耦，通过引入一个适配器类来重用现有的适配者类，而无须修改原有代码。
- 增加了类的透明性和复用性，将具体的实现封装在适配者类中，对于客户端类来说是透明的，而且提高了适配者的复用性。
- 灵活性和扩展性都非常好，通过使用配置文件，可以很方便地更换适配器，也可以在不修改原有代码的基础上增加新的适配器类，完全符合“开闭原则”。
- 由于适配器类是适配者类的子类，因此可以在适配器类中置换一些适配者的方法，使得适配器的灵活性更强。

**缺点**如下：

对于Java、C#等不支持多重继承的语言，一次最多只能适配一个适配者类，而且目标抽象类只能为抽象类，不能为具体类，其使用有一定的局限性，不能将一个适配者类和它的子类都适配到目标接口。

## 模式适应环境

在以下情况下可以使用适配器模式：

- 系统需要使用现有的类，而这些类的接口不符合系统的需要。
- 想要建立一个可以重复使用的类，用于与一些彼此之间没有太大关联的一些类，包括一些可能在将来引进的类一起工作。

## 模式应用

1. Sun公司在1996年公开了Java语言的数据库连接工具JDBC，JDBC使得Java语言程序能够与数据库连接，并使用SQL语言来查询和操作数据。JDBC给出一个客户端通用的抽象接口，每一个具体数据库引擎（如SQL Server、Oracle、MySQL等）的JDBC驱动软件都是一个介于JDBC接口和数据库引擎接口之间的适配器软件。抽象的JDBC接口和各个数据库引擎API之间都需要相应的适配器软件，这就是为各个不同数据库引擎准备的驱动程序。

2. 在Spring AOP框架中，对BeforeAdvice、AfterAdvice、ThrowsAdvice三种通知类型借助适配器模式来实现。

   ```java
   public interface AdvisorAdapter { 
   	//将一个Advisor适配成MethodInterceptor 
   	MethodInterceptor getInterceptor(Advisor advisor); 
   	//判断此适配器是否支持特定的Advice 
   	boolean supportsAdvice(Advice advice); 
   }
   ```

3. 在JDK类库中也定义了一系列适配器类，如在 com.sun.imageio.plugins.common 包中定义的 InputStreamAdapter 类，用于包装 ImageInputStream 接口及其子类对象。

   ```java
   public class InputStreamAdapter extends InputStream {
       ImageInputStream stream;
       public InputStreamAdapter(ImageInputStream stream) {
           super();
           this.stream = stream;
       }
       public int read() throws IOException {
           return stream.read();
       }
       public int read(byte b[], int off, int len) throws IOException {
           return stream.read(b, off, len);
       }
   }
   ```

## 模式扩展

**默认适配器模式(Default Adapter Pattern)或缺省适配器模式**

当不需要全部实现接口提供的方法时，可先设计一个抽象类实现接口，并为该接口中每个方法提供一个默认实现（空方法），那么该抽象类的子类可有选择地覆盖父类的某些方法来实现需求，它适用于一个接口不想使用其所有的方法的情况。因此也称为单接口适配器模式。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_042251.png)

**双向适配器**

在对象适配器的使用过程中，如果在适配器中同时包含对目标类和适配者类的引用，适配者可以通过它调用目标类中的方法，目标类也可以通过它调用适配者类中的方法，那么该适配器就是一个双向适配器。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_042313.png)



---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E9%80%82%E9%85%8D%E5%99%A8%E6%A8%A1%E5%BC%8F/  

