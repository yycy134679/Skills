# Provider API Decision Matrix

本文件用于帮助比较多家 provider 的推荐接口路径和能力边界。这里的结论是稳定框架，不是永久不变的事实；涉及“当前官方推荐”时，仍应回到官方文档核实。

## 核心判断

### 优先走 provider-native API 的场景

- 复杂 agent 循环
- 工具调用很多
- 严格结构化输出
- 多模态输入
- 实时音视频
- 厂商内置工具或平台能力
- 复杂会话状态和上下文编辑

### 可以先走统一 chat-like 抽象的场景

- MVP
- 纯文本问答
- 简单工具调用
- 没有实时能力
- 不用厂商内置工具
- 想优先验证产品而不是吃满每家能力

## 推荐接口路径

| Provider | 默认首选 | 次选 / fallback | 何时用 fallback | 备注 |
| --- | --- | --- | --- | --- |
| OpenAI | `Responses API` | `Chat Completions` | 兼容旧系统、简单文本、历史代码已大量依赖 chat | 新项目默认优先看 `Responses` |
| Gemini | `generateContent` / `generateContentStream` | 自己封装的 chat-like 抽象 | 仅当应用层要先保持最小通用语义 | 实时场景另看 `Live API` |
| 火山方舟 | 原生 `Responses API` | OpenAI 兼容 `Chat` / SDK | 快速迁移、简单场景、先求兼容 | 不要把兼容层误当长期最终形态 |

## 能力矩阵应至少覆盖

| 能力 | 为什么重要 |
| --- | --- |
| 结构化输出 | 决定能否可靠返回 schema 约束数据 |
| 工具调用 | 决定 agent 是否能稳定与外部系统交互 |
| 并行工具调用 | 决定多工具 agent 的效率与实现复杂度 |
| 多模态输入 | 决定是否能统一处理图片/文件/视频 |
| 实时流 | 决定是否能支持低延迟交互或实时语音 |
| 会话状态托管 | 决定多轮对话和长任务状态如何维护 |
| 厂商内置工具 | 决定是否必须走原生接口 |
| 成本 / 延迟 / 地域 | 决定路由与 fallback 设计 |

## 推荐分层

### Agent Core

- 任务编排
- 会话 / memory
- 工具注册
- 预算 / 重试 / trace

### Canonical IR

建议至少抽象这些语义：

- system / developer / user 输入
- assistant 输出
- tool spec
- tool call
- tool result
- response schema
- attachment
- streaming event
- usage

### Provider Adapter

每家 adapter 负责：

- 把 `Canonical IR` 映射到原生请求体
- 处理原生返回结构差异
- 把原生能力暴露成统一能力标签
- 提供 provider-specific extras，而不是污染业务层

### Capability Registry

建议作为显式数据结构维护，不要靠口口相传：

- provider
- model
- supports_structured_output
- supports_parallel_tools
- supports_native_tools
- supports_realtime
- supports_multimodal
- supports_managed_state
- cost_tier
- latency_tier
- region_or_compliance_notes

## 常见决策句式

### 如果用户问“要不要统一 chat API”

回答模版：

- 如果只是 MVP 或简单文本场景，可以先统一成 `chat-like` 抽象。
- 如果目标是生产级 agent，不建议统一原始请求体。
- 更好的方案是：统一语义层，保留 provider-native 的接入层。

### 如果用户问“是否只做 model router 就够了”

回答模版：

- 对极简系统可以。
- 对多能力 agent 不够。
- 需要 `Capability Registry`，否则无法根据能力缺口做正确路由和降级。

### 如果用户问“为什么不能直接兼容 OpenAI SDK”

回答模版：

- 兼容层适合迁移和快速接入。
- 但复杂 agent 会遇到原生能力差异。
- 长期应把兼容层视为 `Fallback Transport`，不是唯一主路径。
