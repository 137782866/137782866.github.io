# 链表


## 单链表

因为顺序表的内存空间连续，在插入和删除时，需要移动大量元素。某些应用场景下，需要频繁的插入或删除，这时候顺序表就比较消耗性能了。需要使用链表来解决。

链表是链式存储的顺序表，由多个节点依次连接起来，每个节点的内存空间一般不连续（也可以是连续），通过节点的指针域将线性表的数据元素按其逻辑次序链接在一起。

单链表中，每个节点连接其后继节点，所以在每个节点中，不仅需要存储本身的数据，还需要存储一个指向其直接后继节点的指针域。每个节点存储指针域就是链，将每个节点链起来，这就形成了单链表。节点里存储数据的地方称为**数据域**，节点里存储指针的地方成为**指针域**。

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/clipboard_20231230_100129.png)

线性表，需要有头有尾，链表也一样。

链表的第一个节点的存储位置叫做头指针，整个链表的存取一般都从头指针开始，之后的每一个节点，都是上一个节点的后继指针所指向的地址。

链表的最后一个节点，其指针域指向NULL，表示链表节点就到这里，没有后继节点了。

**头指针和头节点**

有时候为了方便对链表操作，会在单链表的第一个节点前附设一个节点，成为头节点。头节点的指针域存储指向第一个节点的指针，数据域可以根据某些场景需求自由存放各种数据，比如：可存储线性表的长度等，头节点数据与一般无意义。

头指针：

- 头指针是指向链表第一个节点的指针，若链表有头节点，则指向头节点的指针。
- 头指针具有标识作用，所以常用头指针冠以链表的名字。
- 无论链表是否为空，头指针不能为空，头指针是链表必要的元素。

头节点：

- 头节点的作用为了很多操作统一方便而设立的。比如对第一个元素节点前插入节点，和删除第一个节点，其操作和其他节点完全相同，不需要判断当前节点是第一个节点，还是非第一个节点。
- 头结点不是一个链表的必须要素，也可以不设头节点。

## 单边表的实现

### 单链表节点定义

单链表节点的定义如下：

```cpp
template <typename T>
struct ListNode
{
	T m_val;	//数据域
	ListNode* m_next; //指针域
	ListNode(const T& v) : m_val(v), m_next(NULL) {}
};
```

### 单链表的插入

插入新的节点，过程如下图：

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/clipboard_20231230_100146.png)

增加头节点，可以使头节点的插入操作和其他结点统一处理。

```cpp
s->next = p->next;
p->next = s;
```

### 单链表的删除

删除一个节点，过程如下图：

![](https://imagebed-1300052891.cos.ap-shanghai.myqcloud.com/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/clipboard_20231230_100201.png)

```c++
q = p->next;
p->next = q->next;
```

### 代码实现

```cpp
#include <cassert>
#include <iostream>
using namespace std;

template <typename T>
struct ListNode
{
	T m_data;	//数据域
	ListNode* m_next; //指针域
	ListNode(const T& v) : m_data(v), m_next(nullptr) {}
};

template <typename T>
class LinkedList
{
private:
	ListNode<T>* m_head = nullptr;

public:
	//构造函数
	LinkedList()
	{
		m_head = new ListNode<T>(T()); //头节点的数据默认值
	}

	~LinkedList()
	{
		clear();
		delete m_head;
	}

	//在链表头插入数据（头插法）
	bool insertHead(const T& e)
	{
		assert(m_head);
		ListNode<T>* new_node = new ListNode<T>(e);
		new_node->m_next = m_head->m_next;
		m_head->m_next = new_node;
		return true;
	}

	//在链表尾插入数据（尾插法）
	bool insertTail(const T& e)
	{
		ListNode<T>* pre = nullptr;
		ListNode<T>* cur = m_head;
		while (cur)
		{
			pre = cur;
			cur = cur->m_next;
		}
		if (!pre)
			return false;
		ListNode<T>* new_node = new ListNode<T>(e);
		new_node->m_next = pre->m_next;
		pre->m_next = new_node;
		return true;
	}

	//返回链表中第i个元素的数据
	bool getElem(int i, T& ret)
	{
		assert(m_head);
		ListNode<T>* p = m_head->m_next;
		int j = 1;
		while (p && j < i)
		{
			p = p->m_next;
			++j;
		}
		if (!p || j > i)
			return false;
		ret = p->m_data;
		return true;
	}

	//在单链表的第i个位置之前插入元素e，使单链表长度+1
	bool insertNode(int i, const T& e)
	{
		assert(m_head);
		ListNode<T>* p = m_head;
		int j = 1;
		while (p && j < i)
		{
			p = p->m_next;
			++j;
		}

		if (!p || j > i)
			return false;
		ListNode<T>* new_node = new ListNode<T>(e);
		if (!new_node)
			return false;
		//关键的插入操作
		new_node->m_next = p->m_next;
		p->m_next = new_node;
		return true;
	}

	//删除链表中第i个元素，并用e返回其值
	bool deleteNode(int i, T& ret)
	{
		ListNode<T>* p = m_head;
		int j = 1;
		while (p->m_next && j < i)
		{
			p = p->m_next;
			++j;
		}
		if (!(p->m_next) || j > i)
			return false;
		ListNode<T>* del_node = p->m_next; //要删除的节点
		p->m_next = del_node->m_next;
		ret = del_node->m_data;
		delete del_node;
		return true;
	}

	//清空整个链表
	bool clear()
	{
		assert(m_head);
		ListNode<T>* p = m_head->m_next;
		ListNode<T>* q = nullptr;
		while (p)
		{
			q = p->m_next;
			delete(p);
			p = q;
		}
		m_head->m_next = nullptr;
		return true;
	}

	//打印整个链表
	void output()
	{
		assert(m_head);
		cout << "output:";
		ListNode<T>* p = m_head->m_next;
		while (p)
		{
			cout << p->m_data << " ";
			p = p->m_next;
		}
		cout << endl;
	}
};

int main()
{
	LinkedList<int> linkedlist;
	linkedlist.insertNode(1, 100);
	linkedlist.insertNode(2, 200);
	linkedlist.insertNode(3, 300);
	linkedlist.insertNode(4, 400);
	linkedlist.insertNode(5, 500);
	linkedlist.output();

	linkedlist.insertHead(50);
	linkedlist.insertTail(1000);
	linkedlist.output();

	int val = 0;
	linkedlist.getElem(2, val);
	cout << "getElem2:" << val << endl;

	linkedlist.insertNode(2, 80);
	linkedlist.output();

	linkedlist.deleteNode(2, val);
	cout << "deleteNode2:" << val << endl;
	linkedlist.output();

	linkedlist.clear();
	linkedlist.output();

	return 0;
}
```

运行结果：

```
output:100 200 300 400 500
output:50 100 200 300 400 500 1000
getElem2:100
output:50 80 100 200 300 400 500 1000
deleteNode2:80
output:50 100 200 300 400 500 1000
output:
```

## 单链表的性能分析

对单链表的插入和删除时，我们只需要找到第i个位置O(n)，然后通过移动指针即可O(1)，不需要像顺序表那样还需要移动元素位置，显然对于插入或删除越频繁的操作，单链表有更多优势。 

存取数据：

- 顺序表O(1)
- 单链表O(n)

插入和删除：

- 顺序表O(n)

- 找到位置的指针后，时间复杂度仅为O(1)

若线性表需要频繁存取数据，很少进行插入和删除操作时，采用顺序表。

当线性表中的元素个数变化较大或者根本不知道有多大时，可以采用单链表。

## 单链表优缺点

优点：每个结点数据独立，不会多分配空间，造成不必要的浪费。插入和删除性能比线性表好。

缺点：每个节点增加了指针域，需要多占用一定空间。存取性能比顺序表差。



---

> 作者: WindSun  
> URL: http://blog.codepeak.cn/note/sjjg/%E9%93%BE%E8%A1%A8/  

