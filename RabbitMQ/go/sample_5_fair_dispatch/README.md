#Fair dispatch 公平分发
- 分发机制不是那么优雅。默认状态下，RabbitMQ将第n个Message分发给第n个Consumer。当然n是取余后的。它不管Consumer是否还有unacked Message，只是按照这个默认机制进行分发。

- 那么如果有个Consumer工作比较重，那么就会导致有的Consumer基本没事可做，有的Consumer却是毫无休息的机会。

- 通过 ch.Qos 方法设置预读取消息prefetch count=1 。这样RabbitMQ就会使得每个Consumer在同一个时间点最多处理一个Message。换句话说，在接收到该Consumer的ack前，他它不会将新的Message分发给它。

```js
	//创建一个queue
	q, err := channel.QueueDeclare(
		queueName, // name
		true,      // durable
		false,     // delete when unused
		false,     // exclusive
		false,     // no-wait
		nil,       // arguments
	)
	failOnError(err, "Failed to declare a queue")

	//每次只取一条消息
	err = channel.Qos(
		1,     // prefetch count
		0,     // prefetch size
		false, // global
	)
	failOnError(err, "Failed to set QoS")

	log.Printf("Queue bound to Exchange, starting Consume")
	//订阅消息
	msgs, err := channel.Consume(
		q.Name, // queue
		"",     // consumer
		false,  // auto-ack
		false,  // exclusive
		false,  // no-local
		false,  // no-wait
		nil,    // args
	)
```