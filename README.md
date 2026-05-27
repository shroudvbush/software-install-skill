简体中文 | [English](./README.en.md)

# Software Install Skill

> 在 Windows、Linux 和 macOS 上安装、配置、升级和排查软件的通用工作流。

一个将 AI 智能体从"猜测并运行"的安装者转变为有条理、安全优先的部署工程师的综合技能。

## 解决的问题

AI 智能体安装软件时常犯的错误：

- **猜测安装步骤**，不查阅官方文档
- **忽略环境差异**，不考虑不同平台的区别
- **做出不可逆的更改**，没有回滚方案
- **混用安装方式**（apt + snap + 源码），导致冲突
- **跳过验证** — "应该能用" 而不是证明能跑

## 解决方案

内置安全护栏的九阶段工作流：

| 阶段 | 目的 |
|------|------|
| **1. 识别** | 明确软件、版本、平台、约束条件 |
| **2. 调研** | 官方文档优先，社区资源辅助 |
| **3. 审查** | 变更前对环境进行只读检查 |
| **4. 计划** | 安装方案与回滚方案成对制定 |
| **5. 安装** | 最小权限，仅使用官方方法 |
| **6. 验证** | 用具体检查证明安装成功 |
| **7. 排查** | 结构化错误分析与修复 |
| **8. 清理** | 出问题时精确回滚 |
| **9. 报告** | 记录做了什么以及如何撤销 |

## 核心原则

- **不猜测** — 首先查阅官方文档
- **最小权限** — 优先用户级安装而非系统级
- **先读取后修改** — 变更前审查环境
- **每步操作都有撤销** — 每个安装步骤配备对应的回滚操作
- **永不关闭 TLS** — 正确修复网络问题
- **验证而非假设** — 在宣称成功之前运行 `command -v` 和 `--version`

## 安装

### 方式 A：Claude Code 插件（推荐）

在 Claude Code 中：

```bash
/plugin marketplace add shroudvbush/software-install-skill
/plugin install software-install-skill
```

### 方式 B：CLAUDE.md（按项目）

新项目：

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/shroudvbush/software-install-skill/main/CLAUDE.md
```

已有项目（追加）：

```bash
echo "" >> CLAUDE.md
curl https://raw.githubusercontent.com/shroudvbush/software-install-skill/main/CLAUDE.md >> CLAUDE.md
```

### 方式 C：OpenClaw

克隆到 skills 目录：

```bash
# Linux/macOS
git clone https://github.com/shroudvbush/software-install-skill.git ~/.openclaw/skills/software-install

# Windows
git clone https://github.com/shroudvbush/software-install-skill.git %USERPROFILE%\.openclaw\skills\software-install
```

或者直接将 `SKILL.md` 放到你的 OpenClaw skills 目录中。

## 生效标志

当出现以下情况时，说明此技能正在生效：

- **优先查阅官方文档** — 而非随机博客文章
- **变更前审查环境** — 已验证操作系统、架构、已有安装
- **提供回滚方案** — 每个安装步骤都有撤销方法
- **不执行大范围系统升级** — 仅在明确批准时执行 `apt upgrade`
- **安装后验证** — 自动运行 `command -v` 和 `--version`
- **失败时清理** — 失败的安装不留残留

## 自定义

向已有设置中添加项目特定规则：

```markdown
## 项目特定安装规则

- 优先使用用户级 npm 安装（仅在必要时使用 `npm install --global`）
- 使用 nvm 管理 Node.js 版本
- Python 项目：优先使用 uv 而非 pip（当可用时）
```

## 平台支持

| 平台 | 状态 | 备注 |
|----------|:------:|-------|
| Windows（原生） | ✅ | PowerShell, CMD, winget, choco, scoop |
| Windows（WSL） | ✅ | Ubuntu, Debian 及其他 WSL 发行版 |
| Linux | ✅ | apt, dnf, pacman, snap, flatpak |
| macOS | ✅ | brew, port, 官方安装包 |

## 参与贡献

1. Fork 本仓库
2. 创建特性分支（`git checkout -b feature/amazing-improvement`）
3. 提交变更（`git commit -m '添加某个改进'`）
4. 推送到分支（`git push origin feature/amazing-improvement`）
5. 发起 Pull Request

## 致谢

本项目灵感来源于 [forrestchang](https://github.com/forrestchang) 的 [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills)，README 格式和插件系统设计参考了该项目的模式。

## 许可证

MIT
