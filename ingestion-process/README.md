# 日志的处理过程

### 概述

在这个章节里面，将向读者介绍 Loki 对日志的具体处理过程。

你可以通过此章节了解如下方面。

* 负责接收、处理日志的组件
* 详细的处理机制
* 日志存储块的缓冲与缓冲的提交
* Write-Ahead Log 机制 （译者注：通常简称WAL，即预写日志。简单讲，为了保证数据一致性，优化缓解io压力的数据落盘方式）
* 无序日志与有序日志

### 接收日志相关的组件

![Ingestion Overview](https://github.com/zhcsyncer/grafana-loki-deep-dive-cn/blob/main/.gitbook/assets/ingestion-process-overview.png)

上图中展示了具体负责处理日志的组件。

Fluentd，Fluentd Bit，Logstash和Promtail 都作为loki的客户端，Distributor 与 Ingester 为Loki 的组件。

另外，AWS S3 作为日志块与索引的存储，Memcache 用作缓存。

让我们按顺序来逐个了解下这些组件。

#### Loki 的客户端

这些组件都可以从你的应用那采集日志并且通过Distributor的Http 接口发送给Loki。

#### 分发器

此组件负责校验投递请求并且通过 content-hash 算法来决定应该转发给具体哪一个Ingester 实例。

#### AWS S3 作为存储

最终的日志均存储于此处。

持久化层也支持Cassandra，GCS，DynamoDB和其他一些产品。

具体支持清单见 [Supported Stores](https://grafana.com/docs/loki/latest/operations/storage/)

#### Memcache 作为缓存

这是一个可选的组件，但是能够有显著效果的提升查询性能。

Loki 当前支持4种类型的缓存组件。

你可以通过下面的文档了解更多。

{% content-ref url="../cache-strategy.md" %}
[cache-strategy.md](../cache-strategy.md)
{% endcontent-ref %}

### 持久化日志写入的全链路

* 采集端发动日志至Loki的Distributor 模块。

待续...
