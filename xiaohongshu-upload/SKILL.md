---
name: xiaohongshu-upload
description: 当 agent 需要通过已安装的 `sau` CLI 完成小红书登录、cookie 校验、视频上传或图文发布时使用这个 skill。该 skill 适用于已经安装 `social-auto-upload` 且可调用 `sau` 命令的环境。优先使用这个 skill 进行稳定的命令式小红书工作流，而不是一开始就阅读 uploader 源码。
---

# 小红书上传 Skill

优先把 `sau` 作为主接口。

不要假设当前环境一定能读取仓库源码。
不要一开始就去读 `uploader/`。
只有在命令不可用或 CLI 执行失败时，才回退到故障排查说明。

## 功能概览

| 功能 | 命令入口 | 说明 |
| --- | --- | --- |
| 小红书登录 | `sau xiaohongshu login --account <name>` | 生成或刷新指定账号的 cookie |
| cookie 校验 | `sau xiaohongshu check --account <name>` | 检查指定账号 cookie 是否有效 |
| 视频上传 | `sau xiaohongshu upload-video ...` | 上传并发布小红书视频 |
| 图文上传 | `sau xiaohongshu upload-note ...` | 上传并发布小红书图文 |

元数据约定：

- 视频使用 `title + desc + tags`
- 图文使用 `title + note + tags`

## 默认工作流

1. 先确认 `references/runtime-requirements.md` 里的运行前提。
2. 再确认 `references/cli-contract.md` 里的命令契约。
3. 执行匹配的 `sau xiaohongshu ...` 命令。
4. 如果命令失败，再看 `references/troubleshooting.md`。

## 支持动作

- 使用 `sau xiaohongshu login --account <name>` 登录小红书
- 使用 `sau xiaohongshu check --account <name>` 校验 cookie 是否有效
- 使用 `sau xiaohongshu upload-video ...` 上传小红书视频
- 使用 `sau xiaohongshu upload-note ...` 上传小红书图文

## 命令选择建议

- 当用户需要新的 cookie，或现有 cookie 已失效时，使用 `login`
- 当用户只需要确认 cookie 状态时，使用 `check`
- 当用户要发布视频时，使用 `upload-video`
- 当用户要发布图文时，使用 `upload-note`

## 执行前检查

- 先确认当前 shell 里是否可以调用 `sau`
- 如果 `sau` 不可用，按 `references/runtime-requirements.md` 里的回退方式处理
- 当用户明确指定无头或有头模式时，显式传 `--headless` 或 `--headed`
- 只有用户明确要求定时发布时，才使用 `--schedule`
- 登录二维码本身就是登录态凭证：默认把本地二维码图片的路径给用户，让用户在本机直接打开扫码
- 只有在用户明确表示信任该聊天通道时，才把二维码图片经会话发送（会途经模型服务商）
- 口径以根目录 README 的「安全说明」为准，不要把二维码内容转述成文本或链接


## 平台差异（先看这个）

跨平台对照见仓库根目录 `PLATFORM-MATRIX.md`。

小红书是四个平台里**参数最少**的：只有通用参数加 `--images` / `--note`，
没有 `--collection`、`--declaration`、`--product-*`、`--bgm`。

**B 站不支持图文发布**（无 `upload-note`），需要图文时不要选 B 站。

## 发布前检查清单

执行 `upload-*` 之前逐条确认：

- [ ] `sau xiaohongshu --help` 能跑通
- [ ] `--account` 与目标账号一致（一个 account_name 一个账号文件）
- [ ] `sau xiaohongshu check --account <name>` 已通过，**别跳过直接 upload**
- [ ] 必填齐全：视频 `--file` `--title`；图文 `--images` `--title`
- [ ] **不要把 cookie 路径、二维码内容写进对话或日志**

## 模板文件

当你需要稳定的命令模板时，使用 `scripts/examples/` 下的文件：

- `xiaohongshu_commands.ps1`
- `xiaohongshu_commands.sh`
- `xiaohongshu_cli_template.py`

## 参考文档

- 运行前提：`references/runtime-requirements.md`
- CLI 契约：`references/cli-contract.md`
- 故障排查：`references/troubleshooting.md`
