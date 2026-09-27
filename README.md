# social-upload-skills

> 面向 AI Agent 的社媒发布技能包（SKILL.md 格式），统一通过 [`social-auto-upload`] 的 `sau` 命令完成登录、cookie 校验、视频与图文发布。
>
> 支持平台：**抖音 · 快手 · 小红书 · 哔哩哔哩**

[`social-auto-upload`]: https://github.com/dreammis/social-auto-upload

## 技能清单

| 技能 | 平台 | 能力 |
| --- | --- | --- |
| `douyin-upload` | 抖音 | `login` / `check` 登录与 cookie 校验，`upload-video` 视频、`upload-note` 图文发布 |
| `kuaishou-upload` | 快手 | 同上，快手平台的命令式发布工作流 |
| `xiaohongshu-upload` | 小红书 | 同上，小红书平台的命令式发布工作流 |
| `bilibili-upload` | 哔哩哔哩 | `sau bilibili` 登录、账号校验、视频上传（自动准备 `biliup`，无需手动安装） |

四个技能结构一致，均包含：

- `SKILL.md` — 技能入口（含 YAML frontmatter，可被 MiMo Desktop 直接加载）
- `references/runtime-requirements.md` — 运行前提与安装方式
- `references/cli-contract.md` — `sau` 命令契约（参数与语义）
- `references/troubleshooting.md` — 常见故障排查
- `scripts/examples/` — PowerShell / Bash / Python 命令模板
- `locales/` — 中英文显示元数据

## 前置条件

1. **Python 3.10+**，推荐配合 [uv]
2. **安装 social-auto-upload**（提供 `sau` 命令）——在其项目根目录执行：

   ```bash
   uv pip install -e .
   ```

   > **必须锁定上游版本**：本仓库已用 [UPSTREAM.md](UPSTREAM.md) 记录验证过的上游 commit、
   > 关键依赖版本与平台覆盖情况；升级前先读其中的「升级上游的固定流程」，
   > 避免被静默变更的 CLI 契约或新引入的风险代码影响。

3. **安装 patchright 浏览器**（Windows PowerShell）：

   ```powershell
   $env:PLAYWRIGHT_DOWNLOAD_HOST="https://npmmirror.com/mirrors/playwright"; patchright install chromium
   ```

   Linux / macOS：

   ```bash
   PLAYWRIGHT_DOWNLOAD_HOST="https://npmmirror.com/mirrors/playwright" patchright install chromium
   ```

4. 各平台账号：首次 `login` 通过扫码在**本机**生成 cookie

[uv]: https://github.com/astral-sh/uv

## 安装到 MiMo Desktop

把需要的技能目录复制到用户技能目录，重启引擎或新开会话即可加载：

```powershell
# Windows（PowerShell）
Copy-Item -Recurse .\douyin-upload "$env:USERPROFILE\.config\mimocode\skills\"
```

```bash
# macOS / Linux
cp -r ./douyin-upload ~/.config/mimocode/skills/
```

也可以只挑选需要的平台，四个技能彼此独立、互不依赖。

## 使用

安装后在对话中直接说需求即可触发，例如：

- 「登录抖音并检查 cookie 是否有效」
- 「把 `demo.mp4` 上传到快手，标题 xx，标签 xx」
- 「发一条小红书图文」
- 「上传视频到 B 站」

Agent 侧的默认工作流（每个技能的 `SKILL.md` 中有完整说明）：

1. 读 `references/runtime-requirements.md` 确认运行前提
2. 读 `references/cli-contract.md` 确认命令契约
3. 执行对应的 `sau <平台> ...` 命令
4. 失败时再查 `references/troubleshooting.md`

典型命令：

```bash
sau douyin login --account <name>     # 登录/刷新 cookie
sau douyin check --account <name>     # 校验 cookie 有效性
sau douyin upload-video ...           # 发布视频
sau douyin upload-note ...            # 发布图文
```

快手 / 小红书 / B 站同构，替换平台子命令即可。

## 安全说明

- **本仓库不含任何账号密码、cookie、token、二维码或个人路径**；发布前已做两轮正则全量扫描（凭据字段 / 手机号 / 邮箱 / 盘符路径 / IP）加人工抽读，命中为 0
- 登录生成的 cookie、二维码图片、虚拟环境等本地敏感产物已被 `.gitignore` 排除，不会进入提交历史
- 请勿把本机 cookie 文件复制进仓库，公开仓库尤其注意
- **登录二维码本身即登录态凭证**：经聊天通道发送给 agent 时会途经模型服务商，仅在信任该通道时使用；更稳妥的做法是让用户直接在本机打开二维码图片路径扫码

## 免责声明

使用前请阅读并遵守各平台的用户协议与自动化发布规则；本技能包仅作个人工作流自动化用途，因滥用导致的账号风险或法律责任由使用者自行承担。

## 许可证

本仓库以 [Apache License 2.0](LICENSE) 开源，版权声明见 [NOTICE](NOTICE)。

本仓库只包含技能文档与命令契约描述，**未复制上游 `social-auto-upload` 的任何源码**；
上游项目为 MIT 许可（Copyright (c) 2023 dreammis），对其 `sau` CLI 的描述性引用不构成代码再分发。

贡献内容同样按 Apache-2.0 授权，见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 更新记录

见 [CHANGELOG.md](CHANGELOG.md)；上游版本锁定与平台覆盖见 [UPSTREAM.md](UPSTREAM.md)。
