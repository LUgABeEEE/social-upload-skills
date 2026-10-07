---
name: kuaishou-upload
description: 当 agent 需要通过已安装的 `sau` CLI 完成快手登录、cookie 校验、视频上传或图文发布时使用这个 skill。该 skill 适用于已经安装 `social-auto-upload` 且可调用 `sau` 命令的环境。优先使用这个 skill 进行稳定的命令式快手工作流，而不是一开始就阅读 uploader 源码。
---

# 快手上传 Skill

优先把 `sau` 作为主接口。

不要假设当前环境一定能读取仓库源码。
不要一开始就去读 `uploader/`。
只有在命令不可用或 CLI 执行失败时，才回退到故障排查说明。

## 功能概览

| 功能 | 命令入口 | 说明 |
| --- | --- | --- |
| 快手登录 | `sau kuaishou login --account <name>` | 生成或刷新指定账号的 cookie |
| cookie 校验 | `sau kuaishou check --account <name>` | 检查指定账号 cookie 是否有效 |
| 视频上传 | `sau kuaishou upload-video ...` | 上传并发布快手视频 |
| 图文上传 | `sau kuaishou upload-note ...` | 上传并发布快手图文 |

元数据约定：

- 视频使用 `title + desc + tags`
- 图文使用 `title + note + tags`

## 默认工作流

1. 先确认 `references/runtime-requirements.md` 里的运行前提。
2. 再确认 `references/cli-contract.md` 里的命令契约。
3. 执行匹配的 `sau kuaishou ...` 命令。
4. 如果命令失败，再看 `references/troubleshooting.md`。

## 支持动作

- 使用 `sau kuaishou login --account <name>` 登录快手
- 使用 `sau kuaishou check --account <name>` 校验 cookie 是否有效
- 使用 `sau kuaishou upload-video ...` 上传快手视频
- 使用 `sau kuaishou upload-note ...` 上传快手图文

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

跨平台对照见仓库根目录 `PLATFORM-MATRIX.md`。快手相关的两条最容易踩：

- **图文要求真实多张图片文件**，不能用同一路径重复多次凑数
- `--collection` **必须是账号内已存在的合集**，不存在会失败

快手独有参数只有 `--collection`。小红书参数最简，B 站没有图文发布。

## 发布前检查清单

执行 `upload-*` 之前逐条确认：

- [ ] `sau kuaishou --help` 能跑通
- [ ] `--account` 与目标账号一致（一个 account_name 一个账号文件）
- [ ] `sau kuaishou check --account <name>` 已通过，**别跳过直接 upload**
- [ ] 必填齐全：视频 `--file` `--title`；图文 `--images` `--title`
- [ ] 图文是**多张不同的真实图片**
- [ ] 带 `--collection` 的，确认账号内合集已存在
- [ ] **不要把 cookie 路径、二维码内容写进对话或日志**

## 模板文件

当你需要稳定的命令模板时，使用 `scripts/examples/` 下的文件：

- `kuaishou_commands.ps1`
- `kuaishou_commands.sh`
- `kuaishou_cli_template.py`

## 参考文档

- 运行前提：`references/runtime-requirements.md`
- CLI 契约：`references/cli-contract.md`
- 故障排查：`references/troubleshooting.md`
