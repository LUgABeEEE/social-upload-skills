# 贡献指南

感谢贡献。本仓库是**文档与工作流层**，所有命令契约必须与真实 CLI 行为一致，这是最重要的规则。

## 报告问题

- 命令报错、参数与文档不符、二维码/登录流程问题 → 提 [Issue](https://github.com/missfor100/social-upload-skills/issues)，附上：
  - 你执行的完整命令（**脱敏**：去掉账号名、cookie、本机路径）
  - 实际输出与期望输出
  - `sau --help` 与对应平台 `--help` 的输出

## 修改文档（硬性要求）

1. **先实测，再落笔**：任何参数、子命令、行为描述的改动，必须先在本机执行
   `sau <platform> <command> --help`（或实际跑一遍流程）确认，不允许凭记忆或想象写。
2. 同步更新受影响的 `SKILL.md`、`references/cli-contract.md`、`references/runtime-requirements.md`、
   `references/troubleshooting.md` 与 `scripts/examples/`，保持四个技能结构一致。
3. 在 `CHANGELOG.md` 的 `Unreleased` 段记录改动；若因上游升级引起，先按
   [UPSTREAM.md](UPSTREAM.md) 的「升级上游的固定流程」更新 pin。
4. 提交前自查：不得引入任何 cookie、二维码、账号名、token、本机盘符路径等敏感信息。

## 新增平台技能

按 [UPSTREAM.md](UPSTREAM.md) 的「扩充新平台时的最小流程」执行，先实测 `--help` 拿到真实契约。

## 提交规范

- commit message 使用英文、约定式前缀：`docs:` / `fix:` / `feat:` / `ci:` / `chore:`
- 一个提交只做一件事

## 许可

贡献的内容按 [Apache License 2.0](LICENSE) 授权；提交即表示你有权以该许可证贡献这些内容。
