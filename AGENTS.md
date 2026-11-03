# 项目开发约定

本项目是 put-on-record（记录在案）agent skill，内容是给 agent 阅读并遵照执行的指令文本。开发本项目 = 编写/修改这个 skill。

## 目录结构

- `skills-cn/put-on-record/`：skill 正文目录（中文正文），`SKILL.md` 必需。
- `SKILL.md` 的 frontmatter：`name`（与目录名一致的 kebab-case）、`description`（中文，写清"做什么 + 什么场景/什么话触发"，触发词要具体，agent 靠它决定是否加载）。
- 较长的参考资料放 skill 目录下的 `references/`，静态资源放 `assets/`；`SKILL.md` 本身保持精简、可直接执行，只引用真正需要时再读的文件。
- 安装由用户手工完成（复制或软链到 agent 工具的 skills 目录，如 pi 的 `~/.pi/agent/skills/`），不要在仓库里写安装脚本。

## 修改 skill 时

- 正文一律中文；skill 产出的文件名、格式等以正文中的规则为准，保持一致。
- 改动后 SKILL.md 必须自洽完整：agent 只会读 SKILL.md 及其中引用的文件，不能依赖你的对话上下文。
- 修改行为规则后，必须按下面的方式实际调用该 skill 测试，而不是只检查文本。

## 测试

- 用 pi 非交互模式实测：`pi -p --no-session "<指令>"`（每次都是全新 session；`--no-session` 保证不落会话文件）。
- 测试在临时目录中进行（`mktemp -d`），测试产生的工作记录等一切文件都在该临时目录内。
- 测试完成后删除整个临时目录，不留后台进程。
- 测试中不得向系统安装任何东西。
