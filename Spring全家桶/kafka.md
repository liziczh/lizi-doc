Kafka

kafka消息中间件，每条消息是由一个key、一个value和时间戳组成。

- producer：生产者，生产消息。
- consumer：消费者，消费消息。
- topic：消息主题，每一类消息称为一个主题（本质是队列，不同类别的消息放不同的队列）。
- broker：存放消息。

kafka API：

- Producer API：发布消息到1个或多个topic（主题）。
- Consumer API：订阅一个或多个topic，并处理产生的消息。
- Streams API：充当一个流处理器，从1个或多个topic消费输入流，并生产一个输出流到1个或多个输出topic，有效地将输入流转换到输出流。
- Connector API：

