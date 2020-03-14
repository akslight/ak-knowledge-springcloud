Spring cloud 为开发人员提供了快速构建分布式系统的一些工具，包括配置管理、服务发现、断路器、路由、微代理、
事件总线、全局锁、决策竞选、分布式会话等等。Eureka是一个服务注册和发现模块。

服务端：
    1.启动一个服务注册中心，在启动application类加上@EnableEurekaServer。
    2.eureka是一个高可用的组件，它没有后端缓存，每一个实例注册之后需要向注册中心发送心跳（因此可以在内存中完成），
    在默认情况下erureka server也是一个eureka client ,必须要指定一个 server。 在配置文件appication.yml中提添加：
        server:
          port: 8761
        
        eureka:
          instance:
            hostname: localhost
          client:
            registerWithEureka: false
            fetchRegistry: false
            serviceUrl:
              defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
              
    通过eureka.client.registerWithEureka：false和fetchRegistry：false来表明自己是一个eureka server
    
    
服务提供者（client）：
    当client向server注册时，它会提供一些元数据，例如主机和端口，URL，主页等。Eureka server 从每个client实例接收心跳消息。 
如果心跳超时，则通常将该实例从注册server中删除。

    1.通过注解@EnableEurekaClient 表明自己是一个eurekaclient。
    2.配置文件中添加：
        eureka:
          client:
            serviceUrl:
              defaultZone: http://localhost:8761/eureka/
        server:
          port: 8762
        spring:
          application:
            name: eureka-client
    
    需要指明spring.application.name,这个很重要，这在以后的服务与服务之间相互调用一般都是根据这个name 。 
    启动工程，打开http://localhost:8761 ，即eureka server 的网址
    这时打开 http://localhost:8762/hi?name=ak ，你会在浏览器上看到 :hi ak,i am from port:8762
    
    
spring boot版本和spring cloud版本的匹配关系：

Spring Cloud	    Spring Boot
Hoxton              兼容Spring Boot 2.2.x
Greenwich           兼容Spring Boot 2.1.x
Finchley	        兼容Spring Boot 2.0.x，不兼容Spring Boot 1.5.x
Dalston和Edgware	兼容Spring Boot 1.5.x，不兼容Spring Boot 2.0.x
Camden	            兼容Spring Boot 1.4.x，也兼容Spring Boot 1.5.x
Brixton	            兼容Spring Boot 1.3.x，也兼容Spring Boot 1.4.x
Angel	            兼容Spring Boot 1.2.x

springcloud eureka server 官方文档:https://projects.spring.io/spring-cloud/spring-cloud.html#spring-cloud-eureka-server
springcloud eureka client 官方文档:https://projects.spring.io/spring-cloud/spring-cloud.html#_service_discovery_eureka_clients



