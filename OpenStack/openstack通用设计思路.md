# OpenStack 通用设计思路

- API 前端服务
- Scheduler 调度服务
- Worker 工作服务
- Driver 框架

## API 前端服务

每个组件都包含一个API服务，负责接收客户请求
例如：nova-api, neutron-server

## Scheduler 调度服务

对于某项操作，如果多个实体都可以完成，通常会有一个scheduler调度执行

## Worker 工作服务

执行工作负载

## Driver 框架

负责对接实际的具体技术，例如

- nova-compute对接各种hypervisor
- cinder对接各种存储
- neutron对接各种网络
