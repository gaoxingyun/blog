---
title: RabbitMq使用
date: 2017-07-25 17:40:14
categories: 
- 工具
tags:
- mq
---

#### 安装

###### 使用docker安装

```
docker run -d --name rabbitmq --hostname rabbitmq  -e RABBITMQ_DEFAULT_USER=user -e  RABBITMQ_DEFAULT_PASS=123456 -p 15672:15672 -p 5672:5672 -v /opt/docker/rabbitmq:/var/lib/rabbitmq rabbitmq:3-management
```

###### 打开管理页面
```
http://localhost:15672
```

###### 使用mq
```
localhost:5672
```

#### 概念

- Broker：简单来说就是消息队列服务器实体。
- Exchange：消息交换机，它指定消息按什么规则，路由到哪个队列。
- Queue：消息队列载体，每个消息都会被投入到一个或多个队列。
- Binding：绑定，它的作用就是把exchange和queue按照路由规则绑定起来。
- Routing Key：路由关键字，exchange根据这个关键字进行消息投递。
- vhost：虚拟主机，一个broker里可以开设多个vhost，用作不同用户的权限分离。
- producer：消息生产者，就是投递消息的程序。
- consumer：消息消费者，就是接受消息的程序。
- channel：消息通道，在客户端的每个连接里，可建立多个channel，每个channel代表一个会话任务。

###### Exchange说明

- fanout
- direct,
- topic
- header
- 性能排序：fanout > direct >> topic


#### 博客

- [springboot使用示例](http://www.jianshu.com/p/e1258c004314)


#### 插件

- http://www.rabbitmq.com/community-plugins.html

###### 常用插件

- 延时消息插件  https://bintray.com/rabbitmq/community-plugins/rabbitmq_delayed_message_exchange/v3.6.x#files

###### 插件操作

- `rabbitmq-plugins enable rabbitmq_management`       启用插件
- `rabbitmq-plugins disable rabbitmq_management`      禁用插件
- `rabbitmq-plugins list`                             查看插件列表


###### springboot集成rabbitmq

###### 启动延时队列注意实现
1. 实现延迟队列需要Spring在4.1以上，spring-amqp在1.6以上。
2. 延时队列需配置topicExchange.setDelayed(true);
3. 配置好需删除之前的消息队列，重新生成消息队列。

- RabbitMq配置
``` java
@Configuration
public class RabbitConfig {

	private final static String queueName = PayNotifyContant.MQ_QUEEUE_NOTIFY_NAME;
	private final static String exchangeName = PayNotifyContant.MQ_EXCHANGE_NOTIFY_NAME;
	private final static String bindKey = PayNotifyContant.MQ_BIND_KEY_NOTIFY_NAME;

	@Bean
	Queue queue() {
		return new Queue(queueName, true);
	}

	@Bean
	TopicExchange exchange() {
		TopicExchange topicExchange =  new TopicExchange(exchangeName, true, false);
		// 是否启用延时
		topicExchange.setDelayed(true);
		return topicExchange;
	}

	@Bean
	Binding binding(Queue queue, TopicExchange exchange) {
		return BindingBuilder.bind(queue).to(exchange).with(bindKey);
	}

	@Bean
	SimpleMessageListenerContainer container(ConnectionFactory connectionFactory,
			MessageListenerAdapter listenerAdapter, RabbitProperties rabbitProperties) {
		SimpleMessageListenerContainer container = new SimpleMessageListenerContainer();
		container.setConnectionFactory(connectionFactory);
		container.setQueueNames(queueName);
		container.setMessageListener(listenerAdapter);
		container.setAcknowledgeMode(rabbitProperties.getListener().getSimple().getAcknowledgeMode());
		container.setMaxConcurrentConsumers(rabbitProperties.getListener().getSimple().getMaxConcurrency());
		container.setConcurrentConsumers(rabbitProperties.getListener().getSimple().getConcurrency());
		return container;
	}

	@Bean
	MessageListenerAdapter listenerAdapter(ChannelAwareMessageListener receiver,
			Jackson2JsonMessageConverter jackson2JsonMessageConverter) {
		return new MessageListenerAdapter(receiver, jackson2JsonMessageConverter);
	}

	@Bean
	Jackson2JsonMessageConverter jackson2JsonMessageConverter() {
		ObjectMapper objectMapper = new ObjectMapper();
		return new Jackson2JsonMessageConverter(objectMapper);
	}

	@Bean
	SimpleMessageConverter simpleMessageConverter() {
		return new SimpleMessageConverter();
	}
	
	@Bean
	@ConditionalOnClass(DelayMessagePostProcessor.class)
	DelayMessagePostProcessor messagePostProcessor()
	{
		return new DelayMessagePostProcessor();
	}

}

```

- 消息发送
``` java
@Component
public class MessageQueueSender implements RabbitTemplate.ConfirmCallback, ReturnCallback {

	private final static Logger log = LoggerFactory.getLogger(MessageQueueSender.class);
	private final static String exchangeName = PayNotifyContant.MQ_EXCHANGE_NOTIFY_NAME;
	private final static String routingKey = PayNotifyContant.MQ_ROUTING_KEY_NOTIFY_NAME;

	private RabbitTemplate rabbitTemplate;

	/**
	 *  配置 RabbitTemplate
	 */
	@Autowired
	public void setRabbitTemplate(RabbitTemplate rabbitTemplate,
			Jackson2JsonMessageConverter jackson2JsonMessageConverter, DelayMessagePostProcessor messagePostProcessor) {
		rabbitTemplate.setConfirmCallback(this);
		rabbitTemplate.setReturnCallback(this);
		rabbitTemplate.setMessageConverter(jackson2JsonMessageConverter);
		rabbitTemplate.setBeforePublishPostProcessors(messagePostProcessor);
		this.rabbitTemplate = rabbitTemplate;
	}

	/**
	 * 发送消息
	 * 
	 * @param msg
	 * @throws AmqpException
	 *             (uncheck exception)
	 */
	public void send(Object msg) {
		CorrelationData correlationData = new CorrelationData(UUID.randomUUID().toString());
		log.debug("开始发送消息 : {}", msg);
		rabbitTemplate.convertAndSend(exchangeName, routingKey, msg, correlationData);
		log.debug("结束发送消息 : {}", msg);
	}

	@Override
	public void returnedMessage(Message message, int replyCode, String replyText, String exchange, String routingKey) {
		log.debug("发送失败 : {}", message);
	}

	@Override
	public void confirm(CorrelationData correlationData, boolean ack, String cause) {
		if (ack) {
			log.debug("消息发送成功 : {}", correlationData);
		} else {
			log.debug("消息发送失败 : {}", cause);
		}
	}
}

```

- 消息接收
``` java

@Component
public class MessageQueueReceiver implements ChannelAwareMessageListener {

	private final static Logger log = LoggerFactory.getLogger(MessageQueueReceiver.class);

	@Autowired
	private Jackson2JsonMessageConverter jackson2JsonMessageConverter;
	
	@Override
	public void onMessage(Message message, Channel channel) throws Exception {
		
		System.out.println(message);
		
		Object notifyMessageObject = jackson2JsonMessageConverter.fromMessage(message);
		if(!(notifyMessageObject instanceof NotifyMessage)){
			throw new ClassCastException();
		}
		NotifyMessage notifyMessage = (NotifyMessage)notifyMessageObject;
		log.debug("接收到消息 : {}", notifyMessage);
		
		//消息的标识，false只确认当前一个消息收到，true确认所有consumer获得的消息
		channel.basicAck(message.getMessageProperties().getDeliveryTag(), false);
		//ack返回false，并重新回到队列 
		//channel.basicNack(message.getMessageProperties().getDeliveryTag(), false, true);
		//拒绝消息
		//channel.basicReject(message.getMessageProperties().getDeliveryTag(), true);
		
	}

}
```

- 延时消息处理
``` java
public class DelayMessagePostProcessor implements MessagePostProcessor{

	@Override
	public Message postProcessMessage(Message message) throws AmqpException {
		
		message.getMessageProperties().setDelay(6000);
		return message;
	}

}
```