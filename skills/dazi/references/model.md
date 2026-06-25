# Life State 数据模型

四层对象定义与字段规范，独立于具体存储后端。核心责任、权责边界与共享状态契约见 [SKILL.md](../SKILL.md)。

## 通用规则

- 每个对象含标题与正文，排版与内容组织自由。
- 字段名与枚举值使用下文标识符原样存储。
- 所有字段可选：缺失取默认值，无默认则未知；只保存已知的非默认值，回归默认时删除该字段。
- 上下级关系由引用方通过 `parent` 单向保存，可暂缺；原始对象只存一手状态，正文可自由使用双链表达其他关联。
- 各对象状态独立：`Task` 完成不自动等于上级 `Initiative` 或 `Experiment` 完成。
- 字段类型：枚举（`a | b`）、文本、日期（`YYYY-MM-DD`）、关系（`→ 目标对象`）。

---

## Layer 1：目的与人

### `Compass`

核心价值、长期愿景与底线（Tier 2 变更）。

- `status`: `inactive`（缺省 = 有效）

### `Self`（`self.md`）

关于“这个人是谁”的长期自我画像，作为判断与协作的持续基准。

日常只读，仅在人类当面下达新硬约定时于末尾追加，仅在人类在场整理时统一修剪；条目注明日期并区分事实、人类所说与 Agent 推断。

---

## Layer 2：策略与推进

### `Strategy`（`strategy.md`）

当前的全局策略与重心取舍（Tier 2 变更），随现实反馈与判断变化随时更新。

### `Decision`

改变长期方向、依赖关键前提、值得日后复核的关键取舍。

- `status`: `validated | invalidated | superseded`（缺省 = `active`）
- `revisit_on`：日期，计划复核日
- `parent` → `Compass` 或 `Initiative`（可选）

### `Experiment`

面向未知路径的小步现实验证。

- `status`: `proposed | validated | invalidated`（缺省 = `running`）
- `revisit_on`：日期，观测到期日或判定日
- `anchor`：文本或列表，外部载体锚点
- `parent` → `Compass`、`Initiative` 或 `Decision`（可选）

### `Initiative`

已确认值得投入或必须交付、有明确阶段终点的正式项目；正文保留人生与项目层视角，具体工程或创作细节留在 `anchor` 载体中。

- `status`: `on_hold | completed | cancelled`（缺省 = 活跃）
- `anchor`：文本或列表，外部载体锚点
- `parent` → `Compass` 或上级 `Initiative`（可选）

### `Task`

可直接执行的原子行动或等待人类介入的事项，通过 `owner` 和 `status` 区分归属与进展。

- `owner`: `human`（缺省 = `agent`），客观必须人类介入时标 `human`
- `status`: `waiting | done | cancelled`（缺省 = `ready`）
- `revisit_on`：日期，外部等待节点、截止提醒或复核日
- `parent` → `Experiment`、`Initiative` 或 `Decision`（可选）

`agent + ready` 当场自主推进，`human + ready` 附完整说明与材料等待（见 [SKILL.md](../SKILL.md)）。

---

## Layer 3：世界与资源

### `Person`

人物或机构的关系上下文。

### `Knowledge`

外部资料与历次实践沉淀的经验规律。

### `Capabilities`（`capabilities.md`）

当前可调动的外部能力与现实资源清单（日常只读，Tier 2 变更）。

---

## Layer 4：轨迹

### `Daily`（`YYYY-MM-DD`）

按日期记录的共享日志（有实质变化才写入），正文可双链任意对象；只追加有回看价值或影响后续判断的记录，清楚区分事实、感受、决定与推断。
