## 1.简介

__Spring Cloud Stream__ 是一个创建消息驱动微服务应用的框架。它基于__Spring Boot__创建独立的、工业级的Spring应用，使用__Spring Integration__实现与消息代理之间的连接。它约定了不同中间件相应的配置，引入了持久化发布订阅机制、消费者组和分区的概念。

通过添加__@EnableBinding__注解，你可以立刻实现与消息代理连接，而在方法上加__@StreamListener__注解让方法能收到流处理事件。举个简单的接收外部消息的例子。

```java
@SpringBootApplication
@EnableBinding(Sink.class)
public class VoteRecordingSinkApplication {

  public static void main(String[] args) {
    SpringApplication.run(VoteRecordingSinkApplication.class, args);
  }

  @StreamListener(Sink.INPUT)
  public void processVote(Vote vote) {
      votingService.recordVote(vote);
  }
}
```

@EnableBinding注解使用一个或多个注解作为参数（上例中只使用了Sink接口作为参数）。这个接口声明输入、输出通道。Spring Cloud Stream提供了Srouce，Sink，Processor接口，当然也提供用户自定义接口。

如下使Sink接口的定义：

```java
public interface Sink {
  String INPUT = "input";

  @Input(Sink.INPUT)
  SubscribableChannel input();
}
```

@Input注解标识这是一个输入通道，应用借此接收进入到应用的信息；@Output注解则标识是输出通道，应用借此来发布消息。它们可以将通道名称作为参数，如果没有通道名称则将使用注解所修饰的方法名作为参数。

Spring Cloud Stream将为你创建一个接口的实现。你可以在项目中通过自动装配来使用它。如下是测试案例。

```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = VoteRecordingSinkApplication.class)
@WebAppConfiguration
@DirtiesContext
public class StreamApplicationTests {

  @Autowired
  private Sink sink;

  @Test
  public void contextLoads() {
    assertNotNull(this.sink.input());
  }
}
```



##  2.主要概念

Spring Cloud Stream 提供了很多抽象和元组件来简化消息驱动微服务应用。概述如下：

- Spring Cloud Stream的应用模型
- Binder的抽象
- 持久化发布-订阅的支持
- 消费者组支持
- 分区支持
- 可插拔的Binder API









































