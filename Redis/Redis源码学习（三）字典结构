### 参考文章：http://blog.csdn.net/men_wen/article/details/69787532

### 1.介绍
字典又称符号表(symbol table)、关联数组(associative array)或映射(map)，是一种用于保存键值对(key-value pair)的抽象数据结构。例如：redis中所有key到
value的映射，还有hash类型的键值，就是通过字典结构维护。


### 2.字典的实现
redis的字典是由hash table实现的，一个hash table有多个节点，每个节点保存一个键值对。

#### 2.1 哈希表
redis中哈希表定义在dict.h/dictht中。
  typedef struct dictht {
      dictEntry **table;    //存放一个数组的地址，数组存放着哈希表节点dictEntry的地址。
      unsigned long size;    //哈希表table的大小，初始化大小为4
      unsigned long sizemask; //用于将哈希值映射到table的位置索引，它的值总是等于(size-1)。
      unsigned long used;   //记录哈希表已有的节点（键值对）数量。
  } dictht;
  
#### 2.2 哈希表节点
哈希表的table指向的数组存放着dictEntry的地址。也定义在dict.h/dictEntryt中。
    typedef struct dictEntry {
        void *key;   //key
        union {
            void *val;
            uint64_t u64;
            int64_t s64;
            double d;
        } v;       //value
        struct dictEntry *next;    //指向下一个hash节点，用来解决hash冲突
    } dictEntry;
    
#### 2.3 字典
字典结构定义在dict.h/dict中
    typedef struct dict {
        dictType *type;    //指向dictType结构，dictType结构中包含自定义的函数，这些函数使得key和value能存储任何类型的数据
        void *privadata;   //私有数据，保存dictType结构中函数的参数
        dictht ht[2];      //2张哈希表
        long rehashidx;    //rehash的标记，rehashidx==-1,表示没在进行rehash
        int iterators;     //正在迭代的迭代器数量
   } dict;
   
dictType中保存着操作字典不同类型key和value的方法  的指针。
    typedef struct dictType {
        unsigned int (*hashFunction)(const void *key);    //计算hash值的函数
        void *(*keyDup)(void *privdata, const void *key); //复制key的函数
        void *(*valDup)(void *privdata, const void *obj); //复制value的函数
        int (*keyCompare)(void *privdata, const void *key1, const void *key2); //比较key的函数
        void (*keyDestructor)(void *privdata, void *key); //销毁key的析构函数
        void (*valDestructor)(void *privdata, void *obj); //销毁value的析构函数
    } dictType;
   
