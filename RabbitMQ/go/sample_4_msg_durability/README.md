 ###Message durability消息持久化
 
 
- 如果服务器死机或程序 crash了，数据仍然会丢失。为了确保消息不会丢失，我们需要将queue和Message做持久化操作。
- 将durable设置为true可以做持久化处理（生产者和消息者的代码里都要设置），如果是已经存 在的一个queue 没有设置过持久化，再重新设置是不起作用的，我们需要重新为queue设置一个名字。
- 最后在Producer发布消息的时候，我们需要设置DeliveryMode为amqp.Persistent，持久化的工作就做完了。