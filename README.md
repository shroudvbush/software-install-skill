# Software Install Skill

> Universal workflow for installing, configuring, upgrading, and troubleshooting software on Windows, Linux, and macOS.

A comprehensive skill that transforms AI agents from "guess and run" installers into methodical, safety-first deployment engineers.

## The Problems

When AI agents install software, they often:

- **Guess installation steps** without checking official docs
- **Overlook environment differences** between platforms
- **Make irreversible changes** without rollback plans
- **Mix installation methods** (apt + snap + source) causing conflicts
- **Skip verification** — "it should work" instead of proving it works

## The Solution

Nine-phase workflow with built-in safety guardrails:

| Phase | Purpose |
|-------|---------|
| **1. Identify** | Clarify software, version, platform, constraints |
| **2. Research** | Official docs first, community sources second |
| **3. Audit** | Read-only environment inspection before changes |
| **4. Plan** | Installation + rollback plan paired together |
| **5. Install** | Least privilege, official methods only |
| **6. Verify** | Prove it works with concrete checks |
| **7. Troubleshoot** | Structured error analysis and fix |
| **8. Cleanup** | Precise rollback when things go wrong |
| **9. Report** | Document what was done and how to undo it |

## Key Principles

- **Don't guess** — Check official documentation first
- **Least privilege** — User-level over system-wide when possible
- **Read-only before mutating** — Audit environment before changes
- **Every action has an undo** — Paired rollback for every install step
- **Never disable TLS** — Fix network issues properly
- **Verify, don't assume** — `command -v` and `--version` before declaring success

## Install

### Option A: Claude Code Plugin (Recommended)

From within Claude Code:

```bash
/plugin marketplace add shroudvbush/software-install-skill
/plugin install software-install-skill
```

### Option B: CLAUDE.md (Per-Project)

New project:

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/shroudvbush/software-install-skill/main/SKILL.md
```

Existing project (append):

```bash
echo "" >> CLAUDE.md
curl https://raw.githubusercontent.com/shroudvbush/software-install-skill/main/SKILL.md >> CLAUDE.md
```

### Option C: OpenClaw

Clone to skills directory:

```bash
# Linux/macOS
git clone https://github.com/shroudvbush/software-install-skill.git ~/.openclaw/skills/software-install

# Windows
git clone https://github.com/shroudvbush/software-install-skill.git %USERPROFILE%\.openclaw\skills\software-install
```

Or place `SKILL.md` directly in your OpenClaw skills directory.

## How to Know It's Working

This skill is working when you see:

- **Official documentation checked first** — Not random blog posts
- **Environment audited before changes** — OS, arch, existing installs verified
- **Rollback plan presented** — Every install step has an undo
- **No broad system upgrades** — `apt upgrade` only when explicitly approved
- **Verification after install** — `command -v` and `--version` run automatically
- **Cleanup on failure** — Failed installs leave no mess behind

## Customization

Add project-specific rules to your existing setup:

```markdown
## Project-Specific Installation Rules

- Prefer user-level npm installs (`npm install --global` only when necessary)
- Use nvm for Node.js version management
- Python projects: use uv instead of pip when available
```

## Platform Support

| Platform | Status | Notes |
|----------|:------:|-------|
| Windows (native) | ✅ | PowerShell, CMD, winget, choco, scoop |
| Windows (WSL) | ✅ | Ubuntu, Debian, and other WSL distros |
| Linux | ✅ | apt, dnf, pacman, snap, flatpak |
| macOS | ✅ | brew, port, official installers |

## Contributing

1. Fork this repository
2. Create your feature branch (`git checkout -b feature/amazing-improvement`)
3. Commit your changes (`git commit -m 'Add some amazing improvement'`)
4. Push to the branch (`git push origin feature/amazing-improvement`)
5. Open a Pull Request

## License

MIT
