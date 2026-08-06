# 智能体技能工具集

[![skills.sh](https://skills.sh/b/polpo-space/skills)](https://skills.sh/polpo-space/skills)

这是一组面向真实工程开发与日常工作流的智能体技能，适用于 Codex、Claude Code，以及其他兼容 Agent Skills 标准的工具。

这些技能强调小而清晰、可组合、可修改。你可以按需安装单个技能，也可以安装整套技能，并根据自己的项目流程继续调整。

## 安装

`skills.sh` 会把可编辑的技能文件复制到项目中；Claude Code 插件会把整套技能作为受管理的只读插件加载。选择一种方式即可，同时安装会产生重复技能。

### Codex 和其他智能体

查看可安装的技能：

```bash
npx skills@latest add polpo-space/skills --list
```

交互式选择技能和目标智能体：

```bash
npx skills@latest add polpo-space/skills
```

无交互地全局安装指定技能，例如 `to-spec`：

```bash
npx skills@latest add polpo-space/skills \
  --skill to-spec \
  --agent codex \
  --global \
  --yes
```

### Claude Code 插件

先添加本仓库的 marketplace，再安装其中的插件：

```bash
claude plugin marketplace add polpo-space/skills
claude plugin install mattpocock-skills@mattpocock
```

也可以在 Claude Code 会话中运行：

```text
/plugin marketplace add polpo-space/skills
/plugin install mattpocock-skills@mattpocock
```

插件名称和 marketplace 名称沿用上游的 `mattpocock-skills@mattpocock`。

### 首次配置

安装后，在每个仓库中运行一次 `/setup-matt-pocock-skills`，配置问题跟踪器、triage 标签和文档目录。

## 技能索引

技能按调用方式分为两类：

- **用户调用**：只有用户明确输入技能名称时才会运行。
- **模型调用**：用户可以直接调用，模型也可以在任务匹配时自动使用。

### 工程开发

#### 用户调用

- **[ask-matt](./skills/engineering/ask-matt/SKILL.md)**：根据当前问题推荐合适的技能或工作流。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)**：通过深入提问明确需求，同时完善项目领域模型、`CONTEXT.md` 和架构决策记录。
- **[triage](./skills/engineering/triage/SKILL.md)**：按照状态机和角色分工推进问题分类。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)**：扫描代码库中的架构深化机会，生成可视化报告并继续分析选中的问题。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)**：为当前项目配置问题跟踪器、triage 标签和领域文档目录。
- **[to-spec](./skills/engineering/to-spec/SKILL.md)**：把当前对话整理成规格说明并发布到问题跟踪器。
- **[to-tickets](./skills/engineering/to-tickets/SKILL.md)**：把计划、规格说明或对话拆成带依赖关系的 tracer-bullet 任务。
- **[implement](./skills/engineering/implement/SKILL.md)**：根据规格或任务集合完成实现，并用测试驱动开发和代码审查收尾。
- **[wayfinder](./skills/engineering/wayfinder/SKILL.md)**：把超出单次会话容量的大型工作整理成可逐步解决的决策任务地图。

#### 模型调用

- **[prototype](./skills/engineering/prototype/SKILL.md)**：构建可丢弃原型，用一个可运行程序或多套界面方案回答设计问题。
- **[pzero-skills](./skills/engineering/pzero-skills/SKILL.md)**：应用 pzero 的 REST、RPC、数据库、迁移和代码生成约定。
- **[diagnosing-bugs](./skills/engineering/diagnosing-bugs/SKILL.md)**：通过复现、缩小范围、假设、观测、修复和回归测试系统诊断问题。
- **[research](./skills/engineering/research/SKILL.md)**：基于高可信一手资料进行研究，并在仓库中留下带引用的 Markdown 结论。
- **[tdd](./skills/engineering/tdd/SKILL.md)**：按照红灯、绿灯、重构循环完成测试驱动开发。
- **[domain-modeling](./skills/engineering/domain-modeling/SKILL.md)**：持续完善项目的领域术语、边界、场景和架构决策记录。
- **[codebase-design](./skills/engineering/codebase-design/SKILL.md)**：使用深模块、清晰边界和可测试接口改进代码设计。
- **[code-review](./skills/engineering/code-review/SKILL.md)**：分别从代码规范和规格符合度两个维度审查变更。
- **[resolving-merge-conflicts](./skills/engineering/resolving-merge-conflicts/SKILL.md)**：根据双方修改意图逐个解决合并或变基冲突，并完成当前操作。
- **[wizard](./skills/engineering/wizard/SKILL.md)**：为必须由人执行的基础设施、凭据、控制台和迁移步骤生成交互式 Bash 向导。

### 效率工具

#### 用户调用

- **[grill-me](./skills/productivity/grill-me/SKILL.md)**：围绕计划、设计或想法持续提问，直到关键分支和决策明确。
- **[handoff](./skills/productivity/handoff/SKILL.md)**：把当前对话压缩成可供另一个智能体继续工作的交接文档。
- **[i-have-adhd](./skills/productivity/i-have-adhd/SKILL.md)**：把回复改成 ADHD 友好输出，先给下一步动作，编号步骤并持续重申状态。
- **[teach](./skills/productivity/teach/SKILL.md)**：通过多次会话教授新技能或概念，并在当前目录保存学习状态。
- **[to-questionnaire](./skills/productivity/to-questionnaire/SKILL.md)**：把需要他人回答的决策整理成可异步填写或在会议中共同完成的 Markdown 问卷。
- **[wait-what](./skills/productivity/wait-what/SKILL.md)**：当一段说明没有讲清楚时，用缺失的上下文和项目词汇重新解释。

#### 模型调用

- **[grilling](./skills/productivity/grilling/SKILL.md)**：围绕计划、决策或想法执行可复用的深度提问流程。
- **[writing-for-agents](./skills/productivity/writing-for-agents/SKILL.md)**：编写供智能体读取的技能、`AGENTS.md`、`CLAUDE.md` 和被引用文档。
