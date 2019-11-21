# Openstack架构

## Structure

- Region
- Cell
- Available Zone
- Host Aggregate

## Management

- domain
- project


https://blog.csdn.net/ustc_dylan/article/details/17758393

## Region

Region是地理上的概念

- 每个region有自己独立的endpoint
- region之间完全隔离
- 多个regions之间共享同一个keystone和horizon

## AZ

**Available Zone**

- 一个region之内可以有多个AZ
- 一个AZ通常是一个数据中心
- AZ之间是互通的

**AZ内的服务**

- 计算节点
- 网络节点
- 存储节点

创建实例时，用户可以指定AZ。虚拟机、虚拟网络和虚拟磁盘将在该AZ内创建

## Cell

将Nova集群进一步划分成API层和Cell层

- API层负责响应user请求，并发送请求到cells
- 每个cell有自己的MQ, DB
- Cell内只运行nova-compute和nova-conductor

## Host Aggregate

将同类型主机划分为一组，例如ssd集群、high memory集群，以便于nova-scheduler调度
