# Trigger Examples

这个文件用于帮助理解技能的触发边界。

## 应该触发

这些问题应触发本技能：

- “我要同时接 OpenAI、Gemini、方舟做一个 agent，应该统一 chat API 吗？”
- “我们现在只有 OpenAI Chat Completions，后面要接 Gemini 和火山方舟，接口层该怎么拆？”
- “多模型 agent 里 provider adapter 和 capability registry 分别管什么？”
- “Gemini 用 generateContent，OpenAI 用 Responses，这种混搭会不会让系统太复杂？”
- “ARK 的 OpenAI 兼容层适合长期用吗，还是应该直接走它自己的 Responses API？”
- “我们想做多厂商多模态 agent，要怎么做统一抽象才不丢能力？”

## 不应该触发

这些问题通常不该由本技能主导：

- “帮我修一个 Next.js 页面样式”
- “OpenAI 这个字段具体怎么传，给我最新文档链接”
  - 这类更适合通用文档查询或 OpenAI 专用文档技能
- “Gemini 价格是多少”
  - 这是最新信息查询，不是架构决策
- “帮我写一个单独的 OpenAI 请求示例”
  - 如果没有多 provider 或架构语义，这个技能不是首选

## 边界触发

下面这些场景可以触发，但回答要收敛：

- “要不要做 model router”
  - 如果讨论的是单 provider 内部模型切换，简短回答即可
- “要不要统一 SDK 层”
  - 如果本质是多 provider 适配，可以触发
- “我们想支持函数调用和结构化输出”
  - 如果涉及多 provider 差异，应触发

## 触发后的第一反应

如果确定触发，优先先回答一句：

- “不建议把所有厂商压成同一个原始 chat 请求体；更推荐统一语义层，再做 provider adapter。”

然后再根据用户上下文决定是否展开。
