# VoiceTerm

**Voice-first terminal collaboration for Codex and tmux.**

VoiceTerm helps an AI coding assistant and a user work with the same local terminal session through `tmux`. It is designed for voice-driven use: start a named session for a project, then ask the assistant to inspect output or run a task in that session.

> VoiceTerm standardizes a safe workflow. It is not a sandbox and it cannot bypass Codex, operating-system, or terminal permissions.

## 中文说明

### 它解决什么问题？

VoiceTerm 让你和 Codex 通过 `tmux` 协作同一个终端会话。你继续在 iTerm2、Ghostty 或其他终端中查看和手动操作；Codex 可以在你授权后读取同一会话的输出、发送命令，并把确认流程留在语音对话中。

它适合：

- 一边语音对话，一边在终端里开发、跑测试或看日志；
- 同时处理多个项目或同一项目的多个任务；
- 希望用清晰的语音确认规则来避免误操作。

### 前置条件

- 已安装并能使用 Codex Skills。
- 使用支持 shell 的终端，例如 iTerm2、Ghostty、macOS Terminal、Linux 终端，或 Windows 上的 WSL 终端。
- 已安装 `tmux`。

安装 tmux 时请根据系统选择常见方式：

```bash
# macOS（已安装 Homebrew）
brew install tmux

# Ubuntu / Debian / WSL Ubuntu
sudo apt install tmux

# Fedora
sudo dnf install tmux
```

Windows 用户请在 **WSL** 中使用 tmux；原生 Windows PowerShell 本身不是 tmux 环境。PowerShell 可以作为 macOS、Linux 或 WSL 中的 shell 使用。

### 安装 VoiceTerm

克隆仓库后，将 Skill 文件夹复制到 Codex 的全局 Skills 目录：

```bash
git clone git@github.com:houyongsheng/voiceterm-skill.git
cd voiceterm-skill
mkdir -p ~/.codex/skills
cp -R skills/voice-term ~/.codex/skills/voice-term
```

在 Windows 上，请在 WSL 终端中执行相同命令。安装后，新开一个 Codex 任务；如果没有出现 VoiceTerm，重启 Codex 后再试。

### 第一次使用

1. 在目标**项目根目录**打开终端，不要在系统根目录或家目录启动。
2. 为当前终端起一个“项目名-用途”的会话名，例如 `hsk3-web`。
3. 创建会话：

   ```bash
   tmux new -s hsk3-web
   ```

4. 在 Codex 中说：“使用 VoiceTerm 操作 hsk3-web。”

如果会话已存在，重新连接：

```bash
tmux attach -t hsk3-web
```

### 多项目、多终端

用简短且可说出口的名称，而不是时间戳：

| 场景 | 推荐会话名 |
| --- | --- |
| 项目 hsk3 的前端开发 | `hsk3-web` |
| 项目 hsk3 的测试 | `hsk3-test` |
| 项目 hsk3 的日志 | `hsk3-log` |
| 第二个测试任务 | `hsk3-test-2` |

同一个项目可以有多个会话。VoiceTerm 会按“项目名 + 用途”定位；有歧义时，它应先向你确认，而不是猜测。

### 权限与安全边界

- 只读检查可在选定会话后进行。
- 修改文件、启动/停止进程、运行测试和安装依赖应先经过你的语音确认。
- 删除、推送、部署、改 Git 历史、系统级操作和访问敏感信息必须逐项确认。
- 密码、验证码、恢复码和 API 密钥只能由你亲自输入。
- Codex 或操作系统自己的授权弹窗不能被 Skill 绕过；请在桌面版界面亲自批准或拒绝。
- 不要按 `curl`、`cat` 等命令前缀做全量授权。请按“项目内只读”“访问指定公开网站”“运行本项目测试”这类实际效果授权。

## English

### What is VoiceTerm?

VoiceTerm lets you and Codex collaborate through the same local `tmux` terminal session. You keep using iTerm2, Ghostty, or another terminal to see and work in the session; after your approval, Codex can read its output and send commands through a voice-led workflow.

### Requirements

- Codex with Skills available.
- A shell-capable terminal emulator.
- `tmux` installed.

On Windows, run tmux in **WSL**. Native Windows PowerShell is not itself a tmux environment.

### Install

```bash
git clone git@github.com:houyongsheng/voiceterm-skill.git
cd voiceterm-skill
mkdir -p ~/.codex/skills
cp -R skills/voice-term ~/.codex/skills/voice-term
```

Run the commands in WSL on Windows. Start a new Codex task after installation; restart Codex if the skill is not discovered.

### Quick start

Open a terminal at the target project root and create a session named `project-purpose`:

```bash
tmux new -s hsk3-web
```

Then say: “Use VoiceTerm to work with `hsk3-web`.”

For an existing session:

```bash
tmux attach -t hsk3-web
```

Use meaningful role suffixes such as `web`, `api`, `test`, `log`, or `fix`. If the same role needs another independent session, add a short number, such as `hsk3-test-2`.

### Safety model

VoiceTerm uses tmux for shared terminal context, not filesystem confinement. It scopes ordinary voice approvals to the current Codex task and selected tmux session. High-impact actions require specific confirmation; credentials must always be entered by the user. Codex and operating-system permission prompts remain under the user's control.

## Repository layout

```text
skills/voice-term/   The installable VoiceTerm Skill
README.md            Chinese and English user guide
```

## Status

This is an early, source-install release. A packaged plugin distribution can be added after the workflow has been validated across more terminals and platforms.
