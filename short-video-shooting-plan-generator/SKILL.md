---
name: short-video-shooting-plan-generator
description: 根据商品名称与拍摄类目生成 TikTok Shop 带货短视频分镜脚本（30-45 秒、6-8 镜头、2 个 Hook、软性 CTA、基础合规约束）。当用户提到短视频带货、TikTok Shop 口播/分镜、Hook 设计、产品演示、卖点梳理、对比镜头或转化脚本时，即使没有明确说“拍摄方案”也应优先使用本技能。
---

# Short Video Shooting Plan Generator

## 目标

为短视频带货场景生成“可直接开拍”的英文分镜脚本。  
仅要求输入 `product_name` 和 `category_raw`，输出结构化 Markdown。

## 输入与默认值

- 提取输入：
  - `product_name: string`
  - `category_raw: string`
- 提取优先级：
  - 优先读取显式字段（如 `Category: ...`、`category is ...`、`类目: ...`）。
  - 仅在没有显式字段时，从自然语言描述中推断类目。
- 固定默认：
  - 平台：TikTok Shop
  - 语言：English
  - 时长：30-45 秒
  - 镜头数：6-8
  - Hook 数量：2（主推 + 备选）
  - CTA 风格：soft-sell
  - 未知类目：兜底 `generic`

## 读取策略文件

按以下顺序读取并只加载必要文件：
1. 读取 `references/taxonomy.md`，将 `category_raw` 归一化为标准类目。
2. 根据标准类目读取对应 `references/strategy-*.md`。
3. 若无法命中任何类目，改读 `references/strategy-generic.md`。
4. 最后读取 `references/compliance.md`，执行风险表达替换与提示。

## 生成流程

1. 提取 `product_name` 与 `category_raw`。  
2. 归一化类目，生成：
   - `category_normalized`
   - `strategy_file`
3. 从策略文件提取：
   - `Category Intent`
   - `Best Hook Patterns`
   - `Demo Sequence`
   - `Common Objection & Answer`
   - `Comparison Rule`
   - `Soft CTA Patterns`
   - `Do/Don't`
4. 生成 2 个 Hook（主推 + 备选）。  
5. 生成 6-8 个分镜，保证总时长在 30-45 秒。  
6. 若 `comparison_recommended: true`，插入至少 1 个对比镜头；否则不强制。  
7. 使用软性 CTA，不使用高压成交语气。  
8. 应用合规规则：替换高风险词，保留必要风险提示。  
9. 输出固定 Markdown 结构。若走 `generic`，附加兜底说明。

## 示例（用于对齐输出预期）

**Example 1**  
Input: "Product: Niacinamide Brightening Serum; Category: skincare"  
Output: 生成英文 TikTok Shop 分镜脚本，包含 2 个 Hook、6-8 镜头表格、soft CTA、Compliance Notes，不缺六要素列。

**Example 2**  
Input: "Product: Cat Feather Teaser; Category: pet toy"  
Output: 命中 `generic` 策略并输出同样结构，同时附加 `Fallback Note`：`Used generic strategy due to unmatched category.`

## 输出结构（必须完整）

严格输出下列标题，且顺序一致：

1. `Video Brief`
2. `Hook Options (2)`
3. `Shot List (6-8 shots)`（Markdown 表格）
4. `Conversion Design (soft CTA)`
5. `Compliance Notes`
6. `Fallback Note`（仅当 `category_normalized=generic` 时输出）

### Shot List 表格列（六要素，禁止缺失）

| Shot | Duration | Visual Action | Camera Angle | Voiceover/Captions | Selling Point | Conversion Action |
|---|---|---|---|---|---|---|

## 质量约束

- 必须输出英文脚本。
- 镜头数必须在 6-8。
- 总时长必须在 30-45 秒。
- Hook 必须为 2 个。
- 禁止出现绝对化、医疗化、无法证明的承诺表达。
- 对比镜头只能按策略条件触发，不得无条件强插。
- CTA 必须是软性引导（鼓励尝试、强调适配人群与体验）。

## 输出模板（按此骨架填充）

```markdown
## Video Brief
- Product: <product_name>
- Category (raw): <category_raw>
- Category (normalized): <category_normalized>
- Platform: TikTok Shop
- Duration Target: 30-45s
- Tone: Practical, trust-first, soft conversion

## Hook Options (2)
1. Primary Hook: ...
2. Backup Hook: ...

## Shot List (6-8 shots)
| Shot | Duration | Visual Action | Camera Angle | Voiceover/Captions | Selling Point | Conversion Action |
|---|---|---|---|---|---|---|
| 1 | ... | ... | ... | ... | ... | ... |

## Conversion Design (soft CTA)
- CTA Line: ...
- Viewer Type Match: ...
- Friction Reduction: ...

## Compliance Notes
- Replaced Terms: ...
- Risk Reminders: ...

## Fallback Note
Used generic strategy due to unmatched category.
```

## 干跑检查清单

生成前后都执行以下检查：
1. 是否成功归一化类目；若失败是否切换到 `generic`。  
2. 是否存在 2 个 Hook。  
3. 是否严格为 6-8 个镜头。  
4. 每个镜头是否六要素齐全。  
5. 总时长是否在 30-45 秒。  
6. CTA 是否保持软性表达。  
7. 合规替换是否完成，并在 `Compliance Notes` 标记。  
8. 若走 `generic`，是否输出 `Fallback Note`。  
