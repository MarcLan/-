## Terraform模块

[TOC]

### 模块概述

#### 遇到问题

- 配置文件复杂
- 更新配置文件风险大
- 重复数量多
- 不同环境
- 共享部分配置
- 运维困难

#### 模块用途

- 组织配置 -- 更轻松地导航、理解、更新配置
- 封装配置 -- 配置封装在不同的逻辑组件中
- 重用配置 -- 节省时间，共享模块
- 最佳实践 -- 确保一致性
- 自助服务 -- 构建和发布无代码就绪模块

#### 模块概念

- 模块是单个目录中的一组Terraform配置文件

- 运行Terraform命令的模块就是**根模块**

- 目录结构

  ```
  .
  ├── LICENSE
  ├── README.md
  ├── main.tf
  ├── variables.tf
  ├── outputs.tf
  ```

#### 模块调用

- 本地+远程

---

### 构建和使用本地模块

#### 先决条件

- 一个云账号
- AWS CLI
- Terraform CLI

#### 模块结构

- 典型结构如下

  ```
  .
  ├── LICENSE
  ├── README.md
  ├── main.tf
  ├── variables.tf
  ├── outputs.tf
  ```

- LICENSE -- 许可证
- README.md -- 如何使用该模块的markdown格式文档
- main.tf -- 主要配置集
- variables.tf -- 变量定义集
- outputs.tf -- 输出定义集
- terraform.tfstate -- 状态跟踪文件
- .terraform -- 插件

