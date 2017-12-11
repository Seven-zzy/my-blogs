### 参考文章：http://blog.csdn.net/men_wen/article/details/69215222

### 1. redis中链表结构为双向链表，因此，在头部和尾部进行的操作就会非常快。
### 2. list的底层实现**之一**就是链表。
### 3. 链表的实现
#### 3.1 链表节点
    每个链表节点由adlist.h/listNode表示
     typedef struct listNode {
        struct listNode *prev;  //前驱节点，如果是list的头节点，则prev指向NULL
        struct listNode *next;  //后继节点，如果是list的尾节点，则prev指向NULL
        void *value;   //万能指针，能够存放任何信息
     } listNode;

    使用双向链表的好处：获取某个节点的前驱节点和后继节点的复杂度为O(1)。
#### 3.2 表头
    表头用于存放上面双向链表的信息，由adlist.h/list结构表示：
    typedef struct list {
        listNode *head;   //链表头节点指针
        listNode *tail;   //链表尾节点指针
        
        //以下三个函数指针类似类中的成员函数
        void *(*dup)(void *ptr);   //复制链表节点保存的值
        void *(*free)(void *ptr);  //释放链表节点保存的值
        int (*match)(void *ptr, void *key);   //比较链表节点所保存的节点值和另一个输入值是否相等
        unsigned long len;    //链表长度
    } list;
    
    使用list表头管理链表的好处：
        head和tail指针：对于链表头节点和尾节点操作的复杂度为O(1)；
        len长度计数器：获取链表节点数量的复杂度为O(1);
        dup、free和match指针：**实现多态**，listNode使用万能指针void*保存节点的值，list表头使用dup、free、match指针来针对链表中存放的不同对象实现不同的方法。
