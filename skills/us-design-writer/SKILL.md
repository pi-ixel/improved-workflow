---
name: us-design-writer
description: "生成开发前详细设计文档，并基于确认后的设计文档拆分为可独立验收的任务。Use when the user asks for US设计、需求详细设计、技术设计拆解、技术方案评审、详细设计评审、开发方案、实施设计、任务拆分、任务分解、工作拆解。Also trigger when the user provides requirements and wants to plan development work, break down a feature into tasks, or needs a structured design doc before coding."
---

# US Design Writer

基于需求材料输出可开发落地的详细设计文档，经用户审批确认后，进一步拆分为可独立验收的任务分解文件。严格遵循 skill 内置模板。

## Workflow
make a todo list to follow this workflow below
### Phase 1: 创建US目录
用户是否提供US编号？例如：'US-1234'、'AR-1234'
  |__ 若提供则创建目录./.sdd/[US编号]
  |__ 若未提供，则询问用户后创建目录./.sdd/[US编号]

### Phase 2: Launch a subagent to 分析需求对模块影响性
Launch a subagent:
请根据`software_architecture.md`中模块的定义，列举一个表格，分别描述出该需求在这些模块下涉及修改的可能性，不要完全信任software_architecture.md中模块的定义的职责，尽你所能的探索每个模块的代码，再给出可能性：
输出到`./.sdd/[US编号]/module-impact.md`
**参考模板**
模板路径：`<skill-dir>/references/module-impact.md`
**IMPORTENT**: 
- 列举所有模块，不能省略任一模块
- 当software_architecture.md定义的模块无法包含该需求内容时，考虑新增模块，并标记新增

### Phase 3: 请用户检查module-impact.md内容是否正确
while true {
   请用户检查module-impact.md内容是否正确，
   |__ 若有问题 -> LAUNCH A SUBAGENT:根据用户反馈进行修改 -> 回到循环开始
   |__ 若无问题 -> 跳出循环
}

### Phase 4: Launch a subagent to 分析详细修改点
Launch a subagent:
请根据`module-impact.md`中的影响性分析，对所有高、中、低修改可能性的模块进行详细的探索，并且给出明确的修改文件以及修改内容，
输出到`./.sdd/[US编号]/module-modify-detail.md`
**参考模板**
模板路径：`<skill-dir>/references/module-modify-detail.md`
**IMPORTENT**: 
- 列举所有模块，不能省略任一模块，修改内容需要是自然语言
- 当software_architecture.md定义的模块无法包含该需求内容时，考虑新增模块，并标记新增
- 根据`module-impact.md`进行分析

### Phase 5: 请用户检查module-modify-detail.md内容是否正确
while true {
请用户检查module-modify-detail.md内容是否正确，
   |__ 若有问题 -> LAUNCH A SUBAGENT：根据用户反馈进行修改 -> 回到循环开始
   |__ 若无问题 -> 跳出循环
}

### Phase 6：Launch a subagent to 编写US设计文档
Launch a subagent：
请根据`module-modify-detail.md`与`software_architecture.md`编写US设计文档
输出到./.sdd/[US编号]/[US编号]-design.md
**参考模板**
模板路径：模板路径：`<skill-dir>/references/us_design.md`

### Phase 7: 请用户检查[US编号]设计.md内容是否正确
while true {
请用户检查[US编号]设计.md内容是否正确，
   |__ 若有问题 -> Launch a subagent：根据用户反馈进行修改 -> 回到循环开始
   |__ 若无问题 -> 跳出循环
}

### Phase 8: Launch a subagent to 拆分任务
Launch a subagent：
请根据`./.sdd/[US编号]/[US编号]-design.md`与`software_architecture.md`编写任务分解文档
输出到./.sdd/[US编号]/[US编号]-task.md
**参考模板**
模板路径：模板路径：`<skill-dir>/references/task.md`
**IMPORTENT**: 
- 同一模块的修改放在一个任务中完成
- 每个任务最好保证实现的代码不超过400行
- 每个任务要独立可验证
- 顺序通过编号表达，编号顺序是单 agent 默认执行顺序；依赖通过 `依赖: T1, T2` 字段声明。
- 可并行的任务不标注相互依赖，但“可并行”只表示规划属性，不表示执行 agent 可以在当前任务完成前切换到该任务。
- 任务分解文件必须保留模板中的“执行规则”章节，用于约束后续开发 agent 按依赖和编号顺序逐个完成任务。

### Phase 9: 请用户检查[US编号]-任务分解.md内容是否正确
while true {
请用户检查[US编号]-任务分解.md内容是否正确，
   |__ 若有问题 -> LAUNCH A SUBAGENT：根据用户反馈进行修改 -> 回到循环开始
   |__ 若无问题 -> 跳出循环并给用户用简要的语言总结一下整体内容
}