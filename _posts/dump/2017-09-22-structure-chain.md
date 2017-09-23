---
layout: post
title: 单项链表学习
category: dump
description: 数据结构学习
---


之后将会进行数据结构的学习然后学习map结构中的LinkedMap等这里对基本的链表进行学习


链表是对象的引用指向另一个对象构成的链，是一种数据结构，与数组相比其增删比较快（查询弱log(n)）,其单项链表程序实现为

例子
---

	手写。。只是意淫一下大概过程可(yi)能(ding)有错
	public class LinkedList<T>{
		private Node<T> first;
		private Node<T> last;
		
		
		class Node<T>{
			private Node next;
			private T data;
			private Node(T data){this.data=data;n=null};
		}
		
		
		public  void add(T data){
			Node node=new Node<T>(data);
			//首先判断是否有头结点
			if(first==null){
				first=node;
				last=first;
			
			}else{
				last.next=node;
				last=node;
			}
			
		public T get(int i){
			if(first==null){
			return null;
			}
			//先知道有多少个
			if(i>length()){
				throw new RuntimeException();
			}
			Node n=first;
			int i2=0;
			while(n!=null){
				if(i=i2){
				return n.data;
				}
				i++;
				n=n.next;
			
			}
			
			
		
		}	
		
		public int length（）{
			if(first==null)return 0;
			Node e=first;
			int length;
			while(e!=null){
				length++;
				e=e.next;
			}
		
		
		}
		public boolean con(T data){
			
			while(first!=null){
				if(data.equals(first.data)){
					reture true;				
				}
				first=first.next;
				
			}
		
		}
		
		
		}
	}



翻转单项链表
---


	 public static void main(String[] args) {
			/**链表构造*/
			Node node1 = new Node(1);
			Node node2 = new Node(2);
			Node node3 = new Node(3);
			Node node4 = new Node(4);
			Node node5 = new Node(5);
			node1.next = node2;
			node2.next = node3;
			node3.next = node4;
			node4.next = node5;
			 
			show(node1);
			System.out.println("反转过程：");
			 
			Node head = reverse(node1);//反转
			System.out.println("反转后：");
			show(head);        
		}
		static void show(Node head){
			while(head != null){
				System.out.print(head.val+">");
				head = head.next;
			}
		}
		public static Node reverse(Node head){
			Node cur = head;
			Node post = head.next;
			head.next = null;
			
			show(cur);
			System.out.println();
			while(post!=null){    //初始：cur=1
				Node tmp = post;  //首次迭代：tmp = 2,第二次：tmp=3,第三次：tmp=4;
				post = post.next; //post = 3,第二次：post=4,第三次：post=5
				tmp.next = cur; //2>1,第二次：3>2>1,第三次：4>3>2>1
				cur = tmp; //链表结构：2>1,第二次：3>2>1,第三次：4>3>2>1
				show(cur);
				System.out.println();
			}
			return cur;
		}
		 
		static class Node{
			int val;
			Node next;
			Node(int val){
				this.val = val;
				//this.next = null;
			}
		}


	
		
在网上找了个容易理解的，其他的看半天把自己搞的蒙蒙的,以上这种办法思路比较清晰，一定不能死盯着屏幕看，看了半天后大脑短路 。  用程序运行下顿时清晰了很多。。。

对于链表的实现还是比较灵活的，可以指定头指针，和不指定头指针两种方式[关于有头指针和无头指针的区别](http://blog.csdn.net/hitwhylz/article/details/12305021)
	
	
	public static Node reverse(Node head){
	Node post = head.next;
	head.next = null;
	toTeverse(post,head);
	return null;
	}
	 
	private static void toTeverse(Node post, Node head) {//下一个，固定的
		if(post!=null){
			Node tem=post;
			post=post.next;//1
			tem.next=head;//2
			head=tem;
			toTeverse(post,head);
		}
		
		
	}
	
	
java传递机制
---	
	
将上面的程序改了一下在我进行测试的时候不小心将1和2的位置进行了互换 然后就发生了错误
注意这里的顺序千万不能变了！java对于引用的传递本质就是对引用的复制所以之前的错误是因为一跟二
那时候指向的对象是相等的然后改变里指向内容导致post变量所指向的内容也发生改变对于[java是值传递还是引用传递请点我](https://www.zhihu.com/question/31203609)
	
	
	
	
	
	
	









		
		


[Mukosame]:    http://sun035.github.io  "Mukosame"
