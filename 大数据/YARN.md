# YARN

## YARN产生背景

### MapReduce1.x 架构

![1556329043308](D:\note\note\imgs\tracker.png)

* master/slave：JobTracker/TaskTracker
  * JobTracker：资源管理与资源调度
  * TaskTracker：资源节点的健康情况以及Task任务执行情况
* 存在问题
  * JobTracker：单点、压力大
  * 仅仅只能够支持MapReduce作业

### 资源利用率&运维成本

![1556329364394](D:\note\note\imgs\yarnResource.png)

* 资源利用率
  * YARN出现后所有计算框架运行在一个集群中，按需分配资源，提高利用率。
* 运维成本
  * 只需要维护一个集群，降低了运维成本

## YARN概述

YARN是Yet Another Resource Negotiator的缩写

是通用的资源管理系统

为上层应用提供统一的资源管理和调度

### JobTracker拆分

#### ResourceManager（RM）

ResourceManager负责资源管理：

#### ApplicationMaster（AM）

ApplicationMaster负责每一个作业的调度和监控

* 提交一个MapReduce作业就会有一个MapReduceApplicationMaster
* 提交一个Spark作业就会有SparkApplicationMaster
* 作业可以是单独的也可以是相互依赖的一组作业

### TaskTracker演变

将TaskTracker变成了NodeManager（NM），RM是Master，NM是Slave。

## YARN架构

client、Resource Manager、Node Manager、 ApplicationMaster

![1556331329673](D:\note\note\imgs\RMNM.png)

### client

向RM提交任务、杀死任务等

### Application Master

每一个应用程序对应一个AM，

AM向RM申请资源用于在NM上启动对应的Task

进行数据的切分

为每个Task向RM申请资源（container）

与NM进行通信

任务的监控

### Node Manager

有多个NM执行任务

向RM发送心跳信息、任务的执行情况。

接收来自RM的请求来启动任务。

处理来自AM的命令

### Resource Manager

集群中同一时刻只有一个RM对外提供服务，他有以下作用

处理来自客户端的请求：提交、杀死。

启动/监控AM

监控NM

负责资源管理



### container

任务运行抽象，抽象出内存与CPU

在container中既可以运行AM，也可以运行map/reduce task

## yarn执行流程

![1556332841957](D:\note\note\imgs\yarnFlowPath.png)

1. 客户端提交应用程序到RM
2. RM与NM通信为作业分配第一个container
3. 在这个container启动AM
4. AM启动后注册到RM上，RM可以监听AM的情况。AM到RM上申请资源。
5. AM到NM上启动container
6. NM在container上启动task