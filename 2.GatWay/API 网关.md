# 什么是网关
在微服务架构中，一个大应用被拆分成为了多个小的服务系统，这些小系统通常以提供 Rest Api 风格的接口来被 H5, Android, IOS 以及第三方应用程序调用。如果一个外部的应用（浏览器、App）要去访问这个大应用，请求数据来自不同的微服务中，就需要多次调用以检索数据。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/992017/1659862812448-fe388ccc-1311-4b7b-bcb6-b8a82c754103.png#clientId=ueaa52b84-7a5f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=167&id=u167b19e4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=333&originWidth=720&originalType=url&ratio=1&rotation=0&showTitle=false&size=108821&status=done&style=none&taskId=u70504010-c4c7-4358-8208-ef426c40b57&title=&width=360)
因此在基于微服务的项目中为了简化前端的调用逻辑，通常会引入API Gateway作为轻量级网关，同时API Gateway中也会实现相关的认证逻辑从而简化内部服务之间相互调用的复杂度。并且网关还可能具有其他职责，如身份验证、监控、负载均衡、缓存、请求分片与管理、静态响应处理。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/992017/1659862969669-d131e10c-0c31-4cdd-9905-5e3187fea7d3.png#clientId=ueaa52b84-7a5f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=178&id=ub901f3e5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=356&originWidth=720&originalType=url&ratio=1&rotation=0&showTitle=false&size=88715&status=done&style=none&taskId=u3780f028-b542-48e1-9495-25fcc867c73&title=&width=360)
# 网关功能

![image.png](https://cdn.nlark.com/yuque/0/2022/png/992017/1659866041175-09aac802-9fbb-4d1a-b765-801de10c2e76.png#clientId=ueaa52b84-7a5f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=254&id=u1060422a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=507&originWidth=651&originalType=url&ratio=1&rotation=0&showTitle=false&size=77545&status=done&style=none&taskId=u6d1633cb-9853-4f1c-8cf2-a5ea7c13c13&title=&width=326)
**路由功能：**路由是微服务网关的核心能力。通过路由功能微服务网关可以将请求转发到目标微服务。在微服务架构中，网关可以结合注册中心的动态服务发现，实现对后端服务的发现，调用方只需要知道网关对外暴露的服务API就可以透明地访问后端微服务。
**负载均衡：**API网关结合负载均衡技术，利用Eureka或者Consul等服务发现工具，通过轮询、指定权重、IP地址哈希等机制实现下游服务的负载均衡。
**统一鉴权：**一般而言，无论对内网还是外网的接口都需要做用户身份认证，而用户认证在一些规模较大的系统中都会采用统一的单点登录（Single Sign On）系统，如果每个微服务都要对接单点登录系统，那么显然比较浪费资源且开发效率低。API网关是统一管理安全性的绝佳场所，可以将认证的部分抽取到网关层，微服务系统无须关注认证的逻辑，只关注自身业务即可。
**协议转换：**API网关的一大作用在于构建异构系统，API网关作为单一入口，通过协议转换整合后台基于REST、AMQP、Dubbo等不同风格和实现技术的微服务，面向Web Mobile、开放平台等特定客户端提供统一服务。
**指标监控：**网关可以统计后端服务的请求次数，并且可以实时地更新当前的流量健康状态，可以对URL粒度的服务进行延迟统计，也可以使用Hystrix Dashboard查看后端服务的流量状态及是否有熔断发生。
**限流熔断：**在某些场景下需要控制客户端的访问次数和访问频率，一些高并发系统有时还会有限流的需求。在网关上可以配置一个阈值，当请求数超过阈值时就直接返回错误而不继续访问后台服务。当出现流量洪峰或者后端服务出现延迟或故障时，网关能够主动进行熔断，保护后端服务，并保持前端用户体验良好。
**黑白名单：**微服务网关可以使用系统黑名单，过滤HTTP请求特征，拦截异常客户端的请求，例如DDoS攻击等侵蚀带宽或资源迫使服务中断等行为，可以在网关层面进行拦截过滤。比较常见的拦截策略是根据IP地址增加黑名单。在存在鉴权管理的路由服务中可以通过设置白名单跳过鉴权管理而直接访问后端服务资源。
**灰度发布：**微服务网关可以根据HTTP请求中的特殊标记和后端服务列表元数据标识进行流量控制，实现在用户无感知的情况下完成灰度发布。
**流量染色：**和灰度发布的原理相似，网关可以根据HTTP请求的Host、Head、Agent等标识对请求进行染色，有了网关的流量染色功能，我们可以对服务后续的调用链路进行跟踪，对服务延迟及服务运行状况进行进一步的链路分析。
**文档中心：**网关结合Swagger，可以将后端的微服务暴露给网关，网关作为统一的入口给接口的使用方提供查看后端服务的API规范，不需要知道每一个后端微服务的Swagger地址，这样网关起到了对后端API聚合的效果。
**日志审计：**微服务网关可以作为统一的日志记录和收集器，对服务URL粒度的日志请求信息和响应信息进行拦截。
# 常见网关
私有云开源解决方案：

- Netflix Zuul，zuul是spring cloud的一个推荐组件，[https://github.com/Netflix/zuul](https://github.com/Netflix/zuul)
- Kong kong是基于Nginx+Lua进行二次开发的方案， [https://konghq.com/](https://konghq.com/)
- Tyk是2014年创建的开源API网关，甚至比AWS的API网关即服务功能还要早。Tyk用Golang编写并使用Golang自己的HTTP服务器。

公有云解决方案：

- Amazon API Gateway，[https://aws.amazon.com/cn/api-gateway/](https://aws.amazon.com/cn/api-gateway/)
- 阿里云API网关，[https://www.aliyun.com/product/apigateway/](https://www.aliyun.com/product/apigateway/)
- 腾讯云API网关， [https://cloud.tencent.com/product/apigateway](https://cloud.tencent.com/product/apigateway)

自开发解决方案

- 基于Nginx+Lua+ OpenResty的方案，可以看到Kong,orange都是基于这个方案
- 基于Netty、非阻塞IO模型。 通过网上搜索可以看到国内的宜人贷等一些公司是基于这种方案，是一种成熟的方案。
- 基于Node.js的方案。 这种方案是应用了Node.js天生的非阻塞的特性。
- 基于java Servlet的方案。 zuul基于的就是这种方案，这种方案的效率不高，这也是zuul总是被诟病的原因。

[
](https://blog.csdn.net/pushiqiang/article/details/95726137)
# 延伸文章

- 
- 
- 
