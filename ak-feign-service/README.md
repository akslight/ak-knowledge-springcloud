一、Feign简介
Feign是一个声明式的伪Http客户端，它使得写Http客户端变得更简单。使用Feign，只需要创建一个接口并注解。它具有可插拔的注解特性，
可使用Feign 注解和JAX-RS注解。Feign支持可插拔的编码器和解码器。Feign默认集成了Ribbon，并和Eureka结合，默认实现了负载均衡的效果。
简而言之：

    Feign 采用的是基于接口的注解
    Feign 整合了ribbon，具有负载均衡的能力
    整合了Hystrix，具有熔断的能力
    

Feign:
    1.程序的启动类ServiceFeignApplication ，加上@EnableFeignClients注解开启Feign的功能
    2.配置文件添加：
        eureka:
          client:
            serviceUrl:
              defaultZone: http://localhost:8761/eureka/
        server:
          port: 8765
        spring:
          application:
            name: service-feign    
    3.定义一个feign接口，通过@ FeignClient（“服务名”），来指定调用哪个服务。比如在代码中调用eureka-client服务的“/hi”接口
    