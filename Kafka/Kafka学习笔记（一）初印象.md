#### 为什么使用kafka
##### 解耦：很难在一开始就预测到所有的需求，所以约定相同的接口便可以独立扩展或修改生产者或消费者。
##### 易于扩展：解耦后，只需要增加消费group即可扩展消费。
##### 消息顺序保证：kafka可以保证单Partition内的消息顺序。所以如果要顺序消费，topic不能分区。我们的做法是单分区多副本。
##### 可恢复性：系统的部分组件失效时，不会影响到整个系统。即使一个处理消息的进程挂掉，在进程恢复后继续消费即可。
##### 异步通信：允许用户把消息放入队列，但不立即处理。
##### 缓存：消息队列通过一个缓冲层来帮助任务最高效率的执行。
##### 灵活性/峰值处理能力：不可能所有资源按照峰值预留。在访问量剧增时，消息队列能够使关键组件顶住突发的访问压力，不至于因为突发的超负荷而挂掉。


#### kafka的顺序写磁盘，时间复杂度为O(1)。即使是TB级别的数据，也能保证常数时间复杂度的访问性能。

#### kafka高吞吐率通过多Partition来保证和扩展：每个Topic可包含一个或多个Partition（单Partition内消息可保证顺序），每个Partition在物理上对应一个文件夹。
如果把Topic的所有消息保存在一个节点，那么这个节点的IO效率有可能成为瓶颈。而有了Partition，不同的消息可以并行写入不同broker的不同Partition中，极大
地提高了吞吐率。每个Partition存储了这个Partition的所有消息和索引。

#### 每个日志文件都是一个log entires序列。这个log entries又分成多个segment，每个segment以第一条消息的offset命名，以.kafka为文件后缀。
另外还有一个索引文件，保存了每个segment上包含的log entry的offset范围。

#### 因为索引文件的存在，kafka读取任意消息的时间复杂度都是O(1)，与文件大小无关。因此删除过期消息并不能提高读取效率。

#### 消息过期策略：时间（1周），或Partition文件大小（1G）。删除segment List头的数据。

#### Producer消息路由：可以通过制定key，和相应的Partition策略，决定一条消息会写入哪一个Partition。若Partition机制合理，所有消息被均匀写入不同的Partition
中，可以达到负载均衡的效果。

#### broker无状态：broker不保存消息的消费状态。消息消费时，会带有当前消息的offset，一般Consumer消费消息都是把offset递增。也可以通过减小offset实现重复
消费。这样不需要broker来保证同一个Consumer group只有一个consumer消费，也不需要加锁。为高吞吐率提供了保证。


### Consumer Group
#### 使用Consumer high level API时，每个consumer group的消息只能被一个consumer消费。多个consumer group可重复消息这一消息。

#### consumer group是kafka实现单播和广播的手段。单播即所有Consumer在同一个group里，广播则是每个consumer有一个独立的group。

### Push VS Pull
各有优劣。Push模式很难适配不同消费者的消费速度。Push模式的目标是尽可能快的传递消息，因此可能导致consumer来不及处理消息，造成拒绝服务或网络拥塞。
Pull的好处就是各consumer自己决定消费速度和规模(可以批量消费，也可以逐条消费)。还能选择不同的提交方式从而传输不同的语义。

### kafka集群中broker的关系：
非主从，各broker在集群中地位一样。可以随意增加或删除任意一个broker节点。
### 负载均衡
对于kafka 0.7.X依靠zookeeper实现负载均衡，0.8.x提供了一个metadata API来管理broker之间的负载。


