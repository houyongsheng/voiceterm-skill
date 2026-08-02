# VoiceTerm

[English](README.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [Español](README.es.md)

**Voice-first terminal collaboration for AI coding agents and tmux.**

VoiceTerm helps an AI coding assistant and a user work with the same local terminal session through `tmux`. It is designed for voice-driven use: start a named session for a project, then ask the assistant to inspect output or run a task in that session.

> VoiceTerm standardizes a safe workflow. It is not a sandbox and it cannot bypass agent, operating-system, or terminal permissions.

## What is VoiceTerm?

VoiceTerm lets you and a compatible coding agent collaborate through the same local `tmux` terminal session. You keep using iTerm2, Ghostty, or another terminal to see and work in the session; after your approval, the agent can read its output and send commands through a voice-led workflow.

It is useful when you want to:

- develop, test, or inspect logs while keeping a voice conversation open;
- work across several projects or several parallel tasks in one project;
- use clear voice-confirmation rules to reduce accidental actions.

## Requirements

- A compatible coding agent with Skills available.
- A shell-capable terminal emulator.
- `tmux` installed.

Install tmux with a common package manager for your platform:

```bash
# macOS (with Homebrew)
brew install tmux

# Ubuntu / Debian / WSL Ubuntu
sudo apt install tmux

# Fedora
sudo dnf install tmux
```

On Windows, run tmux in **WSL**. Native Windows PowerShell is not itself a tmux environment; PowerShell can be used as a shell on macOS, Linux, or in WSL.

## Install VoiceTerm

### One-command install (recommended)

```bash
npx skills add houyongsheng/voiceterm-skill --skill voice-term --global
```

The installer lets the user choose a supported agent, such as Codex, Claude Code, or Cursor, and installs VoiceTerm globally for that agent. For a fixed Codex-only command, add `--agent codex`. Add `--yes` only when you want to skip the installer's confirmation prompt.

### Manual install

Clone this repository and copy the skill folder into Codex's global skills directory:

```bash
git clone git@github.com:houyongsheng/voiceterm-skill.git
cd voiceterm-skill
mkdir -p ~/.codex/skills
cp -R skills/voice-term ~/.codex/skills/voice-term
```

On Windows, run the same commands in WSL. Start a new task in the selected agent after installation; restart the agent if the skill is not discovered.

## Quick start

Open a terminal at the target project root, then create a session named `project-purpose`:

```bash
tmux new -s mygame-web
```

Then ask your agent to use VoiceTerm with `mygame-web`.

For an existing session:

```bash
tmux attach -t mygame-web
```

Use meaningful role suffixes such as `web`, `api`, `test`, `log`, or `fix`. If the same role needs another independent session, add a short number, such as `mygame-test-2`.

## Multiple projects and terminals

| Scenario | Suggested session name |
| --- | --- |
| Front-end work for `mygame` | `mygame-web` |
| Tests for `mygame` | `mygame-test` |
| Service logs for `mygame` | `mygame-log` |
| A second test task | `mygame-test-2` |

VoiceTerm matches the project and purpose to the session name, and asks instead of guessing when the target is ambiguous.

## Safety model

- Routine inspection can proceed after the target session is selected.
- Project changes, process control, tests, and dependency installation require a clear voice confirmation.
- Destructive actions, Git history changes, pushes, deployments, account changes, and sensitive-data access require specific confirmation for each action.
- Passwords, one-time codes, recovery codes, and API keys must always be entered by the user.
- Agent and operating-system permission prompts remain under the user's control.
- Do not grant broad permission by command prefix. Authorize effects instead, such as project-only reads, public fetches from named domains, or a project's test command.

## Repository layout

```text
skills/voice-term/   The installable VoiceTerm Skill
README.md            English guide
README.zh-CN.md      简体中文指南
```

## Status

This is an early, source-install release. A packaged plugin distribution can be added after the workflow has been validated across more terminals and platforms.

## License

MIT
