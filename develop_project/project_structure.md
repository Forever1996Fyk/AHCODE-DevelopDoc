# 项目整体结构

```text
ah-code
├── ahcloud-denpendencies -- 统一依赖管理
├── ahcloud-parent -- 项目父工程
└── ahcloud-commons -- 系统公共模块管理 
     ├── ahcloud-common-core -- 公共工具类核心包
     ├── ahcloud-common-web --  服务提供端核心公共包
     ├── ahcloud-common-auth-client-starter -- 资源服务器公共封装包
     ├── ahcloud-common-auth-server-starter -- 认证服务器公共封装包
└── ahcloud-auth-server -- 统一认证服务
└── ahcloud-service -- 服务提供者
    ├── ahcloud-base-authority-service -- 基础权限服务
    ├── ahcloud-gateway-service -- 网关服务
└── ahcloud-app -- 服务消费者
    ├── ahcloud-base-authority-app -- 基础权限服务消费者
    ├── ahcloud-gateway-app -- 网关服务消费者
    ├── ahcloud-common-app -- 公共消费者封装包
└── ahcloud-api -- api接口
    ├── ahcloud-base-authority-api -- 基础权限接口
    ├── ahcloud-gateway-api -- 网关服务接口
```		

项目结构会随着开发的过程不断演进。