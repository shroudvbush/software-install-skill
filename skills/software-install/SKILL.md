---
name: software-install
description: Universal workflow for installing, configuring, upgrading, and troubleshooting software, tools, SDKs, runtimes, package managers, and CLIs on Windows, Linux, and macOS.
license: MIT
---

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

If the user provides official documentation text or a trusted official link but web access is unavailable, use the provided material as the basis and clearly state that the plan is based on user-supplied official documentation.

## Operating Modes

If the user only asks for guidance and no system access is available, do not assume the environment. Ask the user to run read-only checks first and paste the output. Provide platform-specific command blocks for Windows, macOS, and Linux when the platform is unknown. Wait for user output before giving modifying commands.

If the assistant has tool access to the target machine, audit the environment directly before making changes.

If web access is unavailable, state the limitation. If the user provides official documentation text or a trusted official link, use it as the basis and clearly state that the plan is based on user-provided official material.

## Read-Only vs Mutating Commands

Read-only commands (`uname`, `which`, `--version`, `env`, `echo`, package query) may be suggested or run without confirmation.

Mutating commands (`install`, `uninstall`, `update`, `remove`, `write`, `chmod`, `chown`, `service enable/disable`, profile edits) require explicit explanation and confirmation before execution.

## Safety and Permission Rules

Use the least privilege required. Do not use sudo/admin privileges unless necessary. Prefer user-level installation when appropriate and supported by official documentation.

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

Do not invent version numbers, compatibility claims, URLs, checksums, or package names. If uncertain, ask for confirmation or official references.

## Additional Safety Defaults

- Do not run broad system upgrades (`apt upgrade`, `brew upgrade`, `dnf upgrade`, `pacman -Syu`) unless required by official documentation and explicitly approved.
- Package index refresh (`apt update`, `brew update`) is low-risk but still mutating/network. Explain before use.
- Do not mix installation methods (e.g. apt + snap + source) for the same software without explaining conflicts.
- If the target version is unspecified, ask: latest stable, LTS, project-required, or exact pinned version.
- For official remote installer scripts, prefer download-review-run over pipe-to-shell. Show source URL and checksum/signature verification if available.
- Never disable TLS/SSL verification as a workaround.
- For network failures: check proxy, corporate TLS interception, registry mirror, DNS — not `strict-ssl false`.

## Platform-Specific Rules

On Windows, distinguish native Windows, WSL, Git Bash, PowerShell, and CMD environments. Do not assume commands are interchangeable across them. Prefer user-level PATH changes over machine-level PATH changes. Avoid mixing winget, Chocolatey, and Scoop for the same software unless the user explicitly chooses.

On Linux/macOS, distinguish user-level vs system-level installations. Use `~/.local/bin`, `~/.local/lib`, or version-manager paths when appropriate.

## Runtime and SDK Version Managers

For runtimes and SDKs with multiple version managers, identify whether the user needs a global system install, per-user install, or project-scoped version.

Avoid mixing package-manager installs with version-manager installs unless the user understands the tradeoffs.

Examples include:

- Node.js: nvm, fnm, Volta, asdf
- Python: pyenv, conda, uv
- Java: SDKMAN!, jenv, asdf
- Ruby: rbenv, RVM, asdf
- Go: official installer, package manager, asdf

## 1. Identify the Request

Clarify if needed: software name, target version, OS, architecture, shell, install scope (user/system), custom path, proxy/mirror/offline restrictions, existing installations.

For non-trivial installs, summarize:

```text
Software:
Target version:
Platform:
Install scope:
Constraints:
```

## 2. Research Official Sources First

Check official sources before giving any command:

- Official installation guide, README, release notes
- Version compatibility matrix, system requirements
- Platform-specific notes, official troubleshooting guide

Then check community sources for known issues:

- GitHub Issues / Discussions
- Stack Overflow, vendor forums
- Zhihu, CSDN, blogs

Summarize findings:

```text
Software:
Target version:
Platform:
Official recommended method:
Alternative methods:
Prerequisites:
Known platform issues:
Recommended approach:
References:
```

If multiple official methods exist, present tradeoffs instead of silently choosing.

## 3. Audit the Local Environment

Before modifying the system, inspect the environment with safe read-only commands.

Check: OS, architecture, shell, package managers, existing installation, PATH, dependencies, permissions, proxy/network, relevant environment variables.

For Windows, also check: Windows version and edition, CPU architecture, PowerShell version, administrator privileges, winget/choco/scoop availability, execution policy, PATH location (user vs system), existing installation in Apps & Features or package manager.

Comparison format:

| Requirement | Required by official docs | Current machine | Status |
|-------------|--------------------------|-----------------|--------|
| OS | … | … | ✅/❌ |
| Architecture | … | … | ✅/❌ |
| Dependency | … | … | ✅/❌ |
| Existing install | … | … | ✅/⚠️ |

If a hard requirement is not met, stop and ask how to proceed.

Do not remove existing versions, overwrite config files, or make system-wide changes without confirmation.

## 4. Plan Installation with Rollback

**Before executing, generate both an install plan and a rollback plan.**

Every action must have a paired undo action:

| Action | Rollback |
|--------|----------|
| `apt install pkg` | `apt remove pkg` |
| `curl -o /path/file URL` | `rm /path/file` |
| extract archive to dir | remove dir |
| add env var to config | remove that line |
| `systemctl enable svc` | `systemctl disable svc && systemctl stop svc` |
| add apt repo source | remove `.list` file and GPG key |
| `brew install pkg` | `brew uninstall pkg` |
| `winget install app` | `winget uninstall app` |
| `npm install -g pkg` | `npm uninstall -g pkg` |

For high-risk installs (services, kernel modules, GPU drivers, databases, multi-version environments), recommend a system snapshot before proceeding.

Record what is created/modified during installation so cleanup is precise, not guesswork.

## 5. Install Using the Recommended Method

Use the least privilege required. Do not use sudo/admin privileges unless necessary. Prefer user-level installation when appropriate and supported by official documentation.

Follow the official method unless there is a clear reason not to.

Before running commands that modify the system, explain: what it does, why it is needed, what it changes, risk level.

Prefer: official package repository → official installer → official binary archive → source build.

Avoid: unofficial mirrors unless required, random blog commands, blindly overwriting PATH or configs, printing secrets, destructive cleanup without confirmation.

Do not pipe remote scripts into a shell unless the official docs require it, the URL is verified, and the user explicitly accepts the risk.

## 6. Verify After Installation

Prove the install works.

Minimum checks: executable found (in expected location), version correct, basic command works, required config/env loaded.

Never print secret values. Use presence checks:

```bash
command -v <tool>
<tool> --version
test -n "$API_KEY" && echo "API_KEY is set"
```

## 7. Troubleshoot Failures

When an error occurs, collect:

```text
Command:
Error message:
Exit code:
Phase:
Recent changes:
```

Then:

1. Re-check official troubleshooting docs.
2. Search exact error in official issues or vendor forums.
3. Search community sources (GitHub Issues, Stack Overflow, Zhihu, CSDN).
4. Compare error against the local environment.
5. Identify the smallest likely fix.
6. Apply one fix at a time.
7. Verify.
8. Retry.

Common causes: wrong OS/architecture, unsupported version, missing dependency, PATH not updated, permission issue, proxy/network failure, conflicting old installation, wrong shell profile, SSL/certificate problem, incompatible runtime.

Do not repeatedly retry the same command without changing anything.

## 8. Cleanup & Rollback

**When installation fails, user cancels, or decides not to proceed, restore the environment.**

Work in order:

1. Stop any installation processes.
2. Uninstall packages installed this session (use package manager).
3. Remove added repositories, GPG keys, sources.
4. Remove files and directories created this session.
5. Remove temporary and cache files.
6. Revert PATH and environment variable changes.
7. Disable and stop services added this session.

Rules:

- Only remove what this install session created. Never delete pre-existing files.
- Never clear system directories (`/usr`, `/etc`, `/var`) indiscriminately.
- Never delete user data directories unless user explicitly confirms.
- If full recovery is impossible (source builds without `make uninstall`, third-party scripts with unknown side effects, shared dependency upgrades), state exactly what remains and why.
- Verify cleanup: `command -v <tool>` should return nothing, install paths should be gone.

## 9. Final Report

Document the result:

```text
Software:
Version:
Platform:
Install method:
Install path:
Config files:
Environment variables:
Workarounds:
Verification result:
Official references:
Community references:
```

If installation cannot be completed, report plus cleanup:

```text
Blocker:
Evidence:
Likely cause:
Attempts made:
Cleanup performed:
Residual items (if any):
Recommended next step:
Information needed from user:
```
