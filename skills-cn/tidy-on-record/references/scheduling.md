# 定期调用"整理在案"

`tidy-on-record` 本身不创建定时任务、不写脚本。需要定期整理时，在所用 agent 工具里配置周期任务；工具不支持定时任务时，用系统 cron / launchd。

## 推荐的 prompt

```text
整理在案：整理 <记录根目录的绝对路径>，无人值守，直接执行，不用询问；结束后给出整理简报。
```

要点：

- **明确"直接执行，不用询问"**：否则 skill 会按交互模式停下来等确认，定时任务会卡住。
- **给绝对路径**：避免 agent 猜记录目录。
- **要求整理简报**：定时任务回传的结果就是本次整理报告。
- 首次使用建议先手动跑一次，确认主题划分和合并风格符合预期，再开自动模式。

## DSH（DeepSeek Harness）

直接对 agent 说："每周五 18:00 帮我整理在案（记录目录：<路径>）"，由 agent 用 `schedule_create` 之类的定时任务能力配置。每周五 18:00（Asia/Shanghai）的 rrule 示例：

```text
DTSTART;TZID=Asia/Shanghai:20261113T180000
RRULE:FREQ=WEEKLY;BYDAY=FR;BYHOUR=18;BYMINUTE=0
```

## cron + pi

```cron
0 18 * * 5  cd "$HOME" && /absolute/path/to/pi -p --no-session "整理在案：整理 $HOME/work-records，无人值守，直接执行，不用询问，结束后给出整理简报" >> "$HOME/work-records/.tidy-cron.log" 2>&1
```

- cron 环境的 `PATH` 通常不含 pi，用绝对路径更稳。
- 日志文件放在记录根目录里没关系：它不是记录文件，整理时会被忽略；也可以放到记录目录之外。
- cron 在机器休眠时会错过执行；介意的话用下面的 launchd。

## macOS launchd

比 cron 更适合桌面环境（唤醒后补跑）：

1. 新建 `~/Library/LaunchAgents/local.tidy-on-record.plist`，`ProgramArguments` 指向 pi 和 `-p --no-session "整理在案：..."`，`StartCalendarInterval` 设成每周五 18:00。
2. `launchctl load -w ~/Library/LaunchAgents/local.tidy-on-record.plist`。

## 频率建议

- 记录较多：每周一次（如周五下班后）。
- 一般使用：每两周一次。
- 每次合并都会在 `_trash/` 留原件，可每月手动"清理整理回收站"一次；清理前 skill 会先列清单确认。
- 记录目录若在云盘同步范围内，避免整理的同时在另一台设备上编辑同一目录。
