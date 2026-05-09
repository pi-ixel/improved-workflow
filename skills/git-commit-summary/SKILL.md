---
name: git-commit-summary
description: Use when user wants to commit code, auto-generated summary, submit local changes, git commit, code merge, submit MR, generate commit message, 帮我提交本地修改, 代码提交, 提交代码, 提交本地修改, 代码合入, 提交MR, 生成commit信息.
---

# Git Commit Summary

## Check Uncommitted Changes
First, run `git status` and `git diff` to see all uncommitted changes in the working directory.

## Ask for Issue Number (Required)
If user did NOT provide an issue number at the start, MUST ask user for the issue number (DTS/IR/SR/US/AR/RR/Issue). Do NOT proceed without issue number.

## Analyze Changes
Analyze the git diff output to determine:
- What files were changed
- What type of change (feat/fix/docs/refactor/test/chore/style/sync)
- What the change does

## Determine Type
Based on the changes, select the appropriate type:
- feat: 新功能（Feature）
- fix: 修改 Bug
- docs: 文档（Documentation）
- refactor: 重构
- test: 增加、修改测试用例
- chore: 构建过程或辅助工具的变动
- style: 修改代码格式，不影响代码逻辑
- sync: 代码同步，分支间同步

## Generate Subject
- 采用动宾结构
- 结尾不加句号
- 不超过50个英文字符（或25个汉字）
- 整个commit描述（type + subject）控制在100个字符以内

## Generate Body
Only if detailed explanation is needed:
- 解释"为什么"和"是什么"，而不是"如何做"
- 每行长度不超过72个英文字符（或36个汉字）

## Format Commit Message (Must Include Footer)
```
<type>:<subject>

<body>

DIR:<issue_number>
```

IMPORTANT: The commit message MUST include Footer with DIR prefix and issue number. The format must be exactly as shown above.

## Show to User for Confirmation
Show the formatted commit message to user and ask for confirmation. Allow user to:
- Confirm and proceed with commit
- Modify the commit message
- Cancel the commit

## Execute Git Commit (After Confirmation)
Only after user confirms, run `git add -A && git commit -m "<message>"` to commit the changes.

## Verify
Run `git log -1` to verify the commit was created successfully.