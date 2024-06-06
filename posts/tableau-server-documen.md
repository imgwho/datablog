---
date: 2024-06-04
title: Tableau Server 知识体系参考
tags:
  - Tableau Server
description: 摘要：本文主要介绍Tableau server 的知识体系，包括官方的帮助文档设计的结构和官方考试指南安装的内容等。
---
# Tableau Server 知识体系参考

# 考试指南 部分

请注意：这不是本考试内容的全面清单。  
[https://www.tableau.com/zh-cn/learn/certification/tableau-server-certified-associate-exam-guide](https://www.tableau.com/zh-cn/learn/certification/tableau-server-certified-associate-exam-guide)


## 考试指南结构
### 领域 1：连接到数据并准备数据

**1.1 用户体验**

- 1.1.1 UI
- 1.1.2 导航

**1.2 拓扑**

- 1.2.1 确定客户端组件
- 1.2.2 确定服务器组件
- 1.2.3 说明二者如何配合工作

**1.3 版本**

- 1.3.1 了解：
  - 1.3.1.1 如何确定 Tableau Server 的当前版本
  - 1.3.1.2 从何处获取最新版本的 Tableau Server
  - 1.3.1.3 在何处查看 Tableau Server 的版本说明

**1.4 最低硬件要求**

- 1.4.1 了解：
  - 1.4.1.1 RAM 要求
  - 1.4.1.2 CPU 要求
  - 1.4.1.3 硬盘要求

**1.5 软件要求**

- 1.5.1 列出支持的操作系统
- 1.5.2 了解：

  - 1.5.2.1 浏览器要求
  - 1.5.2.2 电子邮件通知选项
  - 1.5.2.3 防病毒问题
  - 1.5.3 确定 SMTP 服务器
  - 1.5.4 熟悉潜在的端口问题
  - 1.5.5 解释专用服务器的用途和好处
  - 1.5.6 确定云端运行的注意事项

**1.6 许可**

- 1.6.1 了解基于用户能力的授权
- 1.6.1.1 说明不同的许可证类型
- 1.6.1.2 说明许可证类型如何映射到站点角色

**1.7 Server 进程**

- 1.7.1 说明每个 Tableau 服务管理器和 Tableau Server 进程
- 1.7.2 了解：

  - 1.7.2.1 安装时的默认进程数
  - 1.7.2.2 多实例进程
  - 1.7.2.3 流程到流程工作流
  - 1.7.2.4 分布式环境和高可用性环境中的进程
  - 1.7.2.5 负载均衡器的用途

**1.8 数据源识别**

- 1.8.1 确定所需的端口
- 1.8.2 确定所需的数据库驱动程序
- 1.8.3 了解以下区别：

  - 1.8.3.1 文件、关系与多维数据集
  - 1.8.3.2 数据提取与实时连接
- 1.8.4 说明已发布数据源的优势

**1.9 基础结构网络**

- 1.9.1 了解网络延迟含义
- 1.9.2 说明动态 IP 寻址的风险

### 领域 2：安装和配置

**2.1 安装**

- 2.1.1 了解安装步骤和选项

  - 2.1.1.1 安装路径
  - 2.1.1.2 网关端口
- 2.1.2 了解身份存储区和 SSO 选项：

  - 2.1.2.1 外部 (Active Directory) 与本地
  - 2.1.2.2 受信任票证
  - 2.1.2.3 SAML
  - 2.1.2.4 Kerberos 和 OpenID Connect
- 2.1.3 说明自动登录选项的影响
- 2.1.4 了解如何设置 SSL
- 2.1.5 了解在单一计算机环境中安装 Tableau 的最佳做法
- 2.1.6 了解静默安装

**2.2 Tableau Server 配置**

- 2.2.1 了解缓存设置
- 2.2.2 了解如何：

  - 2.2.2.1 应用进程分布
  - 2.2.2.2 配置电子邮件通知/订阅
  - 2.2.2.3 配置可选自定义
- 2.2.3 描述：

  - 2.2.3.1 站点配置选项
  - 2.2.3.2 用户配额 2.2.3.3 存储配额
  - 2.2.3.4 如何启用和编辑站点订阅
  - 2.2.3.5 项目配置选项
  - 2.2.3.6 分组和用户配置选项
- 2.2.4 了解谁可以添加用户

**2.3 添加用户**

- 2.3.1 许可证类型和站点角色
- 2.3.2 管理员级别
- 2.3.3 发布者级别
- 2.3.4 通过 Active Directory 或本地导入

**2.4 安全性**

- 2.4.1 描述以下项的安全配置：
  - 2.4.1.1 站点级别
  - 2.4.1.2 项目级别
  - 2.4.1.3 组级别
  - 2.4.1.4 用户级别
  - 2.4.1.5 数据源级别
  - 2.4.1.6 工作簿级别

**2.5 权限**

- 2.5.1 了解：
  - 2.5.1.1 系统权限组成
  - 2.5.1.2 权限设计的分支
  - 2.5.1.3 Tableau 安全模型
  - 2.5.2 描述“允许”、“拒绝”和“无”之间的差异

### 领域 3：管理

**3.1 了解如何：**

- 3.1.1 维护数据连接
- 3.1.2 创建计划
- 3.1.3 创建、编辑和删除订阅
- 3.1.4 执行服务器分析
- 3.1.5 完成备份和还原
- 3.1.6 执行清理
- 3.1.7 添加、删除或停用用户
- 3.1.8 更新许可证
- 3.1.9 启动、停止或重启
- 3.1.10 使用 tsm 和 tabcmd
- 3.1.11 使用 REST API
- 3.1.12 使用日志文件
- 3.1.13 了解嵌入
- 3.1.14 监视 Desktop 许可证使用
- 3.1.15 管理工作簿和数据源修订历史记录

**3.2 描述如何：**

- 3.2.1 以多种方法查看服务器状态
- 3.2.2 查看电子邮件通知
- 3.2.3 设置数据驱动型通知
- 3.2.4 使用内置管理视图
- 3.2.5 创建自定义管理视图
- 3.2.6 创建性能记录
- 3.2.7 创建嵌套项目
- 3.2.8 使用站点和站点管理选项

**3.3 将最终用户能力与系统管理员能力进行对比**

**3.4 最终用户能力**

**3.5 了解：**

- 3.5.1 表建议
- 3.5.2 发布视图和数据源
- 3.5.3 重命名工作簿
- 3.5.4 通过 Web 与视图交互
- 3.5.5 Web 制作和编辑
- 3.5.6 如何共享视图
- 3.5.7 数据源认证
- 3.5.8 提取缓存

### 领域 4：故障排除

**4.1 了解浏览器中第三方 cookie 的要求**

**4.2 了解如何：**

- 4.2.1 重置 Tableau 用户或 Tableau 运行身份服务帐户的密码
- 4.2.2 打包日志文件以进行报告
- 4.2.3 使用 tsm 验证站点资源
- 4.2.4 重新生成搜索索引
- 4.2.5 使用维护分析报告
- 4.2.6 创建/打开支持请求

### 领域 5：迁移和升级

**5.1 了解升级过程**

**5.2 说明执行干净重装的方法及原因**

**5.3 描述如何迁移到不同的硬件**

**5.4 了解向后兼容性**

# 帮助文档

这里主要以Linux系统为例  
https://help.tableau.com/current/server-linux/zh-cn/get_started_server.htm

## 帮助文档结构

* Tableau Server 入门指南
    * 服务器管理速查表：Salesforce 集成
    * 关于 Tableau 帮助
* Tableau Server 发行说明
* 规划
    * 服务器管理员概述
    * Tableau 服务管理器概述
    * 基础结构规划
        * 安装之前...
        * 磁盘要求
        * 推荐的基准配置
        * 身份存储
        * 使用外部身份存储的部署中的用户管理
        * 域信任要求
        * 与 Internet 通信
        * 针对 Tableau Server 配置代理
* 部署
    * 安装和配置 Tableau Server
        * 安装之前...
        * 最低硬件要求和推荐配置
        * 安装和初始化 TSM
        * 激活 Tableau
            * 使用授权运行 (ATR) 服务激活 Tableau Server
            * 脱机激活 Tableau
        * 配置初始节点设置
            * 配置文件示例
            * 服务器使用情况数据
                * 基本产品数据
        * 添加管理员帐户
        * 验证安装
        * 初始节点安装默认值
        * 快速启动安装
        * 配置本地防火墙
    * 自动安装
    * 在断开连接（无网络连接）的环境中安装 Tableau Server
    * 克隆 Tableau Server
    * 容器中的 Tableau Server
        * 使用设置工具
        * 使用映像
        * 容器中的 Server 疑难解答
        * 快速入门
    * 安装后任务
        * 安全强化检查表
        * 配置 SMTP 设置
        * TSM 中的文件和权限
        * 配置服务器事件通知
        * 配置数据缓存
        * 数据库驱动程序
        * 服务器崩溃报告程序
            * 配置服务器崩溃报告程序
        * 在 Web UI 的管理区域中导航
        * 将 Tableau Server 移至另一个驱动器
    * 分布式安装和高可用性安装
        * 分布式要求
        * 分布式安装建议
        * 安装和配置附加节点
        * 数据库驱动程序
        * 示例：安装并配置三节点高可用性群集
        * 添加负载平衡器
        * 部署协调服务整体
        * 配置客户端文件服务
        * 存储库故障转移
        * 从初始节点故障中恢复
        * 从节点故障中恢复
        * 配置节点
        * 通过节点角色管理工作负载
        * 在双节点群集上安装服务器
        * 重新启动多节点 Tableau Server 计算机
        * 维护分布式环境
            * 移动存储库进程
            * 移动文件存储进程
            * 移动消息服务进程
            * 移除节点
            * 配置仅协调服务节点
            * 添加负载平衡器
    * 升级
        * 准备升级
            * 服务器升级 - 最低硬件推荐配置
            * 服务器升级 - 查看更改的内容
            * 服务器升级 - 收集配置详细信息
            * 服务器升级 - 验证许可状态
            * 服务器升级 - 验证帐户
            * 服务器升级 - 备份 Tableau Server
            * 服务器升级 - 下载安装程序
            * Tableau Server 升级的工作方式
            * 更新功能 - 升级前须知事项
        * 从 2018.1 及更高版本升级 (Linux)
            * 服务器升级 - 禁用计划任务
            * 单服务器升级 -- 运行安装程序
            * 多节点升级 -- 运行安装程序
            * 多节点升级 -- 在每个节点上运行安装程序
            * 多节点升级 -- 运行升级脚本
            * 验证 Tableau Server 升级
            * 升级后清理
        * 使用蓝/绿方法升级 Tableau Server
        * 从 10.5 升级 Linux 版 Tableau Server
        * 测试升级
        * 安装和升级疑难解答
    * 卸载 Tableau Server
        * 移除 Tableau Server
        * Tableau-server-obliterate 脚本的帮助输出
* 迁移
    * 将 Tableau Server 迁移到 Tableau Cloud
        * 从 Tableau Server 迁移到 Tableau Cloud 的技术注意事项
    * Server 到 Server 的迁移
        * 迁移到新硬件
        * 将 Tableau Server 从 Windows 迁移到 Linux
            * 从 Tabadmin 迁移到 TSM CLI
        * 将 Tableau Server 从本地计算机迁移到云中的 VM
        * 更改身份存储
* 管理站点
    * 什么是站点？
    * 计划站点
    * 站点设置参考
    * 管理用户和组
        * 向站点添加用户
        * 设置用户的站点角色
        * 查看、管理或移除用户
        * 设置用户身份验证类型
        * 导入用户
        * CSV 导入文件准则
        * 管理用户可见性
        * 启用来宾访问
        * 使用组
            * 向组添加用户
            * 创建本地组
            * 通过 Active Directory 创建组
            * 在站点上同步 Active Directory 组
            * 在服务器上同步所有 Active Directory 组
            * 快速开始：按计划同步所有 Active Directory 组
            * 登录时授予角色（登录时授予许可证）
            * 删除组
    * 自助环境的自定义门户
    * 管理项目和内容访问
        * 设置站点的 Web 制作访问权限
        * 针对内容设置 Web 编辑、保存和下载访问权限
        * 配置托管自助服务的项目
        * 使用项目管理内容访问权限
            * 添加项目并将内容移至其中
            * 添加项目图像
        * 允许用户请求访问内容和项目
        * 设置权限和所有权
            * 权限能力和模板
            * 使用项目管理权限
            * 有效权限
            * 权限、站点角色和许可证
            * 快速开始：设置权限
            * 管理内容所有权
            * 管理外部资产的权限
    * 管理数据
        * Tableau Server 数据源
        * 数据提取升级为 .hyper 格式
        * 为数据提取设置站点时区
        * 在 Web 上创建数据提取
        * 查看数据源属性
        * 使数据保持最新
            * 管理刷新任务
            * 向数据提取计划添加工作簿或数据源
            * 快速开始：按计划刷新数据提取
            * 自动刷新任务
            * 处理数据提取刷新通知
            * 自动挂起非活动工作簿的数据提取刷新
        * 在 Tableau Server 上编辑连接
        * 多维数据集数据源
        * Web 数据连接器
        * 审查 Web 数据连接器
        * 启用 Tableau Catalog
            * 获取初始摄取状态
            * 获取事件状态
        * 为影响分析使用世系
        * 数据标签
            * 使用认证来帮助用户查找受信任的数据
            * 设置数据质量警告
            * 敏感度标签
            * 具有自定义类别的标签
            * 管理数据标签
        * 在 Tableau Server 中管理仪表板扩展程序
        * 配置与分析扩展程序的连接
        * 表扩展程序
        * 配置 Einstein Discovery 集成
        * 配置外部操作工作流集成
        * 将 Tableau 与 Slack 集成
        * Creator：连接到 Web 上的数据
        * 运行初始 SQL
        * 在 Web 上创建流程并与其进行交互
            * Web 上的 Tableau Prep
    * 创建视图并在 Web 上与其进行交互
        * 管理凭据
        * 在个人空间中创建和编辑私有内容
        * 使用关系进行多表数据分析
            * Tableau 数据模型中的逻辑层和物理层
            * 关系与联接有何不同
            * 使用性能选项优化关系
        * 将 Web 图像动态添加到工作表
        * 使用“数据问答”（Ask Data）功能自动生成视图
            * 创建针对特定受众聚焦“数据问答”(Ask Data) 功能的镜头
            * 为站点禁用或启用“数据问答”(Ask Data) 功能
            * 针对“数据问答”（Ask Data）功能（Ask Data）优化数据
        * 创建 Tableau 数据故事（仅限英文）
            * 将 Tableau 数据故事添加到仪表板
            * 选择适用于您的 Tableau 数据故事的正确故事类型
            * 配置 Tableau 数据故事的设置
                * 配置 Tableau 数据故事设置：分析
                * 配置 Tableau 数据故事设置：特征
                * 配置 Tableau 数据故事设置：显示
                * 配置 Tableau 数据故事设置：驱动因素
                * 配置 Tableau 数据故事设置：叙述
                * 配置 Tableau 数据故事设置：关系
            * 自定义您的 Tableau 数据故事
                * 自定义您的 Tableau 数据故事：上下文变量
                * 自定义您的 Tableau 数据故事：函数
                * 自定义您的 Tableau 数据故事：隐藏和重新排序内容
            * 为您的 Tableau 数据故事添加更多数据
            * 将弹出式 Tableau 数据故事添加到仪表板
            * 在 Tableau 数据故事中创建自定义度量关系
            * 刷新 Tableau 数据故事中的参数
            * 在 Tableau 数据故事中使用表计算
        * 使用“数据解释”功能更快地发现见解
            * “数据解释”功能入门
            * 解释类型
            * 查看分析的字段
            * 使用“数据解释”功能的要求和注意事项
            * 控制对“数据解释”功能的访问
            * “数据解释”功能的工作原理
            * 为站点禁用或启用“数据解释”功能
        * 使用仪表板扩展程序
        * 设置动画格式
        * 设置数字和 Null 值的格式
        * 自定义日期格式
        * URL 动作
        * 创建视图或工作簿订阅
        * 使用自定义视图
        * 将视图发布到 Salesforce
        * 配置 Tableau Lightning Web 组件和无缝身份验证
        * 与 Tableau Server 上的数据交互
        * 选择背景地图
        * 创建指标并排查其问题
        * 确定其他人访问您发布的数据的方式
        * 使用数据指南浏览仪表板
        * 为查询缓存和视图加速设置数据新鲜度策略
        * 使用动态轴范围
        * 使用动态轴标题
        * 使用动态区域可见性
        * 工作簿优化器
* 管理服务器
    * 安全性
        * 身份验证
            * 本地身份验证
            * SAML
                * SAML 要求
                * 配置服务器范围 SAML
                * 在 Tableau Server 上使用 Salesforce IdP 配置 SAML
                * 为 Tabbleau Viz Lightning Web 组件配置 SAML
                * 在 Tableau Server 上使用 Azure AD IdP 配置 SAML
                * 在 Tableau Server 上使用 AD FS 配置 SAML
                * 将 SAML SSO 与 Kerberos 数据库委派结合使用
                * 配置特定于站点的 SAML
                * 更新 SAML 证书
                * SAML 疑难解答
            * Kerberos
                * Kerberos 要求
                * 了解密钥表要求
                * 配置 Kerberos
                * Kerberos SSO 的浏览器支持
                * Kerberos 疑难解答
            * 相互 SSL
                * 相互 SSL 身份验证的工作原理
                * 在相互身份验证过程中将客户端证书映射到用户
            * OpenID Connect
                * 使用 OpenID Connect 的要求
                * 配置身份提供程序 (IdP)
                * 配置 Tableau Server
                * 使用 OpenID 登录到 Tableau Server
                * 身份验证请求参数
                * 针对 OpenID Connect 更改 Tableau Server 中的 IdP
                * OpenID Connect 疑难解答
            * 受信任的身份验证
                * 向 Tableau Server 添加受信任的 IP 地址
                * 从 Tableau Server 获取票证
                * 显示视图及票证
                * 可选：配置客户端 IP 匹配
                * 测试受信任的身份验证
                * 受信任的身份验证疑难解答
                    * 从 Tableau Server 返回了票证值 -1
                    * HTTP 401 – 未授权
                    * HTTP 404 – 未找到文件
                    * 无效用户（SharePoint 或 C#）
                    * 尝试从错误的 IP 地址检索票证
                    * Cookie 限制错误
                    * 与服务器通信时出错 (403)
            * 个人访问令牌
            * 配置 Tableau 已连接应用以启用嵌入内容的 SSO
                * 已连接应用的访问范围
                * 对已连接应用进行故障排除
            * 注册 EAS 以启用嵌入内容的 SSO
            * 数据连接身份验证
                * 启用 Kerberos 委派
                    * 为 JDBC 连接器启用 Kerberos 委派
                * 为 JDBC 连接器启用 Kerberos 运行身份验证
                * OAuth 数据连接
                    * 将 Salesforce.com OAuth 更改为已保存凭据
                    * 将 Tableau Server 连接到 Salesforce Data Cloud
                    * Hyper Query Processing（测试版）
                    * 为 Google 设置 OAuth
                    * 为 Oauth 和现代身份验证配置 Azure AD
                    * 将 Snowflake OAuth 更改为已保存凭据
                    * 为 QuickBooks Online 设置 OAuth
                    * 为 Dremio 设置 OAuth
                    * 为 Dropbox 设置 OAuth
                    * 设置 Amazon Redshift IAM OAuth
                    * 设置 Amazon Redshift IAM Identity Center OAuth
                    * 允许已保存访问令牌
                    * OAuth 连接疑难解答
                * 配置 SAP HANA SSO
                * 启用 Kerberos 服务帐户访问
                    * 为 JDBC 连接器启用 Kerberos 运行身份验证
                * SQL Server 模拟
                    * 模拟要求
                    * 模拟的工作原理
                    * 使用用户运行身份帐户进行模拟
                    * 使用嵌入式 SQL 凭据进行模拟
            * 配置自定义 TSM 管理组
        * 授权
        * 数据安全
            * Tableau 中的行级安全性选项概述
            * 数据源和工作簿的 RLS 最佳实践
            * 数据库中的行级安全性
            * 管理服务器密文
            * 扩展程序安全性 - 最佳部署做法
            * Tableau Server 密钥管理系统
            * 静态数据提取加密
        * 网络安全
            * Tableau Server 中的单击劫持保护
            * HTTP 响应标头
                * 内容安全策略
            * SSL
                * 配置外部 SSL
                * 示例：生成密钥和 CSR
                * 针对内部 Postgres 通信启用 SSL
                * 为 TSM 控制器配置自定义 SSL 证书
                * 配置用于直接连接的 SSL
                * 配置相互 SSL 身份验证
                * 在相互身份验证过程中将客户端证书映射到用户
            * 将加密通道配置为 LDAP 外部身份存储
        * 系统用户和 sudo 权限
        * 安全强化检查表
    * 管理许可证
        * 许可概述
        * 了解许可证模式和产品密钥
        * 可更新订阅许可
        * 查看服务器许可证
        * 刷新产品密钥的过期日期
        * 添加容量
        * 脱机激活 Tableau Server
        * 停用产品密钥
        * 脱机停用 Tableau
        * 自动执行许可任务
        * 处理未许可的服务器
        * 从基于内核的许可迁移到基于角色的许可
        * 快速入门：将基于登录名的许可证管理与 Tableau Server 结合使用
        * 基于登录名的许可证管理
        * 零停机许可
    * 关于身份迁移
        * 管理身份迁移
        * 解决身份迁移冲突
        * 对身份迁移问题进行故障排除
    * 使用身份池预置和验证用户
        * 设置和管理身份池
    * 将用户添加到服务器
    * 登录 Tableau Server 管理页面
        * 在 Web UI 的管理区域中导航
    * 登录到 Tableau 服务管理器 Web UI
    * 自定义服务器
        * 语言和区域设置
        * 在 Tableau Server 中使用自定义字体
    * 跨服务器管理站点
        * 站点概述
        * 导出或导入站点
        * 添加或删除站点
        * 站点可用性
        * 管理站点角色限制
        * 使用户能够保存修订历史记录
        * Tableau Mobile 应用程序安全设置
    * 数据提取刷新、订阅、数据驱动型通知和指标
        * 启用数据提取刷新计划和失败通知
        * 创建或修改计划
        * 启用自定义订阅计划
        * 如何划分计划任务的优先级
        * 在计划刷新后配置工作簿性能
        * 确保访问订阅和数据驱动的通知
        * 设置订阅站点
        * 为数据驱动型通知设置服务器
        * 针对指标进行设置
        * 编辑已发布数据源
    * 管理后台程序作业
    * 管理 TSM 作业
        * 取消 TSM 作业
    * 管理视图
        * 预先构建的管理视图
            * 视图性能
            * 流程运行的性能
            * 到视图的流量
            * 到数据源的流量
            * 所有用户的动作
            * 特定用户的动作
            * 最近用户的动作
            * 数据提取后台任务
            * 非数据提取后台任务
                * “升级缩略图”作业
            * 后台任务延迟
            * 加载时间统计数据
            * 空间使用情况统计数据
            * 服务器磁盘空间
            * 基于登录名的许可证使用情况
            * 桌面许可证使用
            * 桌面许可证过期
            * 后台程序仪表板
            * 过时内容
            * 数据问答功能使用情况
            * 数据质量警告历史记录
        * 创建自定义管理视图
    * 性能
        * 性能概述
        * 一般性能准则
        * 监视
            * 使用 Tableau Server 存储库收集数据
                * 关于 Tableau Server 数据字典（工作组数据库）
        * 调整
            * 针对用户流量进行优化
                * 配置客户端呈现
            * 针对数据提取进行优化
            * 针对数据提取查询密集型环境进行优化
            * 何时添加工作服务器和重新配置
        * 性能记录
            * 创建性能记录
            * 解释性能记录
        * 性能资源
        * 配置客户端呈现
        * 视图加速
        * 数据提取查询负载平衡
    * 监视服务器运行状况
        * 配置 SMTP 设置
        * 配置通知和订阅
    * 维护
        * 备份和还原
            * 执行 Tableau Server 的完整备份和还原
            * 备份 Tableau Server 数据
            * 还原备份内容
        * 服务器维护
            * 服务器进程状态
                * 远程访问状态
                * 以 XML 形式获取进程状态
                * 服务器进程疑难解答
            * 清除保存的数据连接密码
            * 在服务器上同步所有 Active Directory 组
            * 为所有用户设置默认开始页面
            * 直接从连接的客户端中访问站点
            * 禁用自动客户端身份验证
            * 移除不需要的文件
            * 服务器设置（常规）
            * 停止或重新启动 Tableau Server 计算机
    * TSM - 命令行接口
        * tsm authentication
        * tsm configuration
        * tsm&nbsp;configuration set 选项
        * tsm customize
        * tsm data-access
        * tsm email
        * tsm initialize
        * tsm jobs
        * tsm licenses
        * tsm login
        * tsm logout
        * tsm maintenance
        * tsm pending-changes
        * tsm register
        * tsm reset
        * tsm restart
        * tsm schedules
        * tsm security
        * tsm settings
        * tsm sites
        * tsm start
        * tsm status
        * tsm stop
        * tsm topology
        * tsm user-identity-store
        * tsm version
        * tsm File Paths
    * 实体定义和模板
        * 配置文件示例
        * gatewaySettings 实体
        * identityStore 实体
        * kerberosSettings 实体
        * mutualSSLSettings 实体
        * openIDSettings 实体
        * samlSettings 实体
        * sapHanaSettings 实体
        * shareProductUsageDataSettings 实体
        * trustedAuthenticationSettings 实体
        * Web-data-connector-settings 实体
    * tabcmd
        * tabcmd 命令
        * tabcmd 的安装开关和属性 (Windows)
    * 疑难解答
        * 疑难解答
        * 使用日志文件
            * Tableau Server 日志和日志文件位置
            * 将日志存档
            * 更改日志记录级别
        * 安装和升级疑难解答
            * Systemd 用户服务失败
        * 排查因服务故障导致的作业失败
        * 排查服务器登录问题
        * 许可疑难解答
        * 处理未许可的 VizQL Server 进程
        * TSM 命令超时
        * Tableau 服务管理器 (TSM) 备份疑难解答
        * Cookie 限制错误
        * 订阅疑难解答
    * 参考
        * Tableau Server 进程
            * Tableau Server 管理代理
            * Tableau Server 管理控制器
            * Tableau Server 应用程序服务器
            * Tableau Server 后台程序进程
            * Tableau Server 缓存服务器
            * Tableau Server 客户端文件服务
            * Tableau Server 集合服务
            * Tableau Server 内容探索服务
            * Tableau Server 协调服务
            * Tableau Server 数据引擎
            * Tableau Server Data Server
            * Tableau Server 的“数据源属性”服务
            * Tableau Server 文件存储
            * Tableau Server 网关进程
            * 索引和搜索服务器
            * Tableau Server 的“内部数据源属性”服务
            * Tableau Server 消息服务
            * Tableau Server 指标服务
            * Tableau Server 微服务容器
            * Tableau Server 存储库
            * Tableau Server 资源限制管理器
            * Tableau Server SAML 服务
            * Tableau Server 搜索和浏览
            * Tableau Statistical Service
            * Tableau Server TSM 维护服务
            * Tableau Server VizQL Server
            * Tableau Prep Conductor
            * Tableau Prep Flow Authoring
            * Tableau Server 动态拓扑更改
            * 服务器进程限制
        * Tableau Server 端口
            * 启用 JMX 端口
        * ATRDiag.exe 命令行参考
        * initialize-tsm 脚本的帮助输出
        * Upgrade-tsm 脚本的帮助输出
        * 查看服务器版本
        * 配置 Einstein Discovery 集成
            * 在 Salesforce.com 中为 Tableau Server 中的 Einstein Discovery 集成配置 CORS
        * 配置与分析扩展程序的连接
        * 更改身份存储
        * 外部身份识存储配置参考
        * 基本产品数据
    * 存档的内容
        * 在公共云服务中自托管 Tableau Server
            * 在 Amazon Web 服务上安装 Tableau Server
                * AWS 上的 Tableau Server 部署选项
                * 先决条件
                * 最佳做法
                * AWS 拓扑上的 Tableau Server
                * 选择 AWS 实例类型和大小
                * AWS 上单一 Tableau Server 的自部署
                * 在 AWS 上的分布式环境中对 Tableau Server 进行自部署
                * 在 AWS 上保护 Tableau Server
                * 优化性能
                * 在 AWS 上扩展 Tableau Server
                * AWS 上的 Tableau Server 疑难解答
            * 在 Google 云平台上安装 Tableau Server
                * 在 Google 云平台上部署 Tableau Server 的选项
                * 开始之前需要的内容：
                * 在 Google 云平台上安装 Tableau Server 的最佳做法
                * Google 云平台上的 Tableau Server 拓扑
                * 选择 Google 计算引擎虚拟机类型和大小
                * 在 Google 云平台上自行部署单个 Tableau Server
                * 在分布式环境中的 Google 云平台上自行部署 Tableau Server
                * Google 云平台上的 Tableau Server 疑难解答
            * 在 Microsoft Azure 上安装 Tableau Server
                * Microsoft Azure 上的 Tableau Server 部署选项
                * 开始之前需要的内容：
                * 在 Microsoft Azure 上安装 Tableau Server 的最佳做法
                * Microsoft Azure 拓扑上的 Tableau Server
                * 选择 Microsoft Azure 虚拟机类型和大小
                * 在 Microsoft Azure 上自行部署单个 Tableau Server
                * 在分布式环境中的 Microsoft Azure 上自行部署 Tableau Server
                * Microsoft Azure 上的 Tableau Server 疑难解答
            * 在阿里云中安装 Tableau Server
                * 在阿里云上部署 Tableau Server 的选项
                * 开始之前需要的内容：
                * 在阿里云中安装 Tableau Server 的最佳做法
                * 阿里云拓扑上的 Tableau Server
                * 选择阿里巴巴 ECS 实例类型和大小
                * 在阿里云上自行部署单个 Tableau Server
                * 在分布式环境中的阿里云上自行部署 Tableau Server
                * 阿里云上的 Tableau Server 疑难解答
    * Advanced Management
        * Resource Monitoring Tool
            * Resource Monitoring Tool 入门指南
                * 概念
                * 安装前检查清单
                * 硬件推荐配置
            * 安装
                * 使用 Web 界面安装 RMT Server
                * 使用 Web 界面安装代理
                * 使用命令行安装 RMT Server
                * RMT Server 初始化脚本选项
                * 使用命令行安装代理
                * RMT 代理初始化脚本选项
                * Tableau Resource Monitoring Tool 的外部存储库
                * Tableau Resource Monitoring Tool 的外部消息队列服务 (RabbitMQ)
                * Tableau Resource Monitoring Tool 先决条件 - 许可证
            * 升级
            * 卸载
            * 管理和配置
                * 配置 UI
                * 主配置文件
                * rmtadmin 命令行实用工具
                * Resource Monitoring Tool 通信端口
                * 添加和管理用户
                * 配置事件
                    * 环境关闭
                    * 代理事件
                    * 数据提取失败
                    * 硬件
                    * Hyper 假脱机事件
                    * 查询缓慢
                    * 慢速视图
                * 加密数据收集
                * 主服务器的硬件更改
                * Tableau Server 拓扑更改
                * Resource Monitoring Tool 日志文件
                * Tableau 日志文件
                * Tableau Server 升级
            * 监视 Tableau Server 性能
                * 使用 Tableau Resource Monitoring Tool 监视 Tableau Server 性能
                    * Tableau Resource Monitoring Tool 性能图表
                    * Tableau Resource Monitoring Tool 活动页面
                    * Tableau Resource Monitoring Tool 内容页面
                    * 调查慢速视图加载请求
                * 数据收集中使用的工具
                * 使用 Tableau 数据源文件浏览监视数据
                * 按存储容量使用计费报告
            * 疑难解答
                * 排查硬件性能数据缺失问题
                * 对 RMT Server 服务中断进行故障排除
                * 排查 Tableau Server 进程的未知状态
                * 用户身份验证疑难解答
                * 排查 Web 界面超时问题
            * 将适用于服务器的高级工具升级到 Tableau Resource Monitoring Tool
                * Tableau Resource Monitoring Tool 旧版许可证密钥激活
        * Content Migration Tool
            * Tableau Content Migration Tool 入门
            * 安装 Tableau Content Migration Tool
            * 使用 Tableau Content Migration Tool
            * Tableau Content Migration Tool 用例
            * 迁移规划概述
                * 迁移限制
                * 迁移计划：站点
                * 迁移计划：源项目
                * 迁移计划：工作簿
                * 迁移计划：已发布数据源
                * 迁移计划：权限和所有权
                * 迁移计划：迁移脚本
                * 迁移计划：计划选项
                * 迁移使用数据提取的工作簿和数据源
                * 迁移包含嵌入式凭据的工作簿和数据源
            * 使用 Tableau Content Migration Tool 控制台运行程序
                * 示例：编写迁移计划脚本
            * 使用 Content Migration Tool 命令行界面
            * Tableau Content Migration Tool 设置
            * Tableau Content Migration Tool 日志文件
        * 活动日志
            * 使用活动日志审核权限
            * 活动日志事件类型参考
        * Tableau Server 密钥管理系统
            * AWS 密钥管理系统
            * Azure 密钥库
        * Tableau Server 外部文件存储
            * 使用外部文件存储安装 Tableau Server
            * 重新配置文件存储
            * 使用外部文件存储进行备份和还原
            * 外部文件存储的性能注意事项
        * Tableau Server 外部存储库
            * 在 AWS 关系数据库服务 (RDS) 上创建 PostgreSQL DB 实例
            * 在 Azure 上创建 Azure 数据库 PostgreSQL 实例
            * 在 Google Cloud 上创建 PostgreSQL 实例
            * 以独立安装的形式创建 PostgreSQL 数据库
            * 随外部 PostgreSQL 存储库一起安装 Tableau Server
            * 重新配置 Tableau Server 存储库
            * 针对 PostgreSQL 新的主要版本升级包含外部存储库的 Tableau Server
            * 升级 RDS 实例
        * 通过节点角色管理工作负载
        * Tableau Server 独立网关
            * 安装具有独立网关的 Tableau Server
            * 配置具有独立网关的 Tableau Server
                * 配置具有独立网关的身份验证模块
                * 在独立网关上配置 TLS
            * 升级 Tableau Server 独立网关
            * 卸载 Tableau Server 独立网关
            * initialize-tsig 脚本的帮助输出
        * Tableau Server 后台程序资源限制
        * 容器中的动态扩展 - Tableau Server 后台程序
    * 数据管理
        * 许可 数据管理
        * Tableau Prep Conductor
            * 关于流程工作区
            * 在 Tableau Server 上启用和配置 Tableau Prep Conductor
                * 步骤 1（新安装）：安装包含 Tableau Prep Conductor 的 Tableau Server
                * 步骤 1（现有安装）：启用 Tableau Prep Conductor
                * 步骤 2：配置 Tableau Server 的流程设置
                * 步骤 3：为流程任务创建计划
                * 步骤 4：安全列表输入和输出位置
                * 步骤 5：可选服务器配置
            * 计划流程任务
            * 通知用户流程运行成功
            * 管理流程
            * 监视流程运行状况和性能
                * 流程的管理视图
            * 开发人员资源 - REST API
        * Tableau Catalog
        * 虚拟连接和数据策略
            * 创建虚拟连接
            * 为行级安全性创建数据策略
            * 使用“以用户身份预览”测试行级安全性
            * 发布虚拟连接并设置权限
            * 为虚拟连接计划数据提取刷新
            * 使用虚拟连接
    * 快速帮助概述

<Comment />
