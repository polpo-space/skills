# 智能体技能工具集

这是一组面向真实工程开发与日常工作流的智能体技能，适用于 Codex、Claude Code，以及其他兼容 Agent Skills 标准的工具。

这些技能强调小而清晰、可组合、可修改。你可以按需安装单个技能，也可以安装整套技能，并根据自己的项目流程继续调整。

## 通过 skills.sh 安装

本仓库是公开仓库，默认分支为 `main`，并保留标准的 `skills/**/SKILL.md` 目录结构，可以直接作为 `npx skills` 的安装源。

### 查看可安装的技能

```bash
npx skills@latest add polpo-space/skills --list
```

### 交互式安装

```bash
npx skills@latest add polpo-space/skills
```

安装过程中可以选择具体技能、目标智能体，以及安装到当前项目或全局环境。

### 安装到 Codex

安装指定技能，例如 `to-spec`：

```bash
npx skills@latest add polpo-space/skills \
  --skill to-spec \
  --agent codex \
  --global
```

无交互安装：

```bash
npx skills@latest add polpo-space/skills \
  --skill to-spec \
  --agent codex \
  --global \
  --yes
```

安装所有技能：

```bash
npx skills@latest add polpo-space/skills \
  --skill '*' \
  --agent codex \
  --global
```

也可以直接指定仓库中的某个技能目录：

```bash
npx skills@latest add \
  https://github.com/polpo-space/skills/tree/main/skills/engineering/to-spec
```

## 安装为 Claude Code 插件

仓库同时保留了 Claude Code 插件结构。使用插件安装时，技能会作为受管理的只读插件加载，适合不准备直接修改技能文件的场景。

在 Claude Code 中执行：

```text
/plugin marketplace add polpo-space/skills
/plugin install mattpocock-skills@mattpocock
```

也可以在终端中执行：

```bash
claude plugin marketplace add polpo-space/skills
claude plugin install mattpocock-skills@mattpocock
```

插件名称目前仍沿用上游仓库的 `mattpocock-skills`。如果需要直接修改技能内容，建议使用 `skills.sh` 安装方式。

## 设计目标

这些技能主要用于解决智能体协作中的几类常见问题：

### 需求理解不一致

在开始实现前，通过结构化提问明确目标、边界、异常场景和关键决策，避免智能体过早进入编码阶段。

相关技能：

- [`grill-me`](./skills/productivity/grill-me/SKILL.md)
- [`grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md)

### 项目术语不统一

通过领域建模、上下文文档和架构决策记录，让开发者与智能体使用同一套术语，减少重复解释和含糊表达。

相关技能：

- [`domain-modeling`](./skills/engineering/domain-modeling/SKILL.md)
- [`grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md)

### 缺少可靠反馈

通过测试驱动开发、系统化诊断和代码审查，为智能体提供持续反馈，降低“看起来正确但无法运行”的概率。

相关技能：

- [`tdd`](./skills/engineering/tdd/SKILL.md)
- [`diagnosing-bugs`](./skills/engineering/diagnosing-bugs/SKILL.md)
- [`code-review`](./skills/engineering/code-review/SKILL.md)

### 代码结构持续恶化

通过模块边界、深模块设计和架构检查，减少智能体快速生成代码带来的复杂度累积。

相关技能：

- [`codebase-design`](./skills/engineering/codebase-design/SKILL.md)
- [`improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md)
- [`to-spec`](./skills/engineering/to-spec/SKILL.md)

## 技能索引

技能按调用方式分为两类：

- **用户调用**：需要用户明确输入技能名称，主要负责组织和驱动工作流。
- **模型调用**：用户可以直接调用，智能体也可以在任务匹配时自动使用。

### 工程开发

#### 用户调用

- **[ask-matt](./skills/engineering/ask-matt/SKILL.md)**：根据当前问题推荐合适的技能或工作流。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)**：通过深入提问明确需求，同时完善项目领域模型、`CONTEXT.md` 和架构决策记录。
- **[triage](./skills/engineering/triage/SKILL.md)**：按照预定义状态和角色对问题进行分类与推进。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)**：扫描代码库中的架构改进机会，生成可视化报告，并进一步分析选中的问题。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)**：为当前项目配置问题跟踪器、分类标签和领域文档目录。
- **[to-spec](./skills/engineering/to-spec/SKILL.md)**：把当前对话整理成规格说明，并发布到问题跟踪器。
- **[to-tickets](./skills/engineering/to-tickets/SKILL.md)**：把计划、规格说明或对话拆分成带依赖关系的可执行任务。
- **[implement](./skills/engineering/implement/SKILL.md)**：根据规格说明或任务集合完成实现，并结合测试驱动开发和代码审查进行收尾。
- **[wayfinder](./skills/engineering/wayfinder/SKILL.md)**：把超出单次会话容量的大型工作拆分成一组调查任务，逐步明确实现路径。

#### 模型调用

- **[prototype](./skills/engineering/prototype/SKILL.md)**：构建可丢弃原型，用于验证状态、逻辑或界面设计问题。
- **[diagnosing-bugs](./skills/engineering/diagnosing-bugs/SKILL.md)**：按照复现、缩小范围、提出假设、增加观测、修复和回归测试的流程诊断问题。
- **[research](./skills/engineering/research/SKILL.md)**：基于高可信的一手资料进行研究，并把带引用的结论保存为项目文档。
- **[tdd](./skills/engineering/tdd/SKILL.md)**：按照红灯、绿灯、重构的循环完成测试驱动开发。
- **[domain-modeling](./skills/engineering/domain-modeling/SKILL.md)**：持续完善项目的领域术语、边界、场景和架构决策记录。
- **[codebase-design](./skills/engineering/codebase-design/SKILL.md)**：使用深模块、清晰边界和可测试接口改进代码设计。
- **[code-review](./skills/engineering/code-review/SKILL.md)**：分别从代码规范和规格符合度两个维度审查变更。
- **[resolving-merge-conflicts](./skills/engineering/resolving-merge-conflicts/SKILL.md)**：根据双方修改意图逐个解决合并或变基冲突，并完成当前操作。

### 效率工具

#### 用户调用

- **[grill-me](./skills/productivity/grill-me/SKILL.md)**：围绕计划、设计或想法持续提问，直到关键分支和决策得到明确。
- **[handoff](./skills/productivity/handoff/SKILL.md)**：把当前对话压缩成可供另一个智能体继续工作的交接文档。
- **[teach](./skills/productivity/teach/SKILL.md)**：通过多次会话教授一个新技能或概念，并在当前目录保存学习状态。
- **[writing-great-skills](./skills/productivity/writing-great-skills/SKILL.md)**：提供编写和维护高质量技能的原则、术语和检查方法。

#### 模型调用

- **[grilling](./skills/productivity/grilling/SKILL.md)**：围绕计划、决策或想法执行可复用的深度提问流程。
