# Dazi

一套基于 Agent Skills 规范的个人人生状态与协作技能集。项目以本地纯文本映射人生状态，以**人类持有愿景与主权、超级智能主导实现路径**为视角，让 Agent 在清晰的权责边界内持续验证路径、推动愿景落地并安静维护日常秩序。

关于项目为什么采取这一形态，见核心理念文档 [PHILOSOPHY.md](PHILOSOPHY.md)。

## 设计概览

整套技能由 1 个自动加载的核心技能与 3 个显式场景入口组成：

- 核心技能（`dazi`）：定义核心责任、三档权责边界、四层人生状态模型与共享状态契约，涉及个人人生状态与方向协作时自动加载。
- 显式入口（`pulse`、`ingest`、`onboard`）：分别承接后台自动化唤醒、随手输入与外部材料隔离导入、以及首次建库与迁库引导。

## Skills 一览

| Skill | 触发方式 | 职责 | 文档 |
| --- | --- | --- | --- |
| `dazi` | 自动触发 | 核心责任、三档权责、四层人生状态模型与共享状态契约 | [SKILL.md](skills/dazi/SKILL.md) |
| `pulse` | 显式指定 | 自动化唤醒入口，人类不在场时自主判断最值得推进的事项或安静退出 | [SKILL.md](skills/pulse/SKILL.md) |
| `ingest` | 显式指定 | 随手输入与外部材料导入入口，隔离整理碎片、账单、推送并去重归类 | [SKILL.md](skills/ingest/SKILL.md) |
| `onboard` | 显式指定 | 首次引导入口，通过自然访谈建库、迁库、盘点资源并配置自动化唤醒 | [SKILL.md](skills/onboard/SKILL.md) |

除核心 `dazi` 外，其余 3 个入口均设置了 `disable-model-invocation: true`，仅在人类或外部调度显式指定时运行。

## 人生状态与权责

四层本地纯文本模型（完整定义与字段见 [model.md](skills/dazi/references/model.md)）：

- L1 目的与人：`compass/`、`self.md`
- L2 策略与推进：`strategy.md`、`decisions/`、`experiments/`、`initiatives/`、`tasks/`
- L3 世界与资源：`people/`、`knowledge/`、`capabilities.md`
- L4 轨迹：`daily/`（按日期记录的共享日志）

日常运转遵循三项关键约定：

1. 共享状态契约：日常改属性用字段级操作，写日志和进展用追加写入，不整篇覆写共享文件；原始对象只存一手状态；`self.md` 仅在人类在场整理时统一修剪。
2. 三档权责（详见 [skills/dazi/SKILL.md](skills/dazi/SKILL.md)）：Tier 0 静默自治，涵盖只读调研、筛选与起草；Tier 1 先行后报，涵盖可逆推进、验证与状态维护；Tier 2 人类确认，涵盖不可撤回或高影响操作（如花钱、公开发布、联系他人、修改 `Compass`/`Strategy` 或运行环境配置），须立为 `owner: human, status: ready` 并附完整说明与建议等待。
3. 开放纯文本与参考后端（详见 [obsidian.md](skills/dazi/references/obsidian.md)）：所有对象均采用标准 Markdown 与 YAML frontmatter。项目内置 Obsidian（`obsidian` CLI）作为参考存储实现，也可按 `model.md` 适配其他本地文本环境。

## 安装与使用

### 前提条件

- 支持 Agent Skills 规范（`SKILL.md` 及 `disable-model-invocation` 字段）的运行环境
- 若使用内置的 Obsidian 参考后端，需在本地运行 Obsidian 并可使用 `obsidian` CLI

### 安装

通过 Skills CLI 安装：

```bash
npx skills add liqiha0/dazi
```

也可以直接克隆本仓库，将 `skills/` 下的目录链接或复制到所用 Agent 的 skills 目录中。

### 基本用法

1. 显式调用 `onboard`，通过访谈建立初始目录、核心文件与自动化唤醒配置（或梳理迁入既有笔记）。
2. 日常对话与协作中涉及人生状态、方向或实验时，Agent 自动加载 `dazi`；随手丢入外部资料或推送数据时显式调用 `ingest`。
3. 结合所用运行环境的定时或事件调度，按需运行 `pulse` 在后台持续推进。

## 仓库结构

```text
.
├── LICENSE
├── PHILOSOPHY.md
├── README.md
└── skills/
    ├── dazi/
    │   ├── SKILL.md
    │   └── references/
    │       ├── model.md
    │       └── obsidian.md
    ├── ingest/
    │   └── SKILL.md
    ├── onboard/
    │   └── SKILL.md
    └── pulse/
        └── SKILL.md
```

## 许可

基于 [MIT License](LICENSE) 开源。
