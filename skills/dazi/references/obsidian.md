# Life State 的 Obsidian 存储

字段名、取值与语义见 [model.md](model.md)；字段名和取值原样作为 Property 键和值。

## 对象与文件

每个对象对应一个 Markdown 文件：文件名作标题，正文作对象正文，字段写入 frontmatter。类型由目录决定，不写 `name` 或类型属性。若接入已有 Vault 且目录名不同，可在根目录 `config.yaml` 的 `paths` 中声明映射；默认布局如下：

| 对象 | 默认路径 |
| --- | --- |
| 个人画像（Self） | `self.md` |
| 全局策略（Strategy） | `strategy.md` |
| 能力与资源（Capabilities） | `capabilities.md` |
| Compass | `compass/` |
| Decision | `decisions/` |
| Experiment | `experiments/` |
| Initiative | `initiatives/` |
| Task | `tasks/` |
| Person | `people/` |
| Knowledge | `knowledge/` |
| Daily | `daily/` |

## 字段类型映射

| 字段类型 | Property 类型 |
| --- | --- |
| 枚举、文本 | Text |
| 日期 | Date（`YYYY-MM-DD`） |
| 关系 | List，元素为 `"[[标题]]"`（单值也写成列表） |

## 示例（仅示意 frontmatter 与信息粒度）

**Daily（`YYYY-MM-DD.md`）**：纯正文追加，自然表达以区分事实、感受、决定与推断：

```markdown
昨晚没睡好，上午精力偏低、下午恢复。[[示例实验]] 观测期满，现实反馈低于事前标准，已标为证伪并将经验记入 [[相关经验]]；人类反馈对该方向本身也缺少热情。已整理备选思路，留待下次对话确认是否调整 [[strategy]]。
```

**Experiment（`experiments/示例实验.md`）**：

```markdown
---
revisit_on: 2026-12-03
parent:
  - "[[某项愿景]]"
---
核心假设：在垂直社区直接发布可交互小样，比长文介绍更容易筛出真实付费用户。

评判标准：7 天内获得至少 20 次有效试用并收到 3 条以上明确付费意向视为成立；低于此标准则证伪并转向。

验证做法：搭建最小可运行演示页，附带简洁反馈入口，持续记录访问与回复数据。
```

**Task（`owner: human`）**：

```markdown
---
owner: human
revisit_on: 2026-11-28
parent:
  - "[[示例项目]]"
---
涉及公开定价与对外承诺（须于 11-28 上线前确认），属 Tier 2 人类确认。竞品对比已整理在 [[竞品定价整理]]，落地页文案草稿在 `docs/pricing-draft.md`。

推荐直接采用专业版 199/月：验证核心付费意愿最直接，初期支持负担最低；备选是开放免费版加专业版 99/月，拉新更快但会明显分散精力。确认后我即推进上线准备。
```

## 写入约定（Obsidian CLI）

`obsidian` CLI 由运行中的 Obsidian 主进程串行执行，避免对共享文件整篇覆写：
- **改属性**：用 `obsidian property:set` / `property:remove` 做字段级更新；
- **追加正文**：写 `Daily`、对象进展与 `self.md` 新硬约定时用 `obsidian append` 追加（若目标文件已有明确小节可按需配合 `heading="..."`），不改写旧行；
- **建文件**：用 `obsidian create` 且**不带 `overwrite`**；外部导入条目用稳定标题防重。

## 注意事项（Gotchas）

- **退出码与 Vault 参数**：`obsidian` 命令报错时退出码仍可能为 `0`，必须检查 stdout 确认成功；指定多 Vault 时 `vault="<名称>"` 必须放在命令最前。
- **转义、大文件与双链**：`content=` 会转义 `\n` 与 `\t`，含反斜杠或超过 4KB 的内容改用 `obsidian eval`；移动或重命名一律用 `obsidian move` / `rename` 以维护双链。
