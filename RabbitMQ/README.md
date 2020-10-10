#1. RabbitMQ介绍
######1.1、什么是RabbitMQ？
```js
  RabbitMQ 是由 LShift 提供的一个 Advanced Message Queuing Protocol (AMQP) 的开源实现，由以高性能、健壮以及可伸缩性出名的 Erlang 写成，因此也是继承了这些优点。
```

######1.2、什么是AMQP?
```js
  AMQP，即Advanced Message Queuing Protocol，高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。它从生产者接收消息并递送给消费者，在这个过程中，根据规则进行路由，缓存与持久化。

  AMQP的主要特征是面向消息、队列、路由（包括点对点和发布/订阅）、可靠性、安全。

  RabbitMQ是一个开源的AMQP实现，服务器端用Erlang语言编写，支持多种客户端，如：Python、Ruby、.NET、Java、JMS、C、PHP、ActionScript、XMPP、STOMP等，支持AJAX。用于在分布式系统中存储转发消息，在易用性、扩展性、高可用性等方面表现不俗。

  而在AMQP中主要有两个组件：Exchange 和 Queue （在 AMQP 1.0 里还会有变动），如下图所示，绿色的 X 就是 Exchange ，红色的是 Queue ，这两者都在 Server 端，又称作 Broker ，这部分是 RabbitMQ 实现的，而蓝色的则是客户端，通常有 Producer 和 Consumer 两种类型：

  mshk.top
```
######1.3、RabbitMQ的基础概念
```js
  - Broker：简单来说就是消息队列服务器实体
  - Exchange：消息交换机，它指定消息按什么规则，路由到哪个队列
  - Queue：消息队列载体，每个消息都会被投入到一个或多个队列
  - Binding：绑定，它的作用就是把exchange和queue按照路由规则绑定起来
  - Routing Key：路由关键字，exchange根据这个关键字进行消息投递
  - vhost：虚拟主机，一个broker里可以开设多个vhost，用作不同用户的权限分离
  - producer：消息生产者，就是投递消息的程序
  - consumer：消息消费者，就是接受消息的程序
  - channel：消息通道，在客户端的每个连接里，可建立多个channel，每个channel代表一个会话任务
```
######1.4、RabbitMQ的特性
```js
 - 可靠性：包括消息持久化，消费者和生产者的消息确认
 - 灵活路由：遵循AMQP协议，支持多种Exchange类型实现不同路由策略
 - 分布式：集群的支持，包括本地网络与远程网络
 - 高可用性：支持主从备份与镜像队列
 - 多语言支持：支持多语言的客户端
 - WEB界面管理：可以管理用户权限，exhange，queue，binding，与实时监控
 - 访问控制：基于vhosts实现访问控制
 - 调试追踪：支持tracing，方便调试
```

###2. Docker安装RabbitMq

######2.1 使用docker search rabbitMq命令获取镜像列表

######2.2 使用docker pull docker.io/rabbitmq:3.8-management 拉取镜像

######2.3 使用docker images获取查看rabbitMQ镜像ID

######2.4 执行docker run --name rabbitmq -d -p 15672:15672  -p 5672:5672 4b23cfb64730命令创建rabbitMq容器，关于其中的参数含义如下：
```js
  --name指定了容器名称
  -d 指定容器以后台守护进程方式运行
  -p指定容器内部端口号与宿主机之间的映射，rabbitMq默认要使用15672为其web端界面访问时端口，5672为数据通信端口
```

######2.4 查看容器日志 使用docker logs -f 容器ID命令可以查看容器日志，
