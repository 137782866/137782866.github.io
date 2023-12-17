# 设计模式-装饰模式


## 模式动机

一般有两种方式可以实现给一个类或对象增加行为：

1. **继承机制**，使用继承机制是给现有类添加功能的一种有效途径，通过继承一个现有类可以使得子类在拥有自身方法的同时还拥有父类的方法。但是这种方法是静态的，用户不能控制增加行为的方式和时机。
2. **关联机制**，即将一个类的对象嵌入另一个对象中，由另一个对象来决定是否调用嵌入对象的行为以便扩展自己的行为，我们称这个嵌入的对象为装饰器(Decorator)。

装饰模式以对客户透明的方式动态地给一个对象附加上更多的责任，换言之，客户端并不会觉得对象在装饰前和装饰后有什么不同。装饰模式可以在不需要创造更多子类的情况下，将对象的功能加以扩展。这就是装饰模式的模式动机。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_064633.png)

## 模式定义

装饰模式(Decorator Pattern)：动态地给一个对象增加一些额外的职责(Responsibility)，就增加对象功能来说，装饰模式比生成子类实现更为灵活。其别名也可以称为包装器(Wrapper)，与适配器模式的别名相同，但它们适用于不同的场合。根据翻译的不同，装饰模式也有人称之为“油漆工模式”，它是一种对象结构型模式。

## 模式结构

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_064649.png)

装饰模式包含如下角色：

- **Component**: 抽象构件
- **ConcreteComponent**: 具体构件
- **Decorator**: 抽象装饰类
- **ConcreteDecorator**: 具体装饰类

## 模式分析

与继承关系相比，关联关系的主要优势在于不会破坏类的封装性，而且继承是一种耦合度较大的静态关系，无法在程序运行时动态扩展。在软件开发阶段，关联关系虽然不会比继承关系减少编码量，但是到了软件维护阶段，由于关联关系使系统具有较好的松耦合性，因此使得系统更加容易维护。当然，关联关系的缺点是比继承关系要创建更多的对象。

使用装饰模式来实现扩展比继承更加灵活，它以对客户透明的方式动态地给一个对象附加更多的责任。装饰模式可以在不需要创造更多子类的情况下，将对象的功能加以扩展。

典型的抽象装饰类代码：

```java
public class Decorator extends Component
{
    private Component component;
    public Decorator(Component component)
    {
        this.component=component;
    }
    public void operation()
    {
        component.operation();
    }
}
```

典型的具体装饰类代码：

```java
public class ConcreteDecorator extends Decorator
{
    public ConcreteDecorator(Component component)
    {
        super(component);
    }
    public void operation() //在子类的operation方法中增加一个方法
    {
        super.operation();
        addedBehavior();
    }
    public void addedBehavior()
    {
        //新增方法
    }
}
```

## 模式实例

### 实例1：变形金刚

变形金刚在变形之前是一辆汽车，它可以在陆地上移动。当它变成机器人之后除了能够在陆地上移动之外，还可以说话；如果需要，它还可以变成飞机，除了在陆地上移动还可以在天空中飞翔。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_064707.png)

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_064727.png)

实例代码：

```java
//抽象构建
public interface Transform
{
    public void move();
}

//具体构建
public final class Car implements Transform
{
    public Car()
    {
        System.out.println("变形金刚是一辆车！");
    }

    public void move()
    {
        System.out.println("在陆地上移动！");
    }
}

//抽象装饰类
public class Changer implements Transform
{
    private Transform transform;

    public Changer(Transform transform)
    {
        this.transform=transform;
    }

    public void move()
    {
        transform.move();
    }
}

//具体装饰类
public class Robot extends Changer
{
    public Robot(Transform transform)
    {
        super(transform);
        System.out.println("变成机器人！");
    }

    public void say()
    {
        System.out.println("说话！");
    }
}

//具体装饰类
public class Airplane extends Changer
{
    public Airplane(Transform transform)
    {
        super(transform);
        System.out.println("变成飞机！");
    }

    public void fly()
    {
        System.out.println("在天空飞翔！");
    }
}

//客户端
public class Client
{
    public static void main(String args[])
    {
        Transform camaro;
        camaro=new Car();
        camaro.move();
        System.out.println("-----------------------------");

        Airplane bumblebee=new Airplane(camaro);
        bumblebee.move();
        bumblebee.fly();
    }
}
```

### 实例2：多重加密系统

某系统提供了一个数据加密功能，可以对字符串进行加密。最简单的加密算法通过对字母进行移位来实现，同时还提供了稍复杂的逆向输出加密，还提供了更为高级的求模加密。用户先使用最简单的加密算法对字符串进行加密，如果觉得还不够可以对加密之后的结果使用其他加密算法进行二次加密，当然也可以进行第三次加密。现使用装饰模式设计该多重加密系统。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_064751.png)

实例代码：

```java
//抽象构建
public interface Cipher
{
    public String encrypt(String plainText);
}

//具体构建
public class SimpleCipher implements Cipher
{
    public String encrypt(String plainText)
    {
        String str="";
        for(int i=0;i<plainText.length();i++)
        {
            char c=plainText.charAt(i);
            if(c>='a'&&c<='z')
            {
                c+=6;
            if(c>'z') c-=26;
            if(c<'a') c+=26;
            }
            if(c>='A'&&c<='Z')
            {
                c+=6;
            if(c>'Z') c-=26;
            if(c<'A') c+=26;
            }
            str+=c;
        }
        return str;
    }
}

//抽象装饰类
public class CipherDecorator implements Cipher
{
    private Cipher cipher;

    public CipherDecorator(Cipher cipher)
    {
        this.cipher=cipher;
    }

    public String encrypt(String plainText)
    {
        return cipher.encrypt(plainText);
    }
}
//具体装饰类
public class ComplexCipher extends CipherDecorator
{
    public ComplexCipher(Cipher cipher)
    {
        super(cipher);
    }

    public String encrypt(String plainText)
    {
        String result=super.encrypt(plainText);
        result=reverse(result);
        return result;
    }

    public String reverse(String text)
    {
        String str="";
        for(int i=text.length();i>0;i--)
        {
            str+=text.substring(i-1,i);
        }
        return str;
    }
}

//具体装饰类
public class AdvancedCipher extends CipherDecorator
{
    public AdvancedCipher(Cipher cipher)
    {
        super(cipher);
    }

    public String encrypt(String plainText)
    {
        String result=super.encrypt(plainText);
        result=mod(result);
        return result;
    }

    public String mod(String text)
    {
        String str="";
        for(int i=0;i<text.length();i++)
        {
            String c=String.valueOf(text.charAt(i)%6);
            str+=c;
        }
        return str;
    }
}

//客户端
public class Client
{
    public static void main(String args[])
    {
        String password="sunnyLiu";  //明文
        String cpassword;       //密文
        Cipher sc,cc;

        sc=new SimpleCipher();
        cpassword=sc.encrypt(password);
        System.out.println(cpassword);
        System.out.println("---------------------");

        cc=new ComplexCipher(sc);
        cpassword=cc.encrypt(password);
        System.out.println(cpassword);
        System.out.println("---------------------");

        /*
        ac=new AdvancedCipher(cc);
        cpassword=ac.encrypt(password);
        System.out.println(cpassword);
        System.out.println("---------------------");
        */
    }
}
```

## 模式优缺点

**优点**如下：

- 装饰模式与继承关系的目的都是要扩展对象的功能，但是装饰模式可以提供比继承更多的灵活性。
- 可以通过一种动态的方式来扩展一个对象的功能，通过配置文件可以在运行时选择不同的装饰器，从而实现不同的行为。
- 通过使用不同的具体装饰类以及这些装饰类的排列组合，可以创造出很多不同行为的组合。可以使用多个具体装饰类来装饰同一对象，得到功能更为强大的对象。
- 具体构件类与具体装饰类可以独立变化，用户可以根据需要增加新的具体构件类和具体装饰类，在使用时再对其进行组合，原有代码无须改变，符合“开闭原则”。

**缺点**如下：

- 使用装饰模式进行系统设计时将产生很多小对象，这些对象的区别在于它们之间相互连接的方式有所不同，而不是它们的类或者属性值有所不同，同时还将产生很多具体装饰类。这些装饰类和小对象的产生将增加系统的复杂度，加大学习与理解的难度。
- 这种比继承更加灵活机动的特性，也同时意味着装饰模式比继承更加易于出错，排错也很困难，对于多次装饰的对象，调试时寻找错误可能需要逐级排查，较为烦琐。

## 模式适应环境

在以下情况下可以使用装饰模式：

- 在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。
- 需要动态地给一个对象增加功能，这些功能也可以动态地被撤销。 
- 当不能采用继承的方式对系统进行扩充或者采用继承不利于系统扩展和维护时。不能采用继承的情况主要有两类：第一类是系统中存在大量独立的扩展，为支持每一种组合将产生大量的子类，使得子类数目呈爆炸性增长；第二类是因为类定义不能继承（如final类）。

## 模式应用

1. 在 javax.swing 包中，可以通过装饰模式动态给一些构件增加新的行为或改善其外观显示。如 JList 构件本身并不支持直接滚动，即没有滚动条，要创建可以滚动的列表，可以使用如下代码实现：

   ```java
   JList list = new JList();
   JScrollPane sp = new JScrollPane(list); 
   ```

2. 装饰模式在JDK中最经典的实例是Java IO。

   以InputStream为例：

   ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_064821.png)

   抽象装饰类：FilterInputStream

   ```java
   //...
   protected volatile InputStream in;
   protected FilterInputStream(InputStream in) {
   	this.in = in;
   }
   //...
   ```

   角色分配：

   - 抽象构件类：InputStream
   - 具体构件类：FileInputStream、ByteArrayInputStream等
   - 抽象装饰类：FilterInputStream
   - 具体装饰类：BufferedInputStream、DataInputStream等

   客户端使用：

   ```java
   //...
   FileInputStream inFS=new FileInputStream("temp/fileSrc.txt");
   BufferedInputStream inBS=new BufferedInputStream(inFS);
   //定义一个字节数组，用于存放缓冲数据
   byte[] data = new byte[1024];
   inBS.read(data);
   //...
   ```

## 模式扩展

**装饰模式的简化-需要注意的问题**

- 一个装饰类的接口必须与被装饰类的接口保持相同，对于客户端来说无论是装饰之前的对象还是装饰之后的对象都可以一致对待。
- 尽量保持具体构件类Component作为一个“轻”类，也就是说不要把太多的逻辑和状态放在具体构件类中，可以通过装饰类对其进行扩展。
- 如果只有一个具体构件类而没有抽象构件类，那么抽象装饰类可以作为具体构件类的直接子类。

**装饰模式的简化**

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_064920.png)

**透明装饰模式（多重加密系统）**

在透明装饰模式中，要求客户端完全针对抽象编程，装饰模式的透明性要求客户端程序不应该声明具体构件类型和具体装饰类型，而应该全部声明为抽象构件类型。

```java
Cipher sc,cc,ac;
sc=new SimpleCipher();
cc=new ComplexCipher(sc);    
ac=new AdvancedCipher(cc); 
```

**半透明装饰模式（变形金刚）**

大多数装饰模式都是半透明(semi-transparent)的装饰模式，而不是完全透明(transparent)的。即允许用户在客户端声明具体装饰者类型的对象，调用在具体装饰者中新增的方法。

```java
Transform camaro;
camaro=new Car();
camaro.move();
Robot bumblebee=new Robot(camaro);
bumblebee.move();
bumblebee.say(); 
```



---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note-sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E8%A3%85%E9%A5%B0%E6%A8%A1%E5%BC%8F/  

