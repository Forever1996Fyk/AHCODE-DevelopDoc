# 项目整体结构

```text
ah-code
├── ahcloud-denpendencies -- 统一依赖管理
└── ahcloud-common -- 系统公共模块管理 
     ├── ahcloud-common-core -- 公共工具类核心包
└── ahcloud-pojo -- 系统领域模型管理 
    ├── ahcloud-entity -- 数据对象: 跟数据表字段对应
    ├── ahcloud-dto -- 数据传输对象: 用于不同系统之间的传输对象
    ├── ahcloud-vo -- 展示对象: 传回前端的对象数据模型
    ├── ahcloud-query -- 数据查询对象: 前端传入的查询对象

```		

项目结构会随着开发的过程不断演进。