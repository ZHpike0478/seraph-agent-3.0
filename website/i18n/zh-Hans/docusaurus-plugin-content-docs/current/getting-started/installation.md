---
sidebar_position: 2
title: "安装"
description: "从源码安装 Seraph Agent 3.0；此 fork 尚未发布桌面安装包或 Termux 软件包"
---

# 安装

使用一行安装命令，两分钟内即可启动并运行 Seraph Agent 3.0。

## 快速安装

### 一行安装命令（Linux / macOS / WSL2）

基于 git 的安装方式，跟踪 `main` 分支，可立即获取最新变更：

```bash
curl -fsSL https://raw.githubusercontent.com/ZHpike0478/seraph-agent-3.0/main/scripts/install.sh | bash
```

### Windows（原生，PowerShell）

原生 Windows 无需 WSL 即可运行 Seraph Agent 3.0——CLI、gateway、TUI 和工具均可原生运行。（原生安装与 WSL2 安装可干净共存；唯一仅限 WSL2 的功能见下方功能说明。）遇到 bug 请[在此 fork 中提交 issue](https://github.com/ZHpike0478/seraph-agent-3.0/issues)。

打开 PowerShell 并运行：

```powershell
iex (irm https://raw.githubusercontent.com/ZHpike0478/seraph-agent-3.0/main/scripts/install.ps1)
```

源码安装脚本克隆仓库，再由 PM 准备 Python 3.14、Node.js、npm、pnpm、ripgrep、FFmpeg 和 Python 依赖。
缺少 Git 时，脚本下载经过 SHA-256 验证的 Git for Windows 到工具存储目录。
它不替换系统 Git，也不再使用 MinGit 或 `hermes\node` 的旧布局。
安装完成后，打开新终端以读取用户 PATH。

**桌面应用：** 此 fork 在仓库中提供桌面应用源码，但尚未发布 MSIX、App Installer、DMG 或自动更新包。可通过源码安装后运行 `seraph desktop`。上游 Hermes 的桌面安装包不属于 Seraph 发行版。

### Android / Termux

此 fork 不发布 Termux APT 软件包。[Termux 指南](./termux.md)仅介绍上游 Hermes Agent 软件包，并非 Seraph 安装方式。

### 功能与安装目录

Windows 的 CLI、TUI、gateway 和桌面应用可原生运行。
Dashboard 终端使用 `pywinpty`/ConPTY，不再是尚未实现的 POSIX-only 功能。
部分可选依赖仍受架构限制，请参阅 [Windows 指南](../user-guide/windows-native.md)。

| 安装方式 | 代码 | 命令入口 | 默认数据目录 |
|---|---|---|---|
| POSIX 源码脚本 | `~/.hermes/hermes-agent/` | `seraph` 主命令；`hermes` 兼容别名 | `~/.hermes/` |
| Windows 源码脚本 | `%LOCALAPPDATA%\hermes\hermes-agent\` | `seraph` 主命令；`hermes` 兼容别名 | `%LOCALAPPDATA%\hermes\` |
| 上游 Hermes 桌面软件包（此 fork 未发布） | 应用包内部 | 上游包内启动器 | 平台默认数据目录 |
| 上游 Hermes Docker 镜像（此 fork 未发布） | `/opt/hermes/` | 镜像入口和 `hermes` | 挂载的 `/opt/data/` |
| 上游 Hermes Termux APT（此 fork 未发布） | `$PREFIX/lib/hermes-agent/` | `$PREFIX/bin/` 符号链接 | `~/.hermes/` |

`HERMES_HOME` 选择数据目录。POSIX 的 `--dir` 单独选择源码目录。
以 root 身份运行不再自动选择 `/usr/local/lib` 的 FHS 布局。
PM 的工具和 Python 环境代际位于独立目录，详见 [包管理](../reference/package-management.md)。
不要为了修复应用而删除整个数据目录。

### 安装后

重新加载 shell 并开始聊天：

```bash
source ~/.bashrc   # 或：source ~/.zshrc
seraph             # 开始聊天！
```

`seraph` 是主要命令；`hermes` 保留为兼容别名。

如需稍后重新配置单项设置，使用以下专用命令：

```bash
seraph model          # 选择 LLM 提供商和模型
seraph tools          # 配置启用的工具
seraph gateway setup  # 配置消息平台
seraph config set     # 设置单个配置项
seraph setup          # 或运行完整的设置向导一次性配置所有内容
```

:::tip 最快路径：Nous Portal
一个订阅涵盖 300+ 个模型以及 [Tool Gateway](../user-guide/features/tool-gateway.md)（网络搜索、图像生成、TTS、云端浏览器）。无需逐一管理各工具的密钥：

```bash
seraph setup --portal
```

该命令一次性完成登录、设置 Nous 为提供商并开启 Tool Gateway。
:::

---

## 前置条件

POSIX 源码脚本需要 Git、curl、tar 和 SHA-256 工具。源码构建还可能需要编译器和系统库。
Hermes 要求 Python 3.14（`>=3.14,<3.15`），工具版本由 `pm/lock.json` 决定。
自包含桌面软件包不要求用户自行编译基础依赖。

:::tip Nix 用户
如果你使用 Nix（在 NixOS、macOS 或 Linux 上），有专门的配置路径，包含 Nix flake、声明式 NixOS 模块和可选容器模式。请参阅 **[Nix & NixOS 配置](./nix-setup.md)** 指南。
:::

---

## 手动 / 开发者安装

如果你想克隆仓库并从源码安装——用于贡献代码、从特定分支运行或完全控制虚拟环境——请参阅贡献指南中的[开发环境配置](../developer-guide/contributing.md)章节。

---

## 非 Sudo / 系统服务用户安装

以目标服务用户运行源码安装脚本。由管理员预先安装构建所需工具和 Chromium 的系统库。
当前脚本不运行 Playwright 的 `--with-deps`，也不提供按发行版选择的 sudo 回退。

安装后，将 `$HOME/.local/bin` 加入服务用户的 PATH，并运行 `seraph doctor`。
请使用脚本生成的包装器，不要硬编码 `venv/bin/hermes`。
Linux 用户服务需要在注销后继续运行时，由管理员为该用户启用 lingering。
详见 [消息 Gateway](../user-guide/messaging/index.md)。

---

## 故障排查

| 问题                        | 解决方案                                                                           |
| --------------------------- | ---------------------------------------------------------------------------------- |
| `hermes: command not found` | 重新加载 shell（`source ~/.bashrc`）或检查 PATH                                    |
| `API key not set`           | 运行 `seraph model` 配置提供商，或 `seraph config set OPENROUTER_API_KEY your_key` |
| 更新后配置丢失              | 运行 `seraph config check`，然后运行 `seraph config migrate`                       |

如需更多诊断信息，运行 `seraph doctor`——它会告诉你确切缺少什么以及如何修复。

## 安装方式自动检测

Hermes 会自动检测安装方式（git 安装程序、Docker 或 NixOS），`seraph update` 会打印对应路径的更新命令。无需设置任何环境变量——检测基于安装目录结构（`~/.hermes/hermes-agent/` 检出、Docker 镜像标记或 Nix store 路径）。`seraph doctor` 也会在其环境摘要中显示检测到的安装方式。
