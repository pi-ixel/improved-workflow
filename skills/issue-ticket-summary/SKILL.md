---
name: issue-ticket-summary
description: "Draft issue-ticket content from git changes. Use when Codex needs to summarize code changes into four sections: 原因分析, 实现说明, 测试建议, 开发人员测试报告. Default to the latest commit, and also support a specified commit, commit range, or uncommitted working-tree changes when the user explicitly asks for them. Trigger on requests such as 填写问题单, 根据最新 commit 总结修改, 根据指定 commit 生成问题单, 或根据未提交改动补测试报告."
---

# Issue Ticket Summary

Use git evidence and the changed code as the source of truth. Do not write the ticket from the commit title alone.

## Workflow

1. Resolve the change scope first.
- If the user does not specify a source, default to the latest commit `HEAD`.
- If the user provides a commit id, commit range, or file list, use that exact scope.
- If the user explicitly says "uncommitted changes" or "未提交修改", use the working tree instead of `HEAD`.
- If the working tree mixes unrelated edits, ask the user to narrow with commit ids or file paths.

2. Collect the git evidence.
- Prefer direct git commands instead of helper scripts.
- For the latest commit, start with:

```bash
git show --stat --summary --format=fuller HEAD
git show --unified=3 HEAD -- <path>
```

- For a specified commit, use:

```bash
git show --stat --summary --format=fuller <commit>
git show --unified=3 <commit> -- <path>
```

- For a commit range, use only when the user explicitly asks for multiple commits:

```bash
git log --oneline --decorate <base>..<head>
git diff --stat <base>..<head>
git diff --unified=3 <base>..<head> -- <path>
```

- For uncommitted changes, use:

```bash
git status --short --untracked-files=all
git diff --stat
git diff --unified=3 -- <path>
git diff --cached --stat
git diff --cached --unified=3 -- <path>
git ls-files --others --exclude-standard
```

Common commands:

```bash
git show --stat --summary --format=fuller HEAD
git show --unified=3 HEAD -- lib/tls/openhitls/openhitls-ssl.c
git show --stat --summary --format=fuller 4f3c2ab
git status --short --untracked-files=all
git diff --unified=3 -- lib/tls
```

3. Read beyond the diff when needed.
- Inspect the changed files directly if the reason, behavior, or testing story is still unclear.
- Separate observed facts from inference. If the diff only implies the root cause, say that explicitly.

4. Draft the four required sections.
- The final answer must be a complete Markdown document.
- `原因分析`: Split the analysis into numbered points. Each point should describe one concrete problem only, and each point must cite the relevant code location or changed code snippet.
- `实现说明`: Keep a one-to-one mapping with `原因分析`. For every problem point, describe the corresponding implementation approach and cite the relevant code location or changed code snippet.
- `测试建议`: Describe test scenarios and expected verification focus. Do not mention concrete test case names.
- `开发人员测试报告`: Record only executed or observed verification. Always include the prerequisite setup steps, the execution steps or commands, and the final result. If no verification was run, state `未执行开发自测`.

## Guardrails

- Do not merge unrelated commits into one summary unless the user explicitly asks for a range.
- Do not fabricate root cause, execution results, or coverage claims.
- Do not copy generic wording like "提升稳定性" unless the code evidence actually supports it.
- Distinguish `测试建议` from `开发人员测试报告`.
- Mention key files or commands when they materially support the statement.
- Keep `原因分析` and `实现说明` in the same numbered order so reviewers can match each problem to its fix directly.
- Do not use app-style code references or inline UI annotations for code excerpts. Show all code evidence with fenced Markdown code blocks such as ```c or ```diff.

## Resources

- `references/output-template.md`: Minimal four-section output template and wording rules.
