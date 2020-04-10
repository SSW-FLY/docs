# eureka

## 基础知识
1. 服务治理
在传统的rpc远程框架中，管理每个服务与服务之间以来关系比较复杂，所以需要服务治理，管理服务和服务之间依赖关系，可以实现服务调用、负载均衡、容错等，实现服务发现注册。
2. Eureka两大组件
Eureka client 以及Eureka server

3. 服务注册
服务提供者将自己注册到Eureka server上，成为Eureka client。服务消费者需要调用服务就到Eureka server 中找到相应的服务提供者。Eureka server 和服务提供者通过心跳维持，server长时间没有接收到心跳，就会把服务删除(默认90s)。

```xml
<!--依赖-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactid>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```