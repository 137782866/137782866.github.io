# 设计模式-职责链模式


## 模式动机

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231217_050529.png)

职责链可以是一条直线、一个环或者一个树形结构，最常见的职责链是**直线型**，即沿着一条单向的链来传递请求。

链上的每一个对象都是请求处理者，职责链模式可以将请求的处理者组织成一条链，并使请求沿着链传递，由链上的处理者对请求进行相应的处理，客户端无须关心请求的处理细节以及请求的传递，只需将请求发送到链上即可，将请求的发送者和请求的处理者解耦。这就是职责链模式的模式动机。

## 模式定义

职责链模式(Chain of Responsibility Pattern)：避免请求发送者与接收者耦合在一起，让多个对象都有可能接收请求，将这些对象连接成一条链，并且沿着这条链传递请求，直到有对象处理它为止。由于英文翻译的不同，职责链模式又称为责任链模式，它是一种对象行为型模式。

## 模式结构

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231217_050546.png)

职责链模式包含如下角色：

- **Handler**: 抽象处理者
- **ConcreteHandler**: 具体处理者
- **Client**: 客户类

## 模式分析

在职责链模式里，很多对象由每一个对象对其下家的引用而连接起来形成一条链。

请求在这条链上传递，直到链上的某一个对象处理此请求为止。

发出这个请求的客户端并不知道链上的哪一个对象最终处理这个请求，这使得系统可以在不影响客户端的情况下动态地重新组织链和分配责任。

典型的抽象处理者代码：

```java
public abstract class Handler
{
	protected Handler successor;
	
	public void setSuccessor(Handler successor)
	{
		this.successor=successor;
	}
	
	public abstract void handleRequest(String request);
} 
```

典型的具体处理者代码：

```java
public class ConcreteHandler extends Handler
{
	public void handleRequest(String request)
	{
		if (请求request满足条件)
		{
			//...  //处理请求；
		}
		else
		{
			this.successor.handleRequest(request); //转发请求
		}
	}
}
```

## 模式实例

### 实例1：审批假条

某OA系统需要提供一个假条审批的模块，如果员工请假天数小于3天，主任可以审批该假条；如果员工请假天数大于等于3天，小于10天，经理可以审批；如果员工请假天数大于等于10天，小于30天，总经理可以审批；如果超过30天，总经理也不能审批，提示相应的拒绝信息。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231217_050605.png)

实例代码：

```java
public class LeaveRequest
{
	private String leaveName;
	private int leaveDays;
	
	public LeaveRequest(String leaveName,int leaveDays)
	{
		this.leaveName=leaveName;
		this.leaveDays=leaveDays;
	}
	
	public void setLeaveName(String leaveName) {
		this.leaveName = leaveName; 
	}

	public void setLeaveDays(int leaveDays) {
		this.leaveDays = leaveDays; 
	}

	public String getLeaveName() {
		return (this.leaveName); 
	}

	public int getLeaveDays() {
		return (this.leaveDays); 
	}	
}

public abstract class Leader
{
	protected String name;
	protected Leader successor;
	public Leader(String name)
	{
		this.name=name;
	}
	public void setSuccessor(Leader successor)
	{
		this.successor=successor;
	}
	public abstract void handleRequest(LeaveRequest request);
}


public class Director extends Leader
{
	public Director(String name)
	{
		super(name);
	}
	public void handleRequest(LeaveRequest request)
	{
		if(request.getLeaveDays()<3)
		{
			System.out.println("主任" + name + "审批员工" + request.getLeaveName() + "的请假条，请假天数为" + request.getLeaveDays() + "天。");
		}
		else
		{
			if(this.successor!=null)
			{
				this.successor.handleRequest(request);
			}
		}
	}
}

public class Manager extends Leader
{
	public Manager(String name)
	{
		super(name);
	}
	public void handleRequest(LeaveRequest request)
	{
		if(request.getLeaveDays()<10)
		{
			System.out.println("经理" + name + "审批员工" + request.getLeaveName() + "的请假条，请假天数为" + request.getLeaveDays() + "天。");
		}
		else
		{
			if(this.successor!=null)
			{
				this.successor.handleRequest(request);
			}
		}
	}
}

public class GeneralManager extends Leader
{
	public GeneralManager(String name)
	{
		super(name);
	}
	
	public void handleRequest(LeaveRequest request)
	{
		if(request.getLeaveDays()<30)
		{
			System.out.println("总经理" + name + "审批员工" + request.getLeaveName() + "的请假条，请假天数为" + request.getLeaveDays() + "天。");
		}
		else
		{
			System.out.println("莫非" + request.getLeaveName() + "想辞职，居然请假" + request.getLeaveDays() + "天。");
		}
	}
}

public class ViceGeneralManager extends Leader
{
	public ViceGeneralManager(String name)
	{
		super(name);
	}
	public void handleRequest(LeaveRequest request)
	{
		if(request.getLeaveDays()<20)
		{
			System.out.println("副总经理" + name + "审批员工" + request.getLeaveName() + "的请假条，请假天数为" + request.getLeaveDays() + "天。");
		}
		else
		{
			if(this.successor!=null)
			{
				this.successor.handleRequest(request);
			}
		}
	}
}

public class Client
{
	public static void main(String args[])
	{
		Leader objDirector,objManager,objGeneralManager,objViceGeneralManager;
		
		objDirector=new Director("王明");
		objManager=new Manager("赵强");
		objGeneralManager=new GeneralManager("李波");
		objViceGeneralManager=new ViceGeneralManager("肖红");
		
		objDirector.setSuccessor(objManager);
		objManager.setSuccessor(objViceGeneralManager);
		objViceGeneralManager.setSuccessor(objGeneralManager);
			
		LeaveRequest lr1=new LeaveRequest("张三",2);
		objDirector.handleRequest(lr1);
			
		LeaveRequest lr2=new LeaveRequest("李四",5);
		objDirector.handleRequest(lr2);
		
		LeaveRequest lr3=new LeaveRequest("王五",15);
		objDirector.handleRequest(lr3);
						
		LeaveRequest lr4=new LeaveRequest("赵六",45);
		objDirector.handleRequest(lr4);			
	}
}
```

## 模式优缺点

**优点**如下：

- 降低耦合度
- 可简化对象的相互连接
- 增强给对象指派职责的灵活性
- 增加新的请求处理类很方便

**缺点**如下：

- 不能保证请求一定被接收。
- 系统性能将受到一定影响，而且在进行代码调试时不太方便；可能会造成循环调用。

## 模式适应环境

在以下情况下可以使用职责链模式：

- 有多个对象可以处理同一个请求，具体哪个对象处理该请求由运行时刻自动确定。
- 在不明确指定接收者的情况下，向多个对象中的一个提交一个请求。
- 可动态指定一组对象处理请求。

## 模式应用

1. Java中的异常处理机制。

   ```java
   try
   {
   	//...
   }
   catch(ArrayIndexOutOfBoundsException e1)
   {
   	//...
   }
   catch(ArithmeticException e2)
   {
   	//...
   }
   catch(IOException e3)
   {
   	//...
   }
   finally
   {
   	//...
   }
   ```

   

2. 早期的Java AWT事件模型(JDK 1.0及更早)：事件浮升(Event Bubbling)机制。

   JavaScript事件浮升机制：

   ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231217_050623.png)

   

## 模式扩展

**纯与不纯的职责链模式**

- 一个纯的职责链模式要求一个具体处理者对象只能在两个行为中选择一个：一个是承担责任，另一个是把责任推给下家。不允许出现某一个具体处理者对象在承担了一部分责任后又将责任向下传的情况。
- 在一个纯的职责链模式里面，一个请求必须被某一个处理者对象所接收；在一个不纯的职责链模式里面，一个请求可以最终不被任何接收端对象所接收。 


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note-sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E8%81%8C%E8%B4%A3%E9%93%BE%E6%A8%A1%E5%BC%8F/  

