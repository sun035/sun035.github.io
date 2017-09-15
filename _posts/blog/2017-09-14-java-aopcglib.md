---
layout: post
title: java 动态代理 cglib 
description: java 基于cglib的代理
category: blog
---


![class简介](https://raw.githubusercontent.com/sun035/images/master/class.png)

基于jdk实现的动态代理主要是依赖于实现业务类所需要的接口，并继承proxy类，最终通过handler
转发反射调用方法，在内存中生成的代理类是final的。

没想到cglib的实现比jdk更复杂些....通过网络慢慢学习先初步理解大概


一、cglib 动态代理示例
---
  

``` java ```


``` java
 public class Target{
    public void f(){
        System.out.println("Target f()");
    }
    public void g(){
        System.out.println("Target g()");
    }
}

public class Interceptor implements MethodInterceptor {
    @Override
    public Object intercept(Object obj, Method method,Object[] args,MethodProxy proxy)
	throws Throwable {
        System.out.println("I am intercept begin");
	//Note: 此处一定要使用proxy的invokeSuper方法来调用目标类的方法
        proxy.invokeSuper(obj, args);
        System.out.println("I am intercept end");
        return null;
    }
}

public class Test {
    public static void main(String[] args) {
    System.setProperty(DebuggingClassWriter.DEBUG_LOCATION_PROPERTY, "F:\\code");
         //实例化一个增强器，也就是cglib中的一个class generator
        Enhancer eh = new Enhancer();
         //设置目标类
        eh.setSuperclass(Target.class);
        // 设置拦截对象
        eh.setCallback(new Interceptor());
        // 生成代理类并返回一个实例
        Target t = (Target) eh.create();
        t.f();
        t.g();
    }
}


``` 
通过cglib无需实现代理类
1.首先实例化一个字节码增强器
2.设置目标类
3.设置拦截对象
4.生成代理类并返




二、分析生成代理类代码






``` java ```


``` java
public class Target$$EnhancerByCGLIB$$788444a0 extends Target implements Factory
{
    private boolean CGLIB$BOUND;
    private static final ThreadLocal CGLIB$THREAD_CALLBACKS;
    private static final Callback[] CGLIB$STATIC_CALLBACKS;
    private MethodInterceptor CGLIB$CALLBACK_0;
    private static final Method CGLIB$g$0$Method;
    private static final MethodProxy CGLIB$g$0$Proxy;
    private static final Object[] CGLIB$emptyArgs;
    private static final Method CGLIB$f$1$Method;
    private static final MethodProxy CGLIB$f$1$Proxy;
    
    static void CGLIB$STATICHOOK1()
    {
      CGLIB$THREAD_CALLBACKS = new ThreadLocal();
      CGLIB$emptyArgs = new Object[0];
      Class localClass1 = Class.forName("net.sf.cglib.test.Target$$EnhancerByCGLIB$$788444a0");
      Class localClass2;
      Method[] tmp60_57 = ReflectUtils.findMethods(new String[] { "g", "()V", "f", "()V" }, (localClass2 = Class.forName("net.sf.cglib.test.Target")).getDeclaredMethods());
      CGLIB$g$0$Method = tmp60_57[0];
      CGLIB$g$0$Proxy = MethodProxy.create(localClass2, localClass1, "()V", "g", "CGLIB$g$0");
      CGLIB$f$1$Method = tmp60_57[1];
      CGLIB$f$1$Proxy = MethodProxy.create(localClass2, localClass1, "()V", "f", "CGLIB$f$1");
    }
    
    final void CGLIB$g$0()
    {
      super.g();
    }
    
    public final void g()
    {
      MethodInterceptor tmp4_1 = this.CGLIB$CALLBACK_0;
      if (tmp4_1 == null)
      {
          CGLIB$BIND_CALLBACKS(this);
          tmp4_1 = this.CGLIB$CALLBACK_0;
      }
      if (this.CGLIB$CALLBACK_0 != null) {
          tmp4_1.intercept(this, CGLIB$g$0$Method, CGLIB$emptyArgs, CGLIB$g$0$Proxy);
      }
      else{
          super.g();
      }
    }
}


``` 


分析代码



``` java ```


``` java
   public final void g()
    {
      MethodInterceptor tmp4_1 = this.CGLIB$CALLBACK_0;
      if (tmp4_1 == null)
      {
          CGLIB$BIND_CALLBACKS(this);
          tmp4_1 = this.CGLIB$CALLBACK_0;
      }
      if (this.CGLIB$CALLBACK_0 != null) {
          tmp4_1.intercept(this, CGLIB$g$0$Method, CGLIB$emptyArgs, CGLIB$g$0$Proxy);
      }
      else{
          super.g();
      }
    }

``` 

首先判断是否实现了拦截对象 没有的话则去 CGLIB$BIND_CALLBACKS(this);查找是否存在




``` java ```


``` java
private static final void CGLIB$BIND_CALLBACKS(Object o){
        Target$$EnhancerByCGLIB$$788444a0 temp_1 = (Target$$EnhancerByCGLIB$$788444a0)o;
        Object temp_2;
        Callback[] temp_3
        if(temp_1.CGLIB$BOUND == true){
            return;
        }
        temp_1.CGLIB$BOUND = true;
        temp_2 = CGLIB$THREAD_CALLBACKS.get();
        if(temp_2!=null){
            temp_3 = (Callback[])temp_2;
        }
        else if(CGLIB$STATIC_CALLBACKS!=null){
            temp_3 = CGLIB$STATIC_CALLBACKS;
        }
        else{
            return;
        }
        temp_1.CGLIB$CALLBACK_0 = (MethodInterceptor)temp_3[0];
        return；
    }

```
CGLIB$BIND_CALLBACKS 先从CGLIB$THREAD_CALLBACKS中get拦截对象，

如果获取不到的话，再从CGLIB$STATIC_CALLBACKS来获取，如果也没有则认为该方法不需要代理。

那么拦截对象是如何设置到CGLIB$THREAD_CALLBACKS 

网上解答：


Enhancer：firstInstance

Enhancer：createUsingReflection

Enhancer：setThreadCallbacks

Enhancer：setCallbacksHelper

Target$$EnhancerByCGLIB$$788444a0 ： CGLIB$SET_THREAD_CALLBACKS

在第5步，调用了代理类的CGLIB$SET_THREAD_CALLBACKS来完成拦截对象的注入。




接下来有意思的是 

tmp4_1.intercept(this, CGLIB$g$0$Method, CGLIB$emptyArgs, CGLIB$g$0$Proxy);

不过在这之前先看看第二个参数和第四个参数是的创建，与上边对应的是


``` java ```


``` java
  Method[] tmp60_57 = ReflectUtils.findMethods(new String[] { "g", "()V", "f", "()V" }, (localClass2 = Class.forName("net.sf.cglib.test.Target")).getDeclaredMethods());
  CGLIB$g$0$Method = tmp60_57[0];
  CGLIB$g$0$Proxy = MethodProxy.create(localClass2, localClass1, "()V", "g", "CGLIB$g$0");
  CGLIB$f$1$Method = tmp60_57[1];
  CGLIB$f$1$Proxy = MethodProxy.create(localClass2, localClass1, "()V", "f", "CGLIB$f$1");
  
  
``` 

ReflectUtils.findMethods的功能类似于校验，最终返回的是代理之前原对象的方法

MethodProxy.create(localClass2, localClass1, "()V", "g", "CGLIB$g$0");是对

代理方法的初始化，这里有一点，在jdk中是通过反射的方式调用方法，而cglib不同的

是它创建了对原对象方法的索引，通过索引的方式调用以提高效率，看代码：





``` java ```


``` java

public static MethodProxy create(Class c1, Class c2, String desc, String name1, String name2) {
        MethodProxy proxy = new MethodProxy();
        proxy.sig1 = new Signature(name1, desc);
        proxy.sig2 = new Signature(name2, desc);
        proxy.createInfo = new CreateInfo(c1, c2);
        return proxy;
    }
	
	
	
public Object invokeSuper(Object obj, Object[] args) throws Throwable {
        try {
            init();
            FastClassInfo fci = fastClassInfo;
            return fci.f2.invoke(fci.i2, obj, args);
        } catch (InvocationTargetException e) {
            throw e.getTargetException();
        }
    }	
	
	
	
private static class FastClassInfo
    {
        FastClass f1;
        FastClass f2;
        int i1;
        int i2;
    }
  
	
	

``` 

在生成代理类对象的时候执行静态块的代码初始化参数的对象

在回调方法的时候调用invokeSuper，会进行初始化FastClassInfo（第一次）



关于这个有一个案例：




``` java ```


``` java

public class test10 {
    public static void main(String[] args){
        Test tt = new Test();
        Test2 fc = new Test2();
        int index = fc.getIndex("f()V");
        fc.invoke(index, tt, null);
    }
}

class Test{
    public void f(){
        System.out.println("f method");
    }
    
    public void g(){
        System.out.println("g method");
    }
}
class Test2{
    public Object invoke(int index, Object o, Object[] ol){
        Test t = (Test) o;
        switch(index){
        case 1:
            t.f();
            return null;
        case 2:
            t.g();
            return null;
        }
        return null;
    }
    
    public int getIndex(String signature){
        switch(signature.hashCode()){
        case 3078479:
            return 1;
        case 3108270:
            return 2;
        }
        return -1;
    }
}	
	

``` 	

上例中，Test2是Test的Fastclass，在Test2中有两个方法getIndex和invoke。

在getIndex方法中对Test的每个方法建立索引，并根据入参（方法名+方法的描述符）

来返回相应的索引。Invoke根据指定的索引，以ol为入参调用对象O的方法。

这样就避免了反射调用，提高了效率。


具体在创建实例当中我发现每一个方法都会构建对象....不知道这个对象是创建的单个方法还是整个类
 











[Mukosame]:    http://sun035.github.io  "Mukosame"
