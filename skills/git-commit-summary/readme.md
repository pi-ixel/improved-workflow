### 安装路径
- opencode
    /aaa/bbb/.config/opencode/skills/git-commit-summary
### 使用案例：
- 帮我使用auto-generated summary提交修改，单号是AR.SR.IR00000000.000.000

- 帮我提交本地修改
  a. 补充说明单号：AR.SR.IR00000000.000.000
  b. 确认修改：修改
  c. 编辑修改：编辑后提交

- 提交修改
 a. 请提供issue编号: Issue11

skill调用报错：Unable to connect. Is the computer able to access the url
原因：于OpenCode在调用这些工具时，会用到ripgrep包来检索文件，在没有ripgrep包的情况下，OpenCode会自动进行安装，但是由于内网容器不连通外网
解决：apt install -y ripgrep
