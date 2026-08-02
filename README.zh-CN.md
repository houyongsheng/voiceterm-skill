# VoiceTerm

[English](README.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [Español](README.es.md)

**面向 Codex 与 tmux 的语音终端协作方式。**

VoiceTerm 让 AI 编程助手与用户通过 `tmux` 协作同一个本地终端会话。它面向语音使用：先为项目创建一个有名字的会话，再让助手读取输出或在该会话中执行任务。

> VoiceTerm 用于规范安全的协作流程；它不是沙箱，也不能绕过 Codex、操作系统或终端本身的权限。

## 它解决什么问题？

你继续用 iTerm2、Ghostty 或其他终端查看和手动操作；在获得你的确认后，Codex 可以读取同一 tmux 会话的输出、发送命令，并将确认流程保留在语音对话中。

它适合：

- 一边语音对话，一边开发、跑测试或看日志；
- 同时处理多个项目，或同一项目的多个并行任务；
- 希望用清晰的语音确认规则减少误操作。

## 前置条件

- 已安装并能使用 Codex Skills。
- 使用支持 shell 的终端，例如 iTerm2、Ghostty、macOS Terminal、Linux 终端，或 Windows 上的 WSL 终端。
- 已安装 `tmux`。

请按系统选择常见安装方式：

```bash
# macOS（已安装 Homebrew）
brew install tmux

# Ubuntu / Debian / WSL Ubuntu
sudo apt install tmux

# Fedora
sudo dnf install tmux
```

Windows 用户请在 **WSL** 中使用 tmux；原生 Windows PowerShell 本身不是 tmux 环境。PowerShell 可以作为 macOS、Linux 或 WSL 中的 shell 使用。

## 安装 VoiceTerm

克隆仓库后，将 Skill 文件夹复制到 Codex 的全局 Skills 目录：

```bash
git clone git@github.com:houyongsheng/voiceterm-skill.git
cd voiceterm-skill
mkdir -p ~/.codex/skills
cp -R skills/voice-term ~/.codex/skills/voice-term
```

在 Windows 上，请在 WSL 终端中执行相同命令。安装后新开一个 Codex 任务；如果没有发现 VoiceTerm，重启 Codex 后再试。

## 快速开始

在目标**项目根目录**打开终端，再创建一个名为“项目名-用途”的会话：

```bash
tmux new -s hsk3-web
```

然后对 Codex 说：“使用 VoiceTerm 操作 `hsk3-web`。”

如果会话已存在，重新连接：

```bash
tmux attach -t hsk3-web
```

用途请用简短、好说的词，例如 `web`、`api`、`test`、`log` 或 `fix`。如果同一用途需要第二个独立会话，追加短编号，例如 `hsk3-test-2`。

## 多项目、多终端

| 场景 | 推荐会话名 |
| --- | --- |
| 项目 `hsk3` 的前端开发 | `hsk3-web` |
| 项目 `hsk3` 的测试 | `hsk3-test` |
| 项目 `hsk3` 的服务日志 | `hsk3-log` |
| 第二个测试任务 | `hsk3-test-2` |

VoiceTerm 会按项目名和用途匹配会话；目标有歧义时，它会先确认，而不是猜测。

## 权限与安全边界

- 选定会话后，可进行常规只读检查。
- 修改项目、控制进程、运行测试和安装依赖，需要明确的语音确认。
- 删除、改 Git 历史、推送、部署、改账号或访问敏感信息，必须对每个动作单独确认。
- 密码、验证码、恢复码和 API 密钥只能由你亲自输入。
- Codex 或操作系统自己的授权弹窗仍由你控制。
- 不要按命令前缀做全量授权；请按实际效果授权，例如“仅读取本项目”“从指定公开网站下载”或“运行本项目测试”。

## 仓库结构

```text
skills/voice-term/   可安装的 VoiceTerm Skill
README.md            English guide
README.zh-CN.md      简体中文指南
```

## 状态

当前是通过源码安装的早期版本。等不同终端和平台的流程被进一步验证后，可以再增加一键安装的插件发行方式。
