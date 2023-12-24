# 设计模式-策略模式


## 模式动机

完成一项任务，往往可以有多种不同的方式，每一种方式称为一个策略，我们可以根据环境或者条件的不同选择不同的策略来完成该项任务。

在软件开发中也常常遇到类似的情况，实现某一个功能有多个途径，此时可以使用一种设计模式来使得系统可以灵活地选择解决途径，也能够方便地增加新的解决途径。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231225_123630.png)

在软件系统中，有许多算法可以实现某一功能，如查找、排序等，一种常用的方法是硬编码(Hard Coding)在一个类中，如需要提供多种查找算法，可以将这些算法写到一个类中，在该类中提供多个方法，每一个方法对应一个具体的查找算法；当然也可以将这些查找算法封装在一个统一的方法中，通过if…else…等条件判断语句来进行选择。这两种实现方法我们都可以称之为硬编码，如果需要增加一种新的查找算法，需要修改封装算法类的源代码；更换查找算法，也需要修改客户端调用代码。在这个算法类中封装了大量查找算法，该类代码将较复杂，维护较为困难。

除了提供专门的查找算法类之外，还可以在客户端程序中直接包含算法代码，这种做法更不可取，将导致客户端程序庞大而且难以维护，如果存在大量可供选择的算法时问题将变得更加严重。

为了解决这些问题，可以定义一些独立的类来封装不同的算法，每一个类封装一个具体的算法，在这里，每一个封装算法的类我们都可以称之为策略(Strategy)，为了保证这些策略的一致性，一般会用一个抽象的策略类来做算法的定义，而具体每种算法则对应于一个具体策略类。

## 模式定义

策略模式(Strategy Pattern)：定义一系列算法，将每一个算法封装起来，并让它们可以相互替换。策略模式让算法独立于使用它的客户而变化，也称为政策模式(Policy)。策略模式是一种对象行为型模式。

## 模式结构

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231225_123646.png)

策略模式包含如下角色：

- Context: 环境类
- Strategy: 抽象策略类
- ConcreteStrategy: 具体策略类

## 模式分析

策略模式是一个比较容易理解和使用的设计模式，策略模式是对算法的封装，它把算法的责任和算法本身分割开，委派给不同的对象管理。策略模式通常把一个系列的算法封装到一系列的策略类里面，作为一个抽象策略类的子类。用一句话来说，就是“准备一组算法，并将每一个算法封装起来，使得它们可以互换”。

不使用策略模式的代码：

```java
public class Context
{
    //...
    public void algorithm(String type)  
    {
        ......
        if(type == "strategyA")
        {
            //算法A
        }
        else if(type == "strategyB")
        {
            //算法B
        }
        else if(type == "strategyC")
        {
            //算法C
        }
        //......
    }
    //...
} 
```

重构之后的抽象策略类：

```java
public abstract class AbstractStrategy
{
    public abstract void algorithm();  
} 
```

重构之后的具体策略类：

```java
public class ConcreteStrategyA extends AbstractStrategy
{
    public void algorithm()
    {
        //算法A
    }
}
```

重构之后的环境类：

```java
public class Context
{
    private AbstractStrategy strategy;
    public void setStrategy(AbstractStrategy strategy)
    {
        this.strategy= strategy;
    }
    public void algorithm()
    {
        strategy.algorithm();
    }
} 
```

客户端代码片段：

```java
Context context = new Context();
AbstractStrategy strategy;
strategy = new ConcreteStrategyA();
context.setStrategy(strategy);
context.algorithm();
```

在策略模式中，应当由客户端自己决定在什么情况下使用什么具体策略角色。

策略模式仅仅封装算法，提供新算法插入到已有系统中，以及老算法从系统中“退休”的方便，策略模式并不决定在何时使用何种算法，算法的选择由客户端来决定。这在一定程度上提高了系统的灵活性，但是客户端需要理解所有具体策略类之间的区别，以便选择合适的算法，这也是策略模式的缺点之一，在一定程度上增加了客户端的使用难度。

## 模式实例

### 实例1：排序策略

某系统提供了一个用于对数组数据进行操作的类，该类封装了对数组的常见操作，如查找数组元素、对数组元素进行排序等。现以排序操作为例，使用策略模式设计该数组操作类，使得客户端可以动态地更换排序算法，可以根据需要选择冒泡排序或选择排序或插入排序，也能够灵活地增加新的排序算法。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231225_123705.png)

实例代码：

```java
public interface Sort
{
	public abstract int[] sort(int arr[]);
}

public class SelectionSort implements Sort
{
	public int[] sort(int arr[])
	{
	   int len=arr.length;
       int temp;
       for(int i=0;i<len;i++)
       {
           temp=arr[i];
           int j;
           int samllestLocation=i;
           for(j=i+1;j<len;j++)
           {
              if(arr[j]<temp)
              {
                  temp=arr[j];
                  samllestLocation=j;
              }
           }
           arr[samllestLocation]=arr[i];
           arr[i]=temp;
       }
       System.out.println("选择排序");
       return arr;
	}
}

public class QuickSort implements Sort
{
	public int[] sort(int arr[])
	{
		System.out.println("快速排序");
		sort(arr,0,arr.length-1);
		return arr;
	}

	public void sort(int arr[],int p, int r)
	{
		int q=0;
		if(p<r)
		{
			q=partition(arr,p,r);
			sort(arr,p,q-1);
            sort(arr,q+1,r);
		}
	}
	
	public int partition(int[] a, int p, int r)
	{
		int x=a[r];
		int j=p-1;
		for(int i=p;i<=r-1;i++)
		{
			if(a[i]<=x)
			{
				j++;
				swap(a,j,i);
			}
		}
		swap(a,j+1,r);
		return j+1;	
	}
	
	public void swap(int[] a, int i, int j) 
	{   
        int t = a[i];   
        a[i] = a[j];   
        a[j] = t;   
	}
}

public class InsertionSort implements Sort
{
	public int[] sort(int arr[])
	{
 	   int len=arr.length;
       for(int i=1;i<len;i++)
       {
           int j;
           int temp=arr[i];
           for(j=i;j>0;j--)
           {
              if(arr[j-1]>temp)
              {
                  arr[j]=arr[j-1];
                  
              }else
                  break;
           }
           arr[j]=temp;
		}
		System.out.println("插入排序");
		return arr;
	}
}

public class BubbleSort implements Sort
{
	public int[] sort(int arr[])
	{
	   int len=arr.length;
       for(int i=0;i<len;i++)
       {
           for(int j=i+1;j<len;j++)
           {
              int temp;
              if(arr[i]>arr[j])
              {
                  temp=arr[j];
                  arr[j]=arr[i];
                  arr[i]=temp;
              }             
           }
		}
		System.out.println("冒泡排序");
		return arr;
	}
}

public class ArrayHandler
{
	private Sort sortObj;
	
	public int[] sort(int arr[])
	{
		sortObj.sort(arr);
		return arr;
	}

	public void setSortObj(Sort sortObj) {
		this.sortObj = sortObj; 
	}
}

<?xml version="1.0"?>
<config>
    <className>QuickSort</className>
</config>

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

public class Client
{
	public static void main(String args[])
	{
	   int arr[]={1,4,6,2,5,3,7,10,9};
	   int result[];
	   ArrayHandler ah=new ArrayHandler();
	   
	   Sort sort;
       sort=(Sort)XMLUtil.getBean();
       
       ah.setSortObj(sort); //设置具体策略
       result=ah.sort(arr);
       
       for(int i=0;i<result.length;i++)
       {
       	    System.out.print(result[i] + ",");
       }
	}
}
```

### 实例2：旅游出行策略

旅游出行方式可以有多种，如可以乘坐飞机旅游，也可以乘火车旅游，如果有兴趣自行车游也是一种极具乐趣的出行方式。不同的旅游出行方式有不同的实现过程，客户根据自己的需要选择一种合适的旅游方式。在本实例中我们用策略模式来模拟这一过程。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231225_123723.png)

实例代码：

```java
interface TravelStrategy
{
	
	public void travelMethod();

}

class AirplaneStrategy implements  TravelStrategy
{	
	public void travelMethod()
	{
		System.out.println("飞机游！");
	}
}

class TrainStrategy implements  TravelStrategy
{
	public void travelMethod()
	{
		System.out.println("火车游！");
	}
}

class SelfTravelStrategy implements  TravelStrategy
{
	public void travelMethod()
	{
		System.out.println("自驾游！");
	}
}

class BicycleTravelStrategy implements  TravelStrategy
{
	public void travelMethod()
	{
		System.out.println("自行车游！");
	}
}

class MyContext
{
	private TravelStrategy ts;
	public MyContext(TravelStrategy ts)
	{
		this.ts=ts;
	}
	public void travelMethod()
	{
		ts.travelMethod();
	}
}

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

class Client
{
	public static void main(String args[])
	{
		MyContext mc=new MyContext((TravelStrategy)XMLUtil.getBean());
		mc.travelMethod();
	}
}
```

## 模式优缺点

**优点**如下：

- 策略模式提供了对“开闭原则”的完美支持，用户可以在不修改原有系统的基础上选择算法或行为，也可以灵活地增加新的算法或行为。
- 策略模式提供了管理相关的算法族的办法。
- 策略模式提供了可以替换继承关系的办法。
- 使用策略模式可以避免使用多重条件转移语句。

**缺点**如下：

- 客户端必须知道所有的策略类，并自行决定使用哪一个策略类。
- 策略模式将造成产生很多策略类，可以通过使用享元模式在一定程度上减少对象的数量。

## 模式适应环境

在以下情况下可以使用策略模式：

- 如果在一个系统里面有许多类，它们之间的区别仅在于它们的行为，那么使用策略模式可以动态地让一个对象在许多行为中选择一种行为。
- 一个系统需要动态地在几种算法中选择一种。
- 如果一个对象有很多的行为，如果不用恰当的模式，这些行为就只好使用多重的条件选择语句来实现。
- 不希望客户端知道复杂的、与算法相关的数据结构，在具体策略类中封装算法和相关的数据结构，提高算法的保密性与安全性。

## 模式应用

1. Java SE的容器布局管理就是策略模式应用的一个经典实例。 

   ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231225_123739.png)

2. 在微软公司提供的演示项目PetShop4.0中使用策略模式来处理同步订单和异步订单的问题。

   在Petshop 4.0的BLL子项目中有一个OrderAsynchronous类和一个OrderSynchronous类，它们都继承自IOrderStrategy。OrderSynchronous以一种同步的方式处理订单，而OrderAsynchronous先将订单放在队列里，然后再对队列里的订单进行处理，以一种异步方式对订单进行处理。而在BLL中的Order类里通过反射机制从配置文件中读取策略配置的信息以决定到底是使用哪种订单处理方式。只需要修改配置文件即可更改订单处理方式，提高了系统的灵活性。

## 模式扩展

**策略模式与状态模式**

- 可以通过环境类状态的个数来决定是使用策略模式还是状态模式。
- 策略模式的环境类自己选择一个具体策略类，具体策略类无须关心环境类；而状态模式的环境类由于外在因素需要放进一个具体状态中，以便通过其方法实现状态的切换，因此环境类和状态类之间存在一种双向的关联关系。
- 使用策略模式时，客户端需要知道所选的具体策略是哪一个，而使用状态模式时，客户端无须关心具体状态，环境类的状态会根据用户的操作自动转换。
- 如果系统中某个类的对象存在多种状态，不同状态下行为有差异，而且这些状态之间可以发生转换时使用状态模式；如果系统中某个类的某一行为存在多种实现方式，而且这些实现方式可以互换时使用策略模式。


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8F/  

