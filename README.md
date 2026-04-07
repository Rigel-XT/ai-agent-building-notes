# Helium 10 Automation Agent

## 📖 项目简介 (Introduction)

本项目旨在开发一个高度自动化的 AI Agent，核心目标是**最大化利用 Helium 10 的数据价值与功能**，从而显著**减轻亚马逊运营人员的日常工作量**，提升运营效率。

## 🏗️ 整体系统架构 (Architecture)

本系统依托 AWS Serverless 架构与 Amazon Bedrock 构建，整体数据流与服务连接链路如下：

- **数据存储层 (Storage):** 数据文件存储于 Amazon S3，作为业务知识库的源头。
- **知识库层 (Knowledge Base):** Amazon Bedrock Knowledge Base (KB) 接入并处理 S3 中的数据。
- **大模型与计算层 (Agent & Compute):** Amazon Bedrock 作为核心大脑（底层调用 Amazon 第一方原生大模型），直接连接并触发 AWS Lambda Function 来执行具体的自动化业务逻辑。
- **网关层 (API Gateway):** AWS Lambda (或 Bedrock) 通过 Amazon API Gateway 将服务暴露为 RESTful API。
- **展示层 (Front End):** 前端应用连接 API Gateway，实现与用户的最终交互。

**链路拓扑简图：**
`S3` ➡️ `Bedrock KB` ➡️ `Bedrock (Amazon Native Model)` ➡️ `Lambda Function` ➡️ `API Gateway` ➡️ `Front End`
