# 设计模式-享元模式


## 模式动机

面向对象技术可以很好地解决一些灵活性或可扩展性问题，但在很多情况下需要在系统中增加类和对象的个数。当对象数量太多时，将导致运行代价过高，带来性能下降等问题。

享元模式正是为解决这一类问题而诞生的。享元模式通过共享技术实现相同或相似对象的重用。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_055335.png)



在享元模式中可以共享的相同内容称为内部状态(Intrinsic State)，而那些需要外部环境来设置的不能共享的内容称为外部状态(Extrinsic State)，由于区分了内部状态和外部状态，因此可以通过设置不同的外部状态使得相同的对象可以具有一些不同的特征，而相同的内部状态是可以共享的。

在享元模式中通常会出现工厂模式，需要创建一个享元工厂来负责维护一个享元池(Flyweight Pool)用于存储具有相同内部状态的享元对象。

## 模式定义

享元模式(Flyweight Pattern)：运用共享技术有效地支持大量细粒度对象的复用。系统只使用少量的对象，而这些对象都很相似，状态变化很小，可以实现对象的多次复用。由于享元模式要求能够共享的对象必须是细粒度对象，因此它又称为轻量级模式，它是一种对象结构型模式。

## 模式结构

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_055353.png)

享元模式包含如下角色：

- **Flyweight**: 抽象享元类
- **ConcreteFlyweight**: 具体享元类
- **UnsharedConcreteFlyweight**: 非共享具体享元类
- **FlyweightFactory**: 享元工厂类

## 模式分析

享元模式是一个考虑系统性能的设计模式，通过使用享元模式可以节约内存空间，提高系统的性能。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_055411.png)

享元模式的核心在于享元工厂类，享元工厂类的作用在于提供一个用于存储享元对象的享元池，用户需要对象时，首先从享元池中获取，如果享元池中不存在，则创建一个新的享元对象返回给用户，并在享元池中保存该新增对象。

典型的享元工厂类代码：

```java
public class FlyweightFactory
{
	private HashMap flyweights = new HashMap();
	
	public Flyweight getFlyweight(String key)
	{
		if(flyweights.containsKey(key))
		{
			return (Flyweight)flyweights.get(key);
		}
		else
		{
			Flyweight fw = new ConcreteFlyweight();
			flyweights.put(key,fw);
			return fw;
		}
	}
} 
```

享元模式以共享的方式高效地支持大量的细粒度对象，享元对象能做到共享的关键是区分内部状态(Internal State)和外部状态(External State)。

1. 内部状态是存储在享元对象内部并且不会随环境改变而改变的状态，因此内部状态可以共享。
2. 外部状态是随环境改变而改变的、不可以共享的状态。享元对象的外部状态必须由客户端保存，并在享元对象被创建之后，在需要使用的时候再传入到享元对象内部。一个外部状态与另一个外部状态之间是相互独立的。

典型的享元类代码：

```java
public class Flyweight
{
    //内部状态作为成员属性
	private String intrinsicState;
	
	public Flyweight(String intrinsicState)
	{
		this.intrinsicState = intrinsicState;
	}
	
	public void operation(String extrinsicState)
	{
		//...
	}
}
```

## 模式实例

### 实例1：共享网络设备（无外部状态）

很多网络设备都是支持共享的，如交换机、集线器等，多台终端计算机可以连接同一台网络设备，并通过该网络设备进行数据转发，如图所示，现用享元模式模拟共享网络设备的设计原理。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_055428.png)

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_055526.png)

实例代码：

```java
import java.util.*;

public interface NetworkDevice
{
	public String getType();
	public void use();
}

public class Hub implements NetworkDevice
{
	private String type;
	
	public Hub(String type)
	{
		this.type=type;
	}
	
	public String getType()
	{
		return this.type;
	} 
	
	public void use()
	{
		System.out.println("Linked by Hub, type is " + this.type);
	}	
}

public class Switch implements NetworkDevice
{
	private String type;
	
	public Switch(String type)
	{
		this.type=type;
	}
	
	public String getType()
	{
		return this.type;
	} 
	
	public void use()
	{
		System.out.println("Linked by switch, type is " + this.type);
	}
}

public class DeviceFactory
{
	private ArrayList devices = new ArrayList();
	private int totalTerminal=0;
	
	public DeviceFactory()
	{
		NetworkDevice nd1=new Switch("Cisco-WS-C2950-24");
		devices.add(nd1);
		NetworkDevice nd2=new Hub("TP-LINK-HF8M");
		devices.add(nd2);
	}
	
	public NetworkDevice getNetworkDevice(String type)
	{
		if(type.equalsIgnoreCase("cisco"))
		{
			totalTerminal++;
			return (NetworkDevice)devices.get(0);
		}
		else if(type.equalsIgnoreCase("tp"))
		{
			totalTerminal++;
			return (NetworkDevice)devices.get(1);
		}
		else
		{
			return null;
		}
	}
	
	public int getTotalDevice()
	{
		return devices.size();
	}
	
	public int getTotalTerminal()
	{
		return totalTerminal;
	}
}

public class Client
{
	public static void main(String args[])
	{
		NetworkDevice nd1,nd2,nd3,nd4,nd5;
		DeviceFactory df=new DeviceFactory();
		
		nd1=df.getNetworkDevice("cisco");
		nd1.use();
		
		nd2=df.getNetworkDevice("cisco");
		nd2.use();
		
		nd3=df.getNetworkDevice("cisco");
		nd3.use();
		
		nd4=df.getNetworkDevice("tp");
		nd4.use();
		
		nd5=df.getNetworkDevice("tp");
		nd5.use();
		
		System.out.println("Total Device:" + df.getTotalDevice());
		System.out.println("Total Terminal:" + df.getTotalTerminal());
	}
}
```

### 实例2：共享网络设备（有外部状态）

虽然网络设备可以共享，但是分配给每一个终端计算机的端口(Port)是不同的，因此多台计算机虽然可以共享同一个网络设备，但必须使用不同的端口。我们可以将端口从网络设备中抽取出来作为外部状态，需要时再进行设置。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_055543.png)

实例代码：

```java
import java.util.*;

public interface NetworkDevice
{
	public String getType();
	public void use(Port port);
}

public class Hub implements NetworkDevice
{
	private String type;
	
	public Hub(String type)
	{
		this.type=type;
	}
	
	public String getType()
	{
		return this.type;
	} 
	
	public void use(Port port)
	{
		System.out.println("Linked by Hub, type is " + this.type + ", port is " + port.getPort());
	}	
}

public class Switch implements NetworkDevice
{
	private String type;
	
	public Switch(String type)
	{
		this.type=type;
	}
	
	public String getType()
	{
		return this.type;
	} 
	
	public void use(Port port)
	{
		System.out.println("Linked by switch, type is " + this.type + ", port is " + port.getPort());
	}
}

public class Port
{
	private String port;
	
	public Port(String port)
	{
		this.port=port;	
	}
	
	public void setPort(String port)
	{
		this.port=port;
	}
	
	public String getPort()
	{
		return this.port;
	}
}

public class DeviceFactory
{
	private ArrayList devices = new ArrayList();
	private int totalTerminal=0;
	
	public DeviceFactory()
	{
		NetworkDevice nd1=new Switch("Cisco-WS-C2950-24");
		devices.add(nd1);
		NetworkDevice nd2=new Hub("TP-LINK-HF8M");
		devices.add(nd2);
	}
	
	public NetworkDevice getNetworkDevice(String type)
	{
		if(type.equalsIgnoreCase("cisco"))
		{
			totalTerminal++;
			return (NetworkDevice)devices.get(0);
		}
		else if(type.equalsIgnoreCase("tp"))
		{
			totalTerminal++;
			return (NetworkDevice)devices.get(1);
		}
		else
		{
			return null;
		}
	}
	
	public int getTotalDevice()
	{
		return devices.size();
	}
	
	public int getTotalTerminal()
	{
		return totalTerminal;
	}
}


public class Client
{
	public static void main(String args[])
	{
		NetworkDevice nd1,nd2,nd3,nd4,nd5;
		DeviceFactory df=new DeviceFactory();
		
		nd1=df.getNetworkDevice("cisco");
		nd1.use(new Port("1000"));
		
		nd2=df.getNetworkDevice("cisco");
		nd2.use(new Port("1001"));
		
		nd3=df.getNetworkDevice("cisco");
		nd3.use(new Port("1002"));
		
		nd4=df.getNetworkDevice("tp");
		nd4.use(new Port("1003"));
		
		nd5=df.getNetworkDevice("tp");
		nd5.use(new Port("1004"));
		
		System.out.println("Total Device:" + df.getTotalDevice());
		System.out.println("Total Terminal:" + df.getTotalTerminal());
	}
}
```

## 模式优缺点

**优点**如下：

- 享元模式的优点在于它可以极大减少内存中对象的数量，使得相同对象或相似对象在内存中只保存一份。
- 享元模式的外部状态相对独立，而且不会影响其内部状态，从而使得享元对象可以在不同的环境中被共享。

**缺点**如下：

- 享元模式使得系统更加复杂，需要分离出内部状态和外部状态，这使得程序的逻辑复杂化。
- 为了使对象可以共享，享元模式需要将享元对象的状态外部化，而读取外部状态使得运行时间变长。

## 模式适应环境

在以下情况下可以使用享元模式：

- 一个系统有大量相同或者相似的对象，由于这类对象的大量使用，造成内存的大量耗费。
- 对象的大部分状态都可以外部化，可以将这些外部状态传入对象中。
- 使用享元模式需要维护一个存储享元对象的享元池，而这需要耗费资源，因此，应当在多次重复使用享元对象时才值得使用享元模式。

## 模式应用

1. 享元模式在编辑器软件中大量使用，如在一个文档中多次出现相同的图片，则只需要创建一个图片对象，通过在应用程序中设置该图片出现的位置，可以实现该图片在不同地方多次重复显示。

2. 在JDK类库中定义的String类使用了享元模式。

   ```java
   public class Demo
   {
   	public static void main(String args[])
   	{
   		String str1 = "abcd";
   		String str2 = "abcd";
   		String str3 = "ab" + "cd";
   		String str4 = "ab";
   		str4 += "cd";
   		System.out.println(str1 == str2);
   		System.out.println(str1 == str3);
   		System.out.println(str1 == str4);
   	}
   } 
   ```

## 模式拓展

**单纯享元模式和复合享元模式**

- 单纯享元模式：在单纯享元模式中，所有的享元对象都是可以共享的，即所有抽象享元类的子类都可共享，不存在非共享具体享元类。

  ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_055601.png)

- 复合享元模式：将一些单纯享元使用组合模式加以组合，可以形成复合享元对象，这样的复合享元对象本身不能共享，但是它们可以分解成单纯享元对象，而后者则可以共享。

  ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231216_055617.png)

**享元模式与其他模式的联用**

- 在享元模式的享元工厂类中通常提供一个静态的工厂方法用于返回享元对象，使用简单工厂模式来生成享元对象。
- 在一个系统中，通常只有唯一一个享元工厂，因此享元工厂类可以使用单例模式进行设计。
- 享元模式可以结合组合模式形成复合享元模式，统一对享元对象设置外部状态。


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note-sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E4%BA%AB%E5%85%83%E6%A8%A1%E5%BC%8F/  

