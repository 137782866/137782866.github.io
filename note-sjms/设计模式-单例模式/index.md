# 设计模式-单例模式


## 模式动机

对于系统中的某些类来说，只有一个实例很重要，例如，一个系统中可以存在多个打印任务，但是只能有一个正在工作的任务；一个系统只能有一个窗口管理器或文件系统；一个系统只能有一个计时工具或ID（序号）生成器。

如何保证一个类只有一个实例并且这个实例易于被访问呢？定义一个全局变量可以确保对象随时都可以被访问，但不能防止我们实例化多个对象。

一个更好的解决办法是让类自身负责保存它的唯一实例。这个类可以保证没有其他实例被创建，并且它可以提供一个访问该实例的方法。这就是单例模式的模式动机。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231214_012714.png)

## 模式定义

单例模式(Singleton Pattern)：单例模式确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例，这个类称为单例类，它提供全局访问的方法。

单例模式的要点有三个：一是某个类只能有一个实例；二是它必须自行创建这个实例；三是它必须自行向整个系统提供这个实例。单例模式是一种对象创建型模式。单例模式又名单件模式或单态模式。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231214_012729.png)

## 模式结构

单例模式包含如下角色：

- **Singleton**：单例

## 模式分析

单例模式的目的是保证一个类仅有一个实例，并提供一个访问它的全局访问点。单例模式包含的角色只有一个，就是单例类——Singleton。单例类拥有一个私有构造函数，确保用户无法通过new关键字直接实例化它。除此之外，该模式中包含一个静态私有成员变量与静态公有的工厂方法，该工厂方法负责检验实例的存在性并实例化自己，然后存储在静态成员变量中，以确保只有一个实例被创建。

单例模式的实现代码如下所示：

```java
public class Singleton
{
    private static Singleton instance=null;  //静态私有成员变量
    //私有构造函数
    private Singleton()
    {
    }

    //静态公有工厂方法，返回唯一实例
    public static Singleton getInstance()
    {
        if(instance==null)
            instance=new Singleton();
        return instance;
    }
}
```

在单例模式的实现过程中，需要注意如下三点：

- 单例类的构造函数为私有；
- 提供一个自身的静态私有成员变量；
- 提供一个公有的静态工厂方法。

## 单例模式实例

### 实例1：身份证号码

在现实生活中，居民身份证号码具有唯一性，同一个人不允许有多个身份证号码，第一次申请身份证时将给居民分配一个身份证号码，如果之后因为遗失等原因补办时，还是使用原来的身份证号码，不会产生新的号码。现使用单例模式模拟该场景。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231214_012746.png)

```java
public class IdentityCardNo
{
    private static IdentityCardNo instance=null;
    private String no;

    private IdentityCardNo()
    {
    }

    public static IdentityCardNo getInstance()
    {
        if(instance==null)
        {
            System.out.println("第一次办理身份证，分配新号码！");
            instance=new IdentityCardNo();
            instance.setIdentityCardNo("No400011112222");
        }
        else
        {
            System.out.println("重复办理身份证，获取旧号码！");
        }
        return instance;
    }

    private void setIdentityCardNo(String no)
    {
        this.no=no;
    }

    public String getIdentityCardNo()
    {
        return this.no;
    }

}

public class Client
{
    public static void main(String a[])
    {
       IdentityCardNo no1,no2;
       no1=IdentityCardNo.getInstance();
       no2=IdentityCardNo.getInstance();
       System.out.println("身份证号码是否一致：" + (no1==no2));

       String str1,str2;
       str1=no1.getIdentityCardNo();
       str2=no1.getIdentityCardNo();
       System.out.println("第一次号码：" + str1);
       System.out.println("第二次号码：" + str2);
       System.out.println("内容是否相等：" + str1.equalsIgnoreCase(str2));
       System.out.println("是否是相同对象：" + (str1==str2));
    }
}
```

### 实例2：打印池

在操作系统中，打印池(Print Spooler)是一个用于管理打印任务的应用程序，通过打印池用户可以删除、中止或者改变打印任务的优先级，在一个系统中只允许运行一个打印池对象，如果重复创建打印池则抛出异常。现使用单例模式来模拟实现打印池的设计。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231214_012803.png)

实例代码：

```java
public class PrintSpoolerSingleton
{
    private static PrintSpoolerSingleton instance=null;

    private PrintSpoolerSingleton()
    {
    }

    public static PrintSpoolerSingleton getInstance() throws PrintSpoolerException
    {
        if(instance==null)
        {
            System.out.println("创建打印池！");
            instance=new PrintSpoolerSingleton();
        }
        else
        {
            throw new PrintSpoolerException("打印池正在工作中！");
        }
        return instance;
    }

    public void manageJobs()
    {
        System.out.println("管理打印任务！");
    }
}

public class PrintSpoolerException extends Exception
{
    public PrintSpoolerException(String message)
    {
        super(message);
    }
}

public class Client
{
    public static void main(String a[])
    {
       PrintSpoolerSingleton ps1,ps2;
       try
       {
            ps1=PrintSpoolerSingleton.getInstance();
            ps1.manageJobs();
       }
       catch(PrintSpoolerException e)
       {
               System.out.println(e.getMessage());
       }
            System.out.println("--------------------------");
       try
       {
           ps2=PrintSpoolerSingleton.getInstance();
           ps2.manageJobs();
       }
       catch(PrintSpoolerException e)
       {
           System.out.println(e.getMessage());
       }
    }
}
```

## 模式优缺点

**优点**如下：

- 提供了对唯一实例的受控访问。因为单例类封装了它的唯一实例，所以它可以严格控制客户怎样以及何时访问它，并为设计及开发团队提供了共享的概念。
- 由于在系统内存中只存在一个对象，因此可以节约系统资源，对于一些需要频繁创建和销毁的对象，单例模式无疑可以提高系统的性能。
- 允许可变数目的实例。我们可以基于单例模式进行扩展，使用与单例控制相似的方法来获得指定个数的对象实例。

**缺点**如下：

- 由于单例模式中没有抽象层，因此单例类的扩展有很大的困难。
- 单例类的职责过重，在一定程度上违背了“单一职责原则”。因为单例类既充当了工厂角色，提供了工厂方法，同时又充当了产品角色，包含一些业务方法，将产品的创建和产品的本身的功能融合到一起。
- 滥用单例将带来一些负面问题，如为了节省资源将数据库连接池对象设计为单例类，可能会导致共享连接池对象的程序过多而出现连接池溢出；现在很多面向对象语言(如Java、C#)的运行环境都提供了自动垃圾回收的技术，因此，如果实例化的对象长时间不被利用，系统会认为它是垃圾，会自动销毁并回收资源，下次利用时又将重新实例化，这将导致对象状态的丢失。

## 模式适应环境

在以下情况下可以使用单例模式：

- 系统只需要一个实例对象，如系统要求提供一个唯一的序列号生成器，或者需要考虑资源消耗太大而只允许创建一个对象。
- 客户调用类的单个实例只允许使用一个公共访问点，除了该公共访问点，不能通过其他途径访问该实例。
- 在一个系统中要求一个类只有一个实例时才应当使用单例模式。反过来，如果一个类可以有几个实例共存，就需要对单例模式进行改进，使之成为多例模式。

## 模式应用

1. java.lang.Runtime类

   ```java
   public class Runtime {
       private static Runtime currentRuntime = new Runtime();
       public static Runtime getRuntime() {
       return currentRuntime;
       }
       private Runtime() {}
       //...
   }
   ```

2.  一个具有自动编号主键的表可以有多个用户同时使用，但数据库中只能有一个地方分配下一个主键编号，否则会出现主键重复，因此该主键编号生成器必须具备唯一性，可以通过单例模式来实现。 

3. 默认情况下，Spring会通过单例模式创建bean实例：

   ```java
   <bean id="date" class="java.util.Date" scope="singleton"/>
   ```

## 模式扩展

### **饿汉式单例类**

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231214_012819.png)

### **懒汉式单例类**

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231214_012838.png)

### **饿汉式与懒汉式比较**

- 饿汉式单例类在自己被加载时就将自己实例化。单从资源利用效率角度来讲，这个比懒汉式单例类稍差些。从速度和反应时间角度来讲，则比懒汉式单例类稍好些。

- 懒汉式单例类在实例化时，必须处理好在多个线程同时首次引用此类时的访问限制问题，特别是当单例类作为资源控制器，在实例化时必然涉及资源初始化，而资源初始化很有可能耗费大量时间，这意味着出现多线程同时首次引用此类的机率变得较大，需要通过同步化机制进行控制。

  ```java
  public static Object getInstance(){
          if(obj == null){
              synchronized(Object.class){
                  if(obj == null){
                      obj = new Object();
                  }
              }
          }
          return obj;
      }
  ```



---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note-sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F/  

