#### 背景：某段计算处理在做效率优化时用到了Java8的并行流parallelStream遍历List

刚开始的写法如下：
```java
ExecutorService pool = new ForkJoinPool(8);
pool.submit( () -> {
  list.parallelStream().map(node -> {
    caculateXxx(args...);
    return true;
  });
}).get();
```

但是验证时发现cacilateXxx的方法无论如何都没执行到。后来查了并行流的API文档，换了一种写法即可：
```java
pool.submit(() -> {
  list.parallemStream().forEach(node -> {
    caculatexxx(args...);
  });
}).get();
```
先记录下来，后面再深入学习一下补充原因。

原因分析参考：
https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/

流操作分为2类：
###### Intermediate:一个流可以后面跟随一个或多个Intermediate操作。其主要目的是打开流，对流做出某种程度的数据映射或过滤，然后返回一个新的流，交给下一个操作使用。这些操作都是惰性化的，也就是说，仅仅调用到这类方法，并没有开始真正的流的遍历。
Intermediate操作包括：map(mapToInt,flatMap等)、filter、distinct、sorted、peek、limit、skip、parallel、sequential、unordered


###### Terminal：一个流只能有一个Treminal操作。当这个操作执行后，流就被“用光”了，无法再被操作。所以这必定是流的最后一个操作。Terminal操作的执行，才会真正开始流的遍历，并且声称一个结果，或者一个side effect。
Termimal操作包括：forEach、forEachOrdered、toArray、reduce、collect、min、max、count、anyMatch、allMatch、noneMatch、findFirst、findAny、iterator

所以第一种调用map的方法其实并没有执行map中的方法，因为没有任何Terminal操作。
##### short-circuiting


