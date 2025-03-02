---
title: Setup Data Streams Monitoring for Java
further_reading:
    - link: '/integrations/kafka/'
      tag: 'Documentation'
      text: 'Kafka Integration'
    - link: '/tracing/service_catalog/'
      tag: 'Documentation'
      text: 'Service Catalog'
---

{{< site-region region="ap1" >}}
<div class="alert alert-info">Data Streams Monitoring is not supported in the AP1 region.</a></div>
{{< /site-region >}}

### Prerequisites

* [Datadog Agent v7.34.0 or later][1]

### Supported libraries

| Technology | Library                                                                                    | Minimal tracer version | Recommended tracer version |
|------------|--------------------------------------------------------------------------------------------|------------------------|----------------------------|
| Kafka      | [kafka-clients](https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients)         | 1.9.0                  | 1.42.2 or later            |
| RabbitMQ   | [amqp-client](https://mvnrepository.com/artifact/com.rabbitmq/amqp-client)                 | 1.9.0                  | 1.42.2 or later            |
| Amazon SQS | [aws-java-sdk-sqs (v1)](https://mvnrepository.com/artifact/com.amazonaws/aws-java-sdk-sqs) | 1.27.0                 | 1.42.2 or later            |
| Amazon SQS | [sqs (v2)](https://mvnrepository.com/artifact/software.amazon.awssdk/sqs)                  | 1.27.0                 | 1.42.2 or later            |

### Installation

Java uses auto-instrumentation to inject and extract additional metadata required by Data Streams Monitoring for measuring end-to-end latencies and the relationship between queues and services. To enable Data Streams Monitoring, set the `DD_DATA_STREAMS_ENABLED` environment variable to `true` on services sending messages to (or consuming messages from) Kafka, SQS or RabbitMQ.

Also, set the `DD_TRACE_REMOVE_INTEGRATION_SERVICE_NAMES_ENABLED` variable to `true` so that `DD_SERVICE` is used as the service name in traces.

For example:
```yaml
environment:
  - DD_DATA_STREAMS_ENABLED: "true"
  - DD_TRACE_REMOVE_INTEGRATION_SERVICE_NAMES_ENABLED: "true"
```

As an alternative, you can set the `-Ddd.data.streams.enabled=true` system property by running the following when you start your Java application:

```bash
java -javaagent:/path/to/dd-java-agent.jar -Ddd.data.streams.enabled=true -Ddd.trace.remove.integration-service-names.enabled=true -jar path/to/your/app.jar
```

### One-Click Installation
To set up Data Streams Monitoring from the Datadog UI without needing to restart your service, use [Configuration at Runtime][4]. Navigate to the APM Service Page and `Enable DSM`.

{{< img src="data_streams/enable_dsm_service_catalog.png" alt="Enable the Data Streams Monitoring from the Dependencies section of the APM Service Page" >}}

### Monitoring SQS pipelines
Data Streams Monitoring uses one [message attribute][3] to track a message's path through an SQS queue. As Amazon SQS has a maximum limit of 10 message attributes allowed per message, all messages streamed through the data pipelines must have 9 or fewer message attributes set, allowing the remaining attribute for Data Streams Monitoring.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /agent
[2]: /tracing/trace_collection/dd_libraries/java/
[3]: https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-message-metadata.html
[4]: /agent/remote_config/?tab=configurationyamlfile#enabling-remote-configuration
