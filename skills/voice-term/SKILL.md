---
name: voice-term
description: Guide voice-driven collaboration with a user's local tmux project session from an AI coding agent. Use when the user asks to set up, inspect, run commands in, or share a terminal session through voice, especially with iTerm2, Ghostty, PowerShell, or multiple active projects.
---

# VoiceTerm

Use tmux as the shared shell behind a voice conversation. Treat the session's starting project directory as the working scope, not as an operating-system sandbox.

## First use or a new terminal

1. Identify the operating system and check whether `tmux` is available.
2. If tmux is absent, explain the appropriate installation route and ask before installing:
   - macOS: Homebrew.
   - Linux: the distribution's package manager.
   - Windows: use tmux inside WSL. Native Windows PowerShell alone is not a tmux environment; PowerShell is fine inside macOS, Linux, or WSL.
3. Ask the user to open iTerm2, Ghostty, or another terminal emulator in the intended project root. Never recommend the filesystem root or the user's home directory.
4. Ask only for missing details: a short project name and this terminal's purpose.
5. Derive the session name using the rule below. Check existing sessions before creating one.
6. Show the exact command and have the user run it. For a new session, use `tmux new -s <session-name>`; if it already exists, use `tmux attach -t <session-name>`.
7. Confirm that the session exists and record the selected name for the current conversation.

## Name sessions for voice

Use `project-purpose` in lowercase hyphen-case, such as `hsk3-web`, `hsk3-api`, `hsk3-test`, `hsk3-log`, or `hsk3-fix`.

- Use the project name as the shared prefix for all terminals in that project.
- Use a short meaningful purpose word, not a timestamp, to distinguish parallel work.
- If the same project and purpose need another independent session, append a short number, such as `hsk3-test-2`.
- One terminal tab normally attaches to one named tmux session. Multiple tabs may attach to the same session only when they intentionally share that exact task context.

## Select the target

1. List active tmux sessions and, when terminal access is available, obtain each session's current pane directory.
2. Match the user's project and purpose to a session name; use the directory as a fallback check.
3. If no exact session name exists, offer a new short purpose label rather than guessing.
4. If more than one session could match, ask the user to choose before reading or writing.
5. Stay with the selected session until the user explicitly selects another one. Do not claim a session mapping persists across separate Codex tasks; rediscover it from tmux when needed.

## Operate the selected session

1. Read the recent terminal output first and summarize only the relevant result.
2. Before sending a command, state its purpose in plain language.
3. Use the selected tmux session only. Do not create, attach to, or operate other sessions without the user's request.
4. After a command, read output again and report the verified result.
5. If the session directory is outside the intended project directory, stop and ask the user to confirm the target.

## Permissions and confirmation

Apply these four levels. Do not broaden a user's approval from one level to another.

1. **Read-only, routine**: inspect the selected session, current directory, logs, process status, and project status. These may proceed after the target session is selected.
2. **Project changes**: edit project files, start or stop project processes, run tests, or install project dependencies. State the intended effect and ask for a clear user confirmation first. Never add a production dependency without explicit confirmation.
3. **High-impact actions**: delete or overwrite material data, change branches or history, publish or push changes, deploy, access secrets, alter accounts, or perform system-wide actions. Ask immediately before each specific action and name its consequence.
4. **Credentials and approval prompts**: never type, request, read, copy, or infer passwords, one-time codes, recovery codes, or API keys. Never answer a terminal `yes/no` prompt or a Codex permission prompt without the user's clear, current confirmation.

When the coding agent itself displays a tool or terminal permission prompt, pause and explain what the approval enables. The user must approve or reject it in the agent interface; VoiceTerm cannot bypass it. When a command running inside tmux displays an interactive `yes/no` prompt, show its effect, ask the user verbally, and send a response only after an unambiguous confirmation. For password or multi-factor prompts, ask the user to enter the value directly in their terminal instead.

## Voice approval profiles

Keep ordinary workflow approvals in the voice conversation. A clear verbal confirmation can authorize a narrowly defined class of actions for the current Codex task and the selected tmux session, until the user revokes it or changes the target session.

Record the scope in plain language before applying it. Prefer effect-based profiles over command-name prefixes:

- **Project read**: read ordinary text files and status inside the selected project. Exclude secret files, credentials, private keys, and environment files.
- **Public fetch**: fetch from named public domains without credentials, request bodies, uploads, or piping output into a shell.
- **Project test**: run a named project's test command and report the result.

Do not treat a blanket approval for `curl`, `cat`, or any other command prefix as safe: the same command can access secrets, transmit data, overwrite files, or execute downloaded code. Narrow the approval by project, effect, and destination instead. Never use an approval profile to accept a Codex/OS permission dialog, reveal credentials, approve destructive actions, or cross into another tmux session.

## Limits

- A tmux session provides shared terminal context, not filesystem confinement.
- VoiceTerm standardizes agent behavior; it cannot bypass agent, operating-system, or terminal permissions.
- Connection and command permissions may still prompt the user, and must not be bypassed.
