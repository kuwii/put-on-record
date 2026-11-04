# put-on-record（记录在案）

工作留痕：把当前 session 相关的事情整理为客观、极简的 Markdown 文件，分两类：

- **工作记录**（`record`）：**已完成的工作**，供整理述职报告、在争端中举证、回溯事实等使用。
- **任务记录**（`task`）：**布置给你的工作任务/计划**，要求本地自包含，隔段时间只读该文件就能重新理解任务。

- **触发**：用户说"记录在案"/"Put on record"，或要求工作记录、留存证据、争端留证、工作日志、述职素材等；或要求"记录任务"、"任务留痕"、"记录布置给我的任务"、工作任务/任务安排留痕等。未指明记录哪一类时由 skill 按 session 内容自动判断（有已完成工作记工作，刚布置的任务记任务）。
- **写到哪**：用户指定的 Markdown 文件；用户指定的目录（在其中生成 `YYYY-MM-DD-record-{x}-{work-content}.md` 或 `YYYY-MM-DD-task-{x}-{task-content}.md`，两类各自独立编号）；或用户/项目上下文中明确指定给本 skill 的记录目录。都没有则拒绝执行并要求提供。
- **记什么**：
  - 工作记录：只记已完成的事实（写了什么、跑了什么、调研了什么……），不记计划、未完成部分、推测；git 已提交的改动记录 commit ID。
  - 任务记录：任务内容、背景、完成标准、截止时间、布置人/来源等，可知才写；**本地自包含**（本地文件要点内联、不引用其他本地记录，在线 URL 可保留，严禁写入密钥/token 等凭证）；写前审视自身配置的指令/skill/MCP/项目文档并内联相关事实。
  - 正文记录精确到分钟、带时区的时间；一次调用只产出一个文件。
- **合并**：目录下发现高度相似的同类既有记录（task 只和 task 比）时先询问用户是否合并；合并后全文改写为一次成文的风格，时间部分不变。

完整规则见 [skills-cn/put-on-record/SKILL.md](skills-cn/put-on-record/SKILL.md)。

## 安装

无安装脚本，请将对应 skill 目录复制或软链接到 agent 工具的 skills 目录：

```bash
# 以 pi coding agent 为例
ln -s "$PWD/skills-cn/put-on-record" ~/.pi/agent/skills/put-on-record
# 或
cp -r skills-cn/put-on-record ~/.pi/agent/skills/
```

其他 agent 工具（Claude Code、Codex 等）同理，放到各自的 skills 目录即可。

## 官网

[docs/index.html](docs/index.html) 是项目官网（单页、亮/暗主题），浏览器直接打开或放到任意静态服务器均可。

## 开发约定

见 [AGENTS.md](AGENTS.md)（面向协助开发本项目的 agent）。

## 许可

MIT，见 [LICENSE](LICENSE)。
