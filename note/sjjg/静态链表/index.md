# 静态链表








C语言中有指针，可以非常容易的操作内存中的地址和数据，比其他语言更方便灵活，实现链表比较灵活。有些高级语言是对象的引用机制，从某种角度也可以实现指针的作用。但是有些语言没有指针，如 Basic、Fortran等早期语言没有指针，链表没有办法按照之前的指针方法实现。

可以使用数组来代替指针描述单链表。数组的元素由两个数据域组成，data和cur。数据域data用来存放数据元素，而cur相当于单链表中的next指针，存放该元素的后继在数据中的下标，把cur叫做游标。而这种用数组描述的链表，被成为静态链表，也被叫做游标实现法。

为了方便插入数据，通常会把数据建立的大一些。对数组第一个和最后一个元素作为特殊元素处理，不存数据。通常把未使用的数据元素称为备用链表。数组的第一个元素的cur存放备用链表的第一个结点的下标；数组的最后一个元素的cur则存放第一个有数值的元素的下标，相当于单链表中的头结点的作用，当整个链表为空时，则为0。

![image-20240107231710743](C:\Users\CodePeak\AppData\Roaming\Typora\typora-user-images\image-20240107231710743.png)

当插入数据后，将变为：

![image-20240107231754021](C:\Users\CodePeak\AppData\Roaming\Typora\typora-user-images\image-20240107231754021.png)

数据元素的数据结构如下：

```cpp
# define MAXSIZE 1000
template <typename T>
struct StaticLinkNode
{
    T data;
    int cur;
};
```























---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjjg/%E9%9D%99%E6%80%81%E9%93%BE%E8%A1%A8/  

