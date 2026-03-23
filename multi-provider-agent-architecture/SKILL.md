---
name: multi-provider-agent-architecture
description: Use whenever the user asks about building an AI application or agent that spans multiple model providers, or mentions OpenAI, Gemini, 火山方舟, ARK, unified chat API, Responses API, generateContent, provider adapter, capability registry, model routing, or how to design a multi-provider LLM architecture. This skill should trigger even if the user phrases it as API migration, SDK design, request-body unification, agent platform design, or "should we just use one chat schema for everything?"
---

# Multi-Provider Agent Architecture

这个技能用于回答“多厂商大模型接入与 agent 架构怎么设计”这一类问题。核心目标不是把所有厂商压成同一个 HTTP 请求体，而是帮助用户在应用层保持统一、在 provider 接入层保留各家最佳能力。

## 何时使用

当用户出现下面任一类问题时，优先使用这个技能：

- 想同时接入 OpenAI、Gemini、火山方舟或其他多家模型厂商
- 纠结是否应该统一走 `chat` 风格接口
- 询问 `Responses API`、`generateContent`、`Live API`、OpenAI 兼容层如何取舍
- 设计 agent 框架、模型路由、provider adapter、能力矩阵、工具调用抽象
- 规划从单一 provider 扩展到多 provider 的迁移路径
- 讨论多模态、结构化输出、工具调用、上下文状态、实时能力在不同厂商之间如何适配

## 核心立场

先给结论：

- 不要把“统一原始请求体”当成目标本身。
- 应该统一的是业务语义、能力抽象和架构边界，而不是每家厂商的底层 JSON 形状。
- 简单场景可以先用统一的 `chat-like` 抽象快速落地。
- 复杂 agent 场景应采用 `Canonical IR + Provider Adapter + Capability Registry`。

## 必须遵守的区分

回答时必须明确区分三类内容：

1. 官方事实
2. 工程建议
3. 基于当前场景的推断

不要把主观架构偏好说成官方硬规定。

## 稳定知识与易变知识

### 稳定知识

这些内容通常可以直接回答：

- 为什么“统一语义层”优于“统一原始请求体”
- 推荐分层：`Agent Core -> Canonical IR -> Provider Adapters -> Capability Registry`
- 何时适合统一 `chat-like` 抽象
- 何时必须保留 provider-native API
- 为什么 agent 系统更依赖工具调用、结构化输出、流式事件、多轮状态和多模态，而不是“messages 长得像不像”

### 易变知识

这些内容可能变化，默认先查官方文档再回答：

- 哪家当前官方更推荐哪条 API 路径
- 某个模型或接口的最新字段、限制、兼容性
- 当前可用模型、最新推荐模型、是否弃用旧接口
- 实时/流式/结构化输出/工具调用的最新支持情况

只要用户提到 `最新`、`现在`、`官方推荐`、`最佳实践`、`当前是否支持`，或者你怀疑信息可能变化，就先查官方文档。

## 默认决策框架

### 1. 先判断问题类型

将用户问题归入以下一种或多种：

- 架构决策
- 接口迁移
- provider 对比
- 模型路由
- 实现落地

### 2. 默认先输出一句结论

第一段先明确回答：

- 是否建议统一 `chat API`
- 是否建议走 provider-native API
- 当前阶段更适合 MVP 方案还是生产级方案

### 3. 再给推荐分层

默认推荐以下术语和层次：

- `Agent Core`
  - 负责任务编排、工具注册、重试、预算、会话、日志、trace
- `Canonical IR`
  - 用统一语义表达消息、工具、工具结果、结构化输出、附件、流式事件、usage
- `Provider Adapter`
  - 把统一语义编译到各家最佳 API
- `Capability Registry`
  - 记录每个 provider / model 支持什么能力，并驱动路由和降级

### 4. Provider 默认建议

除非官方文档显示已变化，否则默认使用以下方向，并在需要时标注“请以当前官方文档为准”：

- OpenAI
  - 新项目优先考虑 `Responses API`
  - 若只是简单文本/兼容旧系统，可继续使用 `Chat Completions`
- Gemini
  - 默认走 `generateContent` / `generateContentStream`
  - 实时语音或实时多模态优先考虑 `Live API`
- 火山方舟
  - 优先考虑其原生 `Responses API`
  - OpenAI SDK / Chat 兼容层主要用于迁移、简单场景或快速接入

### 5. 最后补例外条件

必须说明什么时候统一 `chat-like` 抽象仍然合理：

- 纯文本
- 单轮或弱状态对话
- 没有复杂工具调用
- 不依赖厂商内置工具
- 不要求严格结构化输出
- 不要求实时语音/视频
- 当前目标是快速验证而非长期沉淀

## 推荐输出格式

默认用下面的结构回答：

1. `结论`
2. `推荐架构`
3. `为什么不要统一原始请求体`
4. `什么时候统一 chat-like 抽象仍然合理`
5. `针对 OpenAI / Gemini / 方舟的建议`
6. `下一步实施建议`

若用户要求更技术化，再补：

- `Canonical IR` 建议字段
- `Provider Adapter` 映射策略
- `Capability Registry` 维度
- MVP 到生产级的演进路径

## 关键术语

回答时优先使用这些统一术语：

- `Canonical IR`
- `Provider Adapter`
- `Capability Registry`
- `Fallback Transport`
- `Native API Path`

术语说明要简洁，不要堆概念。

## 设计原则

### 原则 1：统一语义，不统一底层 JSON

如果上层问的是：

- “生成一段回复”
- “调用一个工具”
- “返回符合 schema 的结构化 JSON”
- “附带图片/文件/视频输入”

这些都适合放进 `Canonical IR`。

不要要求所有 provider 共享同一份原始请求体，因为这会把设计压缩到最低公共子集，丢掉各家原生能力。

### 原则 2：把能力当成显式元数据管理

`Capability Registry` 至少应覆盖：

- 是否支持结构化输出
- 是否支持并行工具调用
- 是否支持厂商内置工具
- 是否支持会话状态托管
- 是否支持多模态输入
- 是否支持实时流
- 主要成本 / 延迟 / 区域限制

不要只做简单的 `model router`，除非系统非常简单。复杂 agent 应用需要按“能力是否满足任务”路由，而不是只按“哪个模型便宜”路由。

### 原则 3：把兼容层看作 fallback，不是最终形态

兼容 `chat` 风格接口的价值在于：

- 迁移旧系统
- 快速接入
- 保持最小通用面

但不要默认把它当作长期主路径，尤其在 agent 场景下。

### 原则 4：默认先做分层，再决定是否收敛

如果用户还没开始实现，优先建议先定义：

- 统一输入输出语义
- provider adapter 接口
- capability registry
- fallback 策略

然后再决定第一版是否只实现简化路径。

## 常见回答套路

### 用户问“是不是应该统一成一个 chat API”

优先回答：

- 对 MVP：可以，前提是需求简单
- 对生产级 agent：不建议
- 更好的做法是“统一语义层 + 多 adapter”

### 用户问“是不是每家都要写两套代码”

优先回答：

- 不是写两套业务系统
- 是同一套业务层，对外加多个 `Provider Adapter`
- 复杂度应该集中在接入层，而不是扩散到业务层

### 用户问“什么时候才值得做 provider-native”

优先回答：

- 需要工具调用
- 需要严格 schema 输出
- 需要多模态
- 需要实时交互
- 需要厂商内置工具
- 需要状态管理或复杂 agent 循环

出现这些需求之一，就要认真考虑 `Native API Path`。

## 反模式

避免给出下面这些建议：

- “所有厂商统一一份请求体是最佳实践”
- “只做 model router 就够了”
- “先把底层 JSON 统一，再谈能力差异”
- “只要兼容 OpenAI SDK，就等于长期跨 provider 方案已经完成”
- “多 provider 架构等于为每家都重写业务逻辑”

## 如何引用官方文档

当需要确认最新事实时，只使用官方来源：

- OpenAI：官方 OpenAI 开发者文档
- Gemini：Google AI for Developers 或 Google 官方 Gemini 文档
- 火山方舟：火山引擎 / 火山方舟官方文档

回答时：

- 明确哪些是“我查到的当前官方事实”
- 明确哪些是“基于这些事实给出的工程建议”
- 如果不同文档或不同接口存在取舍，解释为什么推荐其中一种

## 参考文件

- `references/provider-api-decision-matrix.md`
  - 用于 provider 对比和能力矩阵
- `references/architecture-template.md`
  - 用于输出标准结构
- `references/trigger-examples.md`
  - 用于理解触发边界和相邻场景

## 参考技能的吸收边界

可以借鉴这些思路：

- `llm-app-patterns`
  - 模式化讲解和决策矩阵
- `agent-native-architecture`
  - agent-native 架构边界、能力映射、分层思维
- `cost-aware-llm-pipeline`
  - 路由、成本意识、fallback 设计

但不要把这个技能写成：

- 泛泛的 LLM 应用大全
- 某个 CLI 的封装文档
- 单一 provider 的最佳实践手册

## 最后规则

- 回答优先简洁，先结论，后展开
- 不要在简单问题上直接展开成长篇框架设计
- 如果用户已经有现有系统，优先给迁移路径而不是重画全图
- 如果用户在问“最新”或“官方推荐”，先查，再答
