# AIAgentShare 项目规范

本文件是 AI 工具与智能体在本项目内创作内容时必须遵循的 operating manual。任何 AI 工具进入本项目时应先读本文件。

---

## 1. 项目定位

AIAgentShare 是用于**创造和存储 Agent 相关内容**的统一仓库。涵盖四类产物：

| 类型 | 说明 | 存放目录 |
|---|---|---|
| Agent 源码/配置 | 可运行的 Agent 产物：系统提示词、工具配置、MCP 配置、入口脚本 | `agents/` |
| 研究报告/文档 | 研究报告、竞品分析、技术方案、学习笔记等文档 | `reports/` |
| Prompt/模板库 | 可复用的提示词、工作流模板 | `prompts/` |
| 多媒体内容 | 图片、视频、音频等媒体产物 | `media/` |

此外，`sources/` 存放**用户提供的源文件**——作为项目创作的素材输入，可能包含书籍 PDF、文档、数据文件、截图等各类原始材料；`beautiful-html-templates/` 是独立的 HTML 模板库子项目（有自己的 AGENTS.md，优先遵循其自身规范）。

---

## 2. 目录结构规范

```
AIAgentShare/
├── AGENTS.md                      # 本规范文件（AI 自动读取）
├── README.md                      # 项目简介
├── sources/                       # 用户提供的源文件（书籍、文档、数据等各类原始素材）
├── agents/                        # Agent 源码与配置
│   └── <agent-name>/              # 每个 Agent 一个目录
│       ├── agent.yml              # 元数据（名称、描述、依赖、入口）
│       ├── system-prompt.md       # 系统提示词
│       ├── tools/                 # 工具配置与脚本
│       └── README.md              # 使用说明
├── prompts/                       # Prompt / 提示词模板库
│   └── <category>/                # 按类别分组
│       └── <prompt-name>.md
├── reports/                       # 研究报告 / 文档产物
│   └── <topic>/                   # 按主题分组
│       └── <report-name>.html
├── media/                         # 多媒体内容
│   ├── images/
│   ├── videos/
│   └── audio/
└── beautiful-html-templates/      # HTML 模板库（独立子项目，遵循自身 AGENTS.md）
```

### 产物该放哪个目录——决策规则

生成任何产物前，按以下顺序判断：

1. **是可运行的 Agent 吗？**（有系统提示词 + 工具/配置 + 可被调用）→ `agents/<agent-name>/`
2. **是文档/报告类产物吗？**（研究、分析、方案、笔记，主要被阅读）→ `reports/<topic>/`
3. **是可复用的提示词/模板吗？**（被其他 Agent 或人反复引用）→ `prompts/<category>/`
4. **是图片/视频/音频吗？** → `media/<对应子目录>/`
5. **是用户提供的源文件/原始素材吗？**（非本项目创作，作为输入被引用：书籍、文档、数据、截图等）→ `sources/`
6. **都不匹配？** → 放 `reports/` 并在文件名中说明类型，或先问用户。

**禁止**在项目根目录直接放散落的产物文件。根目录只允许 AGENTS.md、README.md 和上述规范目录。

---

## 3. 命名规范

### 目录名
- Agent 目录：`<agent-name>`，用英文小写 + 连字符（kebab-case），如 `code-reviewer`、`data-analyst`。
- 主题/类别目录：同理用 kebab-case，如 `competitor-analysis`、`writing`。

### 文件名
- **默认用英文 kebab-case**：`system-prompt.md`、`agent.yml`、`q3-revenue-report.html`。
- 需要中文标识时，采用 `<中文名>_<缩写>_<日期>` 格式，日期用 `YYYYMMDD`，如 `Agent学习路径_QW_20260719.html`（与现有 `books/` 命名一致）。
- 日期缀是可选的，用于版本化或时效性强的内容。

### 元数据文件
- 每个 Agent 目录必须有 `agent.yml`，字段见 §4。
- 文档类产物如需记录来源/时效，在文件头部用注释块说明（HTML 用 `<!-- -->`，MD 用 YAML front matter）。

---

## 4. 各类型产物的创作规范

### agents/ — Agent 源码与配置

每个 Agent 一个独立目录，至少包含：

```
agents/<agent-name>/
├── agent.yml          # 元数据（必需）
├── system-prompt.md   # 系统提示词（必需）
├── tools/             # 工具配置与脚本（按需）
└── README.md          # 使用说明（按需）
```

`agent.yml` 最小结构：

```yaml
name: <agent-name>           # 与目录名一致
description: <一句话说明用途>
version: 1.0.0
entry: <入口文件或调用方式>   # 如 system-prompt.md / main.py
tools:                        # 依赖的工具/MCP
  - <tool-name>
created: 2026-08-02
```

### reports/ — 研究报告与文档

- 按主题建子目录，如 `reports/competitor-analysis/`、`reports/tech-proposal/`。
- 默认输出 **HTML** 格式（自包含、可在浏览器直接打开），除非用户明确要求其他格式。
- 文档优先自包含：CSS/JS 内联或同目录放置，确保单文件可打开。
- 涉及外部数据的，在文件头部标注数据来源与时效。

### prompts/ — Prompt 与模板库

- 按类别建子目录，如 `prompts/writing/`、`prompts/coding/`、`prompts/analysis/`。
- 每个 Prompt 一个 `.md` 文件，文件头部用 YAML front matter 标注元数据：

```markdown
---
name: <prompt-name>
category: <类别>
description: <一句话说明>
variables: [<变量列表>]       # 模板中需替换的占位符
---
```

### media/ — 多媒体内容

- 按媒体类型放入 `images/`、`videos/`、`audio/`。
- 文件名用英文 kebab-case，可带日期：`<name>_<YYYYMMDD>.<ext>`。
- 大文件（>50MB）考虑是否真的需要纳入仓库，避免仓库膨胀。

### sources/ — 用户提供的源文件

- 存放用户上传或提供的**原始素材**，作为项目创作的输入被引用，类型不限：书籍 PDF、文档、数据文件、截图、配置示例等。
- 文件可保留原始文件名；若原名不清或易冲突，按 `<中文名>_<缩写>_<YYYYMMDD>.<ext>` 格式重命名（与现有命名一致）。
- 源文件较多时，可按来源或主题建子目录归类，如 `sources/<source>/`。
- 本目录文件为**只读**，AI 工具不得修改、覆盖或删除；如需加工，产物输出到对应目录（如 `reports/`、`media/`），源文件保持原样。

---

## 5. 工作流程规范

任何 AI 工具在本项目内创作内容时，遵循以下流程：

### Step 1 — 判断产物类型与目标目录
根据 §2 的决策规则，确定产物属于哪一类、应放到哪个目录。不确定时先问用户。

### Step 2 — 确认命名
按 §3 命名规范确定文件/目录名。新建 Agent 目录前检查是否已有同名 Agent。

### Step 3 — 创建产物
按 §4 对应类型的规范创建文件。确保：
- 必需的元数据文件齐全（如 `agent.yml`）。
- 文档类产物自包含、可独立打开。
- 命名、目录位置符合规范。

### Step 4 — 交付与路径反馈
- 在浏览器中打开 HTML 类产物（`open <path>`）。
- 向用户返回产物的**绝对路径**，单独成行，便于点击打开。
- 简述做了什么、有何注意事项，不要逐步复述操作过程。

---

## 6. 通用约束

- **不要在根目录散落文件。** 所有产物进对应目录。
- **不要修改 `sources/` 中的源文件。** 那是用户提供的只读原始素材，作为创作输入被引用，不应被改动或覆盖。
- **`beautiful-html-templates/` 是独立子项目。** 在其中工作时遵循其自身的 AGENTS.md，不要用本文件覆盖其规则。
- **产物超出门槛时主动确认。** 大文件、批量生成、覆盖已有产物等操作前先问用户。
- **保持目录整洁。** 临时文件、预览文件用完即删，或集中放到对应目录的 `_drafts/` 子目录。
- **记录来源与时效。** 引用外部数据或信息的产物，标注数据来源和获取时间，不编造。
