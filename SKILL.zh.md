---
name: project-standards-zh
description: "ALWAYS invoke for ANY task involving code development or open-source projects. Covers: writing/modifying/reviewing code, git operations (branch, commit, merge, rebase, tag), GitHub workflows (PR, Issue, Release, fork, code review), version bumping, changelog, CI/CD, dependency management, license management, project setup, code style/formatting, commit messages, monorepo, hotfix, and any interaction with a git repository. 凡是涉及代码开发或开源项目的任务都必须唤起此技能。覆盖：编写/修改/审查代码、git 操作（分支、提交、合并、变基、标签）、GitHub 工作流（PR、Issue、Release、fork、代码审查）、版本递增、变更日志、CI/CD、依赖管理、许可证管理、项目初始化、代码风格/格式化、commit message、monorepo、热修复，以及任何与 git 仓库交互的操作。当检测不到项目语言时默认使用中文。"
---

# 开源项目协作规范

面向 AI 代理和开发者的通用 GitHub 开源项目协作规范。所有规则均与具体项目无关——代理必须首先检测并遵循各项目自身的约定。

涵盖七个核心部分：

1. [项目约定文件搜索（必须的第一步）](#项目约定文件搜索必须的第一步)
2. [GitHub 协作标准](#github-协作标准)（语言规范 + 行为规范）
3. [Shell CLI 多行文本处理](#shell-cli-多行文本处理)
4. [含所有权验证的发布工作流](#带所有权验证的发布工作流)
5. [开源许可证指南](#开源许可证指南)
6. [常见开发场景](#常见开发场景)（10 个场景，包括发布新项目）
7. [代理配置参考](#代理配置参考)

---

# 项目约定文件搜索（必须的第一步）

> **强制要求：在对项目进行任何代码变更之前，代理必须执行本节描述的约定文件搜索。此步骤不可跳过。**

## 搜索流程

### 步骤 1：搜索约定文件

在修改任何项目之前，主动搜索以下约定/风格文件：

| 优先级 | 文件名 | 用途 |
|----------|-----------|---------|
| 高 | `STYLE_GUIDE.md` / `STYLEGUIDE.md` / `style-guide.md` | 项目风格指南 |
| 高 | `.editorconfig` | 编辑器格式配置（缩进、换行、编码） |
| 高 | `.eslintrc*` / `biome.json` / `.prettierrc*` | JS/TS 格式化 |
| 高 | `pyproject.toml` / `setup.cfg` / `.flake8` / `tox.ini` | Python 格式化 |
| 高 | `rustfmt.toml` / `rust-toolchain.toml` | Rust 格式化 |
| 高 | `.rubocop.yml` / `Gemfile` (Ruby) | Ruby 格式化 |
| 高 | `golangci.yml` / `.golangci.yaml` | Go 代码检查 |
| 高 | `checkstyle.xml` / `spotless.gradle` (Java/Kotlin) | Java/Kotlin 格式化 |
| 高 | `.swiftlint.yml` | Swift 格式化 |
| 高 | `.clang-format` / `.clang-tidy` | C/C++ 格式化 |
| 中 | `CONTRIBUTING.md` / `CONTRIBUTING` | 贡献指南 |
| 中 | `CODE_OF_CONDUCT.md` | 行为准则 |
| 中 | `.github/CODEOWNERS` | 代码审查负责人 |
| 中 | `.github/ISSUE_TEMPLATE/*` / `.github/PULL_REQUEST_TEMPLATE*` | Issue/PR 模板 |
| 中 | `ARCHITECTURE.md` / `DESIGN.md` | 架构决策 |
| 低 | `README.md` 中的约定段落 | README 中的约定信息 |

### 步骤 2：搜索命令

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

### 步骤 3：阅读并遵守

- 如果找到文件 → **必须阅读其内容并严格遵循格式要求**
- 多个约定文件存在冲突 → 优先级更高的胜出（自定义项目指南 > 工具配置 > 通用模板）
- 未找到约定文件 → 遵循本文档的默认值

### 优先级规则

```
# 项目自身的约定文件 > 本文档的默认值 > 通用行业惯例
```

### Monorepo 感知

在 Monorepo 项目（包含多个包的工作区）中，需在以下位置搜索：

- 仓库根目录
- 每个包/工作区目录
- 正在修改的具体包

最具体（最接近被修改代码）的约定优先。

---

# GitHub 协作标准

本节定义了在 GitHub 开源项目中协作的语言和行为规范。这些规范涵盖整个协作生命周期：代码编写、PR 提交、Issue 讨论和版本发布。

---

## 第一部分：语言规范

### 1.1 协作语言检测

**代理在生成任何文本输出之前，必须自动检测项目的协作语言。** 检测优先级：

1. **CONTRIBUTING.md / STYLE_GUIDE.md** 明确指定了语言 → 使用该语言
2. **README.md** 的主要语言 → 使用该语言编写协作文本
3. **现有 PR/Issue/提交历史** 的主要语言 → 使用该语言
4. **用户指令** → 如果用户明确要求使用特定语言，则使用该语言
5. **兜底方案** → 中文

检测到的语言适用于：PR 标题和描述、提交消息、Issue 标题和描述、发布说明、代码审查评论以及所有协作者沟通。

### 1.2 提交消息格式

遵循 [Conventional Commits（约定式提交）](https://www.conventionalcommits.org/)：

```
类型(作用域): 简短描述

[可选正文]
```

**类型：**

| 类型 | 含义 |
|------|---------|
| `feat` | 新功能 |
| `fix` | 缺陷修复 |
| `docs` | 文档变更 |
| `chore` | 构建 / 工具链 / 依赖 |
| `refactor` | 重构（无行为变更） |
| `test` | 测试相关变更 |
| `style` | 格式化（无逻辑变更） |
| `perf` | 性能优化 |
| `ci` | CI/CD 配置变更 |
| `build` | 构建系统或外部依赖 |
| `revert` | 回退之前的提交 |

**作用域（可选）：** 标识受影响的区域，例如 `feat(auth):`、`fix(ui):`、`build(deps):`。在 Monorepo 中，使用包名作为作用域。

**破坏性变更：** 在类型/作用域后附加 `!`，或添加 `BREAKING CHANGE:` 尾部说明：

```
feat(api)!: 移除已弃用的用户端点

BREAKING CHANGE: /v1/users 端点已被移除。
请改用 /v2/users。
```

### 1.3 分支命名

```
类型/简短描述
```

示例：`feat/add-dark-mode`、`fix/memory-leak`、`docs/update-api-docs`、`refactor/config-module`

描述部分使用 `kebab-case`（短横线分隔）。如果项目有自己的约定（例如使用 `feature/` 而非 `feat/`），则遵循项目的约定。

### 1.4 源代码注释语言

| 场景 | 语言 |
|----------|----------|
| 公共 API 文档（docstring / JSDoc） | 遵循项目约定；默认使用项目的主要语言 |
| 内部实现注释 | 与同一文件中已有注释保持一致 |
| 新文件 | 遵循项目约定；默认使用项目的主要语言 |
| 第三方库封装/扩展 | 与原始库的语言保持一致 |

**核心原则：** 新注释必须与同一文件中已有注释的语言保持一致。除非项目明确允许，否则不要在单个文件中混合使用注释语言。

### 1.5 变量和函数命名

- 遵循项目已有的命名风格（`camelCase`、`snake_case`、`PascalCase`、`SCREAMING_SNAKE_CASE` 等）
- **绝不**在同一文件中混合使用命名风格
- 新代码必须与相邻代码的命名保持一致
- 如果存在 linter/formatter 配置，则遵循其规则

### 1.6 PR / Issue 编写

**PR 标题：** 遵循 Conventional Commits 格式，使用协作语言。

**PR 描述模板：**

```markdown
## 概述
简要描述目的和变更内容。

## 变更类型
- [ ] 新功能
- [ ] 缺陷修复
- [ ] 文档
- [ ] 重构
- [ ] 测试
- [ ] 构建/CI

## 详情
变更了什么以及为什么。

## 测试
- [ ] 已通过本地测试
- [ ] 已添加/更新相关测试用例

## 相关 Issue
关闭 #<issue编号>
```

如果项目有 PR 模板（`.github/PULL_REQUEST_TEMPLATE*`），则使用该模板。

**Issue 标题：** 简洁明了，使用协作语言。

```
[Bug] 登录页面偶现白屏
[Feature] 支持自定义主题颜色
[Question] 如何配置多实例部署
```

### 1.7 发布说明语言

- 如果项目的 `CHANGES.md` / `CHANGELOG.md` 使用双语格式 → 发布说明应使用双语
- 如果项目使用单一语言 → 保持一致
- 未找到约定 → 使用检测到的协作语言

### 1.8 依赖更新

更新依赖时：

- 遵循项目的锁文件策略（`package-lock.json`、`yarn.lock`、`pnpm-lock.yaml`、`Pipfile.lock`、`Cargo.lock`、`go.sum` 等）
- 绝不提交与清单文件变更不一致的锁文件变更
- 使用项目现有的包管理器——不要引入新的包管理器
- 将相关的依赖更新合并到单个提交/PR 中（例如 `chore(deps): 将 react 更新至 v19`）
- 将包含破坏性变更的依赖更新隔离到单独的 PR 中

---

## 第二部分：行为规范

### 2.1 尊重已有代码风格

**这是最重要的行为规则。** 在修改任何代码之前：

1. **必须搜索**（参见[约定文件搜索](#项目约定文件搜索必须的第一步)）风格指南、`.editorconfig`、linter 配置
2. **必须阅读**同一目录中的已有源文件，了解项目的代码风格，包括：缩进风格（制表符/空格、宽度）、引号风格（单引号/双引号）、尾逗号策略、导入排序、错误处理模式
3. **必须遵循**项目的已有风格，即使它与你的偏好不同
4. **绝不**在同一 PR 中引入与项目风格不一致的格式变更

### 2.2 最小变更原则

- PR 应仅包含与 PR 目的直接相关的变更
- **绝不**在功能 PR 中捆绑无关的格式化、重构或注释变更
- 大规模的格式化变更应作为单独的 `style:` PR
- 不要修改你不理解的代码
- 不要删除看起来"多余"的代码，除非你确定其确实多余

### 2.3 禁止无关变更

以下内容不应出现在功能/修复 PR 中（除非它们就是该 PR 的目的）：

- 无关的导入添加/删除
- 无关的文件重命名
- 无关代码区域的格式化变更
- 个人偏好风格变更
- IDE 生成的配置变更

### 2.4 测试与质量

- 确保修改后的代码通过项目的现有测试
- 新功能应包含相应的测试用例
- 缺陷修复最好添加回归测试
- 提交前运行项目现有的 lint/format 检查
- 不要降低现有的测试覆盖率

### 2.5 沟通规范

- 对事不对人：在代码审查中，仅讨论代码质量
- 建设性反馈：提出问题时附带建议的解决方案
- 所有沟通使用协作语言
- 尊重项目维护者的最终决定
- 开设新 Issue 前搜索类似的已有 Issue
- 不要 @ 提及他人，除非确实需要他们的关注

### 2.6 安全与隐私

- **绝不**在代码、提交消息或 PR 描述中提交密钥、密码、令牌或 API 密钥
- 不要在 Issue 中泄露用户隐私数据
- 使用 `.gitignore` 排除敏感文件
- 通过私有渠道（Security Advisory）报告安全漏洞，而非公开 Issue

### 2.7 许可证合规

- 新源文件必须包含许可证头部（参见[许可证指南](#开源许可证指南)）
- 在引入第三方代码前验证其许可证兼容性
- 保留已有的版权声明——绝不删除他人的版权信息
- 修改他人代码时，在文件头部适当注明变更

### 2.8 禁止在项目输出中留下工具签名

**严格禁止**在任何项目输出中嵌入 AI 工具签名、生成标记或品牌水印，包括但不限于 `Generated with ...`、`Co-authored-by: ...bot` 等：

- 提交消息（标题和正文）
- PR 标题和描述
- Issue 标题和描述
- 发布说明 / 变更日志
- 源文件内容
- 文档文件
- 配置文件

**理由：** 开源项目的提交历史和文档是项目自身的资产。工具签名会污染 git 历史、降低专业性，并可能引起项目维护者和社区的不满。

### 2.9 提交前检查清单

在提交任何提交或创建 PR 之前，确认以下事项：

```
# 提交前检查清单
- [ ] 已搜索并阅读项目约定文件（STYLE_GUIDE.md 等）
- [ ] 已阅读同目录下的已有代码以了解项目风格
- [ ] 代码风格与项目已有风格一致
- [ ] 无无关变更
- [ ] 测试通过
- [ ] Lint/格式检查通过
- [ ] 新文件有许可证头部
- [ ] 无敏感信息泄露
- [ ] 提交消息遵循 Conventional Commits 格式
- [ ] 无任何输出中包含工具签名/品牌水印
- [ ] 依赖变更遵循项目的包管理器策略
```

---

## 第三部分：PR 与发布工作流

### 标准 PR 流程

```
# 标准 PR 流程
1. Fork / 创建分支 → 2. 搜索约定 → 3. 编码与测试 → 4. 提交 → 5. 创建 PR → 6. 所有权检查 → 7. 审查 → 8. 合并（有条件）
```

**详细步骤：**

1. **创建分支**：从 `main`（或项目的默认分支）创建，命名遵循[分支命名](#13-分支命名)
2. **搜索约定**：执行[约定文件搜索](#项目约定文件搜索必须的第一步)
3. **编码和测试**：遵循项目风格，确保现有测试和 lint 通过
4. **提交**：遵循 [Conventional Commits](#12-提交消息格式) 格式
5. **创建 PR**：
   - 标题遵循 [PR 编写标准](#16-pr--issue-编写)
   - 如果项目有 `.github/PULL_REQUEST_TEMPLATE*`，则使用 PR 模板
   - 多行内容使用 `--body-file` 或 `--notes-file`（参见 [CLI 文本处理](#shell-cli-多行文本处理)）
   - 关联相关 Issue（`Closes #xxx`）
6. **所有权检查**：执行[所有权验证](#步骤-5所有权验证)。如果不匹配，仅保留 PR
7. **代码审查**：保持耐心，及时响应，清晰描述变更
8. **合并**（仅在所有权匹配时）：使用 `--merge --delete-branch`。**合并后必须同时删除本地分支（`git branch -d <branch-name>`），不要留下废弃分支。**

### 发布

遵循[发布工作流](#带所有权验证的发布工作流)部分。

---

# Shell CLI 多行文本处理

不同的 Shell 对换行符的处理方式不同。使用错误的语法会导致 PR/Release 描述中出现字面量 `\n` 文本。

## Shell 对比

| Shell | 换行符转义 | 备注 |
|-------|---------------|------|
| **Bash / Zsh** | `\n` | 标准转义序列 |
| **PowerShell** | `` `n `` | 反引号+n，`\n` 会被当作**字面文本** |
| **Fish** | `\n` | 标准转义序列 |

## 推荐方式：基于文件（所有 Shell）

这是跨所有环境最可靠的方法：

```bash
# Bash / Zsh / Fish
cat > body.md << 'EOF'
## 变更内容

描述...
EOF

gh pr create --title "feat: 新功能" --body-file body.md
gh release create vX.Y.Z --title "vX.Y.Z" --notes-file release-notes.md
```

```powershell
# PowerShell
@"
## 变更内容

描述...
"@ | Out-File -FilePath body.md -Encoding UTF8

gh pr create --title "feat: 新功能" --body-file body.md
gh release create vX.Y.Z --title "vX.Y.Z" --notes-file release-notes.md
```

## Bash / Zsh 内联方式

```bash
gh pr create --title "feat: 新功能" --body "$(cat <<'EOF'
## 变更内容

包含 `代码` 片段的描述。
EOF
)"
```

## PowerShell 内联方式

```powershell
# 使用 Here-String
gh pr create --title "feat: 新功能" --body @"
## 变更内容

描述...
"@

# 或者使用反引号换行
gh pr create --title "feat: 新功能" --body "## 变更内容`n`n描述"
```

## 关键规则

1. **优先使用 `--body-file` / `--notes-file`** 处理所有包含多行内容的 `gh` 命令

2. **PowerShell：绝不在双引号字符串中使用 `\n`** — 它不会被解释为换行符

3. **PowerShell：包含反引号的内容**必须使用 `--body-file`，否则 Shell 会吞掉反引号包裹的内容

4. 以上规则适用于所有 `gh` 命令：`pr create/edit`、`release create/edit`、`issue create/edit`

---

# 带所有权验证的发布工作流

**核心原则：遵循项目的版本控制策略。在合并/发布前验证所有权。**

> **重要提示：合并和发布操作受所有权验证限制。只有当前 git 用户与仓库所有者匹配时，才允许合并和发布 Release；否则仅创建 PR。**

## 工作流概述

```
1. 代码变更 → 2. 版本号更新 → 3. 分支与提交 → 4. 创建 PR → 5. 所有权检查 → 6. 合并（有条件） → 7. 发布（有条件）
```

## 步骤 1：代码变更

实现修复或功能。遵循项目的行尾和编码规范（检查 `.editorconfig` 和 `.gitattributes`）。

**前置条件：** 必须在编码前执行[规范文件搜索](#项目约定文件搜索必须的第一步)。

## 步骤 2：版本号更新

**检测项目的版本控制策略：**

| 策略 | 检测方式 | 示例 |
|----------|-----------|---------|
| **SemVer** | 已有标签如 `v1.2.3` 或 `CHANGELOG.md` 使用 SemVer | `v1.2.3` → `v1.3.0`（次版本）或 `v1.2.4`（补丁版本） |
| **CalVer** | 标签如 `2024.06.1` 或基于日期的版本 | 遵循项目的日期格式 |
| **自定义** | 项目特定的版本文件或模式 | 遵循现有约定 |

**需要更新的文件**（如果存在）：

- `package.json` / `pyproject.toml` / `Cargo.toml` / `go.mod` / `*.csproj` 等
- `CHANGELOG.md` / `CHANGES.md` / `HISTORY.md`
- `README.md` 中的版本引用
- 项目特定的版本文件

**变更日志条目格式**（遵循项目现有格式；如不存在则使用以下格式）：

```markdown
## [X.Y.Z] - YYYY-MM-DD

### 新增
- 新功能描述

### 修复
- 缺陷修复描述

### 变更
- 其他变更描述
```

## 步骤 3：分支、提交、推送

**分支命名：** 遵循[分支命名](#13-分支命名)。

**提交信息：** 遵循[约定式提交](#12-提交消息格式)。

## 步骤 4：创建 PR

多行内容请使用 `--body-file`。参见 [CLI 文本处理](#shell-cli-多行文本处理)。

PR 标题和描述遵循 [PR/Issue 编写标准](#16-pr--issue-编写)。

## 步骤 5：所有权验证

> **此步骤为强制执行，不可跳过。必须在合并前执行。**

创建 PR 后、合并前，验证当前 git 用户身份是否与远程仓库所有者匹配。

### 验证方法

```bash
# 1. 获取本地 git 用户名
git config user.name

# 2. 获取远程仓库所有者（GitHub 用户名/组织）
gh repo view --json owner --jq ".owner.login"

# 3. 获取当前登录的 GitHub 用户
gh api user --jq ".login"
```

### 判定规则

比较以下三个值：

| 信息 | 命令 | 示例 |
|------|---------|---------|
| 本地 git 用户 | `git config user.name` | `ZhangSan` |
| GitHub 登录用户 | `gh api user --jq ".login"` | `zhangsan` |
| 仓库所有者 | `gh repo view --json owner --jq ".owner.login"` | `zhangsan` 或 `my-org` |

**匹配：** GitHub 登录用户（不区分大小写）等于仓库所有者 → **继续执行步骤 6 和步骤 7**（合并 + 发布）

**不匹配：** GitHub 登录用户与仓库所有者不同 → **立即停止。仅保留 PR。不合并、不发布。** 通知仓库所有者审核并合并。

### 组织仓库

对于组织拥有的仓库，还需检查团队成员身份：

```bash
# 检查当前用户是否为该组织成员
gh api orgs/<组织名>/members/<用户名> --silent 2>&1
# 退出码 0 = 成员，非零 = 非成员
```

即使用户是组织成员，合并/发布仍需项目维护者的明确许可（通过 CODEOWNERS 审批或运行 Agent 的用户直接指示）。

### 不匹配时的操作

1. 在 PR 中添加评论 @仓库所有者，说明 PR 已准备好进行审核

2. 向运行 Agent 的用户报告所有权不匹配情况

3. **禁止：** 自动执行 `gh pr merge` 或 `gh release create`

## 步骤 6：PR 合并（仅在所有权匹配时）

**前置条件：步骤 5 结果为"匹配"。否则跳过此步骤。**

```bash
gh pr merge <编号> --merge --delete-branch
git checkout main && git pull origin main
```

> **⚠️ 必须清理已合并的分支：**
>
> `--delete-branch` 仅删除**远程**分支。合并后**必须**同时删除本地分支，否则本地会积累大量废弃分支：
>
> ```bash
> # 删除本地已合并的分支
> git branch -d <branch-name>
>
> # 批量清理所有已合并的本地分支（不含 main/master/develop）
> git branch --merged main | grep -vE '^\*|main$|master$|develop$' | xargs -r git branch -d
> ```
>
> **如果忘记删除本地分支，远程分支已被删除后，本地分支将变为"幽灵分支"——指向已不存在的远程追踪引用。定期运行 `git fetch --prune` 清理失效的远程追踪引用。**

合并策略遵循项目的偏好：

- `--merge` — 标准合并提交
- `--squash` — 将所有提交压缩为一个
- `--rebase` — 变基到目标分支

如果项目通过分支保护规则强制执行特定合并策略，则遵循该策略。

## 步骤 7：发布（仅在所有权匹配时）

**前置条件：步骤 5 结果为"匹配"且步骤 6 已执行。否则跳过此步骤。**

> **⚠️ 强制规则：所有权验证通过后，必须检查本次修改是否影响了上一个 Release 的 assets 中包含的文件。如果影响了，必须强制发布新 Release。**

### 检测本次修改是否影响 Release Assets（强制）

所有权验证通过后，在执行发布前，必须先判断本次变更是否修改了上一个 Release 的 assets 中包含的源文件：

```bash
# 1. 获取本次变更涉及的文件（合并后在 main 上执行）
git diff HEAD~1 --name-only

# 2. 查看上一个 Release 的 assets 列表
gh release view --json assets --jq ".assets[].name"
```

**判定规则：**

- 如果本次变更修改的文件属于上一个 Release assets 中的产物源文件（如 `.zip`、`.tar.gz` 等打包产物所包含的源文件） → **必须发布新 Release，不可跳过**
- 如果本次变更未涉及 assets 中的文件（如仅修改了 CI 配置、`.gitignore`、与产物无关的文档等） → 可跳过发布

**常见触发场景：** 项目的 `SKILL.md`、`LICENSE` 等被打包进 Release 的 `.zip` 产物中，修改这些文件即触发强制发布。

### 检查之前发布的 Assets（创建新 Release 前必须执行）

> **在创建新 Release 之前，必须先查询之前发布的 assets 列表，确保新 Release 不会遗漏任何预期的产物。**

```bash
# 查看最近一次发布的所有 assets
gh release view --json assets --jq ".assets[].name"

# 查看所有发布的 assets 概览（含发布标签）
gh release list --limit 5
gh release view <previous-tag> --json assets --jq ".assets[].name"
```

**对比之前 Release 的 assets 列表，确认新 Release 需要上传的产物。** 如果之前每个 Release 都包含特定文件（如 `.zip`、`.tar.gz`、`.whl`、`.exe` 等），新 Release 也应包含对应的产物。

### 发布说明格式

遵循项目现有的发布说明格式。如不存在则使用以下格式：

```markdown
## [X.Y.Z] 变更日志

### 新增
- **功能标题** — 包含 `代码` 的描述

### 修复
- **修复标题** — 包含 `代码` 的描述

---

完整变更日志请参见 [CHANGELOG.md](CHANGELOG.md)。
```

### 创建发布

```bash
# 先将说明写入文件（推荐用于所有 Shell）
gh release create vX.Y.Z --title "vX.Y.Z — 概述" --notes-file release-notes.md
```

### 发布产物

如果项目构建可分发的产物：

1. 遵循项目的构建说明（通常在 `README.md`、`CONTRIBUTING.md` 或 `Makefile` 中）

2. 使用项目的标准构建工具链构建产物

3. 上传资产：

```bash
gh release upload vX.Y.Z "artifact-file" --clobber
```

**常见构建/发布场景：**

| 生态系统 | 构建命令 | 发布方式 |
|-----------|---------------|---------|
| npm | `npm run build` | `npm publish` |
| PyPI | `python -m build` | `twine upload dist/*` |
| Cargo | `cargo build --release` | `cargo publish` |
| Go | `goreleaser release` | 通过 goreleaser 自动发布 |
| Docker | `docker build` | `docker push` |
| GitHub Release | 按项目文档构建 | `gh release upload` |

### 发布后

- 删除本地构建产物（zip、tar.gz 等）

- **验证 GitHub 上的发布页面显示所有预期的 assets：**

```bash
# 查看刚创建的 Release 的 assets 列表
gh release view vX.Y.Z --json assets --jq ".assets[].name"
```

- **与之前 Release 的 assets 数量进行对比。如果之前的 Release 有 N 个 assets，新 Release 应有相同或更多数量的 assets。数量不一致时必须暂停并确认是否遗漏了产物。**

- 删除已合并的本地分支（参见步骤 6 中的分支清理说明）

- 验证包注册表（如适用）显示新版本

## 发布检查清单

```
- [ ] 已搜索并阅读项目规范文件
- [ ] 已按项目版本控制策略更新版本号
- [ ] 已更新 CHANGELOG（如果项目有的话）
- [ ] 已更新 README 中的版本引用（如适用）
- [ ] 行尾符合项目规范
- [ ] 已使用 --body-file 创建 PR
- [ ] 已执行所有权验证（步骤 5）
- [ ] 已检测本次修改是否影响上一个 Release 的 assets 文件（影响则强制发布）
- [ ] 已检查之前 Release 的 assets 列表
- [ ] 所有权匹配：已合并 PR、已创建 Release、已上传资产
- [ ] 已验证新 Release 的 assets 与之前 Release 一致（数量和内容）
- [ ] 所有权不匹配：仅创建 PR，已通知所有者
- [ ] 已删除已合并的远程分支（--delete-branch）
- [ ] 已删除已合并的本地分支（git branch -d）
- [ ] 已清理本地构建产物
```

## 常见陷阱

| 问题 | 原因 | 修复方法 |
|---------|-------|-----|
| 发布说明丢失代码片段 | Shell 吞掉了反引号 | 使用 `--notes-file` |
| PR 正文显示字面量 `\n` | PowerShell 不解析 `\n` | 使用 Here-String 或文件 |
| 批处理文件乱码 | 行尾不正确 | `.gitattributes` 强制使用 CRLF |
| 误在他人的仓库上合并/发布 | 跳过了所有权检查 | 必须执行步骤 5 |
| 版本号更新错误 | 忽略了项目的版本控制策略 | 从现有标签检测策略 |
| 新 Release 遗漏 assets | 未检查之前 Release 的 assets 列表 | 发布前用 `gh release view --json assets` 对比 |
| 修改了 assets 源文件但未发新 Release | 未执行资产变更检测 | 所有权通过后必须执行 `git diff HEAD~1 --name-only` 对比 assets 文件 |
| 本地分支未清理 | 只删除了远程分支，忘记删除本地分支 | 合并后立即执行 `git branch -d <branch>` |

---

# 开源许可证指南

## 选择许可证

| 许可证 | 类型 | 关键特征 | 最适合 |
|---------|------|---------------------|----------|
| **MIT** | 宽松型 | 非常简短，允许一切操作 | 库、工具、大多数项目 |
| **Apache 2.0** | 宽松型 | 包含专利授权 | 企业开源、大型项目 |
| **BSD 2-Clause** | 宽松型 | 类似 MIT，措辞略有不同 | 学术项目 |
| **BSD 3-Clause** | 宽松型 | BSD 2 + 禁止背书条款 | 需要名称保护的学术项目 |
| **GPL-3.0** | 著佐权 | 衍生作品必须使用 GPL-3.0 | 希望确保开放性的项目 |
| **LGPL-3.0** | 弱著佐权 | 允许在专有代码中链接 | 希望广泛采用且保持开放的库 |
| **AGPL-3.0** | 强著佐权 | 网络使用 = 分发 | Web 服务、SaaS |
| **MPL-2.0** | 文件级著佐权 | 修改的文件必须开源 | 混合开源和专有的项目 |
| **Unlicense / CC0** | 公有领域 | 无任何限制 | 释放到公有领域 |

## 常用许可证模板

### MIT 许可证

以下为 MIT 许可证标准法律文本（保持英文原文）：

```
MIT License

Copyright (c) <年份> <版权持有人>

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

### Apache 许可证 2.0

使用完整的标准 Apache 2.0 文本（可从 https://www.apache.org/licenses/LICENSE-2.0 获取）。如果项目需要归属声明，请添加 `NOTICE` 文件。

### BSD 3-条款

以下为 BSD 3-条款许可证标准法律文本（保持英文原文）：

```
BSD 3-Clause License

Copyright (c) <年份>, <版权持有人>

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

```
GNU General Public License v3.0

Copyright (c) <年份> <版权持有人>

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

完整许可证文本: https://www.gnu.org/licenses/gpl-3.0.html
```

## 源文件头

根据语言调整注释语法：Python `#`、JS/TS/C/C++/Java/Go `//`、Batch `REM`、HTML `<!-- -->`

**简短头部（推荐用于宽松许可证）：**

```python
# Copyright (c) <年份> <版权持有人>
# SPDX-License-Identifier: <SPDX标识符>
```

**完整头部（推荐用于 GPL 等著佐权许可证）：**

```python
# <程序名称> - <一行描述>
# Copyright (c) <年份> <版权持有人>
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

## 常见错误

1. **粘贴 GPL 完整法律文本**（700 行）→ 使用简短声明模板
2. **GPL 缺少"或更高版本"** → 必须包含 `either version 3 of the License, or (at your option) any later version`
3. **混淆 GPL 和 CC** → GPL 用于软件，CC 用于创意作品
4. **没有 SPDX 标识符** → 始终在文件头中添加 `SPDX-License-Identifier` 以确保机器可读性
5. **混合不兼容的许可证** → 在组合来自不同项目的代码之前验证许可证兼容性

## 参考资料

- [SPDX 许可证列表](https://spdx.org/licenses/)
- [选择许可证](https://choosealicense.com/)
- [GPL-3.0 官方](https://www.gnu.org/licenses/gpl-3.0.html)
- [如何应用 GPL](https://www.gnu.org/licenses/gpl-howto.html)

---

# 常见开发场景

## 场景 1: Fork 工作流（为他人项目做贡献）

```
1. Fork 仓库
2. 将你的 fork 克隆到本地
3. 添加上游远程仓库: git remote add upstream <原始仓库地址>
4. 开始工作前同步: git fetch upstream && git merge upstream/main
5. 创建功能分支，编码，测试
6. 推送到你的 fork
7. 从你的 fork 向上游仓库创建 PR
8. 所有权检查将不匹配 → 仅创建 PR，不合并/发布
```

**Fork 的关键规则：**

- 永远不要直接推送到上游仓库
- 保持你的 fork 默认分支与上游同步
- 始终从功能分支创建 PR，不要从默认分支创建

## 场景 2: 热修复 / 紧急修复

```
1. 从发布标签或 main 创建分支: git checkout -b hotfix/fix-critical-bug v1.2.3
2. 修复问题
3. 更新补丁版本: v1.2.3 → v1.2.4
4. 创建 PR，执行所有权检查，匹配则合并
5. 创建包含热修复的发布
6. 如适用，将修复合并回 main 分支
```

## 场景 3: 依赖更新

```
1. 创建分支: chore/update-react-19
2. 更新包清单文件（package.json、pyproject.toml 等）
3. 运行 install 更新锁文件
4. 运行完整测试套件
5. 修复所有破坏性变更
6. 提交: chore(deps): 将 react 更新至 v19
7. 创建 PR，详细说明破坏性变更的日志
```

## 场景 4: Monorepo 变更

```
1. 确定受影响的包
2. 在每个包目录中搜索约定
3. 创建分支: feat/包名/功能描述
4. 仅修改受影响的包——避免触及其它包
5. 运行受影响包的测试和集成测试
6. 提交消息中使用与包名匹配的作用域
7. 遵循 monorepo 的发布工具（changesets、lerna、nx、turborepo 等）
```

## 场景 5: 仅文档变更

```
1. 创建分支: docs/update-readme
2. 遵循项目的文档风格（Markdown、reStructuredText 等）
3. 验证链接可用
4. 检查拼写和语法
5. 提交: docs: 更新 README 中的新 API 示例
6. 无需更新版本号（除非项目约定要求）
```

## 场景 6: CI/CD 配置变更

```
1. 创建分支: ci/add-deploy-step
2. 尽可能在本地测试 CI 变更（act、docker-compose 等）
3. 确保变更不会破坏现有流水线
4. 提交: ci: 添加预发布部署步骤
5. 创建 PR——注意 CI 变更可能需要管理员审批
```

## 场景 7: 大规模重构

```
1. 先与维护者讨论（开设 issue 或 discussion）
2. 创建分支: refactor/模块名
3. 拆分为小型、可审查的提交
4. 每个提交都应保持测试通过
5. 使用 fixup 提交处理审查反馈，合并前压缩
6. 确保测试覆盖率不降低
```

## 场景 8: 回滚有问题的发布

```
1. 创建分支: revert/bad-release
2. git revert <问题提交的哈希值>（或 revert 合并提交）
3. 如果发布已推送，发布一个修正版本
4. 对于 npm/PyPI：考虑弃用问题版本
5. 对于 GitHub Release：编辑或删除问题发布，创建修正版本
```

## 场景 9: 为项目添加新语言 / 框架

```
1. 先开设 discussion/issue 以获得维护者的同意
2. 为新语言添加适当的 linter/formatter 配置
3. 为新语言的构建/测试流水线添加 CI 步骤
4. 遵循现有项目的目录结构约定
5. 为新组件添加文档
```

## 场景 10: 发布新项目

从零开始创建和发布全新开源项目时：

```
1. 初始化 git 仓库: git init && git checkout -b main
2. 为你的语言/生态系统设置 .gitignore
3. 添加 LICENSE 文件（参见许可证指南部分）
4. 编写 README.md（默认中文（简体））：
   - 项目名称和一行描述
   - 安装 / 快速入门说明
   - 使用示例
   - 许可证徽章和链接
5. 添加源代码，每个文件包含许可证头部
6. 为你的生态系统设置 linter/formatter 配置：
   - JS/TS: .eslintrc / biome.json / .prettierrc + editorconfig
   - Python: pyproject.toml / ruff.toml / .flake8
   - Rust: rustfmt.toml + clippy 配置
   - Go: golangci.yml
   - 其他：使用生态系统适配的工具
7. 添加 CONTRIBUTING.md（默认中文（简体）），包括：
   - 如何设置开发环境
   - 如何运行测试
   - 提交消息格式（推荐 Conventional Commits）
   - PR 流程和审查期望
8. 添加 CODE_OF_CONDUCT.md（例如 Contributor Covenant）
9. 设置 CI/CD 流水线：
   - GitHub Actions / GitLab CI 等
   - 最低要求：PR 时执行 lint + test
   - 可选：自动发布、覆盖率报告
10. 配置 .github/ 目录：
    - ISSUE_TEMPLATE/（缺陷报告、功能请求）
    - PULL_REQUEST_TEMPLATE.md
    - CODEOWNERS（如果是多维护者项目）
11. 初始提交: chore: 初始化项目
12. 创建 GitHub 仓库：
    gh repo create <名称> --public --description "<描述>"
13. 推送: git remote add origin <地址> && git push -u origin main
14. （可选）创建初始发布：
    - 标签: v0.1.0 或 v1.0.0
    - 发布说明描述初始版本
    - 上传可分发产物
```

**新项目清单：**

```
- [ ] .gitignore 覆盖所有构建产物、密钥、操作系统文件
- [ ] LICENSE 文件存在，包含正确的年份和版权持有人
- [ ] README.md 包含描述、安装、使用和许可证信息
- [ ] 所有源文件有许可证头部
- [ ] Linter/formatter 已配置并通过
- [ ] CONTRIBUTING.md 说明了如何贡献
- [ ] CODE_OF_CONDUCT.md 已存在
- [ ] CI 流水线在每个 PR 上运行 lint + test
- [ ] GitHub issue/PR 模板已配置
- [ ] 初始提交已推送到 main 分支
- [ ] 仓库描述和主题已在 GitHub 上设置
- [ ] 任何文件中均无密钥、凭证或个人数据
```

---

# 代理配置参考

本节帮助 AI 代理在加载此技能时进行自我配置。

## 自动检测清单

在开始处理新项目时，代理应该：

```
1. [ ] 搜索约定文件（第 0 节）
2. [ ] 检测协作语言（第 1.1 节）
3. [ ] 检测版本控制策略（第 4 节，步骤 2）
4. [ ] 检测包管理器（检查锁文件）
5. [ ] 检测构建系统（检查 package.json scripts、Makefile 等）
6. [ ] 检测测试框架（检查测试目录、配置文件）
7. [ ] 检测 CI 系统（检查 .github/workflows、.gitlab-ci.yml 等）
8. [ ] 检测许可证类型（读取 LICENSE 文件）
9. [ ] 检测默认分支（git symbolic-ref refs/remotes/origin/HEAD）
10. [ ] 在合并/发布前执行所有权验证（第 4 节，步骤 5）
```

## 项目档案模板

代理可以为每个处理的项目填写此模板：

```
项目: <名称>
仓库: <地址>
所有者: <用户/组织>
默认分支: main | master | develop | ...
协作语言: zh | en | ja | ko | ... | 双语
版本控制: SemVer | CalVer | 自定义
包管理器: npm | yarn | pnpm | pip | cargo | go mod | ...
构建系统: <描述>
测试框架: jest | pytest | cargo test | go test | ...
CI 系统: GitHub Actions | GitLab CI | Jenkins | ...
许可证: MIT | Apache-2.0 | GPL-3.0 | ...
合并策略: merge | squash | rebase
找到的约定文件: <列表>
```

## 通用规则（覆盖项目检测）

无论项目检测结果如何，以下规则始终适用：

1. **永远不要**在项目文件中输出工具签名或品牌水印
2. **永远不要**提交密钥或敏感信息
3. **始终**在合并/发布前验证所有权
4. **始终**在进行更改前搜索约定文件
5. **始终**遵循约定式提交（Conventional Commits），除非项目明确使用其他格式
6. **始终**匹配正在修改的文件中的现有代码风格

## 参考资料

- [约定式提交](https://www.conventionalcommits.org/)
- [语义化版本](https://semver.org/)
- [维护变更日志](https://keepachangelog.com/)
- [GitHub CLI 手册](https://cli.github.com/manual/)
- [SPDX 许可证列表](https://spdx.org/licenses/)
- [选择许可证](https://choosealicense.com/)
