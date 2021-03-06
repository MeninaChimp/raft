[![release](https://img.shields.io/github/release/MeninaChimp/raft.svg)](https://github.com/MeninaChimp/raft/releases)

## 动机

作为基础中间件实现分布式一致性的支撑组件，基于Rail RPC框架作为底层通信组件.

## 实现

实现借鉴了ETCD的消息流转，Kafka的日志存储.

> 特性

```
1.逻辑时钟
2.事件驱动 + EventLoop
3.单线程状态机
4.组提交
5.全异步(客户端异步 + 服务端异步)
6.WAL(Append-Only + Sequential Write)COMBIANTION
7.Storage[MEMORY,DISK,COMBIANTION]
```

> 层次

```
1.Leader election
2.Transporter
3.Raft Apis
4.RaftEventLoop, GroupCommitLoop, ApplyEventLoop
5.RaftLog/Storage
6.WAL
```

> 架构图：

![raft](https://www.menina.cn/upload/2019/05/sd67hm84pcheiq77nvvo9ang4u.jpg)

## 功能点

* Leader选举
* 日志对齐,同步
* 快照
* 故障恢复
* 数据校验
* 原子性
* 快照，日志清理

## 性能数据

Storage：Memory
CPU: i5-6200u 
DISK: 7200/s

> Without Snapshot trigger:

node |message size | concurrent | sync | throughput
--- | --- | --- | --- | ---
2 | 512 bytes | 1  | 是 | 1181/s
2 | 512 bytes | 10 | 是 | 4649/s
2 | 512 bytes | 50 | 是 | 10445/s
2 | 512 bytes | 100| 是 | 17881/s 

> With snapshot trigger and build:

node | message size | concurrent | sync | total message | snapshot trigger count | throughput
--- | --- | --- | --- | --- | --- | ---
2 | 512 bytes | 1  | 是 | 20000 | 5 |1128/s
2 | 512 bytes | 10 | 是 | 100000 | 10 | 4167/s

## RoadMap

* Mutil raft
* 线性一致
* 成员关系变更

## 使用

基于raft.yml提供默认配置，支持env或-DRAFT_CONFIG_PATH注入yml文件路径.

> 使用方契约：
```
1.使用Proposer.propose同步消息.
2.实现上层应用StateMachine注入RaftNode.
```

* 一致性由模块保证.

## 实例

![amq](https://www.menina.cn/upload/2019/02/rfifo8c3qaik3odd672k87ohia.png)
