# 设计模式-观察者模式


## 模式动机

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_081352.png)

建立一种对象与对象之间的依赖关系，一个对象发生改变时将自动通知其他对象，其他对象将相应做出反应。在此，发生改变的对象称为观察目标，而被通知的对象称为观察者，一个观察目标可以对应多个观察者，而且这些观察者之间没有相互联系，可以根据需要增加和删除观察者，使得系统更易于扩展，这就是观察者模式的模式动机。

## 模式定义

观察者模式(Observer Pattern)：定义对象间的一种一对多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新。观察者模式又叫做发布-订阅（Publish/Subscribe）模式、模型-视图（Model/View）模式、源-监听器（Source/Listener）模式或从属者（Dependents）模式。观察者模式是一种对象行为型模式。

## 模式结构

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_081408.png)

观察者模式包含如下角色：

- **Subject**: 目标
- **ConcreteSubject**: 具体目标
- **Observer**: 观察者
- **ConcreteObserver**: 具体观察者

## 模式分析

观察者模式描述了如何建立对象与对象之间的依赖关系，如何构造满足这种需求的系统。

这一模式中的关键对象是观察目标和观察者，一个目标可以有任意数目的与之相依赖的观察者，一旦目标的状态发生改变，所有的观察者都将得到通知。

作为对这个通知的响应，每个观察者都将即时更新自己的状态，以与目标状态同步，这种交互也称为发布-订阅(publish-subscribe)。目标是通知的发布者，它发出通知时并不需要知道谁是它的观察者，可以有任意数目的观察者订阅它并接收通知。

典型的抽象目标类代码如下所示：

```java
import java.util.*;
public abstract class Subject
{
            protected ArrayList observers = new ArrayList();
	public abstract void attach(Observer observer);
	public abstract void detach(Observer observer);
	public abstract void notify();
} 
```

典型的具体目标类代码如下所示：

```java
public class ConcreteSubject extends Subject
{
	public void attach(Observer observer)
	{
		observers.add(observer);
	}
	
	public void detach(Observer observer)
	{
		observers.remove(observer);
	}
	
	public void notify()
	{
		for(Object obs:observers)
		{
			((Observer)obs).update();
		}
	}	
} 
```

典型的抽象观察者代码如下所示：

```java
public interface Observer
{
	public void update();
}
```

典型的具体观察者代码如下所示：

```java
public class ConcreteObserver implements Observer
{
	public void update()
	{
		//具体更新代码
	}
}
```

客户端代码片段如下所示：

```java
Subject subject = new ConcreteSubject();
Observer observer = new ConcreteObserver();
subject.attach(observer);
subject.notify(); 
```

观察者模式顺序图如下所示：

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_081423.png)

## 模式实例

### 实例1：猫、狗与老鼠

假设猫是老鼠和狗的观察目标，老鼠和狗是观察者，猫叫老鼠跑，狗也跟着叫，使用观察者模式描述该过程。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_081442.png)

实例代码：

```java
import java.util.*;

public abstract class MySubject
{
	protected ArrayList observers = new ArrayList();
	
	//注册方法
	public void attach(MyObserver observer)
	{
		observers.add(observer);
	} 
	
	//注销方法
	public void detach(MyObserver observer)
	{
		observers.remove(observer);
	}
	
	public abstract void cry(); //抽象通知方法
}

public class Cat extends MySubject
{
	public void cry()
	{
		System.out.println("猫叫?");
		System.out.println("----------------------------");		
		
		for(Object obs:observers)
		{
			((MyObserver)obs).response();
		}
		
	}	   	
}

public interface MyObserver
{
	void response();  //抽象响应方法
}

public class Mouse implements MyObserver
{
	public void response()
	{
		System.out.println("老鼠努力逃跑！");
	}
}

public class Dog implements MyObserver
{
	public void response()
	{
		System.out.println("狗跟着叫！");
	}	
}

public class Pig implements MyObserver
{
	public void response()
	{
		System.out.println("猪没有反应！");
	}	
}

public class Client
{
	public static void main(String a[])
	{
		MySubject subject=new Cat();
		
		MyObserver obs1,obs2,obs3;
		obs1=new Mouse();
		obs2=new Mouse();
		obs3=new Dog();
		
		subject.attach(obs1);
		subject.attach(obs2);
		subject.attach(obs3);
		
		MyObserver obs4;
		obs4=new Pig();
		subject.attach(obs4);
		
		subject.cry();		
	}
}
```

### 实例2：自定义登录控件

Java事件处理模型中应用了观察者模式，下面通过一个实例来学习如何自定义Java控件，并给该控件增加相应的事件。该实例基于Java Swing/AWT控件，在Swing/AWT的相关类中封装了对事件的底层处理。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_081501.png)

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_081518.png)

实例代码：

```java
import javax.swing.*;
import java.awt.event.*;
import java.awt.*;
import java.util.EventObject;

//Concrete Subject
public class LoginBean extends JPanel implements ActionListener
{
	private JLabel labUserName,labPassword;
	private JTextField txtUserName;
	private JPasswordField txtPassword;
	private JButton btnLogin,btnClear;
	
	private LoginEventListener lel;  //Abstract Observer
	
	private LoginEvent le;
	
	public LoginBean()
	{
		this.setLayout(new GridLayout(3,2));
		labUserName=new JLabel("User Name:");
		add(labUserName);
		
		txtUserName=new JTextField(20);
		add(txtUserName);
		
		labPassword=new JLabel("Password:");
		add(labPassword);
		
		txtPassword=new JPasswordField(20);
		add(txtPassword);
		
		btnLogin=new JButton("Login");
		add(btnLogin);
		
		btnClear=new JButton("Clear");
		add(btnClear);
		
		btnClear.addActionListener(this);
		btnLogin.addActionListener(this);//As a concrete observer for another subject,ActionListener as the abstract observer.
	}
	
	//Add an observer.
	public void addLoginEventListener(LoginEventListener lel)
	{
		this.lel=lel;
	}
	
	//private or protected as the notify method
	private void fireLoginEvent(Object object,String userName,String password)
	{
		le=new LoginEvent(btnLogin,userName,password);
		lel.validateLogin(le);
	}
	
	public void actionPerformed(ActionEvent event)
	{
		if(btnLogin==event.getSource())
		{
			String userName=this.txtUserName.getText();
			String password=this.txtPassword.getText();
			
			fireLoginEvent(btnLogin,userName,password);
		}
		if(btnClear==event.getSource())
		{
			this.txtUserName.setText("");			
			this.txtPassword.setText("");
		}
	}
}

public class LoginEvent extends EventObject
{
	private String userName;
	private String password;
	public LoginEvent(Object source,String userName,String password)
	{
		super(source);
		this.userName=userName;
		this.password=password;
	}
	public void setUserName(String userName)
	{
		this.userName=userName;
	}
	public String getUserName()
	{
		return this.userName;
	}
	public void setPassword(String password)
	{
		this.password=password;
	}
	public String getPassword()
	{
		return this.password;
	}
}

import java.util.EventListener;

//Abstract Observer
public interface LoginEventListener extends EventListener
{
	public void validateLogin(LoginEvent event);
}

//Concrete Observer
public class LoginValidatorA extends JFrame implements LoginEventListener
{
	private JPanel p;
	private LoginBean lb;
	private JLabel lblLogo;
	public LoginValidatorA()
	{
		super("Bank of China");
		p=new JPanel();
		this.getContentPane().add(p);
		lb=new LoginBean();
		lb.addLoginEventListener(this);
		
		Font f=new Font("Times New Roman",Font.BOLD,30);
		lblLogo=new JLabel("Bank of China");
		lblLogo.setFont(f);
		lblLogo.setForeground(Color.red);
		
		p.setLayout(new GridLayout(2,1));
		p.add(lblLogo);
		p.add(lb);
		p.setBackground(Color.pink);
		this.setSize(600,200);
		this.setVisible(true);
	}
	public void validateLogin(LoginEvent event)
	{
		String userName=event.getUserName();
		String password=event.getPassword();
		
		if(0==userName.trim().length()||0==password.trim().length())
		{
			JOptionPane.showMessageDialog(this,new String("Username or Password is empty!"),"alert",JOptionPane.ERROR_MESSAGE);
		}
		else
		{
			JOptionPane.showMessageDialog(this,new String("Valid Login Info!"),"alert",JOptionPane.INFORMATION_MESSAGE);			
		}
	}
	public static void main(String args[])
	{
		new LoginValidatorA().setVisible(true);
	}
}

public class LoginValidatorB extends JFrame implements LoginEventListener
{
	private JPanel p;
	private LoginBean lb;
	private JLabel lblLogo;
	public LoginValidatorB()
	{
		super("China Mobile");
		p=new JPanel();
		this.getContentPane().add(p);
		lb=new LoginBean();
		lb.addLoginEventListener(this);
		
		Font f=new Font("Times New Roman",Font.BOLD,30);
		lblLogo=new JLabel("China Mobile");
		lblLogo.setFont(f);
		lblLogo.setForeground(Color.blue);
		
		p.setLayout(new GridLayout(2,1));
		p.add(lblLogo);
		p.add(lb);
		p.setBackground(new Color(163,185,255));
		this.setSize(600,200);
		this.setVisible(true);
	}
	public void validateLogin(LoginEvent event)
	{
		String userName=event.getUserName();
		String password=event.getPassword();
		
		if(userName.equals(password))
		{
			JOptionPane.showMessageDialog(this,new String("Username must be different from password!"),"alert",JOptionPane.ERROR_MESSAGE);
		}
		else
		{
			JOptionPane.showMessageDialog(this,new String("Rigth details!"),"alert",JOptionPane.INFORMATION_MESSAGE);			
		}
	}
	public static void main(String args[])
	{
		new LoginValidatorB().setVisible(true);
	}
}
```

## 模式优缺点

**优点**如下：

- 观察者模式可以实现表示层和数据逻辑层的分离，并定义了稳定的消息更新传递机制，抽象了更新接口，使得可以有各种各样不同的表示层作为具体观察者角色。
- 观察者模式在观察目标和观察者之间建立一个抽象的耦合。
- 观察者模式支持广播通信。
- 观察者模式符合“开闭原则”的要求。

**缺点**如下：

- 如果一个观察目标对象有很多直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。
- 如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。
- 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。

## 模式适应环境

在以下情况下可以使用观察者模式：

- 一个抽象模型有两个方面，其中一个方面依赖于另一个方面。将这些方面封装在独立的对象中使它们可以各自独立地改变和复用。
- 一个对象的改变将导致其他一个或多个对象也发生改变，而不知道具体有多少对象将发生改变，可以降低对象之间的耦合度。
- 一个对象必须通知其他对象，而并不知道这些对象是谁。
- 需要在系统中创建一个触发链，A对象的行为将影响B对象，B对象的行为将影响C对象……，可以使用观察者模式创建一种链式触发机制。

## 模式应用

1. DK1.1 版本及以后的各个版本中，事件处理模型采用基于观察者模式的委派事件模型(Delegation Event Model, DEM)。在DEM中，事件的发布者称为事件源(Event Source)，而订阅者叫做事件监听器(Event Listener)，在这个过程中还可以通过事件对象(Event Object)来传递与事件相关的信息，可以在事件监听者的实现类中实现事件处理，因此事件监听对象又可以称为事件处理对象。

   事件源对象、事件监听对象（事件处理对象）和事件对象构成了Java事件处理模型的三要素。

2. 除了AWT中的事件处理之外，Java语言解析XML的技术SAX2以及Servlet技术的事件处理机制都基于DEM，它们都是观察者模式的应用。
3. 观察者模式在软件开发中应用非常广泛，如某电子商务网站可以在执行发送操作后给用户多个发送商品打折信息，某团队战斗游戏中某队友牺牲将给所有成员提示等等，凡是涉及到一对一或者一对多的对象交互场景都可以使用观察者模式。

Java语言提供的对观察者模式的支持：

在JDK的java.util包中，提供了Observable类以及Observer接口，它们构成了Java语言对观察者模式的支持。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_081537.png)



Java语言提供的对观察者模式的支持Observer接口：

Observer接口

```
void update(Observable o, Object arg);
```

Observable类

```
Observable()
addObserver(Observer o) 
deleteObserver (Observer o) 
notifyObservers()、notifyObservers(Object arg) 
deleteObservers 
setChanged() 
clearChanged() 
hasChanged() 
countObservers()
```

## 模式拓展

**MVC模式**

- MVC模式是一种架构模式，它含三个角色：模型(Model)，视图(View)和控制器(Controller)。观察者模式可以用来实现MVC模式，观察者模式中的观察目标就是MVC模式中的模型(Model)，而观察者就是MVC中的视图(View)，控制器(Controller)充当两者之间的中介者(Mediator)。当模型层的数据发生改变时，视图层将自动改变其显示内容。

  ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231224_081552.png)


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F/  

