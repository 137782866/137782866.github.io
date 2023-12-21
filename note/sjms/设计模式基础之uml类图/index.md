# 设计模式基础之UML类图


## UML简介
学习设计模式需要先学习一些基本的统一建模语言（Unified Modeling Language，UML）知识，这是一种可视化的面向对象软件分析和设计建模的标准建模语言，可用来构造软件系统的蓝图，应用非常广泛。

UML中有非常多图和视图，需要参考软件工程相关书籍，在设计模式中使用频率最高的是类图。

## UML类

在UML中，类使用包含类名、属性和操作且带有分隔线的长方形来表示，如定义一个 `Employee` 类，它包含属性 `name`、`age` 和 `email`，以及操作 `modifyInfo()`，表示如下：

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_111029.png)

```java
public class Employee {
    private String name;
    private int age;
    private String email;

    public void modifyInfo() {
        //...
    }
}
```

在UML类图中，类一般由三部分组成：

1. 第一部分是**类名**：每个类都必须有一个名字，类名是一个字符串。

2. 第二部分是类的**属性**(Attributes)：属性是指类的性质，即类的成员变量。一个类可以有任意多个属性，也可以没有属性.

   UML规定属性的表示方式为：**可见性 名称:类型 [ = 缺省值 ]**

   其中：

   - “可见性”表示该属性对于类外的元素而言是否可见，包括`公有(public)`、`私有(private)`和`保护(protected)`三种，在类图中分别用符号 `+`、 `-` 和 `#` 表示。
   - “名称”表示属性名，用一个字符串表示。
   - “类型”表示属性的数据类型，可以是基本数据类型，也可以是用户自定义类型。
   - “缺省值”是一个可选项，即属性的初始值。

3. 第三部分是**类的操作**(Operations)：操作是类的任意一个实例对象都可以使用的行为，是类的成员方法。

   UML规定操作的表示方式为：**可见性 名称(参数列表) [ : 返回类型]**

   其中：

   - “可见性”的定义与属性的可见性定义相同。
   - “名称”即方法名，用一个字符串表示。
   - “参数列表”表示方法的参数，其语法与属性的定义相似，参数个数是任意的，多个参数之间用逗号 `,` 隔开。
   - “返回类型”是一个可选项，表示方法的返回值类型，依赖于具体的编程语言，可以是基本数据类型，也可以是用户自定义类型，还可以是空类型(void)，如果是构造方法，则无返回类型。

   下图中：
   
   ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_111056.png)
   
   操作 method1 的可见性为 public(+)，带入了一个 Object 类型的参数 par，返回值为空(void)；操作 method2 的可见性为 protected(#)，无参数，返回值为 String 类型；操作 method3 的可见性为private(-)，包含两个参数，其中一个参数为int类型，另一个为int[]类型，返回值为int类型。
   
   由于在Java语言中允许出现内部类，因此可能会出现包含四个部分的类图。
   
   ![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_111109.png)

## UML类之间的关系

类不是孤立存在，而是互相存在关系，主要有以下几种。

### 依赖关系

依赖关系(Dependency)是一种使用关系，特定事物的改变有可能会影响到使用该事物的其他事物，在需要表示一个事物使用另一个事物时使用依赖关系。通常，**依赖关系体现在某个类的方法使用另一个类的对象作为参数**。

在UML中，依赖关系用带箭头的虚线表示，由依赖的一方指向被依赖的一方。

比如：驾驶员开车，Driver 类的 driver 方法中将 Car 类的对象 car 作为参数传递，以便在 driver 方法中调用 car 的 move() 方法，且驾驶员的 driver 方法依赖 car 的 move() 方法，因此 Driver 类依赖于 Car 类。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_111125.png)

```java
public class Driver
{
    public void drive(Car car)
    {
        car.move();
    }
}

public class Car
{
    public void move()
    {
        ...
    }
}
```

如果在一个类的方法中调用了另一个类的静态方法，或者在一个类的方法中定义了另一个类的对象作为其局部变量，也是依赖关系的一种表现形式。

### 关联关系

关联关系有以下几种：**双向关联，单项关联，自关联，多重性关联，聚合关系，组合关系**。

关联关系(Association)是类与类之间最常用的一种关系，它是一种结构化关系，用于表示一类对象与另一类对象之间有联系。通常将一个类的对象作为另一个类的属性。

在UML类图中，用实线连接有关联的对象所对应的类，可以在关联线上标注角色名。

比如：一个登录界面的 LoginForm 中包含一个 JButton 类的注册按钮 loginButton 它们之间可以表示为关联关系。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_112536.png)

```java
public class LoginForm
{
    private JButton loginButton;
    ...
}

public class JButton
{
    ...
}
```

#### 双向关联

默认情况下，关联是双向的，双向关联用直线表示。

比如：顾客(Customer)购买商品(Product)并拥有商品，反之，卖出的商品总是有对应的顾客与之对应。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_112721.png)

```java
public class Customer
{
    private Product[] products;
    ...
}

public class Product
{
    private Customer customer;
    ...
}
```

#### 单向关系

类的关联关系也可以是单向的，单向关联用带箭头的实线表示。

比如：顾客(Customer)拥有地址(Address)，则顾客和地址拥有单向关联关系。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_112819.png)

```java
public class Customer
{
    private Address address;
    ...
}

public class Address
{
    ...
}
```

#### 自关联

在系统中可能会存在一些类的属性对象类型为该类本身，这种特殊的关联关系称为自关联。

比如：节点类(Node)的成员又是节点对象。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_112924.png)

```java
1 public class Node
2 {
3     private Node subNode;
4     ...
5 } 
```

#### 多重关联

重数性关联关系又称为多重性关联关系(Multiplicity)，表示一个类的对象与另一个类的对象连接的个数。在UML中多重性关系可以直接在关联直线上增加一个数字表示与之对应的另一个类的对象的个数。

| 表示方式 | 多重性说明                                                 |
| -------- | ---------------------------------------------------------- |
| 1..1     | 表示另一个类的一个对象只与一个该类对象有关系               |
| 0..*     | 表示另一个类的一个对象与零个或多个该类对象有关系           |
| 1..*     | 表示另一个类的一个对象与一个或多个该类对象有关系           |
| 0..1     | 表示另一个类的一个对象没有或只与一个该类对象有关系         |
| m..n     | 表示另一个类的一个对象与最少m、最多n个该类对象有关系(m<=n) |

比如：一个界面(Form)可以拥有零个或者多个按钮(Buttom)，但是一个按钮只能属于一个界面。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_113041.png)

```java
public class Form
{
    private Button buttons[];
    ...
}

public class Button
{
    ...
}
```

#### 聚合

聚合关系(Aggregation)表示一个整体与部分的关系。通常在定义一个整体类后，再去分析这个整体类的组成结构，从而找出一些成员类，该整体类和成员类之间就形成了聚合关系。

在聚合关系中，成员类是整体类的一部分，即成员对象是整体对象的一部分，但是成员对象可以脱离整体对象独立存在。通常不直接实例化成员对象，通过构造函数或者Setter方法注入。

在UML中，聚合关系用带空心菱形的直线表示。

比如：发动机是汽车的组成部分，但是发动机可以独立于汽车存在，则汽车和发动机是聚合关系。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_113222.png)

```java
public class Car
{
    private Engine engine;
    public Car(Engine engine)
   {
        this.engine = engine;
    }

    public void setEngine(Engine engine)
    {
        this.engine = engine;
    }
    ...
}

public class Engine
{
    ...
}
```

Car 中定义了 Engine 类型的成员对象，Engine 是 Car 的一部分，但是 Engine 可以脱离 Car 单独存在，因为在 Car 类中并没有直接实例化 Engine，而是通过构造函数或者 Setter 方法将类外部实例化好的对象以参数形式传入 Car 中，这种方式成为注入。因为 Engine 和 Car 的实例化时刻不相同，因此它们之间不存在生命周期的制约关系仅仅只是部分与整体之间的关系。聚合关系表示整体与部分的关系较弱。

#### 组合

组合关系(Composition)也表示类之间整体和部分的关系，但是组合关系中部分和整体具有统一的生存期。一旦整体对象不存在，部分对象也将不存在，部分对象与整体对象之间具有同生共死的关系。通常定义了成员对象后通过构造函数实例化。

在组合关系中，成员类是整体类的一部分，而且整体类可以控制成员类的生命周期，即成员类的存在依赖于整体类。

在UML中，组合关系用带实心菱形的直线表示。

比如：人的头和嘴巴，嘴把是头的组成部分。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_113437.png)

```java
public class Head
{
    private Mouth mouth;
    public Head()
    {
    mouth = new Mouth();
    }
    ...
}

public class Mouth
{
    ...
}
```

因为 Head 的构造函数中实例化了 Mouth 对象，因此在创建 Head 对象同时将创建 Mouth 对象，销毁 Head 对象同时销毁 Mouth 对象。整体控制局部的生命周期。组合关系表示整体与部分的关系较弱。

- 聚合与组合的共同点就是一个类的实例时另外一个类的成员对象。

- 聚合与组合和普通关联关系主要是语义上的区别。比如：客户与产品的关系就不能用聚合和组合，产品并不是客户的一部分，不存在整体与部分的关系，只能用普通的关联。

### 泛化关系

泛化关系(Generalization)也就是继承关系，也称为 `is-a-kind-of` 关系，泛化关系用于描述父类与子类之间的关系，父类又称作基类或超类，子类又称作派生类。在 UML 中，泛化关系用带空心三角形的直线来表示。

在代码实现时，使用面向对象的继承机制来实现泛化关系，如在 Java 语言中使用 extends 关键字、在 C++/C# 中使用冒号 `:` 来实现。

比如：Student 类和 Teacher 类都继承了 Person 类的属性和方法，Student 类中增加了属性学号 studentNo 和方法 study()，Teacher 类中增加了属性编号 teacherNo 和方法 teach()。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_113730.png)

```java
public class Person
{
    protected String name;
    protected int age;
    public void move()
    {
        ...
    }
    public void say()
   {
        ...
    }
}

public class Student extends Person
{
    private String studentNo;
    public void study()
    {
        ...
    }
}
```

### 接口实现关系

接口之间也可以有与类之间关系类似的继承关系和依赖关系，但是接口和类之间还存在一种实现关系(Realization)，在这种关系中，类实现了接口，类中的操作实现了接口中所声明的操作。在UML中，类与接口之间的实现关系用带空心三角形的虚线来表示。

比如：定义了交通工具类 Vehicle，有一个抽象操作 move()，类 Ship 和 Car 都实现了这些操作，但实现细节不一样。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/clipboard_20231209_114013.png)

```java
public interface Vehicle
{
    public void move();
}

public class Ship implements Vehicle
{
    public void move()
    {
        ...
    }
}

public class Car implements Vehicle
{
    public void move()
    {
        ...
    }
}
```



---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjms/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E5%9F%BA%E7%A1%80%E4%B9%8Buml%E7%B1%BB%E5%9B%BE/  

