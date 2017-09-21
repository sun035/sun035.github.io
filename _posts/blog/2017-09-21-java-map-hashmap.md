---
layout: post
title: hashMap学习
description: 学习hashMap的内容
category: blog
---



位运算
---

Java提供的位运算符有：左移( << )、右移( >> ) 、无符号右移( >>> ) 、位与( & ) 、位或( | )、位非( ~ )、位异或( ^ )，除了位非( ~ )是一元操作符外，其它的都是二元操作符。

1、左移( << )

Test1、将5左移2位：

	[java] view plain copy
	package com.xcy;  
	  
	public class Test {  
		public static void main(String[] args) {  
			System.out.println(5<<2);//运行结果是20  
		}  
	}  
运行结果是20，但是程序是怎样执行的呢？
首先会将5转为2进制表示形式(java中，整数默认就是int类型,也就是32位):

0000 0000 0000 0000 0000 0000 0000 0101           然后左移2位后，低位补0：

0000 0000 0000 0000 0000 0000 0001 0100           换算成10进制为20

2、右移( >> ) ，右移同理，只是方向不一样罢了(感觉和没说一样)

	[java] view plain copy
	System.out.println(5>>2);//运行结果是1  
	
还是先将5转为2进制表示形式：

0000 0000 0000 0000 0000 0000 0000 0101 然后右移2位，高位补0：

0000 0000 0000 0000 0000 0000 0000 0001

3、无符号右移( >>> )

我们知道在Java中int类型占32位，可以表示一个正数，也可以表示一个负数。正数换算成二进制后的最高位为0，负数的二进制最高为为1

例如  -5换算成二进制后为：

1111 1111 1111 1111 1111 1111 1111 1011   (刚开始接触二进制时，不知道最高位是用来表示正负之分的，当时就总想不通。。明明算起来得到的就是一个正数-_-)

我们分别对5进行右移3位、 -5进行右移3位和无符号右移3位：

	[java] view plain copy
	package com.xcy;  
	  
	public class Test {  
		public static void main(String[] args) {  
			System.out.println(5>>3);//结果是0  
			System.out.println(-5>>3);//结果是-1  
			System.out.println(-5>>>3);//结果是536870911  
		}  
	}  


我们来看看它的移位过程(可以通过其结果换算成二进制进行对比)：

5换算成二进制： 0000 0000 0000 0000 0000 0000 0000 0101

5右移3位后结果为0，0的二进制为： 0000 0000 0000 0000 0000 0000 0000 0000        // (用0进行补位)

 -5换算成二进制： 1111 1111 1111 1111 1111 1111 1111 1011
 
-5右移3位后结果为-1，-1的二进制为： 

1111 1111 1111 1111 1111 1111 1111 1111   // (用1进行补位)

-5无符号右移3位后的结果 536870911 换算成二进制： 

0001 1111 1111 1111 1111 1111 1111 1111   // (用0进行补位)


通过其结果转换成二进制后，我们可以发现，正数右移，高位用0补，负数右移，高位用1补，当负数使用无符号右移时，用0进行部位(自然而然的，就由负数变成了正数了)

注意：笔者在这里说的是右移，高位补位的情况。正数或者负数左移，低位都是用0补。(自行测试)



4、位与( & )

	[java] view plain copy
	package com.xcy;  
	  
	public class Test {  
		public static void main(String[] args) {  
			System.out.println(5 & 3);//结果为1  
		}  
	}  
	
还是老套路，将2个操作数和结果都转换为二进制进行比较：

5转换为二进制：0000 0000 0000 0000 0000 0000 0000 0101

3转换为二进制：0000 0000 0000 0000 0000 0000 0000 0011

-------------------------------------------------------------------------------------

1转换为二进制：0000 0000 0000 0000 0000 0000 0000 0001

位与：第一个操作数的的第n位于第二个操作数的第n位如果都是1，那么结果的第n为也为1，否则为0



5、位或( | )

	[java] view plain copy
	package com.xcy;  
	  
	public class Test {  
		public static void main(String[] args) {  
			System.out.println(5 | 3);//结果为7  
		}  
	}  
	
5转换为二进制：0000 0000 0000 0000 0000 0000 0000 0101

3转换为二进制：0000 0000 0000 0000 0000 0000 0000 0011

-------------------------------------------------------------------------------------


7转换为二进制：0000 0000 0000 0000 0000 0000 0000 0111

位或操作：第一个操作数的的第n位于第二个操作数的第n位 只要有一个是1，那么结果的第n为也为1，否则为0

6、位异或( ^ )

	[java] view plain copy
	package com.xcy;  
	  
	public class Test {  
		public static void main(String[] args) {  
			System.out.println(5 ^ 3);//结果为6  
		}  
	}  
	
5转换为二进制：0000 0000 0000 0000 0000 0000 0000 0101

3转换为二进制：0000 0000 0000 0000 0000 0000 0000 0011

-------------------------------------------------------------------------------------


6转换为二进制：0000 0000 0000 0000 0000 0000 0000 0110

位异或：第一个操作数的的第n位于第二个操作数的第n位 相反，那么结果的第n为也为1，否则为0



7、位非( ~ )           位非是一元操作符

	[java] view plain copy
	package com.xcy;  
	  
	public class Test {  
		public static void main(String[] args) {  
			System.out.println(~5);//结果为-6  
		}  
	}  

 5转换为二进制：0000 0000 0000 0000 0000 0000 0000 0101
 
-------------------------------------------------------------------------------------

-6转换为二进制：1111 1111 1111 1111 1111 1111 1111 1010

位非：操作数的第n位为1，那么结果的第n位为0，反之。



由位运算操作符衍生而来的有：

&= 按位与赋值

|=  按位或赋值

^= 按位非赋值

>>= 右移赋值

>>>= 无符号右移赋值

<<= 赋值左移


和 += 一个概念而已。


举个例子：

	[java] view plain copy
	package com.xcy;  
	  
	public class Test {  
		public static void main(String[] args) {  
			int a = 5  
			a &= 3;  
			System.out.println(a);//结果是1  
		}  
	}  																		




hashMap学习
---

1.7以前hashmap的结构是链表的数组的形式

java8后对hashmap进行优化，主要为扩容后的处理链表大于8后将转化为红黑树




	/**
     * The table, initialized on first use, and resized as
     * necessary. When allocated, length is always a power of two.
     * (We also tolerate length zero in some operations to allow
     * bootstrapping mechanics that are currently not needed.)
     */
    transient Node<K,V>[] table;

	
	
	static class Node<K,V> implements Map.Entry<K,V> {
			final int hash;    //用来定位数组索引位置
			final K key;
			V value;
			Node<K,V> next;   //链表的下一个node

			Node(int hash, K key, V value, Node<K,V> next) { ... }
			public final K getKey(){ ... }
			public final V getValue() { ... }
			public final String toString() { ... }
			public final int hashCode() { ... }
			public final V setValue(V newValue) { ... }
			public final boolean equals(Object o) { ... }
	}	


hashMap根据哈希表来进行定位，以数组加链表的形式，为解决冲突 根据对象的hashcode进行进行hash算法（高位和取模运算）
如果相同的key发生了hash碰撞，将会进行链表存储，好的hash算法和扩容机制对在桶中的定位起到决定作用，提高数据的均匀分布


table初始为16，为提高性能hashmap会根据算法进行扩容，当数据>负载因子(默认0.75)*table容量会进行扩容为之前的两倍，其实就是
新增一个两倍的数组并将原来的数据进行复制



定位hash数组索引位置
--



	方法一：
	static final int hash(Object key) {   //jdk1.8 & jdk1.7
		 int h;
		 // h = key.hashCode() 为第一步 取hashCode值
		 // h ^ (h >>> 16)  为第二步 高位参与运算
		 return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
	}
	方法二：
	static int indexFor(int h, int length) {  //jdk1.7的源码，jdk1.8没有这个方法，但是实现原理一样的
		 return h & (length-1);  //第三步 取模运算
	}

这里的Hash算法本质上就是三步：取key的hashCode值、高位运算、取模运算


![hashmap算法图例](https://raw.githubusercontent.com/sun035/images/master/hashMap%E5%93%88%E5%B8%8C%E7%AE%97%E6%B3%95%E4%BE%8B%E5%9B%BE.png)		



		public V put(K key, V value) {
			return putVal(hash(key), key, value, false, true);
		}

		
		final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
					   boolean evict) {
			Node<K,V>[] tab; Node<K,V> p; int n, i;
				tab为空则创建
			if ((tab = table) == null || (n = tab.length) == 0)
				n = (tab = resize()).length;
				为null说明没有发生冲突，正常存
			if ((p = tab[i = (n - 1) & hash]) == null)
				tab[i] = newNode(hash, key, value, null);
			else {
				Node<K,V> e; K k;
				//存在冲突但是key是相同的情况
				if (p.hash == hash &&
					((k = p.key) == key || (key != null && key.equals(k))))
					e = p;
				为树结构时	
				else if (p instanceof TreeNode)
					e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
				else {
				//链表
					for (int binCount = 0; ; ++binCount) {
						if ((e = p.next) == null) {
							p.next = newNode(hash, key, value, null);
							数量大于限制转化为红黑树
							if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
								treeifyBin(tab, hash);
							break;
						}
						覆盖相同的key
						if (e.hash == hash &&
							((k = e.key) == key || (key != null && key.equals(k))))
							break;
						p = e;
					}
				}
				if (e != null) { // existing mapping for key
					V oldValue = e.value;
					if (!onlyIfAbsent || oldValue == null)
						e.value = value;
					afterNodeAccess(e);
					return oldValue;
				}
			}
			++modCount;
			if (++size > threshold)
				resize();
			afterNodeInsertion(evict);
			return null;
		}

		
		
		
resize方法	
---
		
resize方法扩充HashMap的时候，不需要像JDK1.7的实现那样重新计算hash，只需要看看原来的hash值新增的那个bit是1还是0就好了
，是0的话索引没变，是1的话索引变成“原索引+oldCap”，可以看看下图为16扩充为32的resize示意图
		
		
![resize示意图](https://raw.githubusercontent.com/sun035/images/master/jdk1.8%20hashMap%E6%89%A9%E5%AE%B9%E4%BE%8B%E5%9B%BE.png)		
		


这个设计确实非常的巧妙，既省去了重新计算hash值的时间，而且同时，由于新增的1bit是0还是1可以认为是随机的，因此resize的过程，
均匀的把之前的冲突的节点分散到新的bucket了。这一块就是JDK1.8新增的优化点。有一点注意区别，JDK1.7中rehash的时候，
旧链表迁移新链表的时候，如果在新表的数组索引位置相同，则链表元素会倒置，但是从上图可以看出，JDK1.8不会倒置。






[Mukosame]:    http://sun035.github.io  "Mukosame"
