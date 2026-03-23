# Architecture Response Template

当这个技能被触发时，默认优先按以下结构输出。先短后长，先决策后细节。

## 标准回答模板

### 结论

- 直接回答用户问题
- 明确是否建议统一 `chat API`
- 明确当前更适合 MVP 方案还是生产级方案

### 推荐架构

默认推荐：

`Agent Core -> Canonical IR -> Provider Adapters -> Capability Registry`

并用一句话解释每层职责。

### 为什么不要统一原始请求体

至少覆盖以下 3 点：

- 原生能力差异会被最低公共子集吞掉
- agent 场景依赖工具、结构化输出、会话状态、多模态和流式事件
- 长期维护成本应集中在 adapter，而不是扩散到业务层

### 什么时候统一 chat-like 抽象仍然合理

列出例外条件：

- 纯文本
- 弱状态
- 简单工具调用
- 不用厂商内置工具
- 快速验证优先

### 针对 OpenAI / Gemini / 方舟的建议

回答时按以下顺序：

1. 当前推荐接口路径
2. 什么时候退回兼容路径
3. 哪类能力必须走原生路径

### 下一步实施建议

根据用户上下文给一个最小可执行方案，例如：

- 第一步先定义 `Canonical IR`
- 第二步只实现 OpenAI + Gemini 两个 adapter
- 第三步补 `Capability Registry`
- 第四步再加 ARK 原生 adapter 和 fallback transport

## 何时展开到“实施蓝图”

只有在用户明确想落地或已有代码系统时，再展开：

- IR 字段建议
- adapter 接口签名
- capability registry 字段
- 路由策略
- fallback 规则
- 测试面

## 何时必须先查文档

出现这些词时先查官方文档，再用本模板组织答案：

- 最新
- 当前
- 官方推荐
- 现在是否支持
- 字段差异
- 是否弃用
- 哪个接口更推荐

## 示例收束语

- “如果你要，我可以继续把这个结论展开成 provider adapter 接口设计。”
- “如果你已经有 OpenAI Chat 的旧实现，我可以直接按迁移路径给你拆下一步。”
- “如果你更关心工程实现，我可以继续给出 `Canonical IR` 和 `Capability Registry` 的最小字段集。”
