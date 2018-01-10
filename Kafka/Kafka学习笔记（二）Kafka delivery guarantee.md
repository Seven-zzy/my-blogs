### 有如下3种delivery guarantee策略：
#### At least once
#### At most once
#### Exactly once

### Producer发送消息给broker后，如果遇到网络问题导致无法判断是否成功地发送给broker，Producer可以生成一种类似主键的东西，进行幂等性的多次重试。这样即做到了
Exactly once。详细的Producer exactly once实现可以参考：http://www.cnblogs.com/jasongj/p/7912348.html

### consumer的delivery guarantee
#### consumer在读取消息后，可以进行commit。该操作会在zookeeper中保存该Consumer在该Partition中读取消息的offset。该Consumer下一次读取消息时，会从下一条
开始读。如果没有commit，下一次读取消息的offset则与上一次相同。也可以设置自动commit，如果只考虑读取消息，此时就是exactly once。
但如果是读取后没有处理就自动commit，或非自动commit，那么就是 at most once。

#### consumer读取消息处理完成再commit，当处理完成尚未commit时consumer宕机，那么consumer恢复时就会重复消费，对应at lease once。
