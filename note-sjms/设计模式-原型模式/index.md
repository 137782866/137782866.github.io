# 设计模式-原型模式


## 模式动机

在面向对象系统中，使用原型模式来复制一个对象自身，从而克隆出多个与原型对象一模一样的对象。

在软件系统中，有些对象的创建过程较为复杂，而且有时候需要频繁创建，原型模式通过给出一个原型对象来指明所要创建的对象的类型，然后用复制这个原型对象的办法创建出更多同类型的对象，这就是原型模式的意图所在。

## 模式定义

原型模式(Prototype Pattern)：原型模式是一种对象创建型模式，用原型实例指定创建对象的种类，并且通过复制这些原型创建新的对象。原型模式允许一个对象再创建另外一个可定制的对象，无须知道任何创建的细节。

原型模式的基本工作原理是通过将一个原型对象传给那个要发动创建的对象，这个要发动创建的对象通过请求原型对象拷贝原型自己来实现创建过程。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231214_010122.png)

## 模式结构

原型模式包含如下角色：

- **Prototype**：抽象原型类
- **ConcretePrototype**：具体原型类
- **Client**：客户类

## 模式分析

在原型模式结构中定义了一个抽象原型类，所有的Java类都继承自java.lang.Object，而Object类提供一个clone()方法，可以将一个Java对象复制一份。因此在Java中可以直接使用Object提供的clone()方法来实现对象的克隆，Java语言中的原型模式实现很简单。

能够实现克隆的Java类必须实现一个标识接口Cloneable，表示这个Java类支持复制。如果一个类没有实现这个接口但是调用了clone()方法，Java编译器将抛出一个CloneNotSupportedException异常。

示例代码：

```java
public class PrototypeDemo implements Cloneable
{
   //...
　　public Object clone()
　　{
　　　　Object object = null;
　　　　try {
　　　　　　object = super.clone();
　　　　} catch (CloneNotSupportedException exception) {
　　　　　　System.err.println("Not support cloneable");
　　　　}
　　　　return object;
    }
    //...
}
```

通常情况下，一个类包含一些成员对象，在使用原型模式克隆对象时，根据其成员对象是否也克隆，原型模式可以分为两种形式：深克隆和浅克隆。

### 深克隆与浅克隆

浅克隆不会克隆原对象中的引用类型，仅仅拷贝了引用类型的指向。深克隆则拷贝了所有。也就是说深克隆能够做到原对象和新对象之间完全没有影响。而深克隆的实现就是在引用类型所在的类实现Cloneable接口，并使用public访问修饰符重写clone方法。

- Java语言提供的clone()方法将对象复制了一份并返回给调用者。一般而言，clone()方法满足：
  1. 对任何的对象x，都有x.clone() !=x，即克隆对象与原对象不是同一个对象。
  2. 对任何的对象x，都有x.clone().getClass()==x.getClass()，即克隆对象与原对象的类型一样。
  3. 如果对象x的equals()方法定义恰当，那么x.clone().equals(x)应该成立。

 • Java语言也可以使用对象序列化实现深克隆，将对象写入到流中，这样对象的内容就变成了字节流，也就不存在什么引用了。然后读取字节流反序列化为对象就完成了完全的复制操作。

### ==与equals

1. 对于 ==，如果作用于基本数据类型的变量，则直接比较其存储的“值”是否相等；如果作用于引用类型的变量，则比较的是所指向的对象的地址。
2. 对于equals方法，注意：equals方法不能作用于基本数据类型的变量；如果没有对equals方法进行重写，则比较的是引用类型的变量所指向的对象的地址；诸如String、Date等类对equals方法进行了重写的话，比较的是所指向的对象的内容。

## 模式实例

### 实例1：邮件复制（浅克隆）

由于邮件对象包含的内容较多（如发送者、接收者、标题、内容、日期、附件等），某系统中现需要提供一个邮件复制功能，对于已经创建好的邮件对象，可以通过复制的方式创建一个新的邮件对象，如果需要改变某部分内容，无须修改原始的邮件对象，只需要修改复制后得到的邮件对象即可。使用原型模式设计该系统。在本实例中使用浅克隆实现邮件复制，即复制邮件(Email)的同时不复制附件(Attachment)。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231214_010139.png)

```java
public class Email implements Cloneable
{
    private Attachment attachment=null;

    public Email()
    {
        this.attachment=new Attachment();
    }

    public Object clone()
    {
        Email clone=null;
        try
        {
            clone=(Email)super.clone();
        }
        catch(CloneNotSupportedException e)
        {
            System.out.println("Clone failure!");
        }
        return clone;
    }

    public Attachment getAttachment()
    {
        return this.attachment;
    }

    public void display()
    {
        System.out.println("查看邮件");
    }

}

public class Attachment
{
    public void download()
    {
        System.out.println("下载附件");
    }
}

public class Client
{
    public static void main(String a[])
    {
        Email email,copyEmail;

        email=new Email();

        copyEmail=(Email)email.clone();

        System.out.println("email==copyEmail?");
        System.out.println(email==copyEmail);

        System.out.println("email.getAttachment==copyEmail.getAttachment?");
        System.out.println(email.getAttachment()==copyEmail.getAttachment());
    }
}
```

### 实例2：邮件复制（深克隆）

使用深克隆(对象序列化)实现邮件复制，即复制邮件的同时复制附件。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231214_010200.png)

实例代码：

```java
import java.io.*;

public class Email implements Serializable
{
    private Attachment attachment=null;

    public Email()
    {
        this.attachment=new Attachment();
    }

    public Object deepClone() throws IOException, ClassNotFoundException, OptionalDataException
    {
        //将对象写入流中
        ByteArrayOutputStream bao=new ByteArrayOutputStream();
        ObjectOutputStream oos=new ObjectOutputStream(bao);
        oos.writeObject(this);

        //将对象从流中取出
        ByteArrayInputStream bis=new ByteArrayInputStream(bao.toByteArray());
        ObjectInputStream ois=new ObjectInputStream(bis);
        return(ois.readObject());
    }

    public Attachment getAttachment()
    {
        return this.attachment;
    }

    public void display()
    {
        System.out.println("查看邮件");
    }

}

import java.io.*;

public class Attachment implements Serializable
{
    public void download()
    {
        System.out.println("下载附件");
    }
}

public class Client
{
    public static void main(String a[])
    {
        Email email,copyEmail=null;

        email=new Email();

        try{
            copyEmail=(Email)email.deepClone();
        }
        catch(Exception e)
        {
               e.printStackTrace();
        }


        System.out.println("email==copyEmail?");
        System.out.println(email==copyEmail);

        System.out.println("email.getAttachment==copyEmail.getAttachment?");
        System.out.println(email.getAttachment()==copyEmail.getAttachment());
    }
}
```

## 模式优缺点

**优点**

- 当创建新的对象实例较为复杂时，使用原型模式可以简化对象的创建过程，通过一个已有实例可以提高新实例的创建效率。
- 可以动态增加或减少产品类。
- 原型模式提供了简化的创建结构。
- 可以使用深克隆的方式保存对象的状态。

**缺点**

- 需要为每一个类配备一个克隆方法，而且这个克隆方法需要对类的功能进行通盘考虑，这对全新的类来说不是很难，但对已有的类进行改造时，不一定是件容易的事，必须修改其源代码，违背了“开闭原则”。
- 在实现深克隆时需要编写较为复杂的代码。

## 模式适应环境

在以下情况下可以使用原型模式：

- 创建新对象成本较大，新的对象可以通过原型模式对已有对象进行复制来获得，如果是相似对象，则可以对其属性稍作修改。
- 如果系统要保存对象的状态，而对象的状态变化很小，或者对象本身占内存不大的时候，也可以使用原型模式配合备忘录模式来应用。相反，如果对象的状态变化很大，或者对象占用的内存很大，那么采用状态模式会比原型模式更好。
- 需要避免使用分层次的工厂类来创建分层次的对象，并且类的实例对象只有一个或很少的几个组合状态，通过复制原型对象得到新实例可能比使用构造函数创建一个新实例更加方便。

## 模式应用

1. 原型模式应用于很多软件中，如果每次创建一个对象要花大量时间，原型模式是最好的解决方案。很多软件提供的复制(Ctrl + C)和粘贴(Ctrl + V)操作就是原型模式的应用，复制得到的对象与原型对象是两个类型相同但内存地址不同的对象，通过原型模式可以大大提高对象的创建效率。
2. 在Struts2中为了保证线程的安全性，Action对象的创建使用了原型模式，访问一个已经存在的Action对象时将通过克隆的方式创建出一个新的对象，从而保证其中定义的变量无须进行加锁实现同步，每一个Action中都有自己的成员变量，避免Struts1因使用单例模式而导致的并发和同步问题。
3. 在Spring中，用户也可以采用原型模式来创建新的bean实例，从而实现每次获取的是通过克隆生成的新实例，对其进行修改时对原有实例对象不造成任何影响。

## 模式扩展

**带原型管理器的原型模式**

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231214_010219.png)

实现代码：

```java
import java.util.*;

interface MyColor extends Cloneable
{
    public Object clone();
    public void display();
}

class Red implements MyColor
{
   public Object clone()
   {
     Red r=null;
     try
     {
       r=(Red)super.clone();
     }
     catch(CloneNotSupportedException e)
     {

     }
     return r;
   }
   public void display()
   {
     System.out.println("This is Red!");
   }
}

class Blue implements MyColor
{
   public Object clone()
   {
     Blue b=null;
     try
     {
       b=(Blue)super.clone();
     }
     catch(CloneNotSupportedException e)
     {

     }
     return b;
   }
   public void display()
   {
     System.out.println("This is Blue!");
   }
}

class PrototypeManager
{
   private Hashtable ht=new Hashtable();

   public PrototypeManager()
   {
         ht.put("red",new Red());
         ht.put("blue",new Blue());
   }

   public void addColor(String key,MyColor obj)
   {
      ht.put(key,obj);
   }

   public MyColor getColor(String key)
   {
      return (MyColor)((MyColor)ht.get(key)).clone();
   }
}

class Client
{
   public static void main(String args[])
   {
      PrototypeManager pm=new PrototypeManager();

      MyColor obj1=(MyColor)pm.getColor("red");
      obj1.display();

      MyColor obj2=(MyColor)pm.getColor("red");
      obj2.display();

      System.out.println(obj1==obj2);
   }
}
```

**相似对象的复制**

很多情况下，复制所得到的对象与原型对象并不是完全相同的，它们的某些属性值存在异同。通过原型模式获得相同对象后可以再对其属性进行修改，从而获取所需对象。如多个学生对象的信息的区别在于性别、姓名和年龄，而专业、学院、学校等信息都相同，为了简化创建过程，可以通过原型模式来实现相似对象的复制。

实例代码:

```java
class Student implements Cloneable
{
    private String stuName;
    private String stuSex;
    private int stuAge;
    private String stuMajor;
    private String stuCollege;
    private String stuUniversity;

    public Student(String stuName,String stuSex,int stuAge,String stuMajor,String stuCollege,String stuUniversity)
    {
        this.stuName=stuName;
        this.stuSex=stuSex;
        this.stuAge=stuAge;
        this.stuMajor=stuMajor;
        this.stuCollege=stuCollege;
        this.stuUniversity=stuUniversity;
    }

    public void setStuName(String stuName) {
        this.stuName = stuName;
    }

    public void setStuSex(String stuSex) {
        this.stuSex = stuSex;
    }

    public void setStuAge(int stuAge) {
        this.stuAge = stuAge;
    }

    public void setStuMajor(String stuMajor) {
        this.stuMajor = stuMajor;
    }

    public void setStuCollege(String stuCollege) {
        this.stuCollege = stuCollege;
    }

    public void setStuUniversity(String stuUniversity) {
        this.stuUniversity = stuUniversity;
    }

    public String getStuName() {
        return (this.stuName);
    }

    public String getStuSex() {
        return (this.stuSex);
    }

    public int getStuAge() {
        return (this.stuAge);
    }

    public String getStuMajor() {
        return (this.stuMajor);
    }

    public String getStuCollege() {
        return (this.stuCollege);
    }

    public String getStuUniversity() {
        return (this.stuUniversity);
    }

    public Student clone()
    {
        Student cpStudent=null;
        try
        {
            cpStudent=(Student)super.clone();
        }
        catch(CloneNotSupportedException e)
        {
        }
        return cpStudent;
    }
}

class MainClass
{
    public static void main(String args[])
    {
        Student stu1,stu2,stu3;

        stu1=new Student("张无忌","男",24,"软件工程","软件学院","中南大学"); //状态相似

        //使用原型模式
        stu2=stu1.clone();
        stu2.setStuName("杨过");

        //使用原型模式
        stu3=stu1.clone();
        stu3.setStuName("小龙女");
        stu3.setStuSex("女");

        System.out.print("姓名：" + stu1.getStuName());
        System.out.print("，性别：" + stu1.getStuSex());
        System.out.print("，年龄：" + stu1.getStuAge());
        System.out.print("，专业：" + stu1.getStuMajor());
        System.out.print("，学院：" + stu1.getStuCollege());
        System.out.print("，学校：" + stu1.getStuUniversity());
        System.out.println();

        System.out.print("姓名：" + stu2.getStuName());
        System.out.print("，性别：" + stu2.getStuSex());
        System.out.print("，年龄：" + stu2.getStuAge());
        System.out.print("，专业：" + stu2.getStuMajor());
        System.out.print("，学院：" + stu2.getStuCollege());
        System.out.print("，学校：" + stu2.getStuUniversity());
        System.out.println();

        System.out.print("姓名：" + stu3.getStuName());
        System.out.print("，性别：" + stu3.getStuSex());
        System.out.print("，年龄：" + stu3.getStuAge());
        System.out.print("，专业：" + stu3.getStuMajor());
        System.out.print("，学院：" + stu3.getStuCollege());
        System.out.print("，学校：" + stu3.getStuUniversity());
        System.out.println();
    }
}
```



---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note-sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8F/  

