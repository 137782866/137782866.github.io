# 设计模式-备忘录模式


## 模式动机

为了使软件的使用更加人性化，对于误操作，我们需要提供一种类似“后悔药”的机制，让软件系统可以回到误操作前的状态，因此需要保存用户每一次操作时系统的状态，一旦出现误操作，可以把存储的历史状态取出即可回到之前的状态。

现在大多数软件都有撤销(Undo)的功能，快捷键一般都是Ctrl+Z，目的就是为了解决这个后悔的问题。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_021205.png)

在应用软件的开发过程中，很多时候我们都需要记录一个对象的内部状态。

在具体实现过程中，为了允许用户取消不确定的操作或从错误中恢复过来，需要实现备份点和撤销机制，而要实现这些机制，必须事先将状态信息保存在某处，这样才能将对象恢复到它们原先的状态。

备忘录模式是一种给我们的软件提供后悔药的机制，通过它可以使系统恢复到某一特定的历史状态。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_021219.png)

## 模式定义

备忘录模式(Memento Pattern)：在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样可以在以后将对象恢复到原先保存的状态。它是一种对象行为型模式，其别名为Token。

## 模式结构

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_021235.png)

备忘录模式包含如下角色：

- Originator: 原发器
- Memento: 备忘录
- Caretaker: 负责人

## 模式分析

由于在备忘录中存储的是原发器的中间状态，因此需要防止原发器以外的其他对象访问备忘录。

备忘录对象通常封装了原发器的部分或所有的状态信息，而且这些状态不能被其他对象访问，也就是说不能在备忘录对象之外保存原发器状态，因为暴露其内部状态将违反封装的原则，可能有损系统的可靠性和可扩展性。

为了实现对备忘录对象的封装，需要对备忘录的调用进行控制:

- 对于原发器而言，它可以调用备忘录的所有信息，允许原发器访问返回到先前状态所需的所有数据；
- 对于负责人而言，只负责备忘录的保存并将备忘录传递给其他对象；
- 对于其他对象而言，只需要从负责人处取出备忘录对象并将原发器对象的状态恢复，而无须关心备忘录的保存细节。

理想的情况是只允许生成该备忘录的那个原发器访问备忘录的内部状态。

典型的负责人类代码如下所示：

```java
package dp.memento;
public class Caretaker
{
	private Memento memento;
	public Memento getMemento()
	{
		return memento;
	}
	public void setMemento(Memento memento)
	{
		this.memento=memento;
	}
}
```

备忘录模式属于对象行为型模式，负责人向原发器请求一个备忘录，保留一段时间后，再根据需要将其送回给原发器，其顺序图如下所示：

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_021250.png)

## 模式实例

### 实例1：用户信息操作撤销

某系统提供了用户信息操作模块，用户可以修改自己的各项信息。为了使操作过程更加人性化，现使用备忘录模式对系统进行改进，使得用户在进行了错误操作之后可以恢复到操作之前的状态。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_021305.png)

```java
package dp.memento;

public class Caretaker
{
	private Memento memento;
	public Memento getMemento()
	{
		return memento;
	}
	public void setMemento(Memento memento)
	{
		this.memento=memento;
	}
}

class Memento
{
	private String account;
	private String password;
	private String telNo;
	
	public Memento(String account,String password,String telNo)
    {
    	this.account=account;
    	this.password=password;
    	this.telNo=telNo;
    }
	public String getAccount()
	{
		return account;
	}
	
	public void setAccount(String account)
	{
		this.account=account;
	}

	public String getPassword()
	{
		return password;
	}
	
	public void setPassword(String password)
	{
		this.password=password;
	}
	
	public String getTelNo()
	{
		return telNo;
	}
		
	public void setTelNo(String telNo)
	{
		this.telNo=telNo;
	}
}

public class UserInfoDTO
{
	private String account;
	private String password;
	private String telNo;
	
	public String getAccount()
	{
		return account;
	}
	
	public void setAccount(String account)
	{
		this.account=account;
	}

	public String getPassword()
	{
		return password;
	}
	
	public void setPassword(String password)
	{
		this.password=password;
	}
	
	public String getTelNo()
	{
		return telNo;
	}
	
	public void setTelNo(String telNo)
	{
		this.telNo=telNo;
	}
		
	public Memento saveMemento()
	{
		return new Memento(account,password,telNo);
	}
	
	public void restoreMemento(Memento memento)
	{
		this.account=memento.getAccount();
		this.password=memento.getPassword();
		this.telNo=memento.getTelNo();
	}
	
	public void show()
	{
		System.out.println("Account:" + this.account);
		System.out.println("Password:" + this.password);
		System.out.println("TelNo:" + this.telNo);		
	}
}

public class Client
{
	public static void main(String a[])
	{
	UserInfoDTO user=new UserInfoDTO();
	Caretaker c=new Caretaker();
	
	user.setAccount("zhangsan");
	user.setPassword("123456");
	user.setTelNo("13000000000");
	System.out.println("状态一：");
	user.show();
	c.setMemento(user.saveMemento());//保存备忘录
	System.out.println("---------------------------");

	user.setPassword("111111");
	user.setTelNo("13100001111");
	System.out.println("状态二：");	
	user.show();
	System.out.println("---------------------------");
		
	user.restoreMemento(c.getMemento());//从备忘录中恢复
	System.out.println("回到状态一：");
	user.show();
    System.out.println("---------------------------");
	}
}


```

## 模式优缺点

**优点**如下：

- 提供了一种状态恢复的实现机制，使得用户可以方便地回到一个特定的历史步骤，当新的状态无效或者存在问题时，可以使用先前存储起来的备忘录将状态复原。
- 实现了信息的封装，一个备忘录对象是一种原发器对象的表示，不会被其他代码改动，这种模式简化了原发器对象，备忘录只保存原发器的状态，采用堆栈来存储备忘录对象可以实现多次撤销操作，可以通过在负责人中定义集合对象来存储多个备忘录。

**缺点**如下：

- 资源消耗过大，如果类的成员变量太多，就不可避免占用大量的内存，而且每保存一次对象的状态都需要消耗内存资源，如果知道这一点大家就容易理解为什么一些提供了撤销功能的软件在运行时所需的内存和硬盘空间比较大了。

## 模式适应环境

在以下情况下可以使用备忘录模式：

- 保存一个对象在某一个时刻的状态或部分状态，这样以后需要时它能够恢复到先前的状态。
- 如果用一个接口来让其他对象得到这些状态，将会暴露对象的实现细节并破坏对象的封装性，一个对象不希望外界直接访问其内部状态，通过负责人可以间接访问其内部状态。

## 模式应用

1. 几乎所有的文字或者图像编辑软件都提供了撤销(Ctrl+Z)的功能，即撤销操作，但是当软件关闭再打开时不能再进行撤销操作，也就是说不能再回到关闭软件前的状态，实际上这中间就使用到了备忘录模式，在编辑文件的同时可以保存一些内部状态，这些状态在软件关闭时从内存销毁，当然这些状态的保存也不是无限的，很多软件只提供有限次的撤销操作。

2. 数据库管理系统DBMS所提供的事务管理应用了备忘录模式，当数据库某事务中一条数据操作语句执行失败时，整个事务将进行回滚操作，系统回到事务执行之前的状态。

## 模式扩展

**备忘录的封装性**

- 为了确保备忘录的封装性，除了原发器外，其他类是不能也不应该访问备忘录类的，在实际开发中，原发器与备忘录之间的关系是非常特殊的，它们要分享信息而不让其他类知道，实现的方法因编程语言的不同而不同。
- C++可以用friend关键字，使原发器类和备忘录类成为友元类，互相之间可以访问对象的一些私有的属性；
- 在Java语言中可以将两个类放在一个包中，使它们之间满足默认的包内可见性，也可以将备忘录类作为原发器类的内部类，使得只有原发器才可以访问备忘录中的数据，其他对象都无法使用备忘录中的数据。

**多备份实现**

- 在负责人中定义一个集合对象来存储多个状态，而且可以方便地返回到某一历史状态。
- 在备份对象时可以做一些记号，这些记号称为检查点(Check Point)。在使用HashMap等实现时可以使用Key来设置检查点。


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E5%A4%87%E5%BF%98%E5%BD%95%E6%A8%A1%E5%BC%8F/  

