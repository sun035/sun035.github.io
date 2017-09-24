---
layout: post
title: 算法 1.1.19题
category: dump
description: 递归题
---


递归函数
---

	  jvm是基于栈操作,每一个线程都有自己的操作栈，遇到方法调用的时候会开辟

	栈帧（其实帧就是内存中一段连续的空间）它含有自己的返回值,局部变量表，

	操作栈，以及对常量池的符号引用，如果是基本类型则存放在栈里的是值，

	如果是对象则存在栈里的是堆中对象的地址，递归占用内存就是因为没有释放

	当前函数的资源并且不断的占用栈的资源，直到达到条件才会一层一层返回，
	
	如果没有设计好容易导致栈溢出..


1.1.19题
---

	在计算机上运行以下程序:
	public class Fibonacci{
	
		public static long F(int N){
		
			if (N == 0) return 0;
			if (N == 1) return 1;
			return F(N-1) + F(N-2);
		}
		
		public static void main(String[] args){
			for (int N = 0; N < 100; N++)
			StdOut.println(N + " " + F(N));
		}
	}
	
计算机用这段程序在一个小时之内能够得到 F(N) 
结果的最大 N 值是多少?开发 F(N) 的一个更好的实现，用数组保存已经计算过的值。

答案
---	


	public class Testc {
		
		public static long F(int N,int []a) {
			if (N == 0){
				a[N]=0;
				return 0;
			}
			if (N == 1){
				a[N]=1;
				return 1;
			}
			
			return a[N]=a[N-1]+a[N-2];
		}

		public static void main(String[] args){
			int a[] =new int[100];
			for (int N = 0; N < 100; N++)
			StdOut.println(N + " " + F(N,a));
		}
		}

	
	溢出了我就不管了 啊哈哈哈.... 	（不过跟递归没啥关系了。。）




		
		


[Mukosame]:    http://sun035.github.io  "Mukosame"
