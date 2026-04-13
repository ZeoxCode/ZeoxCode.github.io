---
title: 架构师备考 18 - 大数据架构
date: 2022-09-26
tags: [软考, 系统架构设计师]
categories:
  - 系统架构
  - 系统架构设计师
---

## 大数据特征（5V）

- **Volume（大量）**：数据规模从TB到PB、EB级
- **Velocity（高速）**：数据产生和处理速度快（实时流数据）
- **Variety（多样）**：结构化、半结构化（JSON/XML）、非结构化（图片/视频）
- **Veracity（真实性）**：数据质量和可信度问题
- **Value（价值）**：价值密度低，需要从海量数据中挖掘

## Hadoop 生态

### HDFS（分布式文件系统）

```
Client → NameNode（元数据）
       → DataNode1（数据块）
       → DataNode2（副本）
       → DataNode3（副本）
```

- **NameNode**：管理文件系统元数据（文件目录树、块位置）
- **DataNode**：存储实际数据块，定期向NameNode汇报
- **默认副本数**：3（一个机架内2个，另一机架1个）
- **块大小**：默认128MB（HDFS优化顺序读写，不适合小文件）

### MapReduce

**编程模型**：将计算分为两个阶段
```
输入数据 → Map（映射）→ Shuffle（混洗分组）→ Reduce（归约）→ 输出
```

**经典例子——词频统计**：
```
Map:    ("hello world") → [("hello",1), ("world",1)]
Shuffle: 相同key的value汇聚到同一Reducer
Reduce: ("hello",[1,1,1]) → ("hello", 3)
```

### YARN（资源调度）

- MapReduce 2.0 的资源管理层
- **ResourceManager**：全局资源管理，接收作业提交
- **NodeManager**：单节点资源管理，执行任务
- **ApplicationMaster**：每个应用独立的AM，协商资源、管理任务

## Lambda 架构

解决同时需要**批处理**（高吞吐、低延迟不要求）和**流处理**（低延迟、高吞吐不要求）的场景。

```
数据源 → 批处理层（Batch Layer）  → 批处理视图  ─┐
       ↘ 速度层（Speed Layer）    → 实时视图    ─┤→ 服务层 → 查询
                                                └─
```

- **批处理层**：处理所有历史数据，定期（小时/天）重新计算（Hadoop MapReduce/Spark）
- **速度层**：处理最新数据，低延迟，可能不精确（Spark Streaming/Flink）
- **服务层**：合并批处理结果和实时结果，对外提供查询

**优点**：兼顾精确性和低延迟
**缺点**：维护两套代码（批和流），复杂

## Kappa 架构

Lambda 的简化版，**统一用流处理**代替批处理层。

```
数据源 → 消息队列（Kafka，保留所有历史数据）→ 流处理引擎 → 服务层
                              ↑
                  需要重算时，回放历史数据
```

- 只有一套处理代码
- 依赖消息队列的长时间数据保留（Kafka可以保留几天到几周）
- **适合**：逻辑统一、数据可以流式处理的场景

## 流处理技术

### Apache Kafka

- 分布式消息队列（更准确说是分布式流平台）
- **核心概念**：Topic（主题）、Partition（分区，并发单位）、Consumer Group
- 消息持久化到磁盘，支持回放（Replay）
- 适合：日志收集、事件流、微服务解耦

### Apache Flink

- 有状态的流计算框架
- 支持**事件时间（Event Time）**处理（处理乱序数据）
- **水位线（Watermark）**：用于处理延迟数据，表示"这个时间点之前的数据都已到达"
- 支持 Exactly-Once 语义（精确一次处理）
- 可以同时做批处理和流处理

### Apache Spark

- 基于内存的分布式计算框架（比MapReduce快10-100倍）
- **RDD（弹性分布式数据集）**：核心抽象，不可变的分布式数据集合
- **Spark Streaming**：微批处理（Micro-Batch），将流数据切分为小批次处理
- **Spark SQL**：支持SQL查询
- **MLlib**：机器学习库

## 数据湖 vs 数据仓库

| 对比 | 数据仓库 | 数据湖 |
|------|---------|--------|
| 数据结构 | 结构化 | 结构化+非结构化 |
| Schema | Write时定义 | Read时定义 |
| 用户 | 业务分析师 | 数据科学家 |
- | 工具 | SQL | SQL+Python+机器学习 |
| 例子 | Hive、Redshift | S3+Glue、HDFS+Spark |

## NoSQL 在大数据中的应用

- **HBase**：基于HDFS的列族数据库，支持海量随机读写（实时查询历史数据）
- **Elasticsearch**：全文搜索引擎，适合日志分析（ELK）
- **Redis**：内存数据库，用于大数据系统的缓存层
- **Cassandra**：高可用的分布式键值/宽列数据库，无单点故障

## 考试重点

- Lambda = 批处理 + 速度层 + 服务层，解决精确性和低延迟的权衡
- Kappa = 全流处理，Kafka 保留历史支持回放
- MapReduce 两阶段：Map（并行）→ Shuffle → Reduce
- HDFS：NameNode 存元数据，DataNode 存数据，默认3副本
