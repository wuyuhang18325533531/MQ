###Message acknowledgment 消息确认

每个Consumer可能需要一段时间才能处理完收到的数据。如果在这个过程中，Consumer出错了，异常退出了，而数据还没有处理完成，那么非常不幸，这段数据就丢失了。因为我们的代码，一旦RabbitMQ Server发送给Consumer消息后，会立即把这个Message标记为完成，然后从queue中删除。我们将无法再操作这个尚未处理完成的消息。

  实际场景中，如果一个Consumer异常退出了，我们希望它处理的数据能够被另外的Consumer处理，这样数据在这种情况下（通道关闭、连接关闭、TCP连接丢失等情况）就不会丢失了。

  为了保证数据不被丢失，RabbitMQ支持消息确认机制，ack(nowledgments)是从Consumer消费后发送到一个特定的消息告诉RabbitMQ已经收到、处理结束，RabbitMQ可以去安全的删除它了。

  如果Consumer退出了但是没有发送ack，那么RabbitMQ就会把这个Message重新排进队列，发送到下一个Consumer。这样就保证了在Consumer异常退出的情况下数据也不会丢失。

  这里并没有用到超时机制。RabbitMQ仅仅通过Consumer的连接中断来确认该Message并没有被正确处理。也就是说，RabbitMQ给了Consumer足够长的时间来做数据处理。

  消息确认默认是关闭的，我们需要通过，d.ACK(false)来告诉RabbitMQ我们已经完成任务。