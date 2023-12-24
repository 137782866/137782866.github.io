# 设计模式-模板方法模式


## 模式动机

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231225_010144.png)

模板方法模式是基于继承的代码复用基本技术，模板方法模式的结构和用法也是面向对象设计的核心之一。在模板方法模式中，可以将相同的代码放在父类中，而将不同的方法实现放在不同的子类中。

在模板方法模式中，我们需要准备一个抽象类，将部分逻辑以具体方法以及具体构造函数的形式实现，然后声明一些抽象方法来让子类实现剩余的逻辑。不同的子类可以以不同的方式实现这些抽象方法，从而对剩余的逻辑有不同的实现，这就是模板方法模式的用意。模板方法模式体现了面向对象的诸多重要思想，是一种使用频率较高的模式。

## 模式定义

模板方法模式(Template Method Pattern)：定义一个操作中算法的骨架，而将一些步骤延迟到子类中，模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。模板方法是一种类行为型模式。

## 模式结构

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231225_010200.png)

模板方法模式包含如下角色：

- AbstractClass**:** 抽象类
- ConcreteClass**:** 具体子类

## 模式分析

模板方法模式是一种类的行为型模式，在它的结构图中只有类之间的继承关系，没有对象关联关系。

在模板方法模式的使用过程中，要求开发抽象类和开发具体子类的设计师之间进行协作。一个设计师负责给出一个算法的轮廓和骨架，另一些设计师则负责给出这个算法的各个逻辑步骤。实现这些具体逻辑步骤的方法称为基本方法(Primitive Method)，而将这些基本法方法汇总起来的方法称为模板方法(Template Method)，模板方法模式的名字从此而来。

模板方法：一个模板方法是定义在抽象类中的、把基本操作方法组合在一起形成一个总算法或一个总行为的方法。

基本方法：基本方法是实现算法各个步骤的方法，是模板方法的组成部分。

- 抽象方法(Abstract Method)

- 具体方法(Concrete Method)

- 钩子方法(Hook Method)：“挂钩”方法和空方法

  ```java
  public void template()
  {
      open();
      display();
      if(isPrint())
      {
          print();
      }
  }
  
  public boolean isPrint()
  {
      return true;
  }
  ```

典型的抽象类代码如下所示：

```java
public abstract class AbstractClass
{
    public void templateMethod()  //模板方法
    {
        primitiveOperation1();
        primitiveOperation2();
        primitiveOperation3();
    }
    
    public void primitiveOperation1()    //基本方法—具体方法
    {
        //实现代码
    }
    public abstract void primitiveOperation2();    //基本方法—抽象方法
    
    public void primitiveOperation3()    //基本方法—钩子方法
    {
    }
}
```

典型的具体子类代码如下所示：

```java
public class ConcreteClass extends AbstractClass
{
     public void primitiveOperation2()
    {
        //实现代码
    }
    public void primitiveOperation3()   
    {
        //实现代码
    }
}
```

## 模式分析

在模板方法模式中，由于面向对象的多态性，子类对象在运行时将覆盖父类对象，子类中定义的方法也将覆盖父类中定义的方法，因此程序在运行时，具体子类的基本方法将覆盖父类中定义的基本方法，子类的钩子方法也将覆盖父类的钩子方法，从而可以通过在子类中实现的钩子方法对父类方法的执行进行约束，实现子类对父类行为的反向控制。

## 模式实例

### 实例1：银行业务办理流程

在银行办理业务时，一般都包含几个基本步骤，首先需要取号排队，然后办理具体业务，最后需要对银行工作人员进行评分。无论具体业务是取款、存款还是转账，其基本流程都一样。现使用模板方法模式模拟银行业务办理流程。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231225_010216.png)

实例代码：

```java
public abstract class BankTemplateMethod
{
    public void takeNumber()
	{
		System.out.println("取号排队。");
	}
	
	public abstract void transact();
	
	public void evaluate()
	{
		System.out.println("反馈评分。");
	}

    public void process()
    {
        this.takeNumber();
        this.transact();
        this.evaluate();
    }
} 

public class Deposit extends BankTemplateMethod
{
	public void transact()
	{
		System.out.println("存款");		
	}
}

public class Transfer extends BankTemplateMethod
{
	public void transact()
	{
		System.out.println("转账");		
	}
}

public class Withdraw extends BankTemplateMethod
{
	public void transact()
	{
		System.out.println("取款");		
	}
}

<?xml version="1.0"?>
<config>
    <className>Transfer</className>
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
	public static void main(String a[])
	{
		BankTemplateMethod bank;
		bank=(BankTemplateMethod)XMLUtil.getBean();
		bank.process();
		System.out.println("---------------------------------------");
	}
}
```

### 实例2：数据库操作模板

对数据库的操作一般包括连接、打开、使用、关闭等步骤，在数据库操作模板类中我们定义了connDB()、openDB()、useDB()、closeDB()四个方法分别对应这四个步骤。对于不同类型的数据库（如SQL Server和Oracle），其操作步骤都一致，只是连接数据库connDB()方法有所区别，现使用模板方法模式对其进行设计。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231225_010231.png)

实例代码：

```java
abstract class DBOperator
{
	public abstract void connDB();
    public void openDB()
	{
		System.out.println("打开数据库");
	}
	public void useDB()
	{
		System.out.println("使用数据库");
	}
	public void closeDB()
	{
		System.out.println("关闭数据库");	
	}
   public void process()
   {
    connDB();
   	openDB();
   	useDB();
   	closeDB();
   }
} 

class DBOperatorSubA extends DBOperator
{
	public void connDB()
	{
		System.out.println("使用JDBC-ODBC桥接连接数据库");		
	}
}

class DBOperatorSubB extends DBOperator
{
	public void connDB()
	{
		System.out.println("使用连接池连接数据库");		
	}
}

class Client
{
	public static void main(String a[])
	{
		DBOperator db1;
		
		db1=new DBOperatorSubA();
		db1.process();
		System.out.println("---------------------------------------");			
		db1=new DBOperatorSubB();
		db1.process();
	}
}
```

## 模式优缺点

**优点**如下：

- 模板方法模式在一个类中形式化地定义算法，而由它的子类实现细节的处理。
- 模板方法模式是一种代码复用的基本技术。
- 模板方法模式导致一种反向的控制结构，通过一个父类调用其子类的操作，通过对子类的扩展增加新的行为，符合“开闭原则”。

**缺点**如下：

- 每个不同的实现都需要定义一个子类，这会导致类的个数增加，系统更加庞大，设计也更加抽象，但是更加符合“单一职责原则”，使得类的内聚性得以提高。

## 模式适应环境

在以下情况下可以使用模板方法模式：

- 一次性实现一个算法的不变的部分，并将可变的行为留给子类来实现。
- 各子类中公共的行为应被提取出来并集中到一个公共父类中以避免代码重复。
- 对一些复杂的算法进行分割，将其算法中固定不变的部分设计为模板方法和父类具体方法，而一些可以改变的细节由其子类来实现。
- 控制子类的扩展。

## 模式应用

1. 模板方法模式广泛应用于框架设计（如Spring，Struts等）中，以确保父类控制处理流程的逻辑顺序（如框架的初始化）。

2. Java单元测试工具JUnit中的TestCase类的设计：

   ```java
   public void runBare() throws Throwable {
   	setUp();
   	try {
   		runTest();
   	}
   	finally {
   		tearDown();
   	}
   }
   ```

## 模式扩展

**关于继承的讨论**

模板方法模式鼓励我们恰当使用继承，此模式可以用来改写一些拥有相同功能的相关类，将可复用的一般性的行为代码移到父类里面，而将特殊化的行为代码移到子类里面。这也进一步说明，虽然继承复用存在一些问题，但是在某些情况下还是可以给开发人员带来方便，模板方法模式就是体现继承优势的模式之一。

**好莱坞原则**

- 在模板方法模式中，子类不显式调用父类的方法，而是通过覆盖父类的方法来实现某些具体的业务逻辑，父类控制对子类的调用，这种机制被称为好莱坞原则(Hollywood Principle)，好莱坞原则的定义为：“不要给我们打电话，我们会给你打电话(Don‘t call us, we’ll call you)”。
- 在模板方法模式中，好莱坞原则体现在：子类不需要调用父类，而通过父类来调用子类，将某些步骤的实现写在子类中，由父类来控制整个过程。 

**钩子方法的使用** 

- 钩子方法的引入使得子类可以控制父类的行为。

- 最简单的钩子方法就是空方法，也可以在钩子方法中定义一个默认的实现，如果子类不覆盖钩子方法，则执行父类的默认实现代码。

- 比较复杂一点的钩子方法可以对其他方法进行约束，这种钩子方法通常返回一个boolean类型，即返回true或false，用来判断是否执行某一个基本方法。

  ```java
  public abstract class HookDemo
  {
  	public abstract void getData();
  	
      public void convertData()
  	{
  		System.out.println("通用的数据转换操作。");
  	}
  	
  	public abstract void displayData();
  
      public void process()
      {
          getData();
          if(isValid())
          {
              convertData();	
          }
     	    displayData();
      }
      
      public boolean isValid()
      {
      	return true;
      }
  } 
  
  class SubHookDemo extends HookDemo
  {
  	public void getData()
  	{
  		System.out.println("从XML配置文件中获取数据。");
  	}
  	
  	public void displayData()
  	{
  		System.out.println("以柱状图显示数据。");
  	}
  	
  	public boolean isValid()
      {
      	return false;
      }	
  }
  
  class Client
  {
  	public static void main(String a[])
  	{
  		HookDemo hd;
  		
  		hd=new SubHookDemo();
  		hd.process();
  	}
  }
  ```


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E6%A8%A1%E6%9D%BF%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8F/  

