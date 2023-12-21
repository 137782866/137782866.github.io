# 设计模式-外观模式


## 模式动机

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_045245.png)

引入外观角色之后，用户只需要直接与外观角色交互，用户与子系统之间的复杂关系由外观角色来实现，从而降低了系统的耦合度。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_045302.png)

## 模式定义

外观模式(Facade Pattern)：外部与一个子系统的通信必须通过一个统一的外观对象进行，为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。外观模式又称为门面模式，它是一种对象结构型模式。

## 模式结构

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_045318.png)

外观模式包含如下角色：

- Facade: 外观角色
- SubSystem:子系统角色

## 模式分析

根据“单一职责原则”，在软件中将一个系统划分为若干个子系统有利于降低整个系统的复杂性，一个常见的设计目标是使子系统间的通信和相互依赖关系达到最小，而达到该目标的途径之一就是引入一个外观对象，它为子系统的访问提供了一个简单而单一的入口。

外观模式也是“迪米特法则”的体现，通过引入一个新的外观类可以降低原有系统的复杂度，同时降低客户类与子系统类的耦合度。

外观模式要求一个子系统的外部与其内部的通信通过一个统一的外观对象进行，外观类将客户端与子系统的内部复杂性分隔开，使得客户端只需要与外观对象打交道，而不需要与子系统内部的很多对象打交道。

外观模式的目的在于降低系统的复杂程度。

外观模式从很大程度上提高了客户端使用的便捷性，使得客户端无须关心子系统的工作细节，通过外观角色即可调用相关功能。

典型的外观角色代码：

```java
public class Facade
{
    private SubSystemA obj1 = new SubSystemA();
    private SubSystemB obj2 = new SubSystemB();
    private SubSystemC obj3 = new SubSystemC();
    public void method()
    {
        obj1.method();
        obj2.method();
        obj3.method();
    }
}
```

## 模式实例

### 实例1：电源总开关

现在考察一个电源总开关的例子，以便进一步说明外观模式。为了使用方便，一个电源总开关可以控制四盏灯、一个风扇、一台空调和一台电视机的启动和关闭。通过该电源总开关可以同时控制上述所有电器设备，使用外观模式设计该系统。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_045338.png)

代码实现：

```java
//外观角色类
public class GeneralSwitchFacade {
    private Light lights[]=new Light[4];
    private Fan fan;
    private AirConditioner ac;
    private Television tv;

    public GeneralSwitchFacade()
    {
        lights[0]=new Light("左前");
        lights[1]=new Light("右前");
        lights[2]=new Light("左后");
        lights[3]=new Light("右后");
        fan=new Fan();
        ac=new AirConditioner();
        tv=new Television();
    }

    public void on()
    {
        lights[0].on();
        lights[1].on();
        lights[2].on();
        lights[3].on();
        fan.on();
        ac.on();
        tv.on();
    }

    public void off()
    {
        lights[0].off();
        lights[1].off();
        lights[2].off();
        lights[3].off();
        fan.off();
        ac.off();
        tv.off();
    }
}

//子系统角色类
public class Light
{
    private String position;

    public Light(String position)
    {
        this.position=position;
    }

    public void on()
    {
        System.out.println(this.position + "灯打开！");
    }

    public void off()
    {
        System.out.println(this.position + "灯关闭！");
    }
}

//子系统角色
public class Fan
{
    public void on()
    {
        System.out.println("风扇打开！");
    }

    public void off()
    {
        System.out.println("风扇关闭！");
    }
}

//子系统角色
public class AirConditioner
{
    public void on()
    {
        System.out.println("空调打开！");
    }

    public void off()
    {
        System.out.println("空调关闭！");
    }
}

//子系统角色
public class Television
{
    public void on()
    {
        System.out.println("电视机打开！");
    }

    public void off()
    {
        System.out.println("电视机关闭！");
    }
}

//客户端
public class Client
{
    public static void main(String args[])
    {
        GeneralSwitchFacade gsf=new GeneralSwitchFacade();
        gsf.on();
        System.out.println("-----------------------");
        gsf.off();
    }
}
```

### 实例2：文件加密

某系统需要提供一个文件加密模块，加密流程包括三个操作，分别是读取源文件、加密、保存加密之后的文件。读取文件和保存文件使用流来实现，这三个操作相对独立，其业务代码封装在三个不同的类中。现在需要提供一个统一的加密外观类，用户可以直接使用该加密外观类完成文件的读取、加密和保存三个操作，而不需要与每一个类进行交互，使用外观模式设计该加密模块。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_045357.png)

## 模式优缺点

**优点**包括：

- 对客户屏蔽子系统组件，减少了客户处理的对象数目并使得子系统使用起来更加容易。通过引入外观模式，客户代码将变得很简单，与之关联的对象也很少。
- 实现了子系统与客户之间的松耦合关系，这使得子系统的组件变化不会影响到调用它的客户类，只需要调整外观类即可。
- 降低了大型软件系统中的编译依赖性，并简化了系统在不同平台之间的移植过程，因为编译一个子系统一般不需要编译所有其他的子系统。一个子系统的修改对其他子系统没有任何影响，而且子系统内部变化也不会影响到外观对象。
- 只是提供了一个访问子系统的统一入口，并不影响用户直接使用子系统类。

**缺点**包括：

- 不能很好地限制客户使用子系统类，如果对客户访问子系统类做太多的限制则减少了可变性和灵活性。
- 在不引入抽象外观类的情况下，增加新的子系统可能需要修改外观类或客户端的源代码，违背了“开闭原则”。

## 模式适应环境

在以下情况下可以使用外观模式：

- 当要为一个复杂子系统提供一个简单接口时可以使用外观模式。该接口可以满足大多数用户的需求，而且用户也可以越过外观类直接访问子系统。
- 客户程序与多个子系统之间存在很大的依赖性。引入外观类将子系统与客户以及其他子系统解耦，可以提高子系统的独立性和可移植性。
- 在层次化结构中，可以使用外观模式定义系统中每一层的入口，层与层之间不直接产生联系，而通过外观类建立联系，降低层之间的耦合度。

## 模式应用

1. 外观模式应用于JDBC数据库操作

   ```java
   public class JDBCFacade {
       private Connection conn=null;
       private Statement statement=null;
       public void open(String driver,String jdbcUrl,String userName,String userPwd) {
          //...
       }
       public int executeUpdate(String sql) {
           //...
       }
       public ResultSet executeQuery(String sql) {
           //...
       }
       public void close() {
           //...
       }
   }
   ```

2. Session外观模式是外观模式在Java EE框架中的应用

   ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_045414.png)

## 模式扩展

**一个系统有多个外观类**

在外观模式中，通常只需要一个外观类，并且此外观类只有一个实例，换言之它是一个单例类。在很多情况下为了节约系统资源，一般将外观类设计为单例类。当然这并不意味着在整个系统里只能有一个外观类，在一个系统中可以设计多个外观类，每个外观类都负责和一些特定的子系统交互，向用户提供相应的业务功能。

**不要试图通过外观类为子系统增加新行为**

不要通过继承一个外观类在子系统中加入新的行为，这种做法是错误的。外观模式的用意是为子系统提供一个集中化和简化的沟通渠道，而不是向子系统加入新的行为，新的行为的增加应该通过修改原有子系统类或增加新的子系统类来实现，不能通过外观类来实现。

**外观模式与迪米特法则**

外观模式创造出一个外观对象，将客户端所涉及的属于一个子系统的协作伙伴的数量减到最少，使得客户端与子系统内部的对象的相互作用被外观对象所取代。外观类充当了客户类与子系统类之间的“第三者”，降低了客户类与子系统类之间的耦合度，外观模式就是实现代码重构以便达到“迪米特法则”要求的一个强有力的武器。

**抽象外观类的引入**

外观模式最大的缺点在于违背了“开闭原则”，当增加新的子系统或者移除子系统时需要修改外观类，可以通过引入抽象外观类在一定程度上解决该问题，客户端针对抽象外观类进行编程。对于新的业务需求，不修改原有外观类，而对应增加一个新的具体外观类，由新的具体外观类来关联新的子系统对象，同时通过修改配置文件来达到不修改源代码并更换外观类的目的。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_045431.png)


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E5%A4%96%E8%A7%82%E6%A8%A1%E5%BC%8F/  

