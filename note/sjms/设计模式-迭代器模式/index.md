# 设计模式-迭代器模式


## 模式动机

一个聚合对象，如一个列表(List)或者一个集合(Set)，应该提供一种方法来让别人可以访问它的元素，而又不需要暴露它的内部结构。

针对不同的需要，可能还要以不同的方式遍历整个聚合对象，但是我们并不希望在聚合对象的抽象层接口中充斥着各种不同遍历的操作。

怎样遍历一个聚合对象，又不需要了解聚合对象的内部结构，还能够提供多种不同的遍历方式，这就是迭代器模式所要解决的问题。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231221_022658.png)

在迭代器模式中，提供一个外部的迭代器来对聚合对象进行访问和遍历，迭代器定义了一个访问该聚合元素的接口，并且可以跟踪当前遍历的元素，了解哪些元素已经遍历过而哪些没有。

有了迭代器模式，我们会发现对一个复杂的聚合对象的操作会变得如此简单。

## 模式定义

迭代器模式(Iterator Pattern)：提供一种方法来访问聚合对象，而不用暴露这个对象的内部表示，其别名为游标(Cursor)。迭代器模式是一种对象行为型模式。

## 模式结构

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231221_022713.png)

迭代器模式包含如下角色：

- **Iterator**: 抽象迭代器
- **ConcreteIterator**: 具体迭代器
- **Aggregate**: 抽象聚合类
- **ConcreteAggregate**: 具体聚合类

## 模式分析

聚合是一个管理和组织数据对象的数据结构。

聚合对象主要拥有两个职责：一是存储内部数据；二是遍历内部数据。

存储数据是聚合对象最基本的职责。

将遍历聚合对象中数据的行为提取出来，封装到一个迭代器中，通过专门的迭代器来遍历聚合对象的内部数据，这就是迭代器模式的本质。迭代器模式是“单一职责原则”的完美体现。 

自定义迭代器：

MyIterator ——抽象迭代器
MyCollection ——抽象聚合类
NewCollection ——具体聚合类
NewIterator ——具体迭代器
Client

自定义迭代器：

```java
interface MyCollection
{
	MyIterator createIterator();
}
interface MyIterator
{
	void first();
	void next();
	boolean isLast();
	Object currentItem();
}

class NewCollection implements MyCollection
{
   private Object[] obj={"dog","pig","cat","monkey","pig"};
   public MyIterator createIterator()
   {
  	  return new NewIterator();
   }
   
   private class NewIterator implements MyIterator
   {
        private int currentIndex=0;

        public void first()
        {
            currentIndex=0;
        }

        public void next()
        {
            if(currentIndex<obj.length)
            {
                currentIndex++;
            }
        }

        public void previous()
        {
            if(currentIndex>0)
            {
                currentIndex--;
            }
        }	

        public boolean isLast()
        {
            return currentIndex==obj.length;
        }

        public boolean isFirst()
        {
            return currentIndex==0;
        }

        public Object currentItem()
        {
            return obj[currentIndex];
        }
   }
}

class Client
{
	public static void process(MyCollection collection)
	{
		MyIterator i=collection.createIterator();
		
		while(!i.isLast())
		{
			System.out.println(i.currentItem().toString());
			i.next();
		}
	}
	
	public static void main(String a[])
	{
		MyCollection collection=new NewCollection();
		process(collection);
	}
}
```

在迭代器模式中应用了工厂方法模式，聚合类充当工厂类，而迭代器充当产品类，由于定义了抽象层，系统的扩展性很好，在客户端可以针对抽象聚合类和抽象迭代器进行编程。

由于很多编程语言的类库都已经实现了迭代器模式，因此在实际使用中我们很少自定义迭代器，只需要直接使用Java、C#等语言中已定义好的迭代器即可，迭代器已经成为我们操作聚合对象的基本工具之一。

## 模式实例

### 实例1：电视机遥控器

电视机遥控器就是一个迭代器的实例，通过它可以实现对电视机频道集合的遍历操作，本实例我们将模拟电视机遥控器的实现。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231221_022732.png)

实例代码：

```java
public interface Television
{
	TVIterator createIterator();
}

public interface TVIterator
{
	void setChannel(int i);
	void next();
	void previous();
	boolean isLast();
	Object currentChannel();
    boolean isFirst();
}

public class TCLTelevision implements Television
{
	private Object[] obj={"湖南卫视","北京卫视","上海卫视","湖北卫视","黑龙江卫视"};
	public TVIterator createIterator()
	{
		return new TCLIterator();
	}
   
	class TCLIterator implements TVIterator
	{
	   	private int currentIndex=0;
	   	 	
		public void next()
		{
			if(currentIndex<obj.length)
			{
				currentIndex++;
			}
		}
		
		public void previous()
		{
			if(currentIndex>0)
			{
				currentIndex--;
			}
		}	
		
		public void setChannel(int i)
		{
			currentIndex=i;
		}
		
		
		public Object currentChannel()
		{
			return obj[currentIndex];
		}
		
		public boolean isLast()
		{
			return currentIndex==obj.length;
		}
	
		public boolean isFirst()
		{
			return currentIndex==0;
		}
	}
}

public class SkyworthTelevision implements Television
{
	private Object[] obj={"CCTV-1","CCTV-2","CCTV-3","CCTV-4","CCTV-5","CCTV-6","CCTV-7","CCTV-8"};
	public TVIterator createIterator()
	{
		return new SkyworthIterator();
	}

	private class SkyworthIterator implements TVIterator
	{
	   	private int currentIndex=0;
	   	 	
		public void next()
		{
			if(currentIndex<obj.length)
			{
				currentIndex++;
			}
		}
		
		public void previous()
		{
			if(currentIndex>0)
			{
				currentIndex--;
			}
		}	
		
		public void setChannel(int i)
		{
			currentIndex=i;
		}
		
		
		public Object currentChannel()
		{
			return obj[currentIndex];
		}
		
		public boolean isLast()
		{
			return currentIndex==obj.length;
		}
		
		public boolean isFirst()
		{
			return currentIndex==0;
		}
	}
}

<?xml version="1.0"?>
<config>
	<className>SkyworthTelevision</className>
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
	public static void display(Television tv)
	{
		TVIterator i=tv.createIterator();
		System.out.println("电视机频道：");
		while(!i.isLast())
		{
			System.out.println(i.currentChannel().toString());
			i.next();
		}
	}
	
	public static void reverseDisplay(Television tv)
	{
		TVIterator i=tv.createIterator();
		i.setChannel(5);
		System.out.println("逆向遍历电视机频道：");
		while(!i.isFirst())
		{
			i.previous();
			System.out.println(i.currentChannel().toString());
		}
	}
	
	public static void main(String a[])
	{
		Television tv;
		tv=(Television)XMLUtil.getBean();
		display(tv);
		System.out.println("--------------------------");
		reverseDisplay(tv);
	}
}
```

## 模式优缺点

**优点**如下：

- 它支持以不同的方式遍历一个聚合对象。
- 迭代器简化了聚合类。
- 在同一个聚合上可以有多个遍历。
- 在迭代器模式中，增加新的聚合类和迭代器类都很方便，无须修改原有代码，满足“开闭原则”的要求。

**缺点**如下：

- 由于迭代器模式将存储数据和遍历数据的职责分离，增加新的聚合类需要对应增加新的迭代器类，类的个数成对增加，这在一定程度上增加了系统的复杂性。

## 模式适应环境

在以下情况下可以使用迭代器模式：

- 访问一个聚合对象的内容而无须暴露它的内部表示。
- 需要为聚合对象提供多种遍历方式。
- 为遍历不同的聚合结构提供一个统一的接口。

## 模式应用

1. JDK1.2引入了新的Java聚合框架Collections。

   ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231221_022749.png)

   Collection是所有Java聚合类的根接口。

   在JDK类库中，Collection的iterator()方法返回一个java.util.Iterator类型的对象，而其子接口java.util.List的listIterator()方法返回一个java.util.ListIterator类型的对象，ListIterator是Iterator的子类。它们构成了Java语言对迭代器模式的支持，Java语言的java.util.Iterator接口就是迭代器模式的应用。

   ```java
   import java.util.*;
   public class IteratorDemo
   {	
      public static void process(Collection c)
      {
      	  	Iterator i=c.iterator();
   		
   		while(i.hasNext())
   		{
   			System.out.println(i.next().toString());
   		}
      }
   
   	public static void main(String args[])
   	{
   		Collection list=new HashSet();
   		list.add("Cat");
   		list.add("Dog");
   		list.add("Pig");
   		list.add("Dog");
   		list.add("Monkey");
   		
   		process(list);
   	}
   }
   ```

## 模式拓展

1. java迭代器

   在JDK中，Iterator接口具有如下3个基本方法：

   1. Object next()：通过反复调用next()方法可以逐个访问聚合中的元素。

   2. boolean hasNext()：hasNext()方法用于判断聚合对象中是否还存在下一个元素，为了不抛出异常，必须在调用next()之前先调用hasNext()。如果迭代对象仍然拥有可供访问的元素，那么hasNext()返回true。

   3. void remove()：用于删除上次调用next()时所返回的元素。 

   Java迭代器可以理解为它工作在聚合对象的各个元素之间，每调用一次next()方法，迭代器便越过下个元素，并且返回它刚越过的那个元素的地址引用。但是，它也有一些限制，如某些迭代器只能单向移动。在使用迭代器时，访问某个元素的唯一方法就是调用next()。

   ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231221_022803.png)

   实例代码：

   ```java
   Iterator iterator = collection.iterator();   //collection是已实例化的集合对象
   iterator.next();	 	// 跳过第一个元素
   iterator.remove(); 	                  // 删除第一个元素
   
   iterator.remove();
   iterator.next();  //该语句不能去掉
   iterator.remove(); 
   ```

   


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E8%BF%AD%E4%BB%A3%E5%99%A8%E6%A8%A1%E5%BC%8F/  

