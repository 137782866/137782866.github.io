# 顺序表


## 线性表

线性表（linear list）是数据结构的一种，一个线性表是n个具有相同特性的数据表项的有限序列。线性表中表项之间的关系是一对一的关系，即除了第一个和最后一个数据元素之外，其它数据元素都是首尾相接的（注意，这句话只适用大部分线性表，而不是全部。比如，循环链表逻辑层次上也是一种线性表（存储层次上属于链式存储，但是把最后一个数据元素的尾指针指向了首位结点）。

线性表的存储方式有两种：

1. 顺序存储（顺序表）
2. 链表存储（链表）

## 顺序表

顺序表是线性表基于数组的存储方式。其特点是：

1. 各个表项逻辑顺序与物理顺序一致，即第i个表项存储在第i个物理位置。
2. 对顺序表中所有表项，既可以进行顺序访问（从第一个开始逐个访问），也可以进行随机访问（使用下标访问）。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/clipboard_20231230_100102.png)

顺序表的存储有两种方式：

1. 静态方式：存储数组的大小固定。数据空间占满，新的数据将不能继续存储。

   ```cpp
   struct SeqList
   {
     	T m_data[maxSize];	//数组data
       int m_length;		//当前长度
   };
   ```

2. 动态方式：存储数组的空间是在程序执行过程中通过动态分配的，一旦空间占满，可另外分配一块更大的存储空间替换原来的空间，达到扩充存储数组空间的目的。

   ```cpp
   struct SeqList
   {
       T* m_data;		//数组data
       int m_capacity;	//容量空间
       int m_length;	//当前长度
   }
   ```

## 顺序表实现

使用C++语言实现一个非常简单的顺序表，采用静态存储方式。我们约定线性表长度从1开始，数组的下标是从0开始，如上图所示。

注：动态存储的实现方式相同，不过需要增加数组扩容接口，在已申请的空间已满的情况下，若继续插入，需要重新申请空间，并将数据进行拷贝的处理。

```cpp
#include <iostream>
using namespace std;

#define MAXSIZE 10

template <typename T>
class SeqList
{
protected:
	T m_data[MAXSIZE];
	int m_length;

public:
	SeqList()
	{
		init();
	}
	//初始化
	bool init()
	{
		m_length = 0;
		return true;
	}
	//是否为空
	bool isEmpty()
	{
		return m_length == 0;
	}
	//清空顺序表
	void clear()
	{
		m_length = 0;
	}

	//返回顺序表的元素个数
	int length()
	{
		return m_length;
	}

	//第i个数据元素的值（从1开始），用out传出
	bool getElem(int i, T& out)
	{
		if (m_length == 0)
			return false;
		if (i < 1 || i > m_length)
			return false;
		out = m_data[i - 1];
		return true;
	}

	//定位第1个与数据元素e相等的位序（从1开始）
	int locateElem(const T& e)
	{
		if (m_length == 0)
			return 0;
		for (int i = 0; i < m_length; ++i)
		{
			if (m_data[i] == e)
				return i + 1;
		}
		return 0;
	}

	//在第i个位置之前插入新的元素e，length加1
	bool insertElem(int i, const T& e)
	{
		if (m_length == MAXSIZE)
			return false;
		if (i < 1 || i > m_length + 1)
			return false;
		for (int k = m_length; k >= i; --k)
		{
			m_data[k] = m_data[k - 1];
		}
		m_data[i - 1] = e;
		m_length++;
		return true;
	}
	
    //删除第i个位置的元素，将删除的元素的值用e返回
	bool deleteElem(int i, T& e)
	{
		if (m_length == 0)
			return false;
		if (i < 1 || i > m_length)
			return false;
		e = m_data[i - 1];
		for (int k = i; k < m_length; ++k)
		{
			m_data[k - 1] = m_data[k];
		}
		m_length--;
		return true;
	}
	
    //输出顺序表的元素
	void output()
	{
		cout << "output:";
		for (int i = 0; i < m_length; ++i)
		{
			cout << m_data[i] << " ";
		}
		cout << endl;
	}
};


int main()
{
	SeqList<int> seqlist;
	seqlist.insertElem(1, 100);
	seqlist.insertElem(2, 100);
	seqlist.insertElem(3, 100);
	seqlist.insertElem(4, 100);
	seqlist.insertElem(5, 100);
	seqlist.output();

	int del = 0;
	seqlist.deleteElem(1, del);
	seqlist.output();

	cout << seqlist.length() << endl;
	seqlist.deleteElem(1, del);
	seqlist.deleteElem(1, del);
	seqlist.deleteElem(1, del);
	seqlist.deleteElem(1, del);
	seqlist.output();
	cout << "length:" << seqlist.length() << endl;
	cout << "isempty:" << seqlist.isEmpty() << endl;

	seqlist.insertElem(1, 100);
	cout << "length:" << seqlist.length() << endl;
	cout << "isempty:" << seqlist.isEmpty() << endl;

	cout << seqlist.locateElem(100) << endl;

	int val;
	seqlist.getElem(1, val);
	cout << val << endl;

	seqlist.clear();
	cout << "length:" << seqlist.length() << endl;
	cout << "isempty:" << seqlist.isEmpty() << endl;
}
```

运行结果：

```
output:100 100 100 100 100
output:100 100 100 100
4
output:
length:0
isempty:1
length:1
isempty:0
1
100
length:0
isempty:1
```

## 顺序表的性能分析

**插入和删除的时间复杂度：**

最好的情况：插入或删除最后一个位置，不需要移动元素，此时时间复杂度为O(1)。

最坏的情况：插入或删除第一个位置，需要将所有的元素后移，此时时间复杂度为O(n)。

平均的情况：由于元素插入或删除第i个位置，每个元素需要移动n-i个位置，靠前移动元素多，靠后移动元素少，平均移动次数和最中间移动次数相等，平均时间复杂度为 (n-1)/2，也就是 O(n);

**存数据和取数据时的复杂度：**

因为数组可以用下标访问，所以不管什么时候，时间复杂度都是O(1);

所以顺序表适合元素个数不太变化，更多是存取数据的应用。

## 顺序表的优缺点

优点：

- 无须为表中元素之间的逻辑关系增加额外的存储空间，直接用数组存储即可。
- 可以快速的存取表中任一位置元素。

缺点：

- 插入和删除操作需要移动大量元素，效率低。
- 当前线性表长度变化较大时，不好确定存储空间的容量，可能造成空间浪费。分大了，浪费，分小了，容易发生上溢。

## 顺序表扩展

1. 在C++标准库中，与顺序表对应的就是std::vector，我们现在实现的顺序表只是用来理解顺序表的概念和实现原理，因为太过于局限基本不会真正使用，在工作或生产环境下，基本都会使用用stl标准库实现的最通用且最完整的std::vector。

2. 代码中，仅仅实现了静态存储方式，并没有实现动态存储方式，因为两种方式基本接口都一样，只是动态存储时，数组是动态申请的，当数组没有容量时，需要重新申请一块更大的内存空间，让原内存数据拷贝到新内存里，再释放原内存。至于扩容多大，std::vector中采用2倍扩容。


---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjjg/%E9%A1%BA%E5%BA%8F%E8%A1%A8/  

