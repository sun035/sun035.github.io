---
layout: post
title: linkedList学习
description: 学习linkedList源码
category: blog
---

双端链表的结构图
---


![双端链表的结构图](https://raw.githubusercontent.com/sun035/images/master/%E5%8F%8C%E7%AB%AF%E9%93%BE%E8%A1%A8.png)



add方法

	public boolean add(E e) {
			linkLast(e);
			return true;
		}

其内部调用linkLast()

	void linkLast(E e) {
			final Node<E> l = last;
			final Node<E> newNode = new Node<>(l, e, null);//创建一个新节点并将它的上一个指向last
			last = newNode;//在将它本身作为最后一个
			if (l == null)
				first = newNode;
			else
				l.next = newNode;//将原本的最后节点的下一个节点改为新创建的
			size++;
			modCount++;
		}

	  private static class Node<E> {
			E item;
			Node<E> next;
			Node<E> prev;

			Node(Node<E> prev, E element, Node<E> next) {
				this.item = element;
				this.next = next;
				this.prev = prev;
			}
		}
		
add方法的结构
---

![](https://raw.githubusercontent.com/sun035/images/master/add.JPG)


当添加第一个数据时结构为
---


![](https://raw.githubusercontent.com/sun035/images/master/%E5%A2%9E%E5%8A%A0%E7%AC%AC%E4%B8%80%E4%B8%AA%E6%97%B6.png)

add重载方法


	 public void add(int index, E element) {
        checkPositionIndex(index);//验证是否越界

        if (index == size)//直接加载最后
            linkLast(element);
        else
            linkBefore(element, node(index));//中间插入，先根据node（index）定位
    }


	 Node<E> node(int index) {
			// assert isElementIndex(index);
			//这里进行了优化判断下标在前半部还是后半部
			if (index < (size >> 1)) {
				Node<E> x = first;
				for (int i = 0; i < index; i++)
					x = x.next;
				return x;
			} else {
				Node<E> x = last;
				for (int i = size - 1; i > index; i--)
					x = x.prev;
				return x;
			}
		}


	  void linkBefore(E e, Node<E> succ) {
			// assert succ != null;
			在制定的位置前进行插入
			final Node<E> pred = succ.prev;//将当前的上一个暂存
			final Node<E> newNode = new Node<>(pred, e, succ);//让新创建的节点插入到中间
			succ.prev = newNode;//将原来的节点的上一个节点更改
			if (pred == null)
				first = newNode;
			else
				pred.next = newNode;//原节点的上一个节点的下一个节点进行更改
			size++;
			modCount++;
		}
		
其插入也比较好理解，可以根据上边图将第2认为是插入根据代码理解即可



删除方法
---

	  public boolean remove(Object o) {
			if (o == null) {
				for (Node<E> x = first; x != null; x = x.next) {
					if (x.item == null) {
						unlink(x);
						return true;
					}
				}
			} else {
				for (Node<E> x = first; x != null; x = x.next) {
					if (o.equals(x.item)) {
						unlink(x);
						return true;
					}
				}
			}
			return false;
		}

对null进行特殊处理，遍历到内容后调用unlink方法进行删除		
		
		
	 E unlink(Node<E> x) {
			// assert x != null;
			final E element = x.item;
			final Node<E> next = x.next;
			final Node<E> prev = x.prev;

			if (prev == null) {//是否是头结点
				first = next;
			} else {
				prev.next = next;//改变引用将原来的上一个节点指向他的下一个节点
				x.prev = null;
			}

			if (next == null) {//同上
				last = prev;
			} else {
				next.prev = prev;
				x.next = null;
			}

			x.item = null;
			size--;
			modCount++;
			return element;
		}



iterator
---
		
对于linkedList本身就是对双向链表的增删改，源码中还实现了栈和队列的操作方法，因此也可以作为栈、队列和双端队列来使用。
LinkedList 是一个继承于AbstractSequentialList的双向链表。它也可以被当作堆栈、队列或双端队列进行操作。
实现 List 接口，能对它进行队列操作。实现 Deque 接口，即能将LinkedList当作双端队列使用。实现了Cloneable接口，
即覆盖了函数clone()，能克隆。实现java.io.Serializable接口，这意味着LinkedList支持序列化，能通过序列化去传输。同时LinkedList 是非同步的


在AbstractSequentialList中有一个抽样方法
	
	public abstract ListIterator<E> listIterator(int index);		
		
其规定的增加删除等都是根据这个抽样方法来实现的，具体定义由实现类来定义，这里我们看一下LinkedList内部类的实现



	public ListIterator<E> listIterator(int index) {
			checkPositionIndex(index);
			return new ListItr(index);
		}

		private class ListItr implements ListIterator<E> {

			
			private Node<E> lastReturned = null;
			private Node<E> next;
			private int nextIndex;
			private int expectedModCount = modCount;

			ListItr(int index) {
				// assert isPositionIndex(index);
				next = (index == size) ? null : node(index);
				nextIndex = index;
			}

			public boolean hasNext() {
				return nextIndex < size;
			}

			public E next() {
				checkForComodification();
				if (!hasNext())
					throw new NoSuchElementException();

				lastReturned = next;
				next = next.next;
				nextIndex++;
				return lastReturned.item;
			}

			public boolean hasPrevious() {
				return nextIndex > 0;
			}

			public E previous() {
				checkForComodification();
				if (!hasPrevious())
					throw new NoSuchElementException();

				lastReturned = next = (next == null) ? last : next.prev;
				nextIndex--;
				return lastReturned.item;
			}

			public int nextIndex() {
				return nextIndex;
			}

			public int previousIndex() {
				return nextIndex - 1;
			}

			public void remove() {
				checkForComodification();
				if (lastReturned == null)
					throw new IllegalStateException();

				Node<E> lastNext = lastReturned.next;
				unlink(lastReturned);
				if (next == lastReturned)
					next = lastNext;
				else
					nextIndex--;
				lastReturned = null;
				expectedModCount++;
			}

			public void set(E e) {
				if (lastReturned == null)
					throw new IllegalStateException();
				checkForComodification();
				lastReturned.item = e;
			}

			public void add(E e) {
				checkForComodification();
				lastReturned = null;
				if (next == null)
					linkLast(e);
				else
					linkBefore(e, next);
				nextIndex++;
				expectedModCount++;
			}

			final void checkForComodification() {
				if (modCount != expectedModCount)
					throw new ConcurrentModificationException();
			}
		}




[Mukosame]:    http://sun035.github.io  "Mukosame"
