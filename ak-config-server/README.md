在分布式系统中，由于服务数量巨多，为了方便服务配置文件统一管理，实时更新，所以需要分布式配置中心组件。在Spring Cloud中，有分布式配置中心
组件spring cloud config ，它支持配置服务放在配置服务的内存中（即本地），也支持放在远程Git仓库中。在spring cloud config 组件中，分两
个角色，一是config server，二是config client。

服务端：

    1.在程序的入口Application类加上@EnableConfigServer注解开启配置服务器的功能
    2.在配置文件中添加：
        spring.application.name=ak-config-server
        server.port=8888
        
        spring.cloud.config.server.git.uri=https://github.com/forezp/SpringcloudConfig/
        spring.cloud.config.server.git.searchPaths=respo
        spring.cloud.config.label=master
        spring.cloud.config.server.git.username=
        spring.cloud.config.server.git.password=
        
        
        spring.cloud.config.server.git.uri：配置git仓库地址
        spring.cloud.config.server.git.searchPaths：配置仓库路径
        spring.cloud.config.label：配置仓库的分支
        spring.cloud.config.server.git.username：访问git仓库的用户名
        spring.cloud.config.server.git.password：访问git仓库的用户密码
        
    启动程序：访问http://localhost:8888/foo/dev
    
    {"name":"foo","profiles":["dev"],"label":"master",
    "version":"792ffc77c03f4b138d28e89b576900ac5e01a44b","state":null,"propertySources":[]}
    
    证明配置服务中心可以从远程程序获取配置信息。
    
    http请求地址和资源文件映射如下:
    
    /{application}/{profile}[/{label}]
    /{application}-{profile}.yml
    /{label}/{application}-{profile}.yml
    /{application}-{profile}.properties
    /{label}/{application}-{profile}.properties    
    
    
客户端：
    
    其配置文件bootstrap.properties：
    
    spring.application.name=config-client
    spring.cloud.config.label=master
    spring.cloud.config.profile=dev
    #spring.cloud.config.uri= http://localhost:8888/
    
    eureka.client.serviceUrl.defaultZone=http://localhost:8889/eureka/
    spring.cloud.config.discovery.enabled=true
    spring.cloud.config.discovery.serviceId=config-server
    server.port=8881
    
    spring.cloud.config.label 指明远程仓库的分支
    spring.cloud.config.profile
    dev开发环境配置文件
    test测试环境
    pro正式环境
    spring.cloud.config.uri= http://localhost:8888/ 指明配置服务中心的网址。
    spring.cloud.config.discovery.enabled 是从配置中心读取文件。
    spring.cloud.config.discovery.serviceId 配置中心的servieId，即服务名。
    
    
config-client从config-server获取了foo的属性，而config-server是从git仓库读取的.


Spring Cloud Bus
Spring Cloud Bus 将分布式的节点用轻量的消息代理连接起来。它可以用于广播配置文件的更改或者服务之间的通讯，也可以用于监控。本文要讲述的是
用Spring Cloud Bus实现通知微服务架构的配置文件的更改。   
    