# 设计模式-状态模式


## 模式动机

在很多情况下，一个对象的行为取决于一个或多个动态变化的属性，这样的属性叫做状态，这样的对象叫做有状态的 (stateful)对象，这样的对象状态是从事先定义好的一系列值中取出的。当一个这样的对象与外部事件产生互动时，其内部状态就会改变，从而使得系统的行为也随之发生变化。

在UML中可以使用状态图来描述对象状态的变化。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_094033.png)

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_094047.png)



## 模式定义

状态模式(State Pattern)：允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类。其别名为状态对象(Objects for States)，状态模式是一种对象行为型模式。

## 模式结构

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_094101.png)

状态模式包含如下角色：

- Context: 环境类
- State: 抽象状态类
- ConcreteState: 具体状态类

## 模式分析

状态模式描述了对象状态的变化以及对象如何在每一种状态下表现出不同的行为。

状态模式的关键是引入了一个抽象类来专门表示对象的状态，这个类我们叫做抽象状态类，而对象的每一种具体状态类都继承了该类，并在不同具体状态类中实现了不同状态的行为，包括各种状态之间的转换。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_094115.png)

不使用状态模式：

```java
//...
if(state=="空闲")
{
	if(预订房间)
	{
		//预订操作;
		state="已预订";
	}
	else if(住进房间)
	{
		//入住操作;
		state="已入住";
	}
}
else if(state=="已预订")
{
	if(住进房间)
	{
		//入住操作;
		state="已入住";
	}
	else if(取消预订)
	{
		//取消操作;
		state="空闲";
	}
}
//...
```

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_094131.png)

在状态模式结构中需要理解环境类与抽象状态类的作用：

- 环境类实际上就是拥有状态的对象，环境类有时候可以充当状态管理器(State Manager)的角色，可以在环境类中对状态进行切换操作。
- 抽象状态类可以是抽象类，也可以是接口，不同状态类就是继承这个父类的不同子类，状态类的产生是由于环境类存在多个状态，同时还满足两个条件：这些状态经常需要切换，在不同的状态下对象的行为不同。因此可以将不同对象下的行为单独提取出来封装在具体的状态类中，使得环境类对象在其内部状态改变时可以改变它的行为，对象看起来似乎修改了它的类，而实际上是由于切换到不同的具体状态类实现的。由于环境类可以设置为任一具体状态类，因此它针对抽象状态类进行编程，在程序运行时可以将任一具体状态类的对象设置到环境类中，从而使得环境类可以改变内部状态，并且改变行为。

## 模式实例

### 实例1：论坛用户等级

- 在某论坛系统中，用户可以发表留言，发表留言将增加积分；用户也可以回复留言，回复留言也将增加积分；用户还可以下载文件，下载文件将扣除积分。该系统用户分为三个等级，分别是新手、高手和专家，这三个等级对应三种不同的状态，这三种状态分别定义如下：
  1. 如果积分小于100分，则为新手状态，用户可以发表留言、回复留言，但是不能下载文件。如果积分大于等于1000分，则转换为专家状态；如果积分大于等于100分，则转换为高手状态。
  2. 如果积分大于等于100分但小于1000分，则为高手状态，用户可以发表留言、回复留言，还可以下载文件，而且用户在发表留言时可以获取双倍积分。如果积分小于100分，则转换为新手状态；如果积分大于等于1000分，则转换为专家状态；如果下载文件后积分小于0，则不能下载该文件。
  3. 如果积分大于等于1000分，则为专家状态，用户可以发表留言、回复留言和下载文件，用户除了在发表留言时可以获取双倍积分外，下载文件只扣除所需积分的一半。如果积分小于100分，则转换为新手状态；如果积分小于1000分，但大于等于100，则转换为高手状态；如果下载文件后积分小于0，则不能下载该文件。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_094148.png)

实例代码：

```java
public class ForumAccount
{
	private AbstractState state;
	private String name;
	public ForumAccount(String name)
	{
		this.name=name;
		this.state=new PrimaryState(this);
		System.out.println(this.name + "注册成功！");	
		System.out.println("---------------------------------------------");	
	}
	
	public void setState(AbstractState state)
	{
		this.state=state;
	}
	
	public AbstractState getState()
	{
		return this.state;
	}
	
	public void setName(String name)
	{
		this.name=name;
	}
	
	public String getName()
	{
		return this.name;
	}
	
    public void downloadFile(int score)
    {	
		state.downloadFile(score);
    }
    
	public void writeNote(int score)
	{
		state.writeNote(score);
	}
	
	public void replyNote(int score)
	{
		state.replyNote(score);
	}
}

public abstract class AbstractState
{
	protected ForumAccount acc;
	protected int point;
	protected String stateName;
    public abstract void checkState(int score);
	
    public void downloadFile(int score)
    {
    	System.out.println(acc.getName() + "下载文件，扣除" + score + "积分。");
		this.point-=score;
		checkState(score);
		System.out.println("剩余积分为：" + this.point + "，当前级别为：" + acc.getState().stateName + "。");
    }
    
	public void writeNote(int score)
	{
		System.out.println(acc.getName() + "发布留言" + "，增加" + score + "积分。");
		this.point+=score;
		checkState(score);
		System.out.println("剩余积分为：" + this.point + "，当前级别为：" + acc.getState().stateName + "。");
	}
	
	public void replyNote(int score)
	{
		System.out.println(acc.getName() + "回复留言，增加" + score + "积分。");
		this.point+=score;
		checkState(score);
	    System.out.println("剩余积分为：" + this.point + "，当前级别为：" + acc.getState().stateName + "。");
	}
	
	public void setPoint(int point) {
		this.point = point; 
	}

	public int getPoint() {
		return (this.point); 
	}
	
	public void setStateName(String stateName) {
		this.stateName = stateName; 
	}

	public String getStateName() {
		return (this.stateName); 
	}
}

public class HighState extends AbstractState
{
	public HighState(AbstractState state)
	{
		this.acc=state.acc;
		this.point=state.getPoint();
		this.stateName="专家";
	}
	
	public void writeNote(int score)
	{
		System.out.println(acc.getName() + "发布留言" + "，增加" + score + "*2个积分。");
		this.point+=score*2;
		checkState(score);
		System.out.println("剩余积分为：" + this.point + "，当前级别为：" + acc.getState().stateName + "。");
	}
	
    public void downloadFile(int score)
    {
    	System.out.println(acc.getName() + "下载文件，扣除" + score + "/2积分。");
		this.point-=score/2;
		checkState(score);
		System.out.println("剩余积分为：" + this.point + "，当前级别为：" + acc.getState().stateName + "。");    }
			
	public void checkState(int score)
	{
		if(point<0)
		{
			System.out.println("余额不足，文件下载失败！");
			this.point+=score;
		}
		else if(point<=100)
		{
			acc.setState(new PrimaryState(this));
		}
		else if(point<=1000)
		{
			acc.setState(new MiddleState(this));
		}
	}
}

public class MiddleState extends AbstractState
{
	public MiddleState(AbstractState state)
	{
		this.acc=state.acc;
		this.point=state.getPoint();
		this.stateName="高手";
	}
	
	public void writeNote(int score)
	{
		System.out.println(acc.getName() + "发布留言" + "，增加" + score + "*2个积分。");
		this.point+=score*2;
		checkState(score);
		System.out.println("剩余积分为：" + this.point + "，当前级别为：" + acc.getState().stateName + "。");
	}
			
	public void checkState(int score)
	{
		if(point>=1000)
		{
			acc.setState(new HighState(this));
		}
		else if(point<0)
		{
			System.out.println("余额不足，文件下载失败！");
			this.point+=score;
		}
		else if(point<=100)
		{
			acc.setState(new PrimaryState(this));
		}
	}
}

public class PrimaryState extends AbstractState
{
	public PrimaryState(AbstractState state)
	{
		this.acc=state.acc;
		this.point=state.getPoint();
		this.stateName="新手";
	}
	
	public PrimaryState(ForumAccount acc)
	{
		this.point=0;
		this.acc=acc;
		this.stateName="新手";
	}
	
	public void downloadFile(int score)
    {
    	System.out.println("对不起，" + acc.getName() + "，您没有下载文件的权限！");
    }
		
	public void checkState(int score)
	{
		if(point>=1000)
		{
			acc.setState(new HighState(this));
		}
		else if(point>=100)
		{
			acc.setState(new MiddleState(this));
		}
	}
}

public class Client
{
	public static void main(String args[])
	{
		ForumAccount account=new ForumAccount("张三");
		account.writeNote(20);
		System.out.println("--------------------------------------");
		account.downloadFile(20);
		System.out.println("--------------------------------------");
		account.replyNote(100);
		System.out.println("--------------------------------------");
		account.writeNote(40);
		System.out.println("--------------------------------------");
		account.downloadFile(80);
		System.out.println("--------------------------------------");
		account.downloadFile(150);
		System.out.println("--------------------------------------");
		account.writeNote(1000);
		System.out.println("--------------------------------------");
		account.downloadFile(80);
		System.out.println("--------------------------------------");
	}
}
```

### 实例2：银行账户 

在某银行系统定义的账户有三种状态：

1. 如果账户(Account)中余额(balance)大于等于0，此时账户的状态为绿色(GreenState)，即正常状态，表示既可以向该账户存款(deposit)也可以从该账户取款(withdraw)；
2. 如果账户中余额小于0，并且大于等于-1000，则账户的状态为黄色(YellowState)，即欠费状态，此时既可以向该账户存款也可以从该账户取款；
3. 如果账户中余额小于-1000，那么账户的状态为红色(RedState)，即透支状态，此时用户只能向该账户存款，不能再从中取款。

现用状态模式来实现状态的转化问题，用户只需要执行简单的存款和取款操作，系统根据余额数量自动转换到相应的状态。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_094205.png)

## 模式优缺点

**优点**如下：

- 封装了转换规则。
- 枚举可能的状态，在枚举状态之前需要确定状态种类。
- 将所有与某个状态有关的行为放到一个类中，并且可以方便地增加新的状态，只需要改变对象状态即可改变对象的行为。
- 允许状态转换逻辑与状态对象合成一体，而不是某一个巨大的条件语句块。
- 可以让多个环境对象共享一个状态对象，从而减少系统中对象的个数。

**缺点**如下：

- 状态模式的使用必然会增加系统类和对象的个数。
- 状态模式的结构与实现都较为复杂，如果使用不当将导致程序结构和代码的混乱。
- 状态模式对“开闭原则”的支持并不太好，对于可以切换状态的状态模式，增加新的状态类需要修改那些负责状态转换的源代码，否则无法切换到新增状态；而且修改某个状态类的行为也需修改对应类的源代码。

## 模式适应环境

在以下情况下可以使用状态模式：

- 对象的行为依赖于它的状态（属性）并且可以根据它的状态改变而改变它的相关行为。
- 代码中包含大量与对象状态有关的条件语句，这些条件语句的出现，会导致代码的可维护性和灵活性变差，不能方便地增加和删除状态，使客户类与类库之间的耦合增强。在这些条件语句中包含了对象的行为，而且这些条件对应于对象的各种状态。

## 模式应用

1. 状态模式在工作流或游戏等类型的软件中得以广泛使用，甚至可以用于这些系统的核心功能设计，如在政府OA办公系统中，一个批文的状态有多种：尚未办理；正在办理；正在批示；正在审核；已经完成等各种状态，而且批文状态不同时对批文的操作也有所差异。使用状态模式可以描述工作流对象（如批文）的状态转换以及不同状态下它所具有的行为。 

2. 在目前主流的RPG（Role Play Game，角色扮演游戏）中，使用状态模式可以对游戏角色进行控制，游戏角色的升级伴随着其状态的变化和行为的变化。对于游戏程序本身也可以通过状态模式进行总控，一个游戏活动包括开始、运行、结束等状态，通过对状态的控制可以控制系统的行为，决定游戏的各个方面，因此可以使用状态模式对整个游戏的架构进行设计与实现。

## 模式扩展

**共享状态**

- 在有些情况下多个环境对象需要共享同一个状态，如果希望在系统中实现多个环境对象实例共享一个或多个状态对象，那么需要将这些状态对象定义为环境的静态成员对象。

  ```java
  class Switch
  {
  	private static State state, onState, offState;
  	private String name;
  	
  	public Switch(String name)
  	{
  		this.name=name;
  		onState=new OnState();
  		offState=new OffState();
  		state=onState;
  	}
  	
  	public void setState(State state)
  	{
  		this.state=state;
  	}
  	
  	public void on()
  	{
  		System.out.print(name);
  		state.on(this);
  	}
  	
  	public void off()
  	{
  		System.out.print(name);
  		state.off(this);
  	}
  	
  	public static State getState(String type)
  	{
  		if(type.equalsIgnoreCase("on"))
  		{
  			return onState;
  		}
  		else
  		{
  			return offState;
  		}
  	}
  }
  
  abstract class State
  {
  	public abstract void on(Switch s);
  	public abstract void off(Switch s);
  }
  
  class OnState extends State
  {
  	public void on(Switch s)
  	{
  		System.out.println("已经打开！");
  	}
  	
  	public void off(Switch s)
  	{
  		System.out.println("关闭！");
  		s.setState(Switch.getState("off"));
  		
  	}
  }
  
  class OffState extends State
  {
  	public void on(Switch s)
  	{
  		System.out.println("打开！");
  		s.setState(Switch.getState("on"));
  	}
  	
  	public void off(Switch s)
  	{
  		System.out.println("已经关闭！");
  	}
  }
  
  class Client 
  {
  	public static void main(String args[])
  	{
  		Switch s1,s2;
  		s1=new Switch("开关1");
  		s2=new Switch("开关2");
  		
  		s1.on();
  		s2.on();
  		s1.off();
  		s2.off();
  		s2.on();
  		s1.on();	
  	}
  }
  ```

**简单状态模式与可切换状态的状态模式**

1. 简单状态模式：简单状态模式是指状态都相互独立，状态之间无须进行转换的状态模式，这是最简单的一种状态模式。对于这种状态模式，每个状态类都封装与状态相关的操作，而无须关心状态的切换，可以在客户端直接实例化状态类，然后将状态对象设置到环境类中。如果是这种简单的状态模式，它遵循“开闭原则”，在客户端可以针对抽象状态类进行编程，而将具体状态类写到配置文件中，同时增加新的状态类对原有系统也不造成任何影响。
2. 可切换状态的状态模式：大多数的状态模式都是可以切换状态的状态模式，在实现状态切换时，在具体状态类内部需要调用环境类Context的setState()方法进行状态的转换操作，在具体状态类中可以调用到环境类的方法，因此状态类与环境类之间通常还存在关联关系或者依赖关系。通过在状态类中引用环境类的对象来回调环境类的setState()方法实现状态的切换。在这种可以切换状态的状态模式中，增加新的状态类可能需要修改其他某些状态类甚至环境类的源代码，否则系统无法切换到新增状态。


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E7%8A%B6%E6%80%81%E6%A8%A1%E5%BC%8F/  

