---
title: "feat: US Design Writer 两阶段流程增强"
type: feat
status: completed
date: 2026-04-03
origin: docs/brainstorms/2026-04-03-us-design-writer-two-phase-requirements.md
---

# feat: US Design Writer 两阶段流程增强

## Overview

将 `us-design-writer` skill 从单阶段扩展为两阶段：先生成设计文档，用户审批确认后，再基于确认后的文档生成独立任务分解文件。

## Problem Frame

当前 skill 生成设计文档后即结束，用户需要手动提取任务。任务拆分内建到 skill 中可确保设计与任务的一致性，并保证每个任务有独立的验收标准。

## Requirements Trace

- R1. 两阶段流程，第二阶段基于确认后的设计文档 — Unit 1, Unit 2
- R2. 审批节点支持多轮修改 — Unit 1
- R3. 第一阶段保持现有行为不变 — Unit 1（保持）
- R4. 用户可确认/修改/中止 — Unit 1
- R5. 多轮修改后重新确认 — Unit 1
- R6. 综合拆分策略 — Unit 2
- R7. 独立模板文件 — Unit 2
- R8. 独立输出文件 — Unit 2
- R9. 文件头部标注来源设计文档路径 — Unit 2
- R10. 里程碑级 DoD checklist — Unit 2
- R11. 任务顺序标识和依赖关系 — Unit 2

## Scope Boundaries

- 不改动 `references/us设计.md` 模板内容
- 不实现任务执行自动化
- 不改动 `agents/openai.yaml`

## Key Technical Decisions

- **两阶段写入同一个 SKILL.md**：不拆分为两个 skill，在同一个 SKILL.md 中定义完整两阶段流程。理由：两个阶段紧密关联（第二阶段依赖第一阶段的输出），用户交互是连贯的
- **新增模板文件**：`references/任务分解.md` 定义任务分解的输出结构。理由：与 us设计.md 平级，保持关注点分离
- **顺序编号 + 依赖声明**：用 `T1, T2, ...` 编号 + `依赖: T1` 字段表达顺序和依赖关系。理由：简洁，不需要可视化图表（见 origin: Key Decisions）

## Open Questions

### Resolved During Planning

- 任务间依赖关系的表达方式：采用编号 + 依赖字段，不用 mermaid 图（用户明确要求）

### Deferred to Implementation

- 任务分解模板的具体章节细节：需在实际编写时根据典型设计文档内容调整

## Implementation Units

- [ ] **Unit 1: 更新 SKILL.md 为两阶段流程**

**Goal:** 将 SKILL.md 从单阶段扩展为两阶段流程，加入审批节点交互逻辑

**Requirements:** R1, R2, R3, R4, R5

**Dependencies:** None

**Files:**
- Modify: `skills/us-design-writer/SKILL.md`

**Approach:**
- 在 Workflow 部分将现有步骤归为"Phase 1: 设计文档生成"，保持内容不变
- 新增"Phase 2: 任务分解"步骤，定义读取确认后的设计文档、使用任务分解模板、输出独立文件的流程
- 在 Phase 1 和 Phase 2 之间插入"审批节点"步骤：询问用户是否确认 / 需要修改 / 中止，支持多轮修改循环
- 更新 Quality Gates：Phase 2 新增任务分解的自检项（每个任务有 DoD、依赖关系、顺序标识）
- 更新 Output Rules：新增任务分解文件的输出规则
- 更新 Quick Start Prompt：反映两阶段流程

**Patterns to follow:**
- 现有 SKILL.md 的格式风格（YAML frontmatter + Markdown 章节）
- 现有 Quality Gates 的检查项格式

**Test scenarios:**
- Test expectation: none — 纯 Markdown 文档修改，通过人工审读验证

**Verification:**
- SKILL.md 完整描述了两阶段流程、审批节点交互、第二阶段输出规则

- [ ] **Unit 2: 创建任务分解模板文件**

**Goal:** 创建 `references/任务分解.md` 模板，定义任务分解文档的输出结构

**Requirements:** R6, R7, R8, R9, R10, R11

**Dependencies:** None（与 Unit 1 无依赖，可并行）

**Files:**
- Create: `skills/us-design-writer/references/任务分解.md`

**Approach:**
- 模板结构包含：元信息（含来源设计文档路径字段）、任务列表、存疑汇总
- 每个任务包含：编号（T1, T2, ...）、标题、描述、依赖（引用编号）、DoD checklist（功能/测试/文档三类）、交付物
- 顺序通过编号表达，依赖通过 `依赖: T1, T2` 字段表达，可并行的任务不标注相互依赖
- 保持与 `us设计.md` 模板一致的简洁风格

**Patterns to follow:**
- `references/us设计.md` 的格式风格（Markdown 标题 + 占位符 + 示例）

**Test scenarios:**
- Test expectation: none — 纯 Markdown 模板创建，通过人工审读验证

**Verification:**
- 模板包含元信息、来源文档路径、任务编号/依赖/DoD checklist、存疑汇总等所有必要章节

## Sources & References

- **Origin document:** [requirements doc](../brainstorms/2026-04-03-us-design-writer-two-phase-requirements.md)
- Current skill definition: `skills/us-design-writer/SKILL.md`
- Current template: `skills/us-design-writer/references/us设计.md`
