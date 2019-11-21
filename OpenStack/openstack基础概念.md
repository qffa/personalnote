# Openstack 基础概念

## Service

Openstack的Service包括

- keystone
- glance
- nova
- cinder
- neutron
- placement
- horizon
- swift
- ceilometer
- ...

通信方式

- Service内部各个组件通过RabbitMQ实现RPC
- Service之间使用REST API相互调用


## Endpoint

Service通过Endpoint暴露自己的API

查看endpoint

```
$ openstack catalog list
$ openstack endpoint list
```

## User

User指代任何使用openstack的实体，可以是真正的用户、其他系统或服务

## Credentials

Cdentials是User用来证明自己身份的信息，可以是

1. 用户名/密码
2. Token
3. API Key
4. 其他高级方式

## Token

Token是有数字和字母组成的字符串，User认证成功后，它由Keystone分配给User

- Token用作访问Service的Credential
- Service会通过Keystone验证Token的有效性
- Token的有效期默认是24小时

## Project

即租户或项目组，用来对openstack提供的资源进行隔离。

资源属于Project，而不是User。User需要在Project内，才能访问该资源

## Role

Keystone借助Role来实现Authentication
