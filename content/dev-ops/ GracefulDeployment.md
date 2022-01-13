---
title: "无损发布"
date: 2021-12-30T23:04:28+08:00
draft: false
tags: ["无损发布"]
categories: ["Dev Ops"]

showShare: false
---

# 什么是无损？

在应用进行部署时，流程都是停止应用，然后更新代码包，然后启动，如果不进行处理，会导致用户请求失败，影响用户体验，当然不同的部署环境，需要处理的问题时不一样的，接下来针对指定场景的无损进行说明。

# 无损发布

![部署架构](img/server-architecture.png)

## 业务网关

业务网关对接流量网关，假定为Nginx，想要做到无损，在Nginx将业务网关实例从upstream中剔除前，业务网关应该是正常服务的，所以在业务网关重启前，应该将Nginx对业务网关的探活接口改为异常状态，然后Nginx将实例从upstream中剔除，等待一定时间后（nginx剔除实例，并且存量请求正常处理完成），进行部署操作。

## 应用

业务网关转发请求到应用跟应用间调用的链路是一致的，都是通过应用从注册中心获取的服务列表进行调用的，所以，想要做到无损，在应用中的服务列表将下线机器剔除前，应用应该是正常服务的，所以在应用启动前，需要先取消注册，等待一定时间后（消费者缓存中的服务列表无此实例，并且存量请求正常处理完成），进行部署操作。

## 其他

上面两个场景主要关注web方面，但是在实际场景中，可能会更加复杂，比如消息的消费者，如果不能想web api一样，提前停止pull msg，可能会导致应用停止时还在运行中，导致逻辑丢失，这部分由于没有实际处理过，目前只能简单描述（可以参考一下spring boot 2.3.0-Release的graceful shutdown是否有对消息的消费者进行处理，或者是随着spring对bean的回收，就可以正常停止消费？？？？）。

# 参考

- [Spring-Boot-2.3-Release-Notes#graceful-shutdown](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.3-Release-Notes#graceful-shutdown)
- [Spring boot 2.3优雅下线，距离生产还有多远？](https://zhuanlan.zhihu.com/p/268858056)