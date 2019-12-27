----
2019/12/27
----

#### 1. 这一年的成长

- 对微服务更多的理解，学习并使用了更多的技术
  - Spring Boot的AOP，IoC, Mybatis-plus 等的核心功能的学习和实践，提供开发效率
  - 微服务的划分，模块的限界
  - ISA95等标准的学习和实践，帮助切分微服务
- 对业务价值更深入的挖掘
  - WIP的监控、历史追溯
  - Flowable流程引擎，图形化方式展现
- 采用微服务的形式开发，多个team间的沟通，负责整个系统与其他模块的串接，每周组织整个项目的会议，讨论多服务间的重大问题，基于现在的业务场景，找各个相关team多次讨论定出架构业务逻辑等，推动项目的落地和实施。
  - 思考目前的开发模式，对接模式，找寻更多的方式去节省开发时间；（gitlab, jenkins, gitbook）
  - 基于现在的业务场景，学习一些新技术新理念做架构设计和分享，并去实践验证（Flowable的使用方式）

#### 2. 微服务&SpringBoot

1. 使用SpringBoot&RabbitMQ 微服务的好处或者选型依据？
   - 选型依据1：之前平台受限和领域知识的理解不够，模块功能限界不明确，耦合度很高，且开发效率不高
   - 选型依据2：环境复杂，模块众多，依赖众多，路由繁杂，频繁升级，服务挂掉
   - 微服务优势：模块解耦、横向扩展、调用灵活、容错高可用、可独立部署
   - 独立开发，更小的团队可并行开发，更加快速迭代，应对更多变化；
   - SpringBoot的注解，AOP，IoC与其他组件的集成，MP等等，更好用的平台和框架，节省开发时间，提高开发效率
   - RabbitMQ解耦，将模块的依赖性降低，并行开发等；
   - 相对单体服务，容错性更多，利于根据需要做功能降级，或熔断限流能功能
   - 易扩展，水平扩展将应用部署到多个节点上，灵活性更多，技术选型不受限，各服务可根据不同需要做不同扩展。
   - 技术自由，REST API接口
2. 微服务有哪些挑战或问题？
   - 微服务部署时水平扩容问题
     - 数据库的选型：关系型，NoSql
     - 跨集群、跨地域
     - 数据一致性
     - 缓存
     - 配置管理
     - 一台物理机上部署多个微服务问题
       - 没有体现微服务的隔离
       - K8s+docker
       - 不是通过人为的衡量将多个微服务部署到一起，而是使用一些性能分析工具，通过性能分析，做优化调度
   - 分布式事务
     - 如何保证，如何补偿或rollback
   - 服务发现
     - 如何找到服务，注册发现
     - 多地域部署时，哪个服务是最近效率最快的
   - 服务监控
     - 服务是否挂掉，服务是否需要重启
     - 定位出错服务，Log日志
     - 分部署链路追踪 Zipkin
       - 快速发现问题，判断故障影响范围、梳理服务依赖及依赖的合理性、分析链路性能问题
       - 调用链上的性能指标：吞吐量、响应时间、错误记录
       - 使用全链路监控工具，可：
         - 请求链路追踪，故障快速定位
         - 可视化
         - 依赖优化
         - 数据分析，优化链路
       - Links
         - Sleuth+Zipkin[微服务下的链接追踪](https://blog.csdn.net/pengjunlee/article/details/87797969)
         - [全链路监控：方案概述与比较](https://www.jianshu.com/p/92a12de11f18)
         - Ali[全链路监控](https://blog.csdn.net/b0Q8cpra539haFS7/article/details/82156628)
   - A&A
     - 多个微服务间如何验证
     - RBAC
   - 版本管控
     - 微服务间如何管理mapping关系


#### 3. WIP项目的业务价值

1. 将从订单下单转换为工单后到最后的成品，以最小的粒度将所有的数据记录下来，串联了整个生产过程，给厂区的管理人员和生产人员都提供很多便利；【全局视角，上下游协调成本较高】
   - 生产人员实时的看到当前业务线上的生产状态，及时知晓通知或预警，提高了大约17%的生产效率；
   - 打通了整个厂区的生产线，连接多个软件工具，避免信息孤岛的，让厂区管理人员通过看板可知晓整个厂区（SMT车间）28条产线的实时生产状态，两个星期的排程工单的状态。
   - 最细粒度的数据记录是第一步，有了这些基础数据后，可以提供更多的信息帮助厂区做智能化的企业决策
     - 最新的生产状态，并且可以根据历史数据追溯厂区哪些流程有待提升
     - 哪些工作设备抛料不良品多，提升生产效能；
     - 通过历史追溯记录，提供历史报表，分析或者帮助决策采购，预防性维修等
2. 潜在价值：
   - 与上游客户管理集成，有了WIP的基础数据，能够根据客户需求，提供接口，让客户看到下单之后的整个产品生产的过程，可视化形成呈现。
   - 未来考虑：扩展WIP的生命周期，从订单到成品再到运输整个过程的数据记录，不仅让客户看到生产的过程，运输的过程也可追溯，提升更多的服务，让系统更加数字胡，智能化，更加友好的用户体验。
3. 数字化
   - 最小粒度收集记录产品数据；
   - 串联厂区多个模块系统，接通以工单和产品为基准的流程和数据信息，不再有信息孤岛；
4. 自动化、智能化
   - 智能工具的时候，替代人工操作，减少数据录入错误；
   - 系统自动化生成料表，自动对比，节省人力25%，操作简单易上手，多个中外厂区再使用；
   - 与推荐系统，专家系统集成，通过历史数据分析，提供更多协同功能，预防性维修，预警，通知，取代人工通知，接入微信等其他通信方式，更加智能化；
   - 互联网思维接入到智能制造，提供整个厂区的智能化程度。
5. 标准化
   - 采用更多的业务认可的标准，提供统一规范的接口，一方面让厂区的软件系统管理更加方便，另一方面通过更多的厂区验证，让MES被更多的其他厂商认可，建立更多的信心和实力，引领整个导向，吸引更多的伙伴，建立更好的生态圈。
   - 更好的扩展能力，标准的接口可使得WIP与厂区现有的模块做集成，通过接入数据，不需要弃用之前的系统，新老系统并行使用，节省成本。




---
2019/12/26
----

#### 1. 这一年的成长

- 对微服务更多的理解，学习并使用了更多的技术
  - Spring Boot的AOP，IoC, Mybatis-plus 等的核心功能的学习和实践，提供开发效率
  - 微服务的划分，模块的限界
  - ISA95等标准的学习和实践，帮助切分微服务
- 对业务价值更深入的挖掘
  - WIP的监控、历史追溯
  - Flowable流程引擎，图形化方式展现
- 采用微服务的形式开发，多个team间的沟通，负责整个系统与其他模块的串接，每周组织整个项目的会议，讨论多服务间的重大问题，基于现在的业务场景，找各个相关team多次讨论定出架构业务逻辑等，推动项目的落地和实施。
  - 思考目前的开发模式，对接模式，找寻更多的方式去节省开发时间；（gitlab, jenkins, gitbook）
  - 基于现在的业务场景，学习一些新技术新理念做架构设计和分享，并去实践验证（Flowable的使用方式）



#### 2. 微服务&SpringBoot 

1. 使用SpringBoot&RabbitMQ 微服务的好处或者选型依据？ 

   - 之前平台受限和领域知识的理解不够，模块功能限界不明确，耦合度很高，且开发效率不高
   - 独立开发，更小的团队可并行开发，更加快速迭代，应对更多变化
   - SpringBoot的注解，AOP，IoC与其他组件的集成，MP等等，更好用的平台和框架，节省开发时间，提高开发效率
   - RabbitMQ解耦，将模块的依赖性降低，并行开发等等；
   - 相对单体服务，容错性更多，利于根据需要做功能降级，或熔断限流能功能
   - 易扩展，水平扩展将应用部署到多个节点上，灵活性更多，技术选型不受限，各服务可根据不同需要做不同扩展。

2. 微服务有哪些挑战或问题？

   - 微服务部署时水平扩容问题
     - 数据库的选型：关系型，NoSql
     - 跨集群、跨地域
     - 数据一致性
     - 缓存
     - 一台物理机上部署多个微服务问题
       - 没有体现微服务的隔离
       - K8s+docker 
       - 不是通过人为的衡量将多个微服务部署到一起，而是使用一些性能分析工具，通过性能分析，做优化调度
   - 分布式事务
     - 如何保证，如何补偿或rollback
   - 服务发现
     - 如何找到服务
     - 多地域部署时，哪个服务是最近效率最快的
   - 服务监控
     - 服务是否挂掉，服务是否需要重启
     - 定位出错服务，Log日志
     - 链路追踪

   

#### 3. WIP项目的业务价值

1. 将从订单下单转换为工单后到最后的成品，以最小的粒度将所有的数据记录下来，串联了整个生产过程，给厂区的管理人员和生产人员都提供很多便利；

   - 生产人员实时的看到当前业务线上的生产状态，及时知晓通知或预警，提高了大约17%的生产效率；
   - 打通了整个厂区的生产线，连接多个软件工具，避免信息孤岛的，让厂区管理人员通过看板可知晓整个厂区（SMT车间）28条产线的实时生产状态，两个星期的排程工单的状态。
   - 最细粒度的数据记录是第一步，有了这些基础数据后，可以提供更多的信息帮助厂区做智能化的企业决策
     - 最新的生产状态，并且可以根据历史数据追溯厂区哪些流程有待提升
     - 哪些工作设备抛料不良品多，提升生产效能；
     - 通过历史追溯记录，提供历史报表，分析或者帮助决策采购，预防性维修等

2. 潜在价值：

   - 与上游客户管理集成，有了WIP的基础数据，能够根据客户需求，提供接口，让客户看到下单之后的整个产品生产的过程，可视化形成呈现。
   - 未来考虑：扩展WIP的生命周期，从订单到成品再到运输整个过程的数据记录，不仅让客户看到生产的过程，运输的过程也可追溯，提升更多的服务，让系统更加数字胡，智能化，更加友好的用户体验。

3. 标准化

   - 采用更多的业务认可的标准，提供统一规范的接口，一方面让厂区的软件系统管理更加方便，另一方面通过更多的厂区验证，让MES被更多的其他厂商认可，建立更多的信心和实力，引领整个导向，吸引更多的伙伴，建立更好的生态圈。

   