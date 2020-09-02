# 技术介绍

### 开发环境

- 操作系统: `Windows7`,`Windows8`,`Windows10`
- 开发工具: `Intellij IDEA`, `Eclipse`
- 数据库: `MySQL 5.7`
- Java SDK: `Oracle JDK 1.8`

### 项目管理工具

- 项目构建: `Maven`, `Nexus`
- 代码管理: `Git`, `GitLab`, `Gitee`, `Github`
- 镜像管理: `Docket Registry`

### 后台主要技术栈

- 核心框架: `Spring Boot`, `Spring Cloud Alibaba`
- ORM框架: `Mybatis`
- 数据库连接池: `Alibaba Druid`
- 数据库缓存: `Redis Sentinel`
- 消息中间件: `RocketMQ`
- 全文检索引擎: `ElasticSearch`
- 分布式链路追踪: `SkyWalking`
- 分布式文件系统: `Alibaba OSS`
- 分布式系统网关: `Spring Cloud Gateway`
- 分布式服务中心: `Spring Cloud Alibaba Nacos Server`
- 分布式配置中心: `Spring Cloud Alibaba Nacos Config`
- 分布式熔断降级: `Spring Cloud Alibaba Sentinel`
- 反向代理负载均衡: `Nginx`

### 前台技术栈

- 前端框架: `NodeJS`, `Vue`, `Axios`
- 前端模板: `ElementUI`

### 持续集成

- 持续集成: `GitLab`, `Gitee`, `Github`
- 持续交付: `Jenkins`

### 服务规划

#### Environment Cloud

|服务名称 |服务IP:Port |服务说明 |
|--|--|--|
| MySQL | ip1:3306, ip2:3306 | MySQL集群 |
| Redis | ip1:6379, ip2:6379 | redis cluster集群 |
| Nexus | ip:8081 | 依赖管理 |
| Docker Registry | ip:8080 | 镜像管理 |
| SkyWalking | ip:8080 | 链路追踪 |
| RocketMQ | ip1:8080, ip2:8080 | 消息队列集群 |
| Nacos | ip:8848 | 注册发现/配置中心 |
| Sentinel | ip:8080 | 熔断降级|
| Nginx | ip:80 | 反向代理/负载均衡 |

#### Services

|服务名称 |服务Port |服务说明 |
|--|--|--|
| ahcode-service-provider-shop | 8000 | 商品服务提供者 |
| ahcode-service-consumer-shop | 8000 | 服务商品消费者 |

#### Package Name

**通用包名前缀`com.ahcode.cloud`**
