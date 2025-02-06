##### 网络
Kafka 使用基于 TCP 的二进制协议。

**生产者**
创建 TCP 连接：
 - 在 Producer 实例创建后，建立和在 bootstrap.servers 中配置的Broker 的 TCP 连接，向 Broker 发送 META 请求，获取集群元数据。
 - 集群元数据中没有创建连接的 Broker 建立连接
 - 发消息时，向没有创建连接的 Broker 建立连接
关闭 TCP 连接：
-  connections.max.idle.ms 配置，默认9分钟，由 Broker 关闭 

**消费者**
创建 TCP 连接：
- 查找协调者：向负载最小 Broker，查询哪个 Broker 是它的协调者
- 连接协调者：连接后才能加入消费组
- 消费数据：连接要消费分区的 Broker

关闭 TCP 连接：
- connection.max.idle.ms 配置，默认9分钟，由 Broker 关闭

##### 分区和引导
客户端通过引导 Broker 查询当前集群状态，并控制发送消息到分区，
-  bootstrap.servers 中配置
- 分区能够负载均衡，语义分区，并保障顺序