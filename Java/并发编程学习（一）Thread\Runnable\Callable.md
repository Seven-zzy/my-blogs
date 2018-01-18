### 概述
在Java中，jdk1.5以前，创建线程有2种方法，涉及如下类和接口：


#### java.lang.Runnable，since JDK 1.0
在JDK1.8中，Runnable接口的声明加了@FunctionalInterface注解，告诉编译器这是一个函数式接口，函数式接口中只能有一个抽象方法（即SAM接口，Single Abstract Method接口）。
可用lamda表达式代替原来的匿名类。具体区别后面再讲。

方法1：实现Runnable接口，重写run方法。
```java
//集成Runnable，在run方法中实现线程处理
public class PrimeRunnable implements Runnable
{
    long minPrime;
    
    public PrimeRunnable(long minPrime)
    {
        this.minPrime = minPrime;
    }
    
    @Override
    public void run()
    {
    //TO DO:
    }
}

public class TestThread()
{
    public static void main(String[] args)
    {
        PrimeRunnable myRunnable = new PrimeRunnable(123);
        //启动线程
        new Thread(myRunnable).start();
}
```
注意这里Runnable的run方法返回值是void。与下面要提到的Callable的区别即在返回值类型。
在Runnable接口的注释中，提到了：
@see     java.lang.Thread
@see     java.util.concurrent.Callable
接下来就分别看一下Thread和Callable。


#### java.lang.Thread,since JDK 1.0
Thread类实现了Runnable接口，通过继承Thread类创建线程的代码如下：

方法2：继承Thread类
```java
class PrimeThread extends Thread
{
    private long minPrime;
    
    public PrimeThread(long minPrime)
    {
        this.minPrime = minPrime;
    }
    
    @Override
    public void run()
    {
    //TO DO:
    }
}

//创建线程
PrimeThread pthread = new PrimeThread(123);
pthread.start();
```
与实现Runnable接口比较，继承Thread也要重新run方法，在run方法中实现线程处理。不同的是创建线程的地方，实现Runnable接口直接把Runnable作为参数传给Thread构造函数。


#### java.util.concurrent.Callable,since JDK 1.5
从jdk 1.5开始，jdk提供了concurrent包下的一系列类和接口，用以帮助程序员更好地使用线程，进行线程池管理。
首先看一下Callable接口的声明，也是一个函数式接口，@FunctionalInterface，很简单，只有一个call接口。
```java
@FunctionalInterface
public interface Callable(V)
{
    V call() throws Exception;
}
```
Callable接口类似于Runnable，都是为了其实例能够被另一个线程执行。与Runnable接口的区别是，call方法可以返回计算结果，并且有异常抛出。
以下是JDK的注释：
The {@code Callable} interface is similar to {@link
 * java.lang.Runnable}, in that both are designed for classes whose
 * instances are potentially executed by another thread.  A
 * {@code Runnable}, however, does not return a result and cannot
 * throw a checked exception.


#### Lamda表达式
我们前文提到了Lamda表达式。由于Java是面向对象的语言，在jdk1.8以前是不能直接把功能实现直接作为方法变量传递的。如果想实现类似功能，就要用到匿名类。
比如通过实现Runnable接口来创建线程，在jdk 1.8以前，我们还可以这样写：
```java
new Thread(new Runnable() {
    @Override
    public void run()
    {
    //TODO:
    }
}).start();
```
其实，我们关注和需要的只是TODO的内容，却不得不额外写了一堆代码。有了lamda表达式，我们可以这样实现：
```java
new Thread(() -> {
            //TODO:
        }).start();
```
是不是清爽多了。
简单介绍下Lamda表达式语法：
(args) -> expression;
(args) -> { statements;}

我个人的理解，把一个Lamda表达式看做一个方法，圆括号中的内容为参数，若能推断出参数类型，可以不显示声明参数类型。大括号中的内容为方法实现。
jdk1.8把Lamda表达式作为一个对象传递。

更多Lamda表达式的内容，可以参考：
https://segmentfault.com/a/1190000009186509



