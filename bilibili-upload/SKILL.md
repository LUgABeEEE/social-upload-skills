---
name: bilibili-upload
description: 当 agent 需要通过已安装的 `sau` CLI 完成 Bilibili 登录、账号校验或视频上传时使用这个 skill。优先使用 `sau bilibili ...`，程序会自动准备 `biliup`，不要求用户手动安装。
---

# Bilibili 上传 Skill

优先把 `sau` 作为主接口。

不要一开始就让用户自己找 `biliup` 或手动下载 release。
程序会在运行时自动检查、自动下载、自动更新 `biliup`。

## 功能概览

| 功能 | 命令入口 | 说明 |
| --- | --- | --- |
| 登录 | `sau bilibili login --account <name>` | 需要用户自己在本地真实终端里执行，用于生成或刷新登录信息 |
| 校验 | `sau bilibili check --account <name>` | 检查指定账号当前是否有效 |
| 视频上传 | `sau bilibili upload-video ...` | 上传一条 Bilibili 视频 |

## 默认工作流

1. 先确认 `references/runtime-requirements.md`
2. 再确认 `references/cli-contract.md`
3. 执行匹配的 `sau bilibili ...` 命令
4. 如果命令失败，再看 `references/troubleshooting.md`

## 命令选择建议

- 用户没有登录信息，先让用户自己在本地终端执行 `login`
- 用户只想确认账号状态，先用 `check`
- 用户要发视频，用 `upload-video`

## 执行前检查

- 优先确认当前环境能运行 `sau`
- 如果 `sau` 不在 PATH 中，可以用仓库里的 `sau_cli.py`
- 不要要求用户手动下载 `biliup`
- 第一次运行 Bilibili 命令时，程序可能会自动联网准备 `biliup`
- 对 agent 来说，不要在非交互环境里硬跑 `sau bilibili login`
- 正确做法是让用户自己在本地终端执行 `sau bilibili login --account <name>`
- 如果终端里的二维码显示不完整，提醒用户直接打开当前目录下的 `qrcode.png` 扫码
- 登录二维码即登录态凭证：让用户在本机打开图片扫码，不要把图片经聊天通道转发，也不要把二维码内容转述成文本

## 模板文件

- `scripts/examples/bilibili_commands.ps1`
- `scripts/examples/bilibili_commands.sh`
- `scripts/examples/bilibili_cli_template.py`


## 平台差异（先看这个）

跨平台对照见仓库根目录 `PLATFORM-MATRIX.md`。B 站有三条硬差异：

- **没有图文发布** —— B 站只有 `upload-video`，无 `upload-note`。需要图文请换平台
- `--tid` **第一版必传** —— 分区 ID，不传会失败
- **程序自动准备 `biliup`** —— 会自动检查 / 下载 / 更新。**不要要求用户手动安装或去下载 release**

`--tags` 用逗号分隔。`--schedule` 走 `sau` 统一时间格式 `YYYY-MM-DD HH:MM`。

## 发布前检查清单

执行 `upload-video` 之前逐条确认：

- [ ] `sau bilibili --help` 能跑通
- [ ] `--account` 与目标账号一致
- [ ] `sau bilibili check --account <name>` 已通过
- [ ] 必填齐全：`--file` `--title` **`--tid`**
- [ ] **不要把账号信息写进对话或日志**
- [ ] 首次运行 B 站命令时程序可能自动联网准备 `biliup`，提前告知用户

## 参考文档

- 运行前提：`references/runtime-requirements.md`
- CLI 契约：`references/cli-contract.md`
- 故障排查：`references/troubleshooting.md`
