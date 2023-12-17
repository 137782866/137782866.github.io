# 设计模式-命令模式


## 模式动机

在软件设计中，我们经常需要向某些对象发送请求，但是并不知道请求的接收者是谁，也不知道被请求的操作是哪个，我们只需在程序运行时指定具体的请求接收者即可，此时，可以使用命令模式来进行设计，使得请求发送者与请求接收者消除彼此之间的耦合，让对象之间的调用关系更加灵活。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231218_011815.png)

命令模式可以对发送者和接收者完全解耦，发送者与接收者之间没有直接引用关系，发送请求的对象只需要知道如何发送请求，而不必知道如何完成请求。这就是命令模式的模式动机。

## 模式定义

命令模式(Command Pattern)：将一个请求封装为一个对象，从而使我们可用不同的请求对客户进行参数化；对请求排队或者记录请求日志，以及支持可撤销的操作。命令模式是一种对象行为型模式，其别名为动作(Action)模式或事务(Transaction)模式。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231218_011831.png)

命令模式包含如下角色：

- **Command**: 抽象命令类
- **ConcreteCommand**: 具体命令类
- **Invoker**: 调用者
- **Receiver**: 接收者
- **Client**:客户类

## 模式分析

命令模式的本质是对命令进行封装，将发出命令的责任和执行命令的责任分割开。

每一个命令都是一个操作：请求的一方发出请求，要求执行一个操作；接收的一方收到请求，并执行操作。

命令模式允许请求的一方和接收的一方独立开来，使得请求的一方不必知道接收请求的一方的接口，更不必知道请求是怎么被接收，以及操作是否被执行、何时被执行，以及是怎么被执行的。

命令模式使请求本身成为一个对象，这个对象和其他对象一样可以被存储和传递。

命令模式的关键在于引入了抽象命令接口，且发送者针对抽象命令接口编程，只有实现了抽象命令接口的具体命令才能与接收者相关联。

典型的抽象命令类代码：

```java
public abstract class Command
{
	public abstract void execute();
} 
```

典型的调用者代码：

```java
public class Invoker
{
	private Command command;
	
	public Invoker(Command command)
	{
		this.command=command;
	}
	
	public void setCommand(Command command)
	{
		this.command=command;
	}
	
	//业务方法，用于调用命令类的方法
	public void call()
	{
		command.execute();
	}
}
```

典型的具体命令类代码：

```java
public class ConcreteCommand extends Command
{
	private Receiver receiver;
	public void execute()
	{
		receiver.action();
	}
}
```

典型的请求接收者代码：

```java
public class Receiver
{
	public void action()
	{
		//具体操作
	}
}
```

命令模式顺序图 ：

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231218_011849.png)

## 模式实例

### 实例1：电视机遥控器

电视机是请求的接收者，遥控器是请求的发送者，遥控器上有一些按钮，不同的按钮对应电视机的不同操作。抽象命令角色由一个命令接口来扮演，有三个具体的命令类实现了抽象命令接口，这三个具体命令类分别代表三种操作：打开电视机、关闭电视机和切换频道。显然，电视机遥控器就是一个典型的命令模式应用实例。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231218_011904.png)

代码实例：

```java
public interface AbstractCommand
{
	public void execute();
}

public class TVOpenCommand implements AbstractCommand
{
	private Television tv;
	public TVOpenCommand()
	{
		tv = new Television();
	}
	public void execute()
	{
		tv.open();
	}
}

public class TVCloseCommand implements AbstractCommand
{
	private Television tv;
	public TVCloseCommand()
	{
		tv = new Television();
	}
	public void execute()
	{
		tv.close();
	}
}

public class TVChangeCommand implements AbstractCommand
{
	private Television tv;
	public TVChangeCommand()
	{
		tv = new Television();
	}
	public void execute()
	{
		tv.changeChannel();
	}
}

public class Controller
{
	private AbstractCommand openCommand, closeCommand, changeCommand;
	
	public Controller(AbstractCommand openCommand,AbstractCommand closeCommand,AbstractCommand changeCommand)
	{
		this.openCommand=openCommand;
		this.closeCommand=closeCommand;
		this.changeCommand=changeCommand;
	}
	
	public void open()
	{
		openCommand.execute();
	}
	
	public void change()
	{
		changeCommand.execute();
	}	

	public void close()
	{
		 closeCommand.execute();	
	}
}

public class Television
{
	public void open()
	{
		System.out.println("打开电视机！");
	}
	
	public void close()
	{
		System.out.println("关闭电视机！");		
	}
	
	public void changeChannel()
	{
		System.out.println("切换电视频道！");
	}
}

public class Client
{
	public static void main(String a[])
	{
		AbstractCommand openCommand,closeCommand,changeCommand;
		
		openCommand = new TVOpenCommand();
		closeCommand = new TVCloseCommand();
		changeCommand = new TVChangeCommand();
		
		Controller control = new Controller(openCommand,closeCommand,changeCommand);
		
		control.open();
		control.change();
		control.close();
	}
}
```

### 实例2：功能键设置

为了用户使用方便，某系统提供了一系列功能键，用户可以自定义功能键的功能，如功能键FunctionButton可以用于退出系统(SystemExitClass)，也可以用于打开帮助界面(DisplayHelpClass)。用户可以通过修改配置文件来改变功能键的用途，现使用命令模式来设计该系统，使得功能键类与功能类之间解耦，相同的功能键可以对应不同的功能。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231218_011921.png)

## 模式优缺点

**优点**如下：

- 降低系统的耦合度。
- 新的命令可以很容易地加入到系统中。
- 可以比较容易地设计一个命令队列和宏命令（组合命令）。
- 可以方便地实现对请求的Undo和Redo。

**缺点**如下：

- 使用命令模式可能会导致某些系统有过多的具体命令类。因为针对每一个命令都需要设计一个具体命令类，因此某些系统可能需要大量具体命令类，这将影响命令模式的使用。

## 模式适应环境

**在以下情况下可以使用命令模式：**

- 系统需要将请求调用者和请求接收者解耦，使得调用者和接收者不直接交互。
- 系统需要在不同的时间指定请求、将请求排队和执行请求。
- 系统需要支持命令的撤销(Undo)操作和恢复(Redo)操作。
- 系统需要将一组操作组合在一起，即支持宏命令。

## 模式应用

1. Java语言使用命令模式实现AWT/Swing GUI的委派事件模型 (Delegation Event Model, DEM) 。

   在AWT/Swing中，Frame、Button等界面组件是请求发送者，而AWT提供的事件监听器接口和事件适配器类是抽象命令接口，用户可以自己写抽象命令接口的子类来实现事件处理，即实现具体命令类，而在具体命令类中可以调用业务处理方法来实现该事件的处理。对于界面组件而言，只需要了解命令接口即可，无须关心接口的实现，组件类并不关心实际操作，而操作由用户来实现。

2. 很多系统都提供了宏命令功能，如UNIX平台下的Shell编程，可以将多条命令封装在一个命令对象中，只需要一条简单的命令即可执行一个命令序列，这也是命令模式的应用实例之一。

## 模式扩展

**撤销操作的实现**

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231218_011940.png)



```java
abstract class AbstractCommand
{
	public abstract int execute(int value);
	public abstract int undo();
}

class ConcreteCommand extends AbstractCommand
{
	private Adder adder = new Adder();
	private int value;
		
	public int execute(int value)
	{
		this.value=value;
		return adder.add(value);
	}
	
	public int undo()
	{
		return adder.add(-value);
	}
}

class CalculatorForm
{
	private AbstractCommand command;
	
	public void setCommand(AbstractCommand command)
	{
		this.command=command;
	}
	
	public void compute(int value)
	{
		int i = command.execute(value);
		System.out.println("执行运算，运算结果为：" + i);
	}
	
	public void undo()
	{
		int i = command.undo();
		System.out.println("执行撤销，运算结果为：" + i);
	}
}

class Adder
{
	private int num=0;
	
	public int add(int value)
	{
		num+=value;
		return num;
	}
}

class Client
{
	public static void main(String args[])
	{
		CalculatorForm form = new CalculatorForm();
		ConcreteCommand command = new ConcreteCommand();
		form.setCommand(command);
		
		form.compute(10);
		form.compute(5);
		form.compute(10);
		form.undo();
	}
}
```

**宏命令**

宏命令又称为组合命令，它是命令模式和组合模式联用的产物。

宏命令也是一个具体命令，不过它包含了对其他命令对象的引用，在调用宏命令的execute()方法时，将递归调用它所包含的每个成员命令的execute()方法，一个宏命令的成员对象可以是简单命令，还可以继续是宏命令。执行一个宏命令将执行多个具体命令，从而实现对命令的批处理。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231218_011958.png)



---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note-sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E5%91%BD%E4%BB%A4%E6%A8%A1%E5%BC%8F/  

