# agent-project-standards

[中文](#chinese-version) | [English](#english-version)

---

<a id="chinese-version"></a>
## 中文版

面向 AI agent 和开发者的通用 GitHub 开源项目协作规范。

### 这是什么？

这是一套全面的、项目无关的协作规范，任何 AI 编码 agent（或人类开发者）在参与 GitHub 开源项目时都可以遵循。它覆盖了完整的协作生命周期：从搜索项目现有规范、编写代码、创建 PR、验证所属权，到发布 Release。

### 核心特性

- **规范优先** — 在任何代码修改前强制搜索并遵循项目自身的风格指南
- **语言自动检测** — 适配项目的协作语言，而非硬编码某一种
- **所属权验证** — 防止在他人仓库上误执行合并/发布操作
- **多生态系统支持** — 覆盖 JS/TS、Python、Rust、Go、Ruby、Java/Kotlin、Swift、C/C++ 等
- **10 种常见场景** — Fork 工作流、Hotfix、Monorepo、依赖更新、CI/CD 变更、发布新项目等
- **多 Shell 兼容** — 为 Bash、Zsh、Fish、PowerShell 提供正确的 CLI 多行文本处理
- **许可证指南** — 覆盖 MIT、Apache 2.0、BSD、GPL-3.0 等常见开源许可证

### 版本说明

本项目提供两个版本的规范文档：

| 版本 | 文件 | 说明 |
|------|------|------|
| 国际版（英文） | `SKILL.md` | 主版本，所有内容以英文编写，作为通用回退语言 |
| 中文版 | `SKILL.zh.md` | 完整中文翻译，面向中文用户和中文项目 |

Agent 在使用时会根据项目的协作语言自动选择合适的版本。如果项目以中文为主，优先加载 `SKILL.zh.md`；否则使用 `SKILL.md`（英文回退）。

### 章节概览

| # | 章节 | 描述 |
|---|------|------|
| 0 | 项目规范文件搜索 | 代码修改前的强制前置步骤 |
| 1 | GitHub 协作标准 | 语言规范 + 行为规范 |
| 2 | Shell CLI 多行文本处理 | 跨 Shell 的正确多行文本处理 |
| 3 | 发版流程 | 版本递增、PR、所属权核对、合并、发布 |
| 4 | 许可证指南 | 选择和编写开源许可证 |
| 5 | 常见开发场景 | Fork、Hotfix、Monorepo、依赖更新、CI/CD、发布新项目等 |
| 6 | Agent 配置参考 | 自动检测清单与项目画像模板 |

### 使用方法

#### 方式一：导入 Skill 压缩包（推荐）

从 [Releases 页面](../../releases/latest) 下载最新版本的 `.zip` 压缩包，然后导入你使用的 AI agent 工具：

- **国际版** — 下载 `project-standards-en.zip`，适用于英文项目
- **中文版** — 下载 `project-standards-zh.zip`，适用于中文项目

两个压缩包内的文件均已命名为 `SKILL.md`，可直接导入，无需手动重命名。导入后 agent 会自动遵循规范中的协作约定。

#### 方式二：引用源文件

克隆或下载本仓库，直接引用 `SKILL.md`（国际版/英文）或 `SKILL.zh.md`（中文版）作为 skill 或 prompt 上下文。

**面向 AI Agent** — 加载后 agent 会自动：

1. 在修改前搜索项目规范文件
2. 检测项目的协作语言和版本策略
3. 遵循 Conventional Commits 和最小变更原则
4. 在合并/发布前验证仓库所属权
5. 适配项目的代码风格、命名和工具链

**面向开发者** — 可作为参考指南，用于：

- 编写高质量的 PR 和 commit message
- 参与开源项目协作（尤其是 Fork 场景）
- 搭建新开源项目的规范体系
- 理解发版流程和许可证选择

#### 方式三：QoderWork

在 QoderWork 中导入 `dist/` 目录下的 `.zip` 包即可安装为 skill。安装后，在创建 PR、发布 Release 或修改代码时会自动调用本规范。

### 文件结构

```
agent-project-standards/
├── SKILL.md        # 核心规范文档（国际版/英文，所有章节）
├── SKILL.zh.md     # 核心规范文档（中文版，所有章节）
├── README.md       # 本文件
├── LICENSE         # MIT 许可证
├── .gitignore      # Git 忽略规则
└── dist/           # 可分发的 skill 压缩包
    ├── project-standards-en.zip   # 国际版（SKILL.md + LICENSE）
    └── project-standards-zh.zip   # 中文版（SKILL.md + LICENSE，已重命名）
```

> **提示：** 中文版规范请参阅 `SKILL.zh.md`，国际版/英文版规范请参阅 `SKILL.md`。`dist/` 目录中的 `.zip` 文件可直接导入 agent 工具使用。

### 贡献

欢迎贡献！请遵循以下步骤：

1. Fork 本仓库
2. 创建功能分支（`feat/描述`）
3. 使用 Conventional Commits 格式编写 commit message
4. 创建 PR 并附上清晰的变更描述

### 许可证

MIT 许可证 — 详见 [LICENSE](LICENSE) 文件。

---

<a id="english-version"></a>
## English Version

A universal conventions guide for AI agents and developers collaborating on GitHub open-source projects.

### What Is This?

This is a comprehensive, project-agnostic set of standards that any AI coding agent (or human developer) can follow when working on GitHub open-source projects. It covers the full collaboration lifecycle: from searching for existing project conventions, to writing code, creating PRs, verifying ownership, and publishing releases.

### Key Features

- **Convention-first** — Mandates searching for and following the project's own style guides before any code change
- **Language auto-detection** — Adapts to the project's collaboration language instead of hardcoding one
- **Ownership verification** — Prevents accidental merges/releases on repositories you don't own
- **Multi-ecosystem support** — Covers JS/TS, Python, Rust, Go, Ruby, Java/Kotlin, Swift, C/C++ and more
- **10 common scenarios** — Fork workflows, hotfixes, monorepos, dependency updates, CI/CD changes, publishing new projects, and more
- **Multi-shell compatible** — Provides correct CLI text handling for Bash, Zsh, Fish, and PowerShell
- **License guide** — Covers MIT, Apache 2.0, BSD, GPL-3.0 and other common open-source licenses

### Version Information

This project provides two versions of the standards document:

| Version | File | Description |
|---------|------|-------------|
| International (English) | `SKILL.md` | Primary version, all content in English, serves as the universal fallback |
| Chinese | `SKILL.zh.md` | Complete Chinese translation, for Chinese-speaking users and projects |

The agent automatically selects the appropriate version based on the project's collaboration language. For Chinese-language projects, `SKILL.zh.md` is preferred; otherwise `SKILL.md` (English fallback) is used.

### Sections Overview

| # | Section | Description |
|---|---------|-------------|
| 0 | Convention File Search | Mandatory first step before any code change |
| 1 | GitHub Collaboration Standards | Language specs + behavior specs |
| 2 | Shell CLI Multi-line Text Handling | Correct multi-line text across shells |
| 3 | Release Workflow | Version bump, PR, ownership check, merge, release |
| 4 | License Guide | Choosing and writing open-source licenses |
| 5 | Common Development Scenarios | Fork, hotfix, monorepo, deps, CI/CD, new project, etc. |
| 6 | Agent Configuration Reference | Auto-detection checklist and project profile template |

### Usage

#### Option 1: Import Skill Archive (Recommended)

Download the latest `.zip` archive from the [Releases page](../../releases/latest), then import it into your AI agent tool:

- **International edition** — download `project-standards-en.zip`, for English-language projects
- **Chinese edition** — download `project-standards-zh.zip`, for Chinese-language projects

Both archives contain files already named `SKILL.md` — no renaming needed. Once imported, the agent will automatically follow the collaboration conventions defined in the skill.

#### Option 2: Reference Source Files

Clone or download this repository, then reference `SKILL.md` (international/English) or `SKILL.zh.md` (Chinese) directly as a skill or prompt context.

**For AI Agents** — once loaded, the agent will automatically:

1. Search for project convention files before making changes
2. Detect the project's collaboration language and versioning strategy
3. Follow Conventional Commits and minimal change principles
4. Verify repository ownership before merge/release
5. Adapt to the project's code style, naming, and tooling

**For Developers** — use as a reference guide for:

- Writing good PRs and commit messages
- Contributing to open-source projects (especially forks)
- Setting up a new open-source project's conventions
- Understanding release workflows and license choices

#### Option 3: QoderWork

Import the `.zip` archive from the `dist/` directory in QoderWork to install as a skill. Once installed, the conventions will be automatically invoked when creating PRs, publishing releases, or modifying code.

### File Structure

```
agent-project-standards/
├── SKILL.md        # Main conventions document (international/English, all sections)
├── SKILL.zh.md     # Main conventions document (Chinese version, all sections)
├── README.md       # This file
├── LICENSE         # MIT License
├── .gitignore      # Git ignore rules
└── dist/           # Distributable skill archives
    ├── project-standards-en.zip   # International edition (SKILL.md + LICENSE)
    └── project-standards-zh.zip   # Chinese edition (SKILL.md + LICENSE, renamed)
```

> **Note:** For the Chinese version of the standards, see `SKILL.zh.md`. For the international/English version, see `SKILL.md`. The `.zip` files in `dist/` can be directly imported into agent tools.

### Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`feat/description`)
3. Follow Conventional Commits format for commit messages
4. Create a PR with a clear description of changes

### License

MIT License — see [LICENSE](LICENSE) for details.
