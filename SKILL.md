---
name: project-standards
description: "ALWAYS invoke for ANY task involving code development or open-source projects. Covers: writing/modifying/reviewing code, git operations (branch, commit, merge, rebase, tag), GitHub workflows (PR, Issue, Release, fork, code review), version bumping, changelog, CI/CD, dependency management, license management, project setup, code style/formatting, commit messages, monorepo, hotfix, and any interaction with a git repository. This is the universal conventions guide for AI agents working on software projects — if the task touches code or a repo, use this skill. 凡是涉及代码开发或开源项目的任务都必须唤起此技能。"
---

# 开源项目协作规范 / Open Source Project Standards

面向 AI 代理和开发者的通用 GitHub 开源项目协作规范。所有规则均与具体项目无关——代理必须首先检测并遵循各项目自身的约定。

A universal conventions guide for AI agents and developers collaborating on GitHub open-source projects. All rules are project-agnostic — the agent must detect and follow each project's own conventions first.

涵盖七个核心部分 / Covers seven core sections:

1. [项目约定文件搜索（必须的第一步） / Project Convention File Search (Mandatory First Step)](#project-convention-file-search-mandatory-first-step)
2. [GitHub 协作标准 / GitHub Collaboration Standards](#github-collaboration-standards)（语言规范 + 行为规范 / Language Specs + Behavior Specs）
3. [Shell CLI 多行文本处理 / Shell CLI Multi-line Text Handling](#shell-cli-multi-line-text-handling)
4. [含所有权验证的发布工作流 / Release Workflow with Ownership Verification](#release-workflow-with-ownership-verification)
5. [开源许可证指南 / Open-Source License Guide](#open-source-license-guide)
6. [常见开发场景 / Common Development Scenarios](#common-development-scenarios)（10 个场景，包括发布新项目 / 10 scenarios including publishing new projects）
7. [代理配置参考 / Agent Configuration Reference](#agent-configuration-reference)

---

# 项目约定文件搜索（必须的第一步） / Project Convention File Search (Mandatory First Step)

> **强制要求：在对项目进行任何代码变更之前，代理必须执行本节描述的约定文件搜索。此步骤不可跳过。**
>
> **MANDATORY: Before making ANY code change in a project, the agent MUST perform the convention file search described in this section. This step is never skippable.**

## 搜索流程 / Search Procedure

### 步骤 1：搜索约定文件 / Step 1: Search for Convention Files

在修改任何项目之前，主动搜索以下约定/风格文件：

Before modifying any project, proactively search for these convention/style files:

| 优先级 / Priority | 文件名 / File Name | 用途 / Purpose |
|----------|-----------|---------|
| HIGH | `STYLE_GUIDE.md` / `STYLEGUIDE.md` / `style-guide.md` | 项目风格指南 / Project style guide |
| HIGH | `.editorconfig` | 编辑器格式配置（缩进、换行、编码） / Editor format config (indent, EOL, encoding) |
| HIGH | `.eslintrc*` / `biome.json` / `.prettierrc*` | JS/TS 格式化 / JS/TS formatting |
| HIGH | `pyproject.toml` / `setup.cfg` / `.flake8` / `tox.ini` | Python 格式化 / Python formatting |
| HIGH | `rustfmt.toml` / `rust-toolchain.toml` | Rust 格式化 / Rust formatting |
| HIGH | `.rubocop.yml` / `Gemfile` (Ruby) | Ruby 格式化 / Ruby formatting |
| HIGH | `golangci.yml` / `.golangci.yaml` | Go 代码检查 / Go linting |
| HIGH | `checkstyle.xml` / `spotless.gradle` (Java/Kotlin) | Java/Kotlin 格式化 / Java/Kotlin formatting |
| HIGH | `.swiftlint.yml` | Swift 格式化 / Swift formatting |
| HIGH | `.clang-format` / `.clang-tidy` | C/C++ 格式化 / C/C++ formatting |
| MEDIUM | `CONTRIBUTING.md` / `CONTRIBUTING` | 贡献指南 / Contribution guidelines |
| MEDIUM | `CODE_OF_CONDUCT.md` | 行为准则 / Code of conduct |
| MEDIUM | `.github/CODEOWNERS` | 代码审查负责人 / Code review owners |
| MEDIUM | `.github/ISSUE_TEMPLATE/*` / `.github/PULL_REQUEST_TEMPLATE*` | Issue/PR 模板 / Issue/PR templates |
| MEDIUM | `ARCHITECTURE.md` / `DESIGN.md` | 架构决策 / Architecture decisions |
| LOW | `README.md` 中的约定段落 / convention paragraphs | README 中的约定信息 / Convention info in README |

### 步骤 2：搜索命令 / Step 2: Search Commands

**Bash / Zsh：**

```bash
find . -maxdepth 3 \( \
  -name "STYLE_GUIDE*" -o -name "STYLEGUIDE*" -o -name ".editorconfig" \
  -o -name "CONTRIBUTING*" -o -name "CODE_OF_CONDUCT*" \
  -o -name ".prettierrc*" -o -name ".eslintrc*" -o -name "biome.json" \
  -o -name "pyproject.toml" -o -name "rustfmt.toml" -o -name ".rubocop.yml" \
  -o -name "golangci.yml" -o -name ".golangci*" -o -name "checkstyle.xml" \
  -o -name ".clang-format" -o -name ".swiftlint.yml" \
  -o -name ".flake8" -o -name "setup.cfg" -o -name "ARCHITECTURE.md" \
\) 2>/dev/null

ls -la .github/ 2>/dev/null
```

**PowerShell：**

```powershell
Get-ChildItem -Path . -Recurse -Depth 3 -Include `
  "STYLE_GUIDE*","STYLEGUIDE*",".editorconfig","CONTRIBUTING*","CODE_OF_CONDUCT*",
  ".prettierrc*",".eslintrc*","biome.json","pyproject.toml","rustfmt.toml",
  ".rubocop.yml","golangci.yml",".golangci*","checkstyle.xml",
  ".clang-format",".swiftlint.yml",".flake8","setup.cfg","ARCHITECTURE.md" `
  -ErrorAction SilentlyContinue
```

### 步骤 3：阅读并遵守 / Step 3: Read and Comply

- 如果找到文件 → **必须阅读其内容并严格遵循格式要求** / If files found → **MUST read their contents and strictly follow the format requirements**
- 多个约定文件存在冲突 → 优先级更高的胜出（自定义项目指南 > 工具配置 > 通用模板） / Multiple conflicting convention files → Higher priority wins (custom project guide > tool config > generic template)
- 未找到约定文件 → 遵循本文档的默认值 / No convention files found → Follow this document's defaults

### 优先级规则 / Priority Rule

```
# 项目自身的约定文件 > 本文档的默认值 > 通用行业惯例
# Project's own convention files > This document's defaults > General industry conventions
Project's own convention files > This document's defaults > General industry conventions
```

### Monorepo 感知 / Monorepo Awareness

在 Monorepo 项目（包含多个包的工作区）中，需在以下位置搜索：

In monorepo projects (workspaces with multiple packages), search at:

- 仓库根目录 / Repository root
- 每个包/工作区目录 / Each package/workspace directory
- 正在修改的具体包 / The specific package being modified

最具体（最接近被修改代码）的约定优先。

The most specific (closest to the code being changed) convention takes precedence.

---

# GitHub 协作标准 / GitHub Collaboration Standards

本节定义了在 GitHub 开源项目中协作的语言和行为规范。这些规范涵盖整个协作生命周期：代码编写、PR 提交、Issue 讨论和版本发布。

This section defines the language and behavior standards for collaborating on GitHub open-source projects. These standards span the entire collaboration lifecycle: code writing, PR submission, issue discussion, and release publishing.

---

## 第一部分：语言规范 / Part I: Language Specs

### 1.1 协作语言检测 / Collaboration Language Detection

**代理在生成任何文本输出之前，必须自动检测项目的协作语言。** 检测优先级：

**The agent MUST auto-detect the project's collaboration language before generating any text output.** Detection priority:

1. **CONTRIBUTING.md / STYLE_GUIDE.md** 明确指定了语言 → 使用该语言 / explicitly specifies a language → Use that language
2. **README.md** 的主要语言 → 使用该语言编写协作文本 / primary language → Use that language for collaboration text
3. **现有 PR/Issue/提交历史** 的主要语言 → 使用该语言 / dominant language → Use that language
4. **用户指令** → 如果用户明确要求使用特定语言，则使用该语言 / If the user explicitly requests a specific language, use it
5. **兜底方案** → 英语 / **Fallback** → English

检测到的语言适用于：PR 标题和描述、提交消息、Issue 标题和描述、发布说明、代码审查评论以及所有协作者沟通。

The detected language applies to: PR titles and descriptions, commit messages, issue titles and descriptions, release notes, code review comments, and all collaborator communication.

### 1.2 提交消息格式 / Commit Message Format

遵循 [Conventional Commits（约定式提交）](https://www.conventionalcommits.org/)：

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): short description

[optional body]
```

**类型 / Types:**

| 类型 / Type | 含义 / Meaning |
|------|---------|
| `feat` | 新功能 / New feature |
| `fix` | 缺陷修复 / Bug fix |
| `docs` | 文档变更 / Documentation changes |
| `chore` | 构建 / 工具链 / 依赖 / Build / tooling / deps |
| `refactor` | 重构（无行为变更） / Restructure (no behavior change) |
| `test` | 测试相关变更 / Test-related changes |
| `style` | 格式化（无逻辑变更） / Formatting (no logic change) |
| `perf` | 性能优化 / Performance optimization |
| `ci` | CI/CD 配置变更 / CI/CD configuration changes |
| `build` | 构建系统或外部依赖 / Build system or external dependencies |
| `revert` | 回退之前的提交 / Revert a previous commit |

**作用域（可选）：** 标识受影响的区域，例如 / **Scope (optional):** Identifies the affected area, e.g., `feat(auth):`, `fix(ui):`, `build(deps):`. 在 Monorepo 中，使用包名作为作用域。 / In monorepos, use the package name as scope.

**破坏性变更：** 在类型/作用域后附加 `!`，或添加 `BREAKING CHANGE:` 尾部说明：

**Breaking changes:** Append `!` after type/scope, or add `BREAKING CHANGE:` footer:

```
feat(api)!: remove deprecated user endpoint

BREAKING CHANGE: The /v1/users endpoint has been removed.
Use /v2/users instead.
```

### 1.3 分支命名 / Branch Naming

```
type/short-description
```

示例 / Examples: `feat/add-dark-mode`, `fix/memory-leak`, `docs/update-api-docs`, `refactor/config-module`

描述部分使用 `kebab-case`（短横线分隔）。如果项目有自己的约定（例如使用 `feature/` 而非 `feat/`），则遵循项目的约定。

Use `kebab-case` for the description. If the project has its own convention (e.g., `feature/` instead of `feat/`), follow the project's convention.

### 1.4 源代码注释语言 / Source Code Comment Language

| 场景 / Scenario | 语言 / Language |
|----------|----------|
| 公共 API 文档（docstring / JSDoc） / Public API documentation | 遵循项目约定；默认使用项目的主要语言 / Follow project convention; default to project's primary language |
| 内部实现注释 / Internal implementation comments | 与同一文件中已有注释保持一致 / Match existing comments in the same file |
| 新文件 / New files | 遵循项目约定；默认使用项目的主要语言 / Follow project convention; default to project's primary language |
| 第三方库封装/扩展 / Third-party library wrappers/extensions | 与原始库的语言保持一致 / Match the original library's language |

**核心原则：** 新注释必须与同一文件中已有注释的语言保持一致。除非项目明确允许，否则不要在单个文件中混合使用注释语言。

**Key principle:** New comments MUST match the language of existing comments in the same file. Never mix comment languages within a single file unless the project explicitly allows it.

### 1.5 变量和函数命名 / Variable and Function Naming

- 遵循项目已有的命名风格（`camelCase`、`snake_case`、`PascalCase`、`SCREAMING_SNAKE_CASE` 等） / Follow the project's existing naming style
- **绝不**在同一文件中混合使用命名风格 / **Never** mix naming styles within the same file
- 新代码必须与相邻代码的命名保持一致 / New code must match adjacent code's naming
- 如果存在 linter/formatter 配置，则遵循其规则 / If linter/formatter config exists, follow its rules

### 1.6 PR / Issue 编写 / PR / Issue Writing

**PR 标题：** 遵循 Conventional Commits 格式，使用协作语言。

**PR title:** Follow Conventional Commits format, use the collaboration language.

**PR 描述模板 / PR description template:**

```markdown
## Summary
Brief description of purpose and changes.

## Change Type
- [ ] New feature
- [ ] Bug fix
- [ ] Documentation
- [ ] Refactor
- [ ] Tests
- [ ] Build/CI

## Details
What was changed and why.

## Testing
- [ ] Passed local tests
- [ ] Added/updated relevant test cases

## Related Issues
Closes #<issue-number>
```

如果项目有 PR 模板（`.github/PULL_REQUEST_TEMPLATE*`），则使用该模板。

If the project has a PR template (`.github/PULL_REQUEST_TEMPLATE*`), use that template instead.

**Issue 标题：** 简洁明了，使用协作语言。

**Issue titles:** Concise, use the collaboration language.

```
[Bug] Login page occasionally shows blank screen
[Feature] Support custom theme colors
[Question] How to configure multi-instance deployment
```

### 1.7 发布说明语言 / Release Notes Language

- 如果项目的 `CHANGES.md` / `CHANGELOG.md` 使用双语格式 → 发布说明应使用双语 / If the project uses bilingual format → Release Notes should be bilingual
- 如果项目使用单一语言 → 保持一致 / If the project uses a single language → Stay consistent
- 未找到约定 → 使用检测到的协作语言 / No convention found → Use the detected collaboration language

### 1.8 依赖更新 / Dependency Updates

更新依赖时：

When updating dependencies:

- 遵循项目的锁文件策略（`package-lock.json`、`yarn.lock`、`pnpm-lock.yaml`、`Pipfile.lock`、`Cargo.lock`、`go.sum` 等） / Follow the project's lock file strategy
- 绝不提交与清单文件变更不一致的锁文件变更 / Never commit changes to lock files that are inconsistent with manifest changes
- 使用项目现有的包管理器——不要引入新的包管理器 / Use the project's existing package manager — don't introduce a new one
- 将相关的依赖更新合并到单个提交/PR 中（例如 `chore(deps): update react to v19`） / Group related dependency updates into a single commit/PR
- 将包含破坏性变更的依赖更新隔离到单独的 PR 中 / Isolate breaking dependency updates into separate PRs

---

## 第二部分：行为规范 / Part II: Behavior Specs

### 2.1 尊重已有代码风格 / Respect Existing Code Style

**这是最重要的行为规则。** 在修改任何代码之前：

**This is the most important behavioral rule.** Before modifying any code:

1. **必须搜索**（参见[约定文件搜索](#project-convention-file-search-mandatory-first-step)）风格指南、`.editorconfig`、linter 配置 / **MUST search** (see [Convention File Search](#project-convention-file-search-mandatory-first-step)) for style guides, `.editorconfig`, linter configs
2. **必须阅读**同一目录中的已有源文件，了解项目的代码风格，包括：缩进风格（制表符/空格、宽度）、引号风格（单引号/双引号）、尾逗号策略、导入排序、错误处理模式 / **MUST read** existing source files in the same directory to understand the project's code style, including: indent style (tab/space, width), quote style (single/double), trailing comma policy, import ordering, error handling patterns
3. **必须遵循**项目的已有风格，即使它与你的偏好不同 / **MUST follow** the project's existing style, even if it differs from your preference
4. **绝不**在同一 PR 中引入与项目风格不一致的格式变更 / **Never** introduce format changes inconsistent with the project style in the same PR

### 2.2 最小变更原则 / Minimal Change Principle

- PR 应仅包含与 PR 目的直接相关的变更 / PRs should contain ONLY changes directly related to the PR's purpose
- **绝不**在功能 PR 中捆绑无关的格式化、重构或注释变更 / **Never** bundle unrelated formatting, refactoring, or comment changes in a feature PR
- 大规模的格式化变更应作为单独的 `style:` PR / Large-scale formatting changes should be a separate `style:` PR
- 不要修改你不理解的代码 / Don't modify code you don't understand
- 不要删除看起来"多余"的代码，除非你确定其确实多余 / Don't delete code that looks "redundant" unless you're certain

### 2.3 禁止无关变更 / No Unrelated Changes

以下内容不应出现在功能/修复 PR 中（除非它们就是该 PR 的目的）：

The following should NOT appear in feature/fix PRs (unless they are the PR's purpose):

- 无关的导入添加/删除 / Unrelated import additions/removals
- 无关的文件重命名 / Unrelated file renames
- 无关代码区域的格式化变更 / Formatting changes in unrelated code regions
- 个人偏好风格变更 / Personal preference style changes
- IDE 生成的配置变更 / IDE-generated config changes

### 2.4 测试与质量 / Testing and Quality

- 确保修改后的代码通过项目的现有测试 / Ensure modified code passes the project's existing tests
- 新功能应包含相应的测试用例 / New features should include corresponding test cases
- 缺陷修复最好添加回归测试 / Bug fixes should preferably add regression tests
- 提交前运行项目现有的 lint/format 检查 / Run the project's existing lint/format checks before committing
- 不要降低现有的测试覆盖率 / Don't reduce existing test coverage

### 2.5 沟通规范 / Communication Standards

- 对事不对人：在代码审查中，仅讨论代码质量 / Critique code, not people: In code reviews, discuss code quality only
- 建设性反馈：提出问题时附带建议的解决方案 / Constructive feedback: Raise issues with suggested solutions
- 所有沟通使用协作语言 / Use the collaboration language for all communication
- 尊重项目维护者的最终决定 / Respect project maintainers' final decisions
- 开设新 Issue 前搜索类似的已有 Issue / Search for similar existing issues before opening new ones
- 不要 @ 提及他人，除非确实需要他们的关注 / Don't @ mention people unless their attention is genuinely needed

### 2.6 安全与隐私 / Security and Privacy

- **绝不**在代码、提交消息或 PR 描述中提交密钥、密码、令牌或 API 密钥 / **Never** commit secrets, passwords, tokens, or API keys in code, commit messages, or PR descriptions
- 不要在 Issue 中泄露用户隐私数据 / Don't disclose user privacy data in issues
- 使用 `.gitignore` 排除敏感文件 / Use `.gitignore` to exclude sensitive files
- 通过私有渠道（Security Advisory）报告安全漏洞，而非公开 Issue / Report security vulnerabilities through private channels (Security Advisory), not public issues

### 2.7 许可证合规 / License Compliance

- 新源文件必须包含许可证头部（参见[许可证指南](#open-source-license-guide)） / New source files must include a license header (see [License Guide](#open-source-license-guide))
- 在引入第三方代码前验证其许可证兼容性 / Verify third-party code license compatibility before including it
- 保留已有的版权声明——绝不删除他人的版权信息 / Preserve existing copyright notices — never delete others' copyright info
- 修改他人代码时，在文件头部适当注明变更 / When modifying others' code, note changes in the file header as appropriate

### 2.8 禁止在项目输出中留下工具签名 / No Tool Signatures in Project Output

**严格禁止**在任何项目输出中嵌入 AI 工具签名、生成标记或品牌水印，包括但不限于 `Generated with ...`、`Co-authored-by: ...bot` 等：

**Strictly prohibited** to embed AI tool signatures, generation markers, or brand watermarks in any project output, including but not limited to `Generated with ...`, `Co-authored-by: ...bot`, etc.:

- 提交消息（标题和正文） / Commit messages (both title and body)
- PR 标题和描述 / PR titles and descriptions
- Issue 标题和描述 / Issue titles and descriptions
- 发布说明 / 变更日志 / Release notes / Changelogs
- 源文件内容 / Source code file contents
- 文档文件 / Documentation files
- 配置文件 / Configuration files

**理由：** 开源项目的提交历史和文档是项目自身的资产。工具签名会污染 git 历史、降低专业性，并可能引起项目维护者和社区的不满。

**Rationale:** An open-source project's commit history and documentation are the project's own assets. Tool signatures pollute the git history, reduce professionalism, and may cause dissatisfaction among project maintainers and the community.

### 2.9 提交前检查清单 / Pre-commit Checklist

在提交任何提交或创建 PR 之前，确认以下事项：

Before submitting any commit or creating a PR, confirm:

```
# 提交前检查清单 / Pre-commit Checklist
- [ ] Searched and read project convention files (STYLE_GUIDE.md, etc.)
      已搜索并阅读项目约定文件
- [ ] Read existing code in the same directory to understand project style
      已阅读同目录下的已有代码以了解项目风格
- [ ] Code style matches project's existing style
      代码风格与项目已有风格一致
- [ ] No unrelated changes
      无无关变更
- [ ] Tests pass
      测试通过
- [ ] Lint/format checks pass
      Lint/格式检查通过
- [ ] New files have license headers
      新文件有许可证头部
- [ ] No sensitive information leaked
      无敏感信息泄露
- [ ] Commit message follows Conventional Commits format
      提交消息遵循 Conventional Commits 格式
- [ ] No tool signatures/brand watermarks in any output
      无任何输出中包含工具签名/品牌水印
- [ ] Dependency changes follow project's package manager strategy
      依赖变更遵循项目的包管理器策略
```

---

## 第三部分：PR 与发布工作流 / Part III: PR and Release Workflow

### 标准 PR 流程 / Standard PR Flow

```
# 标准 PR 流程 / Standard PR Flow
1. Fork / Create branch → 2. Search conventions → 3. Code & test → 4. Commit → 5. Create PR → 6. Ownership check → 7. Review → 8. Merge (conditional)
```

**详细步骤 / Detailed steps:**

1. **创建分支 / Create branch**：从 `main`（或项目的默认分支）创建，命名遵循[分支命名](#13-branch-naming) / From `main` (or project's default branch), naming follows [Branch Naming](#13-branch-naming)
2. **搜索约定 / Search conventions**：执行[约定文件搜索](#project-convention-file-search-mandatory-first-step) / Execute [Convention File Search](#project-convention-file-search-mandatory-first-step)
3. **编码和测试 / Code and test**：遵循项目风格，确保现有测试和 lint 通过 / Follow project style, ensure existing tests and lint pass
4. **提交 / Commit**：遵循 [Conventional Commits](#12-commit-message-format) 格式 / Follow [Conventional Commits](#12-commit-message-format) format
5. **创建 PR / Create PR**:
   - 标题遵循 [PR 编写标准](#16-pr--issue-writing) / Title follows [PR Writing Standards](#16-pr--issue-writing)
   - 如果项目有 `.github/PULL_REQUEST_TEMPLATE*`，则使用 PR 模板 / Use PR template if project has `.github/PULL_REQUEST_TEMPLATE*`
   - 多行内容使用 `--body-file` 或 `--notes-file`（参见 [CLI 文本处理](#shell-cli-multi-line-text-handling)） / Use `--body-file` or `--notes-file` for multi-line content (see [CLI Text Handling](#shell-cli-multi-line-text-handling))
   - 关联相关 Issue（`Closes #xxx`） / Link related issues (`Closes #xxx`)
6. **所有权检查 / Ownership check**：执行[所有权验证](#step-5-ownership-verification)。如果不匹配，仅保留 PR / Execute [Ownership Verification](#step-5-ownership-verification). If mismatch, keep PR only
7. **代码审查 / Code Review**：保持耐心，及时响应，清晰描述变更 / Be patient, respond promptly, describe changes clearly
8. **合并 / Merge**（仅在所有权匹配时）：使用 `--merge --delete-branch`。**合并后必须同时删除本地分支（`git branch -d <branch-name>`），不要留下废弃分支。** / (only when ownership matches): Use `--merge --delete-branch`. **After merging, MUST also delete the local branch (`git branch -d <branch-name>`) — never leave stale branches behind.**

### 发布 / Release Publishing

遵循[发布工作流](#release-workflow-with-ownership-verification)部分。

Follow the [Release Workflow](#release-workflow-with-ownership-verification) section.

---/n/n---

# Shell CLI 多行文本处理 / Shell CLI Multi-line Text Handling

不同的 Shell 对换行符的处理方式不同。使用错误的语法会导致 PR/Release 描述中出现字面量 `\n` 文本。

Different shells handle newline characters differently. Using the wrong syntax causes literal `\n` text in PR/Release descriptions.

## Shell 对比 / Shell Comparison

| Shell / Shell | 换行符转义 / Newline Escape | 备注 / Note |
|-------|---------------|------|
| **Bash / Zsh** | `\n` | 标准转义序列 / Standard escape sequence |
| **PowerShell** | `` `n `` | 反引号+n，`\n` 会被当作**字面文本** / Backtick+n, `\n` is treated as **literal text** |
| **Fish** | `\n` | 标准转义序列 / Standard escape sequence |

## 推荐方式：基于文件（所有 Shell）/ Recommended Approach: File-based (All Shells)

这是跨所有环境最可靠的方法：

The most reliable method across all environments:

```bash
# Bash / Zsh / Fish
cat > body.md << 'EOF'
## Changes

Description...
EOF

gh pr create --title "feat: new feature" --body-file body.md
gh release create vX.Y.Z --title "vX.Y.Z" --notes-file release-notes.md
```

```powershell
# PowerShell
@"
## Changes

Description...
"@ | Out-File -FilePath body.md -Encoding UTF8

gh pr create --title "feat: new feature" --body-file body.md
gh release create vX.Y.Z --title "vX.Y.Z" --notes-file release-notes.md
```

## Bash / Zsh 内联方式 / Bash / Zsh Inline

```bash
gh pr create --title "feat: new feature" --body "$(cat <<'EOF'
## Changes

Description with `code` snippets.
EOF
)"
```

## PowerShell 内联方式 / PowerShell Inline

```powershell
# 使用 Here-String / Here-String approach
gh pr create --title "feat: new feature" --body @"
## Changes

Description...
"@

# 或者使用反引号换行 / Or backtick newlines
gh pr create --title "feat: new feature" --body "## Changes`n`nDescription"
```

## 关键规则 / Key Rules

1. **优先使用 `--body-file` / `--notes-file`** 处理所有包含多行内容的 `gh` 命令
1. **Prefer `--body-file` / `--notes-file`** for all `gh` commands with multi-line content

2. **PowerShell：绝不在双引号字符串中使用 `\n`** — 它不会被解释为换行符
2. **PowerShell: never use `\n`** in double-quoted strings — it won't be interpreted as a newline

3. **PowerShell：包含反引号的内容** 必须使用 `--body-file`，否则 Shell 会吞掉反引号包裹的内容
3. **PowerShell: content containing backticks** must use `--body-file`, otherwise the shell will swallow backtick-wrapped content

4. 以上规则适用于所有 `gh` 命令：`pr create/edit`、`release create/edit`、`issue create/edit`
4. Applies to all `gh` commands: `pr create/edit`, `release create/edit`, `issue create/edit`

---

# 带所有权验证的发布工作流 / Release Workflow with Ownership Verification

**核心原则：遵循项目的版本控制策略。在合并/发布前验证所有权。**

**Core principle: Follow the project's versioning strategy. Verify ownership before merge/release.**

> **重要提示：合并和发布操作受所有权验证限制。只有当前 git 用户与仓库所有者匹配时，才允许合并和发布 Release；否则仅创建 PR。**
>
> **IMPORTANT: Merge and release operations are restricted by ownership verification. Only when the current git user matches the repository owner is merging and Release publishing allowed; otherwise, only create a PR.**

## 工作流概述 / Workflow Overview

```
1. 代码变更 / Code changes → 2. 版本号更新 / Version bump → 3. 分支与提交 / Branch & commit → 4. 创建 PR / Create PR → 5. 所有权检查 / Ownership check → 6. 合并（有条件）/ Merge (conditional) → 7. 发布（有条件）/ Release (conditional)
```

## 步骤 1：代码变更 / Step 1: Code Changes

实现修复或功能。遵循项目的行尾和编码规范（检查 `.editorconfig` 和 `.gitattributes`）。

Implement fixes or features. Follow the project's line ending and encoding conventions (check `.editorconfig` and `.gitattributes`).

**前置条件：** 必须在编码前执行[规范文件搜索](#project-convention-file-search-mandatory-first-step)。

**Prerequisite:** Must execute [Convention File Search](#project-convention-file-search-mandatory-first-step) before coding.

## 步骤 2：版本号更新 / Step 2: Version Bump

**检测项目的版本控制策略：**

**Detect the project's versioning strategy:**

| 策略 / Strategy | 检测方式 / Detection | 示例 / Example |
|----------|-----------|---------|
| **SemVer** | 已有标签如 `v1.2.3` 或 `CHANGELOG.md` 使用 SemVer / Existing tags like `v1.2.3` or `CHANGELOG.md` with SemVer | `v1.2.3` → `v1.3.0`（minor 次版本）或 / or `v1.2.4`（patch 补丁版本） |
| **CalVer** | 标签如 `2024.06.1` 或基于日期的版本 / Tags like `2024.06.1` or date-based versions | 遵循项目的日期格式 / Follow the project's date format |
| **自定义 / Custom** | 项目特定的版本文件或模式 / Project-specific version files or patterns | 遵循现有约定 / Follow existing convention |

**需要更新的文件**（如果存在）：

**Files to update** (if they exist):

- `package.json` / `pyproject.toml` / `Cargo.toml` / `go.mod` / `*.csproj` / 等 / etc.
- `CHANGELOG.md` / `CHANGES.md` / `HISTORY.md`
- `README.md` 中的版本引用 / version references
- 项目特定的版本文件 / Any project-specific version files

**变更日志条目格式**（遵循项目现有格式；如不存在则使用以下格式）：

**Changelog entry format** (follow the project's existing format; if none exists):

```markdown
## [X.Y.Z] - YYYY-MM-DD

### Added
- Description of new feature

### Fixed
- Description of bug fix

### Changed
- Description of other changes
```

## 步骤 3：分支、提交、推送 / Step 3: Branch, Commit, Push

**分支命名：** 遵循[分支命名](#13-branch-naming)。

**Branch naming:** Follow [Branch Naming](#13-branch-naming).

**提交信息：** 遵循[约定式提交](#12-commit-message-format)。

**Commit message:** Follow [Conventional Commits](#12-commit-message-format).

## 步骤 4：创建 PR / Step 4: Create PR

多行内容请使用 `--body-file`。参见 [CLI 文本处理](#shell-cli-multi-line-text-handling)。

Use `--body-file` for multi-line content. See [CLI Text Handling](#shell-cli-multi-line-text-handling).

PR 标题和描述遵循 [PR/Issue 编写标准](#16-pr--issue-writing)。

PR title and description follow [PR/Issue Writing Standards](#16-pr--issue-writing).

## 步骤 5：所有权验证 / Step 5: Ownership Verification

> **此步骤为强制执行，不可跳过。必须在合并前执行。**
>
> **This step is MANDATORY and cannot be skipped. Must execute before merging.**

创建 PR 后、合并前，验证当前 git 用户身份是否与远程仓库所有者匹配。

After creating the PR and before merging, verify that the current git user identity matches the remote repository owner.

### 验证方法 / Verification Method

```bash
# 1. 获取本地 git 用户名 / Get local git user name
git config user.name

# 2. 获取远程仓库所有者（GitHub 用户名/组织）/ Get remote repository owner (GitHub username/org)
gh repo view --json owner --jq ".owner.login"

# 3. 获取当前登录的 GitHub 用户 / Get currently logged-in GitHub user
gh api user --jq ".login"
```

### 判定规则 / Decision Rules

比较以下三个值：

Compare these three values:

| 信息 / Info | 命令 / Command | 示例 / Example |
|------|---------|---------|
| 本地 git 用户 / Local git user | `git config user.name` | `ZhangSan` |
| GitHub 登录用户 / GitHub logged-in user | `gh api user --jq ".login"` | `zhangsan` |
| 仓库所有者 / Repository owner | `gh repo view --json owner --jq ".owner.login"` | `zhangsan` 或 / or `my-org` |

**匹配：** GitHub 登录用户（不区分大小写）等于仓库所有者 → **继续执行步骤 6 和步骤 7**（合并 + 发布）

**Match:** GitHub logged-in user (case-insensitive) equals repository owner → **Continue to Step 6 and Step 7** (merge + release)

**不匹配：** GitHub 登录用户与仓库所有者不同 → **立即停止。仅保留 PR。不合并、不发布。** 通知仓库所有者审核并合并。

**Mismatch:** GitHub logged-in user differs from repository owner → **STOP immediately. Keep PR only. No merge, no release.** Notify the repository owner to review and merge.

### 组织仓库 / Organization Repositories

对于组织拥有的仓库，还需检查团队成员身份：

For organization-owned repositories, also check team membership:

```bash
# 检查当前用户是否为该组织成员 / Check if current user is a member of the org
gh api orgs/<org-name>/members/<username> --silent 2>&1
# 退出码 0 = 成员，非零 = 非成员 / Exit code 0 = member, non-zero = not a member
```

即使用户是组织成员，合并/发布仍需项目维护者的明确许可（通过 CODEOWNERS 审批或运行 Agent 的用户直接指示）。

Even if the user is an org member, the merge/release is gated by the project maintainer's explicit permission (via CODEOWNERS approval or direct instruction from the user running the agent).

### 不匹配时的操作 / Actions on Mismatch

1. 在 PR 中添加评论 @仓库所有者，说明 PR 已准备好进行审核
1. Add a PR comment @ the repository owner, explaining the PR is ready for review

2. 向运行 Agent 的用户报告所有权不匹配情况
2. Report the ownership mismatch to the user running the agent

3. **禁止：** 自动执行 `gh pr merge` 或 `gh release create`
3. **Prohibited:** Auto-executing `gh pr merge` or `gh release create`

## 步骤 6：PR 合并（仅在所有权匹配时）/ Step 6: PR Merge (Only When Ownership Matches)

**前置条件：步骤 5 结果为"匹配"。否则跳过此步骤。**

**Prerequisite: Step 5 result is "match". Otherwise, skip this step.**

```bash
gh pr merge <number> --merge --delete-branch
git checkout main && git pull origin main
```

> **⚠️ 必须清理已合并的分支 / MUST clean up merged branches：**
>
> `--delete-branch` 仅删除**远程**分支。合并后**必须**同时删除本地分支，否则本地会积累大量废弃分支：
>
> `--delete-branch` only deletes the **remote** branch. After merging, you **MUST** also delete the local branch — otherwise stale branches accumulate locally:
>
> ```bash
> # 删除本地已合并的分支 / Delete local merged branch
> git branch -d <branch-name>
>
> # 批量清理所有已合并的本地分支（不含 main/master/develop）/ Bulk prune all merged local branches (excluding main/master/develop)
> git branch --merged main | grep -vE '^\*|main$|master$|develop$' | xargs -r git branch -d
> ```
>
> **如果忘记删除本地分支，远程分支已被删除后，本地分支将变为"幽灵分支"——指向已不存在的远程追踪引用。定期运行 `git fetch --prune` 清理失效的远程追踪引用。**
>
> **If you forget to delete the local branch, after the remote branch is deleted the local branch becomes a "ghost branch" — pointing to a remote-tracking reference that no longer exists. Run `git fetch --prune` regularly to clean up stale remote-tracking references.**

合并策略遵循项目的偏好：

Merge strategy follows the project's preference:

- `--merge` — 标准合并提交 / standard merge commit
- `--squash` — 将所有提交压缩为一个 / squash all commits into one
- `--rebase` — 变基到目标分支 / rebase on target branch

如果项目通过分支保护规则强制执行特定合并策略，则遵循该策略。

If the project enforces a specific merge strategy via branch protection rules, follow that.

## 步骤 7：发布（仅在所有权匹配时）/ Step 7: Release (Only When Ownership Matches)

**前置条件：步骤 5 结果为"匹配"且步骤 6 已执行。否则跳过此步骤。**

**Prerequisite: Step 5 result is "match" AND Step 6 has been executed. Otherwise, skip this step.**

### 检查之前发布的 Assets（创建新 Release 前必须执行） / Check Previous Release Assets (MANDATORY Before Creating New Release)

> **在创建新 Release 之前，必须先查询之前发布的 assets 列表，确保新 Release 不会遗漏任何预期的产物。**
>
> **Before creating a new Release, you MUST first query the list of assets from previous releases to ensure the new Release doesn't miss any expected artifacts.**

```bash
# 查看最近一次发布的所有 assets / View all assets from the most recent release
gh release view --json assets --jq ".assets[].name"

# 查看所有发布的 assets 概览（含发布标签）/ View assets overview for all releases (with release tags)
gh release list --limit 5
gh release view <previous-tag> --json assets --jq ".assets[].name"
```

**对比之前 Release 的 assets 列表，确认新 Release 需要上传的产物。** 如果之前每个 Release 都包含特定文件（如 `.zip`、`.tar.gz`、`.whl`、`.exe` 等），新 Release 也应包含对应的产物。

**Compare the assets list from previous releases to confirm which artifacts the new Release should upload.** If every previous release includes specific files (e.g., `.zip`, `.tar.gz`, `.whl`, `.exe`), the new release should include the corresponding artifacts as well.

### 发布说明格式 / Release Notes Format

遵循项目现有的发布说明格式。如不存在则使用以下格式：

Follow the project's existing release notes format. If none exists:

```markdown
## [X.Y.Z] Changelog

### Added
- **Feature title** — Description with `code`

### Fixed
- **Fix title** — Description with `code`

---

See [CHANGELOG.md](CHANGELOG.md) for the full changelog.
```

### 创建发布 / Create Release

```bash
# 先将说明写入文件（推荐用于所有 Shell）/ Write notes to file first (recommended for all shells)
gh release create vX.Y.Z --title "vX.Y.Z — Summary" --notes-file release-notes.md
```

### 发布产物 / Release Assets

如果项目构建可分发的产物：

If the project builds distributable artifacts:

1. 遵循项目的构建说明（通常在 `README.md`、`CONTRIBUTING.md` 或 `Makefile` 中）
1. Follow the project's build instructions (usually in `README.md`, `CONTRIBUTING.md`, or `Makefile`)

2. 使用项目的标准构建工具链构建产物
2. Build artifacts using the project's standard build toolchain

3. 上传资产：
3. Upload assets:

```bash
gh release upload vX.Y.Z "artifact-file" --clobber
```

**常见构建/发布场景：**

**Common build/publish scenarios:**

| 生态系统 / Ecosystem | 构建命令 / Build Command | 发布方式 / Publish |
|-----------|---------------|---------|
| npm | `npm run build` | `npm publish` |
| PyPI | `python -m build` | `twine upload dist/*` |
| Cargo | `cargo build --release` | `cargo publish` |
| Go | `goreleaser release` | 通过 goreleaser 自动发布 / Automatic via goreleaser |
| Docker | `docker build` | `docker push` |
| GitHub Release | 按项目文档构建 / Build per project docs | `gh release upload` |

### 发布后 / Post-Release

- 删除本地构建产物（zip、tar.gz 等）
- Delete local build artifacts (zip, tar.gz, etc.)

- **验证 GitHub 上的发布页面显示所有预期的 assets：**
- **Verify the release page on GitHub shows ALL expected assets:**

```bash
# 查看刚创建的 Release 的 assets 列表 / View assets list of the just-created release
gh release view vX.Y.Z --json assets --jq ".assets[].name"
```

- **与之前 Release 的 assets 数量进行对比。如果之前的 Release 有 N 个 assets，新 Release 应有相同或更多数量的 assets。数量不一致时必须暂停并确认是否遗漏了产物。**
- **Compare asset count with previous releases. If the previous release had N assets, the new release should have the same or more. A discrepancy MUST trigger a pause to verify no artifacts were missed.**

- 删除已合并的本地分支（参见步骤 6 中的分支清理说明）
- Delete the merged local branch (see branch cleanup instructions in Step 6)

- 验证包注册表（如适用）显示新版本
- Verify package registry (if applicable) shows the new version

## 发布检查清单 / Release Checklist

```
- [ ] 已搜索并阅读项目规范文件 / Searched and read project convention files
- [ ] 已按项目版本控制策略更新版本号 / Version bumped according to project's versioning strategy
- [ ] 已更新 CHANGELOG（如果项目有的话）/ CHANGELOG updated (if project has one)
- [ ] 已更新 README 中的版本引用（如适用）/ README version references updated (if applicable)
- [ ] 行尾符合项目规范 / Line endings match project convention
- [ ] 已使用 --body-file 创建 PR / PR created with --body-file
- [ ] 已执行所有权验证（步骤 5）/ Ownership verification executed (Step 5)
- [ ] 已检查之前 Release 的 assets 列表 / Previous Release assets list checked
- [ ] 所有权匹配：已合并 PR、已创建 Release、已上传资产 / Ownership match: PR merged, Release created, assets uploaded
- [ ] 已验证新 Release 的 assets 与之前 Release 一致（数量和内容）/ New Release assets verified against previous releases (count and content)
- [ ] 所有权不匹配：仅创建 PR，已通知所有者 / Ownership mismatch: PR only, owner notified
- [ ] 已删除已合并的远程分支（--delete-branch）/ Merged remote branch deleted (--delete-branch)
- [ ] 已删除已合并的本地分支（git branch -d）/ Merged local branch deleted (git branch -d)
- [ ] 已清理本地构建产物 / Local build artifacts cleaned up
```

## 常见陷阱 / Common Pitfalls

| 问题 / Problem | 原因 / Cause | 修复方法 / Fix |
|---------|-------|-----|
| 发布说明丢失代码片段 / Release notes lose code snippets | Shell 吞掉了反引号 / Shell swallows backticks | 使用 `--notes-file` / Use `--notes-file` |
| PR 正文显示字面量 `\n` / PR body shows literal `\n` | PowerShell 不解析 `\n` / PowerShell doesn't parse `\n` | 使用 Here-String 或文件 / Use Here-String or file |
| 批处理文件乱码 / Batch files garbled | 行尾不正确 / Wrong line endings | `.gitattributes` 强制使用 CRLF / `.gitattributes` enforce CRLF |
| 误在他人的仓库上合并/发布 / Accidentally merge/release on someone else's repo | 跳过了所有权检查 / Skipped ownership check | 必须执行步骤 5 / Must execute Step 5 |
| 版本号更新错误 / Wrong version bump | 忽略了项目的版本控制策略 / Ignored project's versioning strategy | 从现有标签检测策略 / Detect strategy from existing tags |
| 新 Release 遗漏 assets / New Release missing assets | 未检查之前 Release 的 assets 列表 / Didn't check previous release's assets list | 发布前用 `gh release view --json assets` 对比 / Compare with `gh release view --json assets` before release |
| 本地分支未清理 / Local branches not cleaned up | 只删除了远程分支，忘记删除本地分支 / Only deleted remote branch, forgot local | 合并后立即执行 `git branch -d <branch>` / Run `git branch -d <branch>` immediately after merge |/n/n---

# 开源许可证指南 / Open-Source License Guide

## 选择许可证 / Choosing a License

| 许可证 / License | 类型 / Type | 关键特征 / Key Characteristics | 最适合 / Best For |
|---------|------|---------------------|----------|
| **MIT** | 宽松型 / Permissive | 非常简短，允许一切操作 / Very short, allows everything | 库、工具、大多数项目 / Libraries, tools, most projects |
| **Apache 2.0** | 宽松型 / Permissive | 包含专利授权 / Includes patent grant | 企业开源、大型项目 / Corporate open-source, large projects |
| **BSD 2-Clause** | 宽松型 / Permissive | 类似 MIT，措辞略有不同 / Similar to MIT, slightly different wording | 学术项目 / Academic projects |
| **BSD 3-Clause** | 宽松型 / Permissive | BSD 2 + 禁止背书条款 / BSD 2 + no-endorsement clause | 需要名称保护的学术项目 / Academic projects needing name protection |
| **GPL-3.0** | 著佐权 / Copyleft | 衍生作品必须使用 GPL-3.0 / Derivatives must be GPL-3.0 | 希望确保开放性的项目 / Projects wanting to ensure openness |
| **LGPL-3.0** | 弱著佐权 / Weak copyleft | 允许在专有代码中链接 / Linking allowed in proprietary code | 希望广泛采用且保持开放的库 / Libraries wanting wide adoption + openness |
| **AGPL-3.0** | 强著佐权 / Strong copyleft | 网络使用 = 分发 / Network use = distribution | Web 服务、SaaS / Web services, SaaS |
| **MPL-2.0** | 文件级著佐权 / File-level copyleft | 修改的文件必须开源 / Modified files must be open-source | 混合开源和专有的项目 / Projects mixing open and proprietary |
| **Unlicense / CC0** | 公有领域 / Public domain | 无任何限制 / No restrictions | 释放到公有领域 / Releasing into public domain |

## 常用许可证模板 / Common License Templates

### MIT 许可证 / MIT License

```
MIT License

Copyright (c) <year> <copyright holder>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

### Apache 许可证 2.0 / Apache License 2.0

使用完整的标准 Apache 2.0 文本（可从 https://www.apache.org/licenses/LICENSE-2.0 获取）。如果项目需要归属声明，请添加 `NOTICE` 文件。

Use the full standard Apache 2.0 text (available at https://www.apache.org/licenses/LICENSE-2.0). Add a `NOTICE` file if the project requires attribution notices.

### BSD 3-条款 / BSD 3-Clause

```
BSD 3-Clause License

Copyright (c) <year>, <copyright holder>

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this
   list of conditions and the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright notice,
   this list of conditions and the following disclaimer in the documentation
   and/or other materials provided with the distribution.

3. Neither the name of the copyright holder nor the names of its
   contributors may be used to endorse or promote products derived from
   this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
```

### GPL-3.0

**不要**粘贴完整的 700 行法律文本。请使用简短声明模板：

**Don't** paste the full 700-line legal text. Instead, write a short declaration:

```
GNU General Public License v3.0

Copyright (c) <year> <copyright holder>

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License along
with this program. If not, see <https://www.gnu.org/licenses/>.

Full License Text: https://www.gnu.org/licenses/gpl-3.0.html
```

## 源文件头 / Source File Header

根据语言调整注释语法：Python `#`、JS/TS/C/C++/Java/Go `//`、Batch `REM`、HTML `<!-- -->`

Adapt comment syntax per language: Python `#`, JS/TS/C/C++/Java/Go `//`, Batch `REM`, HTML `<!-- -->`

**简短头部（推荐用于宽松许可证）：**

**Short header (recommended for permissive licenses):**

```python
# Copyright (c) <year> <copyright holder>
# SPDX-License-Identifier: <SPDX-ID>
```

**完整头部（推荐用于 GPL 等著佐权许可证）：**

**Full header (recommended for copyleft licenses like GPL):**

```python
# <program-name> - <one-line description>
# Copyright (c) <year> <copyright holder>
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <https://www.gnu.org/licenses/>.
```

## 常见错误 / Common Mistakes

1. **粘贴 GPL 完整法律文本**（700 行）→ 使用简短声明模板 / **Pasting full legal text** for GPL (700 lines) → Use the short declaration template
2. **GPL 缺少"或更高版本"** → 必须包含 `either version 3 of the License, or (at your option) any later version` / **Missing "or later version"** for GPL → Must include `either version 3 of the License, or (at your option) any later version`
3. **混淆 GPL 和 CC** → GPL 用于软件，CC 用于创意作品 / **Confusing GPL and CC** → GPL is for software, CC is for creative works
4. **没有 SPDX 标识符** → 始终在文件头中添加 `SPDX-License-Identifier` 以确保机器可读性 / **No SPDX identifier** → Always add `SPDX-License-Identifier` in file headers for machine readability
5. **混合不兼容的许可证** → 在组合来自不同项目的代码之前验证许可证兼容性 / **Mixing incompatible licenses** → Verify license compatibility before combining code from different projects

## 参考资料 / References

- [SPDX 许可证列表 / SPDX License List](https://spdx.org/licenses/)
- [选择许可证 / Choose a License](https://choosealicense.com/)
- [GPL-3.0 官方 / GPL-3.0 Official](https://www.gnu.org/licenses/gpl-3.0.html)
- [如何应用 GPL / How to Apply GPL](https://www.gnu.org/licenses/gpl-howto.html)

---

# 常见开发场景 / Common Development Scenarios

## 场景 1: Fork 工作流（为他人项目做贡献） / Scenario 1: Fork Workflow (Contributing to Others' Projects)

```
1. Fork the repository
2. Clone your fork locally
3. Add upstream remote: git remote add upstream <original-repo-url>
4. Sync before working: git fetch upstream && git merge upstream/main
5. Create feature branch, code, test
6. Push to your fork
7. Create PR from your fork to the upstream repository
8. Ownership check will MISMATCH → PR only, no merge/release
```

**Fork 的关键规则：**

**Key rules for forks:**

- 永远不要直接推送到上游仓库 / Never push directly to the upstream repository
- 保持你的 fork 默认分支与上游同步 / Keep your fork's default branch synced with upstream
- 始终从功能分支创建 PR，不要从默认分支创建 / Always create PRs from feature branches, not from your default branch

## 场景 2: 热修复 / 紧急修复 / Scenario 2: Hotfix / Urgent Fix

```
1. Create branch from the release tag or main: git checkout -b hotfix/fix-critical-bug v1.2.3
2. Fix the issue
3. Bump patch version: v1.2.3 → v1.2.4
4. Create PR, ownership check, merge if match
5. Create release with hotfix
6. Cherry-pick fix to main branch if applicable
```

## 场景 3: 依赖更新 / Scenario 3: Dependency Update

```
1. Create branch: chore/update-react-19
2. Update package manifest (package.json, pyproject.toml, etc.)
3. Run install to update lock file
4. Run full test suite
5. Fix any breaking changes
6. Commit: chore(deps): update react to v19
7. Create PR with detailed changelog of breaking changes
```

## 场景 4: Monorepo 变更 / Scenario 4: Monorepo Changes

```
1. Identify affected packages
2. Search conventions in each package directory
3. Create branch: feat/package-name/feature-description
4. Only modify affected packages — avoid touching others
5. Run tests for affected packages AND integration tests
6. Use scope in commit message matching the package name
7. Follow the monorepo's release tooling (changesets, lerna, nx, turborepo, etc.)
```

## 场景 5: 仅文档变更 / Scenario 5: Documentation-Only Changes

```
1. Create branch: docs/update-readme
2. Follow project's documentation style (Markdown, reStructuredText, etc.)
3. Verify links work
4. Check spelling and grammar
5. Commit: docs: update README with new API examples
6. No version bump needed (unless project convention requires it)
```

## 场景 6: CI/CD 配置变更 / Scenario 6: CI/CD Configuration Changes

```
1. Create branch: ci/add-deploy-step
2. Test CI changes locally if possible (act, docker-compose, etc.)
3. Ensure changes don't break existing pipelines
4. Commit: ci: add staging deployment step
5. Create PR — note that CI changes may require admin approval
```

## 场景 7: 大规模重构 / Scenario 7: Large Refactoring

```
1. Discuss with maintainers first (open an issue or discussion)
2. Create branch: refactor/module-name
3. Break into small, reviewable commits
4. Each commit should keep tests passing
5. Use fixup commits for review feedback, squash before merge
6. Ensure test coverage doesn't decrease
```

## 场景 8: 回滚有问题的发布 / Scenario 8: Reverting a Bad Release

```
1. Create branch: revert/bad-release
2. git revert <bad-commit-hash> (or revert the merge commit)
3. If the release was published, publish a corrected version
4. For npm/PyPI: consider deprecating the bad version
5. For GitHub Release: edit or delete the bad release, create corrected one
```

## 场景 9: 为项目添加新语言 / 框架 / Scenario 9: Adding a New Language / Framework to a Project

```
1. Open a discussion/issue first to get maintainer buy-in
2. Add appropriate linter/formatter config for the new language
3. Add CI steps for the new language's build/test pipeline
4. Follow the existing project's directory structure conventions
5. Add documentation for the new component
```

## 场景 10: 发布新项目 / Scenario 10: Publishing a New Project

从零开始创建和发布全新开源项目时：

When creating and publishing a brand-new open-source project from scratch:

```
1. Initialize git repo: git init && git checkout -b main
2. Set up .gitignore for your language/ecosystem
3. Add LICENSE file (see License Guide section)
4. Write README.md (default language: English):
   - Project name and one-line description
   - Installation / quickstart instructions
   - Usage examples
   - License badge and link
5. Add source code with license headers on every file
6. Set up linter/formatter config for your ecosystem:
   - JS/TS: .eslintrc / biome.json / .prettierrc + editorconfig
   - Python: pyproject.toml / ruff.toml / .flake8
   - Rust: rustfmt.toml + clippy config
   - Go: golangci.yml
   - Other: ecosystem-appropriate tooling
7. Add CONTRIBUTING.md (default language: English) with:
   - How to set up the dev environment
   - How to run tests
   - Commit message format (Conventional Commits recommended)
   - PR process and review expectations
8. Add CODE_OF_CONDUCT.md (e.g., Contributor Covenant)
9. Set up CI/CD pipeline:
   - GitHub Actions / GitLab CI / etc.
   - At minimum: lint + test on PR
   - Optional: auto-release, coverage reporting
10. Configure .github/ directory:
    - ISSUE_TEMPLATE/ (bug report, feature request)
    - PULL_REQUEST_TEMPLATE.md
    - CODEOWNERS (if multi-maintainer)
11. Initial commit: chore: initial project setup
12. Create GitHub repo:
    gh repo create <name> --public --description "<description>"
13. Push: git remote add origin <url> && git push -u origin main
14. (Optional) Create initial release:
    - Tag: v0.1.0 or v1.0.0
    - Release notes describing the initial offering
    - Upload any distributable artifacts
```

**新项目清单：**

**New project checklist:**

```
- [ ] .gitignore covers all build artifacts, secrets, OS files
- [ ] LICENSE file present with correct year and copyright holder
- [ ] README.md has description, install, usage, and license info
- [ ] All source files have license headers
- [ ] Linter/formatter configured and passing
- [ ] CONTRIBUTING.md describes how to contribute
- [ ] CODE_OF_CONDUCT.md present
- [ ] CI pipeline runs lint + test on every PR
- [ ] GitHub issue/PR templates configured
- [ ] Initial commit pushed to main branch
- [ ] Repository description and topics set on GitHub
- [ ] No secrets, credentials, or personal data in any file
```

---

# Agent 配置参考 / Agent Configuration Reference

本节帮助 AI Agent 在加载此技能时进行自我配置。

This section helps AI agents configure themselves when loading this skill.

## 自动检测清单 / Auto-detection Checklist

在开始处理新项目时，Agent 应该：

When starting work on a new project, the agent should:

```
1. [ ] Search for convention files (Section 0)
2. [ ] Detect collaboration language (Section 1.1)
3. [ ] Detect versioning strategy (Section 4, Step 2)
4. [ ] Detect package manager (check lock files)
5. [ ] Detect build system (check package.json scripts, Makefile, etc.)
6. [ ] Detect test framework (check test directories, config files)
7. [ ] Detect CI system (check .github/workflows, .gitlab-ci.yml, etc.)
8. [ ] Detect license type (read LICENSE file)
9. [ ] Detect default branch (git symbolic-ref refs/remotes/origin/HEAD)
10. [ ] Run ownership verification before merge/release (Section 4, Step 5)
```

## 项目档案模板 / Project Profile Template

Agent 可以为每个处理的项目填写此模板：

The agent can fill in this template for each project it works on:

```
Project: <name>
Repo: <url>
Owner: <user/org>
Default branch: main | master | develop | ...
Collaboration language: en | zh | ja | ko | ... | bilingual
Versioning: SemVer | CalVer | Custom
Package manager: npm | yarn | pnpm | pip | cargo | go mod | ...
Build system: <description>
Test framework: jest | pytest | cargo test | go test | ...
CI system: GitHub Actions | GitLab CI | Jenkins | ...
License: MIT | Apache-2.0 | GPL-3.0 | ...
Merge strategy: merge | squash | rebase
Convention files found: <list>
```

## 通用规则（覆盖项目检测） / Universal Rules (Override Project Detection)

无论项目检测结果如何，以下规则始终适用：

These rules always apply regardless of project detection:

1. **永远不要**在项目文件中输出工具签名或品牌水印 / **Never** output tool signatures or brand watermarks in project files
2. **永远不要**提交密钥或敏感信息 / **Never** commit secrets or sensitive information
3. **始终**在合并/发布前验证所有权 / **Always** verify ownership before merge/release
4. **始终**在进行更改前搜索约定文件 / **Always** search convention files before making changes
5. **始终**遵循约定式提交（Conventional Commits），除非项目明确使用其他格式 / **Always** follow Conventional Commits unless the project explicitly uses a different format
6. **始终**匹配正在修改的文件中的现有代码风格 / **Always** match existing code style in the files being modified

## 参考资料 / References

- [约定式提交 / Conventional Commits](https://www.conventionalcommits.org/)
- [语义化版本 / Semantic Versioning](https://semver.org/)
- [维护变更日志 / Keep a Changelog](https://keepachangelog.com/)
- [GitHub CLI 手册 / GitHub CLI Manual](https://cli.github.com/manual/)
- [SPDX 许可证列表 / SPDX License List](https://spdx.org/licenses/)
- [选择许可证 / Choose a License](https://choosealicense.com/)/n