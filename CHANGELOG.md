# Changelog

本项目的更新记录，日期格式 `YYYY-MM-DD`。

## [Unreleased]

### Added

- `UPSTREAM.md`：上游 `social-auto-upload` 版本锁定（pin commit、关键依赖、升级固定流程）与
  平台覆盖表（上游 10 个平台子命令 vs 本仓库已收录 4 个），README 相应链接

### Changed

- 许可证由 MIT 变更为 **Apache License 2.0**：`LICENSE` 替换为 Apache 官方全文，
  新增 `NOTICE`（版权声明 + 对上游 MIT 项目的归属说明）与 `CONTRIBUTING.md`，README 增加许可证章节
- 统一 4 个技能的登录二维码处置口径，与 README「安全说明」对齐：
  默认把二维码图片的本地路径交给用户在本机扫码（二维码即登录态凭证），
  仅在用户明确信任聊天通道时才经会话发送图片
- 按 pin 住的上游 commit 实测 `sau <platform> <cmd> --help`，同步 CLI 契约文档：
  - `douyin upload-video` 补 `--thumbnail-landscape` / `--thumbnail-portrait` / `--declaration` / `--collection`
  - `douyin upload-note` 补 `--notef` / `--bgm`
  - `kuaishou upload-video` 补 `--collection`
  - `bilibili upload-video` 补可选 `--thumbnail`

### Fixed

- 修正 1.0.0 记录中的文件计数表述（发布时全仓库 41 个文件，其中技能目录 36 个）

## [1.0.0] - 2026-09-25

首个公开版本。

### Added

- 收录 4 个社媒发布技能，均基于 `social-auto-upload` 的 `sau` CLI：
  - `douyin-upload` — 抖音登录 / cookie 校验 / 视频与图文发布
  - `kuaishou-upload` — 快手登录 / cookie 校验 / 视频与图文发布
  - `xiaohongshu-upload` — 小红书登录 / cookie 校验 / 视频与图文发布
  - `bilibili-upload` — B 站登录 / 账号校验 / 视频上传（自动准备 `biliup`）
- 每个技能包含 `SKILL.md`、`references/`（运行前提、CLI 契约、故障排查）、`scripts/examples/`（ps1 / sh / py 命令模板）、`locales/`
- `README.md`（安装、使用、安全说明）与 `.gitignore`

### Security

- 公开发布前对全部 41 个文件（4 个技能目录 36 个 + 根目录与 CI 5 个）做了两轮敏感信息正则扫描（token / 密码 / cookie 值 / API key / 邮箱 / 手机号 / 盘符路径 / IP / 平台凭据字段），结果 0 命中，并人工抽读关键文档复核
- `.gitignore` 排除 cookie、二维码、虚拟环境、本地配置等敏感产物
