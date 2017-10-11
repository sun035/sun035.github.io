---
layout: post
title: arrayList学习
description: 学习arrayList源码
category: blog
---

arrayList简介
---

arrayList基于数组实现，优点在于其查询搜索快，但由于其增长是通过扩容机制，当容量不够时会创新一个新的数组并将数据进行复制
所以当我们要存储的数据量比较大时最好给定数组长度，进行删除的时候也需要对数据进行移动，删除和增加会对性能有直接影响
只看几个比较重要的方法



add方法
---

	 public boolean add(E e) {
			ensureCapacityInternal(size + 1);  // Increments modCount!!
			elementData[size++] = e;
			return true;
		}

	  private void ensureCapacityInternal(int minCapacity) {
			if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {//判断是否为空 为空默认是10
				minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
			}

			ensureExplicitCapacity(minCapacity);
		}

		private void ensureExplicitCapacity(int minCapacity) {
			modCount++;//fastfall机制

			// overflow-conscious code
			if (minCapacity - elementData.length > 0)
				grow(minCapacity);//主要是这里
		}

	private void grow(int minCapacity) {
			// overflow-conscious code
			int oldCapacity = elementData.length; // 目前数组大小
			// 新容量为oldCapacity加上oldCapacity除以2（即>>右移位运算），这个是自动扩展数组大小的算法
			// 当newCapacity大于minCapacity时并且没有到达数组最大值，都会将数组扩展为newCapacity大小
			int newCapacity = oldCapacity + (oldCapacity >> 1);
			if (newCapacity - minCapacity < 0)
				newCapacity = minCapacity; // 如果newCapacity小于指定的minCapacity，newCapacity取minCapacity
			if (newCapacity - MAX_ARRAY_SIZE > 0)
				newCapacity = hugeCapacity(minCapacity);
			// minCapacity is usually close to size, so this is a win:
			elementData = Arrays.copyOf(elementData, newCapacity);//存到新的数组中
		}



remove方法
---


	 public E remove(int index) {
			rangeCheck(index);

			modCount++;
			E oldValue = elementData(index);

			int numMoved = size - index - 1;
			if (numMoved > 0)
				System.arraycopy(elementData, index+1, elementData, index,
								 numMoved); // 将从index+1开始的元素向前移动一个位置
			elementData[--size] = null; // clear to let GC do its work

			return oldValue;
		}


		
		
		


[Mukosame]:    http://sun035.github.io  "Mukosame"
