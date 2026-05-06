# Four-Part Output Template

Use this structure when drafting the issue ticket. The final answer must be a complete Markdown document.

````markdown
# <文档标题>

原因分析
1. 问题1
   - 问题描述：
   - 相关代码：
```c
<贴出相关代码片段>
```
   - 影响范围：

2. 问题2
   - 问题描述：
   - 相关代码：
```c
<贴出相关代码片段>
```
   - 影响范围：

实现说明
1. 对应问题1
   - 实现方案：
   - 修改代码：
```c
<贴出修改后的代码片段>
```
   - 说明：

2. 对应问题2
   - 实现方案：
   - 修改代码：
```c
<贴出修改后的代码片段>
```
   - 说明：

测试建议
- 场景1：
- 场景2：
- 场景3：

开发人员测试报告
- 前置操作：
- 执行过程：
- 最终结果：
````

Wording rules:

- Keep each section factual and concise.
- Prefer code evidence over commit-message wording.
- Avoid generic phrases unless the diff proves them.
- Do not claim tests passed unless they were actually run.
- The final output must be a Markdown document, not plain paragraphs without Markdown structure.
- `原因分析` must be split by problem point; do not merge all problems into one paragraph.
- Every `原因分析` point must include the relevant code.
- `实现说明` must map one-to-one to `原因分析`, and each point must include the implementation code.
- `测试建议` should describe scenarios only; do not mention concrete test case names.
- `开发人员测试报告` must include prerequisite steps and the final result.
- Do not use app-style code references. Show code only with fenced Markdown code blocks, preferably with a language tag such as `c`, `cpp`, `diff`, or `bash`.
