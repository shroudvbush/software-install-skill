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

## Read-Only vs Mutating Commands

Read-only commands (`uname`, `which`, `--version`, `env`, `echo`, package query) may be suggested or run without confirmation.

Mutating commands (`install`, `uninstall`, `update`, `remove`, `write`, `chmod`, `chown`, `service enable/disable`, profile edits) require explicit explanation and confirmation before execution.

## Safety Rules

- Use the least privilege required. Prefer user-level installation when possible.
- Require explicit user confirmation before: uninstalling, overwriting config files, changing system-wide PATH, adding/removing repos, enabling/disabling services, installing drivers/kernel modules/databases, running installer scripts with elevated privileges, modifying shell startup files.
- Before modifying an existing config file, create a timestamped backup. Prefer appending clearly marked blocks over editing unrelated content.
- Do not invent version numbers, compatibility claims, URLs, checksums, or package names.
- Do not run broad system upgrades (`apt upgrade`, `brew upgrade`, `dnf upgrade`) unless explicitly approved.
- Do not mix installation methods (apt + snap + source) for the same software.
- Never disable TLS/SSL verification as a workaround.
- For remote installer scripts, prefer download-review-run over pipe-to-shell.

## Platform-Specific Rules

On Windows, distinguish native Windows, WSL, Git Bash, PowerShell, and CMD. Prefer user-level PATH changes. Avoid mixing winget, Chocolatey, and Scoop.

On Linux/macOS, distinguish user-level vs system-level installations. Use `~/.local/bin`, `~/.local/lib`, or version-manager paths when appropriate.

## Runtime and SDK Version Managers

For Node.js: nvm, fnm, Volta, asdf. For Python: pyenv, conda, uv. For Java: SDKMAN!, jenv, asdf. For Ruby: rbenv, RVM, asdf. For Go: official installer, package manager, asdf.

Avoid mixing package-manager installs with version-manager installs.
