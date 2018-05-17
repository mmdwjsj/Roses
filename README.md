# Roses the next Guns v1.0
   [https://gitee.com/naan1993/guns](https://gitee.com/naan1993/guns)
   
   ## 介绍
   Roses基于Spring Boot, 致力于做更完善的**分布式**和**服务化**解决方案，Roses提供基于Spring Cloud的分布式框架，整合了springmvc + mybatis-plus + eureka + zuul + feign + ribbon + hystrix等等，提供Roses独有的便捷的开发体验，提供可靠消息最终一致性分布式事务解决方案，提供基于调用链的服务治理，提供可靠的服务异常定位方案（Log + Trace），作者认为，一个好的分布式框架不仅需要构建高效稳定的底层开发框架，更需要解决分布式带来的种种挑战。2018目标，用心做国产开源！
   
   
   ## Roses模块介绍
   
   | 模块名称 | 说明 | 端口 | 备注 |
   | :---: | :---: | :---: | :---: |
   | roses-api | 服务接口和model | 无 | 封装所有服务的接口，model，枚举等 |
   | roses-core | 项目骨架 | 无 | 封装框架所需的基础配置，工具类和运行机制等 |
   | roses-scanner | 资源扫描器 | 无 | 接口无须录入，启动即可录入系统，使资源控制更简单 |
   | roses-register | 注册中心 | 8761 | eureka注册中心 |
   | roses-gateway | 网关 | 8000 | 转发，资源权限校验，请求号生成等 |
   | roses-monitor | 监控中心 | 9000 | 监控服务运行状况 |
   | roses-auth | 鉴权服务 | 8001 | 提供用户，资源，权限等接口 |不能
   | roses-message-service | 消息服务 | 10001 | 可靠消息最终一致性（柔性事务解决方案） | 
   | roses-message-checker | 消息恢复和消息状态确认子系统 | 10002 | 可靠消息最终一致性（柔性事务解决方案） |
   | roses-example-order | 订单服务 | 11001 | 演示如何解决分布式事务 |
   | roses-example-account | 账户服务 | 11002 | 演示如何解决分布式事务 |
   
   ## 微服务带来的优势
   1. 打个比喻，单体服务就像在一个小超市中，一个人负责收银，打扫，摆货，咨询等各种事情，服务化就像是一个大超市，采取分工的方式更好的服务顾客，更加高效。
   2. 多业务并行开发更加容易，单体应用按业务拆分后，一个微服务只需要关注某个方面的业务，例如订单，用户，积分，账户服务等，同时并行开发，代码提交互不影响，每个人只需要关注自己的领域迭代。
   3. 上线发布不会因为某个微服务升级，影响到所有服务，每次某个微服务升级，只需要重新部署一个服务即可。
   4. 技术栈不受限制，在微服务架构中，服务之间调用可用restful api方式进行交互，所以技术选型可以根据不同部门的需要设计不同的服务。
   5. 通过对服务的划分，当系统中某些业务出现瓶颈时，可根据需要进行升级，或者直接增加某些服务的机器。
   
   
   ## 微服务面临的挑战
   1. 服务多级调用带来的调试，跟踪困难。访问一个业务功能，要经过网关，之后调用A服务，B服务，C服务，通常会先去寻找A服务的问题，之后去排查B服务，以此类推，排查所有服务后，也可能问题不在A，B，C三个服务上，也许只是A访问B时调用超时了，所以这就导致了服务出现问题，定位非常困难。
   2. 服务多级调用带来了延时，以及服务容错，熔断等问题。如1所述，多级调用通过restful接口交互，难免会有网络上的延时，如果A调用B，B调用C过程中，服务B已经执行超时，那么就没有必要再去调用C，应当直接返回被调用方超时异常。
   3. 分布式事务问题。A服务调用B服务，之后A又调用C服务，若C服务执行时出现异常，那么如何回滚B服务的执行过程。
   4. 运维成本。更多的服务意味着更多的运维投入，单体架构中，只需部署一个项目即可，微服务中，少则部署数个多则部署几十个服务，并且某些服务可能为集群部署，机器的消耗和运维要比单体服务提升很多。
   5. 开发成本，对技术人员的技术要求（基本技能）。
   7. 监控问题。
   
   ## 幂等性，可查询操作，一切基于requestdata ResponseData，