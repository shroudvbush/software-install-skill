# Software Installation

Use this skill whenever the user asks to install, configure, upgrade, uninstall, repair, or troubleshoot software.

Prioritize correctness, reproducibility, and official documentation over speed.

## Core Rule

**Do not guess installation steps.**

Before changing the system:

1. Check the official documentation.
2. Check the target machine environment.
3. Compare the machine against official requirements.
4. Choose the official or recommended installation path.
5. Verify the result.

Community posts (GitHub Issues, Stack Overflow, Zhihu, CSDN, blogs, forums) are supplementary references only. Do not treat them as authoritative when they conflict with official documentation.

If web access is unavailable, say so and ask the user to provide the official documentation link, error logs, or environment details.

## Operating Modes

If the user only asks for guidance and no system access is available, do not assume the environment. Ask the user to run read-only checks first and paste the output.

If the assistant has tool access to the target machine, audit the environment directly before making changes.

## Read-Only vs Mutating Commands

Read-only commands (`uname`, `which`, `--version`, `env`, `echo`, package query) may be suggested or run without confirmation.

Mutating commands (`install`, `uninstall`, `update`, `remove`, `write`, `chmod`, `chown`, `service enable/disable`, profile edits) require explicit explanation and confirmation before execution.

## Safety and Permission Rules

Use the least privilege required. Do not use sudo/admin privileges unless necessary. Prefer user-level installation when appropriate.

Require explicit user confirmation before:

- uninstalling existing software
- overwriting configuration files
- changing system-wide PATH or environment variables
- adding/removing package repositories
- enabling/disabling services
- installing drivers, kernel modules, database servers, or background daemons
- running installer scripts with elevated privileges
- modifying shell startup files

Before modifying an existing config file, create a timestamped backup or propose a reversible patch. Prefer appending clearly marked blocks over editing unrelated content.

Do not invent version numbers, compatibility claims, URLs, checksums, or package names. If uncertain, ask.

## Additional Safety Defaults

- Do not run broad system upgrades (`apt upgrade`, `brew upgrade`, `dnf upgrade`, `pacman -Syu`) unless required by official documentation and explicitly approved.
- Package index refresh (`apt update`, `brew update`) is low-risk but still mutating — explain before use.
- Do not mix installation methods (e.g. apt + snap + source) for the same software without explaining conflicts.
- If the target version is unspecified, ask: latest stable, LTS, project-required, or exact pinned version.
- For official remote installer scripts, prefer download-review-run over pipe-to-shell.
- Never disable TLS/SSL verification as a workaround.
- For network failures: check proxy, corporate TLS interception, registry mirror, DNS — not `strict-ssl false`.

## Platform-Specific Rules

On Windows, distinguish native Windows, WSL, Git Bash, PowerShell, and CMD environments. Prefer user-level PATH changes over machine-level PATH changes. Avoid mixing winget, Chocolatey, and Scoop for the same software.

On Linux/macOS, distinguish user-level vs system-level installations. Use `~/.local/bin`, `~/.local/lib`, or version-manager paths when appropriate.

## Runtime and SDK Version Managers

For runtimes and SDKs with multiple version managers, identify whether the user needs a global system install, per-user install, or project-scoped version.

Avoid mixing package-manager installs with version-manager installs unless the user understands the tradeoffs.

Examples: Node.js (nvm, fnm, Volta, asdf), Python (pyenv, conda, uv), Java (SDKMAN!, jenv, asdf), Ruby (rbenv, RVM, asdf), Go (official installer, package manager, asdf).

## Workflow

### 1. Identify the Request

Clarify: software name, target version, OS, architecture, shell, install scope, constraints, existing installations.

### 2. Research Official Sources First

Check official docs before giving any command. Then check community sources for known issues. If multiple official methods exist, present tradeoffs.

### 3. Audit the Local Environment

Inspect the environment with safe read-only commands before modifying anything.

### 4. Plan Installation with Rollback

Every action must have a paired undo action. For high-risk installs, recommend a system snapshot.

### 5. Install Using the Recommended Method

Least privilege. Official methods first. Prefer: official package repo → official installer → official binary archive → source build.

### 6. Verify After Installation

Minimum checks: executable found, version correct, basic command works.

### 7. Troubleshoot Failures

Collect error info, check official troubleshooting docs, apply one fix at a time, verify, retry. Don't repeatedly retry the same command.

### 8. Cleanup & Rollback

When installation fails, restore the environment. Only remove what this install session created.

### 9. Final Report

Document: software, version, platform, install method, install path, config files, verification result.
