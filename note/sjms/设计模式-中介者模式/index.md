# 设计模式-中介者模式


## 模式动机

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231222_020355.png)

在用户与用户直接聊天的设计方案中，用户对象之间存在很强的关联性，将导致系统出现如下问题：

- 系统结构复杂：对象之间存在大量的相互关联和调用，若有一个对象发生变化，则需要跟踪和该对象关联的其他所有对象，并进行适当处理。
- 对象可重用性差：由于一个对象和其他对象具有很强的关联，若没有其他对象的支持，一个对象很难被另一个系统或模块重用，这些对象表现出来更像一个不可分割的整体，职责较为混乱。
- 系统扩展性低：增加一个新的对象需要在原有相关对象上增加引用，增加新的引用关系也需要调整原有对象，系统耦合度很高，对象操作很不灵活，扩展性差。

在面向对象的软件设计与开发过程中，根据“单一职责原则”，我们应该尽量将对象细化，使其只负责或呈现单一的职责。

对于一个模块，可能由很多对象构成，而且这些对象之间可能存在相互的引用，为了减少对象两两之间复杂的引用关系，使之成为一个松耦合的系统，我们需要使用中介者模式，这就是中介者模式的模式动机。

## 模式定义

中介者模式(Mediator Pattern)定义：用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。中介者模式又称为调停者模式，它是一种对象行为型模式。

## 模式结构

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231222_020412.png)

中介者模式包含如下角色：

- **Mediator**: 抽象中介者
- **ConcreteMediator**: 具体中介者
- **Colleague**: 抽象同事类
- **ConcreteColleague**: 具体同事类

## 模式分析

中介者模式可以使对象之间的关系数量急剧减少：

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231222_020426.png)

中介者承担两方面的职责：

- 中转作用（结构性）：通过中介者提供的中转作用，各个同事对象就不再需要显式引用其他同事，当需要和其他同事进行通信时，通过中介者即可。该中转作用属于中介者在结构上的支持。
- 协调作用（行为性）：中介者可以更进一步的对同事之间的关系进行封装，同事可以一致地和中介者进行交互，而不需要指明中介者需要具体怎么做，中介者根据封装在自身内部的协调逻辑，对同事的请求进行进一步处理，将同事成员之间的关系行为进行分离和封装。该协调作用属于中介者在行为上的支持。

典型的抽象中介者类代码：

```java
public abstract class Mediator
{
	protected ArrayList colleagues;
	public void register(Colleague colleague)
	{
		colleagues.add(colleague);
	}
	
	public abstract void operation();
}
```

典型的具体中介者类代码：

```java
public class ConcreteMediator extends Mediator
{
	public void operation()
	{
		//...
		((Colleague)(colleagues.get(0))).method1();
		//...
	}
}
```

典型的抽象同事类代码：

```java
public abstract class Colleague
{
	protected Mediator mediator;
	
	public Colleague(Mediator mediator)
	{
		this.mediator=mediator;
	}
	
	public abstract void method1();
	
	public abstract void method2();
}
```

典型的具体同事类代码：

```java
public class ConcreteColleague extends Colleague
{
	public ConcreteColleague(Mediator mediator)
	{
		super(mediator);
	}
	
	public void method1()
	{
		//...
	}
	
	public void method2()
	{
		mediator.operation1();
	}
} 
```

## 模式实例

### 实例1：虚拟聊天室

某论坛系统欲增加一个虚拟聊天室，允许论坛会员通过该聊天室进行信息交流，普通会员(CommonMember)可以给其他会员发送文本信息，钻石会员(DiamondMember)既可以给其他会员发送文本信息，还可以发送图片信息。该聊天室可以对不雅字符进行过滤，如“日”等字符；还可以对发送的图片大小进行控制。用中介者模式设计该虚拟聊天室。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231222_020443.png)

实例代码：

```java
import java.util.*;

public abstract class AbstractChatroom
{
	public abstract void register(Member member);
	public abstract void sendText(String from,String to,String message);
	public abstract void sendImage(String from,String to,String message);
}

public abstract class Member
{
	protected AbstractChatroom chatroom;
	protected String name;
	
	public Member(String name)
	{
		this.name=name;
	}
	
	public String getName()
	{
		return name;
	}
	
	public void setName(String name)
	{
		this.name=name;
	}
	
	public AbstractChatroom getChatroom()
	{
		return chatroom;
	}
	
    public void setChatroom(AbstractChatroom chatroom)
    {
    	this.chatroom=chatroom;
    }
	
	public abstract void sendText(String to,String message);
	public abstract void sendImage(String to,String image);

    public void receiveText(String from,String message)
    {
    	System.out.println(from + "发送文本给" + this.name + "，内容为：" + message);
    }
    
    public void receiveImage(String from,String image)
    {
    	System.out.println(from + "发送图片给" + this.name + "，内容为：" + image);
    }	
}


public class ChatGroup extends AbstractChatroom
{
	private Hashtable members=new Hashtable();
	
	public void register(Member member)
	{
		if(!members.contains(member))
		{
			members.put(member.getName(),member);
			member.setChatroom(this);
		}
	}
	
   public void sendText(String from,String to,String message)
   {
   	  Member member=(Member)members.get(to);
   	  String newMessage=message;
   	  newMessage=message.replaceAll("日","*");
	  member.receiveText(from,newMessage);
   }
   
   public void sendImage(String from,String to,String image)
   {
   	  Member member=(Member)members.get(to);
   	  //模拟图片大小判断
   	  if(image.length()>5)
   	  {
   	  	  System.out.println("图片太大，发送失败！");
   	  }
   	  else
   	  {
   	  	  member.receiveImage(from,image);
   	  }
   }
}

public class CommonMember extends Member
{
	public CommonMember(String name)
	{
		super(name);
	}
	
	public void sendText(String to,String message)
	{
	    System.out.println("普通会员发送信息：");
	    chatroom.sendText(name,to,message);  //发送
	}
	
	public void sendImage(String to,String image)
	{
		System.out.println("普通会员不能发送图片！");
	}
}

public class DiamondMember extends Member
{
	public DiamondMember(String name)
	{
		super(name);
	}
	
	public void sendText(String to,String message)
	{
	    System.out.println("钻石会员发送信息：");
	    chatroom.sendText(name,to,message);  //发送
	}
	
	public void sendImage(String to,String image)
	{
		System.out.println("钻石会员发送图片：");
	    chatroom.sendImage(name,to,image);  //发送
	}
}

public class Client
{
	public static void main(String args[])
	{
		AbstractChatroom happyChat=new ChatGroup();
		Member member1,member2,member3,member4,member5;
		member1=new DiamondMember("张三");
		member2=new DiamondMember("李四");
		member3=new CommonMember("王五");
		member4=new CommonMember("小芳");
		member5=new CommonMember("小红");
		
		happyChat.register(member1);
		happyChat.register(member2);
		happyChat.register(member3);
		happyChat.register(member4);
		happyChat.register(member5);
		
		member1.sendText("李四","李四，你好！");
		member2.sendText("张三","张三，你好！");
		member1.sendText("李四","今天天气不错，有日！");
		member2.sendImage("张三","一个很大很大的太阳");
		member2.sendImage("张三","太阳");
		member3.sendText("小芳","还有问题吗？");
		member3.sendText("小红","还有问题吗？");
		member4.sendText("王五","没有了，谢谢！");
		member5.sendText("王五","我也没有了！");
		member5.sendImage("王五","谢谢");
	}
}
```

## 模式优缺点

**优点**如下：

- 简化了对象之间的交互。
- 将各同事解耦。
- 减少子类生成。
- 可以简化各同事类的设计和实现。

**缺点**如下：

- 在具体中介者类中包含了同事之间的交互细节，可能会导致具体中介者类非常复杂，使得系统难以维护。

## 模式适应环境

在以下情况下可以使用中介者模式：

- 系统中对象之间存在复杂的引用关系，产生的相互依赖关系结构混乱且难以理解。
- 一个对象由于引用了其他很多对象并且直接和这些对象通信，导致难以复用该对象。
- 想通过一个中间类来封装多个类中的行为，而又不想生成太多的子类。可以通过引入中介者类来实现，在中介者中定义对象交互的公共行为，如果需要改变行为则可以增加新的中介者类。

## 模式应用

1. 中介者模式在事件驱动类软件中应用比较多，在设计GUI应用程序时，组件之间可能存在较为复杂的交互关系，一个组件的改变将影响与之相关的其他组件，此时可以使用中介者模式来对组件进行协调。
2. MVC是Java EE的一个基本模式，此时控制器Controller作为一种中介者，它负责控制视图对象View和模型对象Model之间的交互。如在Struts中，Action就可以作为JSP页面与业务对象之间的中介者。

## 模式扩展

**中介者模式与迪米特法则**

在中介者模式中，通过创造出一个中介者对象，将系统中有关的对象所引用的其他对象数目减少到最少，使得一个对象与其同事之间的相互作用被这个对象与中介者对象之间的相互作用所取代。因此，中介者模式就是迪米特法则的一个典型应用。

中介者模式与GUI开发

- 中介者模式可以方便地应用于图形界面(GUI)开发中，在比较复杂的界面中可能存在多个界面组件之间的交互关系。
- 对于这些复杂的交互关系，有时候我们可以引入一个中介者类，将这些交互的组件作为具体的同事类，将它们之间的引用和控制关系交由中介者负责，在一定程度上简化系统的交互，这也是中介者模式的常见应用之一。


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E4%B8%AD%E4%BB%8B%E8%80%85%E6%A8%A1%E5%BC%8F/  

