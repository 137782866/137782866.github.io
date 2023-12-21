# 设计模式-组合模式


## 模式动机

对于树形结构，当容器对象（如文件夹）的某一个方法被调用时，将遍历整个树形结构，寻找也包含这个方法的成员对象（可以是容器对象，也可以是叶子对象，如子文件夹和文件）并调用执行。（递归调用）

由于容器对象和叶子对象在功能上的区别，在使用这些对象的客户端代码中必须有区别地对待容器对象和叶子对象，而实际上大多数情况下客户端希望一致地处理它们，因为对于这些对象的区别对待将会使得程序非常复杂。

组合模式描述了如何将容器对象和叶子对象进行递归组合，使得用户在使用时无须对它们进行区分，可以一致地对待容器对象和叶子对象，这就是组合模式的模式动机。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_055754.png)

## 模式定义

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_055811.png)

组合模式包含如下角色：

- **Component**: 抽象构件
- **Leaf**: 叶子构件
- **Composite**: 容器构件
- **Client**: 客户类

## 模式分析

组合模式的关键是定义了一个抽象构件类，它既可以代表叶子，又可以代表容器，而客户端针对该抽象构件类进行编程，无须知道它到底表示的是叶子还是容器，可以对其进行统一处理。

同时容器对象与抽象构件类之间还建立一个聚合关联关系，在容器对象中既可以包含叶子，也可以包含容器，以此实现递归组合，形成一个树形结构。

典型的抽象构件角色代码：

```java
public abstract class Component
{
    public abstract void add(Component c);
    public abstract void remove(Component c);
    public abstract Component getChild(int i);
    public abstract void operation();
}
```

典型的叶子构件角色代码：

```java
public class Leaf extends Component
{
    public void add(Component c)
    { //异常处理或错误提示 }

    public void remove(Component c)
    { //异常处理或错误提示 }

    public Component getChild(int i)
    { //异常处理或错误提示 }

    public void operation()
    {
        //实现代码
    }
}
```

典型的容器构件角色代码：

```java
public class Composite extends Component
{
    private ArrayList list = new ArrayList();

    public void add(Component c)
    {
        list.add(c);
    }

    public void remove(Component c)
    {
        list.remove(c);
    }

    public Component getChild(int i)
    {
        (Component)list.get(i);
    }

    public void operation()
    {
        for(Object obj:list)
        {
            ((Component)obj).operation();
        }
    }
}
```

## 模式实例

### 实例1：水果盘

在水果盘(Plate)中有一些水果，如苹果(Apple)、香蕉(Banana)、梨子(Pear)，当然大水果盘中还可以有小水果盘，现需要对盘中的水果进行遍历（吃），当然如果对一个水果盘执行“吃”方法，实际上就是吃其中的水果。使用组合模式模拟该场景。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_055828.png)

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_055852.png)

实例代码：

```java
//抽象构建
public abstract class MyElement
{
    public abstract void eat();
}

//容器构建
import java.util.*;

public class Plate extends MyElement
{
    private ArrayList list=new ArrayList();

    public void add(MyElement element)
    {
       list.add(element);
    }

    public void delete(MyElement element)
    {
        list.remove(element);
    }

    public void eat()
    {
        for(Object object:list)
        {
            ((MyElement)object).eat();    //递归
        }
    }
}

//叶子构建
public class Apple extends MyElement
{
    public void eat()
    {
        System.out.println("吃苹果！");
    }
}

//叶子构建
public class Banana extends MyElement
{
    public void eat()
    {
        System.out.println("吃香蕉！");
    }
}

//叶子构建
public class Pear extends MyElement
{
    public void eat()
    {
        System.out.println("吃梨子！");
    }
}

//客户端
public class Client
{
    public static void main(String a[])
    {
        MyElement obj1,obj2,obj3,obj4,obj5;
        Plate plate1,plate2,plate3;

        obj1=new Apple();
        obj2=new Pear();
        plate1=new Plate();
        plate1.add(obj1);
        plate1.add(obj2);

        obj3=new Banana();
        obj4=new Banana();
        plate2=new Plate();
        plate2.add(obj3);
        plate2.add(obj4);

        obj5=new Apple();
        plate3=new Plate();
        plate3.add(plate1);
        plate3.add(plate2);
        plate3.add(obj5);

        plate3.eat();
    }
}
```

### 实例2：文件浏览

文件有不同类型，不同类型的文件其浏览方式有所区别，如文本文件和图片文件的浏览方式就不相同。对文件夹的浏览实际上就是对其中所包含文件的浏览，而客户端可以一致地对文件和文件夹进行操作，无须关心它们的区别。使用组合模式来模拟文件的浏览操作。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_055909.png)

## 模式优缺点

**优点**如下：

- 可以清楚地定义分层次的复杂对象，表示对象的全部或部分层次，使得增加新构件也更容易。
- 客户端调用简单，客户端可以一致的使用组合结构或其中单个对象。
- 定义了包含叶子对象和容器对象的类层次结构，叶子对象可以被组合成更复杂的容器对象，而这个容器对象又可以被组合，这样不断递归下去，可以形成复杂的树形结构。
- 更容易在组合体内加入对象构件，客户端不必因为加入了新的对象构件而更改原有代码。

**缺点**如下：

- 使设计变得更加抽象，对象的业务规则如果很复杂，则实现组合模式具有很大挑战性，而且不是所有的方法都与叶子对象子类都有关联。
- 增加新构件时可能会产生一些问题，很难对容器中的构件类型进行限制。

## 模式适应环境

在以下情况下可以使用组合模式：

1. 需要表示一个对象整体或部分层次，在具有整体和部分的层次结构中，希望通过一种方式忽略整体与部分的差异，可以一致地对待它们。
2. 让客户能够忽略不同对象层次的变化，客户端可以针对抽象构件编程，无须关心对象层次结构的细节。
3. 对象的结构是动态的并且复杂程度不一样，但客户需要一致地处理它们。

## 模式应用

1. XML文档解析

   ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_055927.png)

   ```java
   <?xml version="1.0"?>
     <books>
       <book>
           <author>Carson</author>
           <price format="dollar">31.95</price>
           <pubdate>05/01/2001</pubdate>
       </book>
       <pubinfo>
           <publisher>MSPress</publisher>
           <state>WA</state>
       </pubinfo>
     </books>
   ```

2. 操作系统中的目录结构是一个树形结构，因此在对文件和文件夹进行操作时可以应用组合模式，例如杀毒软件在查毒或杀毒时，既可以针对一个具体文件，也可以针对一个目录。如果是对目录查毒或杀毒，将递归处理目录中的每一个子目录和文件。

3. JDK的 AWT/Swing 是组合模式在 Java 类库中的一个典型实际应用。

   ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_055945.png)

## 模式扩展

**更复杂的组合模式**

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_060001.png)

**透明组合模式**

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_060029.png)

**安全组合模式**

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231215_060049.png)


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E7%BB%84%E5%90%88%E6%A8%A1%E5%BC%8F/  

