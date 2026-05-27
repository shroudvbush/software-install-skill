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

### Option A: OpenClaw / QClaw (Recommended)

Clone or download to skills directory:

```bash
# Linux/macOS
git clone https://github.com/你的用户名/software-install.git ~/.qclaw/skills/software-install

# Windows
git clone https://github.com/你的用户名/software-install.git %USERPROFILE%\.qclaw\skills\software-install
