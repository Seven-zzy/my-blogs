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

先记录下来，后面再深入学习一下补充原因。
