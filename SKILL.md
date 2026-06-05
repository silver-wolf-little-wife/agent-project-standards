---
name: project-standards
description: "Generic GitHub open-source project conventions for any AI agent: (0) mandatory project style/convention file search before code changes, (1) collaboration standards (language specs + behavior specs), (2) shell-aware CLI text handling, (3) release workflow with ownership verification, (4) license guide. Use when creating PRs/releases, making code changes, bumping versions, or managing licenses."
---

# Open Source Project Standards

A universal conventions guide for AI agents and developers collaborating on GitHub open-source projects. All rules are project-agnostic — the agent must detect and follow each project's own conventions first.

Covers seven core sections:

1. [Project Convention File Search (Mandatory First Step)](#project-convention-file-search-mandatory-first-step)
2. [GitHub Collaboration Standards](#github-collaboration-standards) (Language Specs + Behavior Specs)
3. [Shell CLI Multi-line Text Handling](#shell-cli-multi-line-text-handling)
4. [Release Workflow with Ownership Verification](#release-workflow-with-ownership-verification)
5. [Open-Source License Guide](#open-source-license-guide)
6. [Common Development Scenarios](#common-development-scenarios)
7. [Agent Configuration Reference](#agent-configuration-reference)

---

# Project Convention File Search (Mandatory First Step)

> **MANDATORY: Before making ANY code change in a project, the agent MUST perform the convention file search described in this section. This step is never skippable.**

## Search Procedure

### Step 1: Search for Convention Files

Before modifying any project, proactively search for these convention/style files:

| Priority | File Name | Purpose |
|----------|-----------|---------|
| HIGH | `STYLE_GUIDE.md` / `STYLEGUIDE.md` / `style-guide.md` | Project style guide |
| HIGH | `.editorconfig` | Editor format config (indent, EOL, encoding) |
| HIGH | `.eslintrc*` / `biome.json` / `.prettierrc*` | JS/TS formatting |
| HIGH | `pyproject.toml` / `setup.cfg` / `.flake8` / `tox.ini` | Python formatting |
| HIGH | `rustfmt.toml` / `rust-toolchain.toml` | Rust formatting |
| HIGH | `.rubocop.yml` / `Gemfile` (Ruby) | Ruby formatting |
| HIGH | `golangci.yml` / `.golangci.yaml` | Go linting |
| HIGH | `checkstyle.xml` / `spotless.gradle` (Java/Kotlin) | Java/Kotlin formatting |
| HIGH | `.swiftlint.yml` | Swift formatting |
| HIGH | `.clang-format` / `.clang-tidy` | C/C++ formatting |
| MEDIUM | `CONTRIBUTING.md` / `CONTRIBUTING` | Contribution guidelines |
| MEDIUM | `CODE_OF_CONDUCT.md` | Code of conduct |
| MEDIUM | `.github/CODEOWNERS` | Code review owners |
| MEDIUM | `.github/ISSUE_TEMPLATE/*` / `.github/PULL_REQUEST_TEMPLATE*` | Issue/PR templates |
| MEDIUM | `ARCHITECTURE.md` / `DESIGN.md` | Architecture decisions |
| LOW | `README.md` convention paragraphs | Convention info in README |

### Step 2: Search Commands

**Bash / Zsh:**

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

**PowerShell:**

```powershell
Get-ChildItem -Path . -Recurse -Depth 3 -Include `
  "STYLE_GUIDE*","STYLEGUIDE*",".editorconfig","CONTRIBUTING*","CODE_OF_CONDUCT*",
  ".prettierrc*",".eslintrc*","biome.json","pyproject.toml","rustfmt.toml",
  ".rubocop.yml","golangci.yml",".golangci*","checkstyle.xml",
  ".clang-format",".swiftlint.yml",".flake8","setup.cfg","ARCHITECTURE.md" `
  -ErrorAction SilentlyContinue
```

### Step 3: Read and Comply

- If files found → **MUST read their contents and strictly follow the format requirements**
- Multiple conflicting convention files → Higher priority wins (custom project guide > tool config > generic template)
- No convention files found → Follow this document's defaults

### Priority Rule

```
Project's own convention files > This document's defaults > General industry conventions
```

### Monorepo Awareness

In monorepo projects (workspaces with multiple packages), search at:
- Repository root
- Each package/workspace directory
- The specific package being modified

The most specific (closest to the code being changed) convention takes precedence.

---

# GitHub Collaboration Standards

This section defines the language and behavior standards for collaborating on GitHub open-source projects. These standards span the entire collaboration lifecycle: code writing, PR submission, issue discussion, and release publishing.

---

## Part I: Language Specs

### 1.1 Collaboration Language Detection

**The agent MUST auto-detect the project's collaboration language before generating any text output.** Detection priority:

1. **CONTRIBUTING.md / STYLE_GUIDE.md** explicitly specifies a language → Use that language
2. **README.md** primary language → Use that language for collaboration text
3. **Existing PR/Issue/commit history** dominant language → Use that language
4. **User instruction** → If the user explicitly requests a specific language, use it
5. **Fallback** → English

The detected language applies to: PR titles and descriptions, commit messages, issue titles and descriptions, release notes, code review comments, and all collaborator communication.

### 1.2 Commit Message Format

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): short description

[optional body]
```

**Types:**

| Type | Meaning |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation changes |
| `chore` | Build / tooling / deps |
| `refactor` | Restructure (no behavior change) |
| `test` | Test-related changes |
| `style` | Formatting (no logic change) |
| `perf` | Performance optimization |
| `ci` | CI/CD configuration changes |
| `build` | Build system or external dependencies |
| `revert` | Revert a previous commit |

**Scope (optional):** Identifies the affected area, e.g., `feat(auth):`, `fix(ui):`, `build(deps):`. In monorepos, use the package name as scope.

**Breaking changes:** Append `!` after type/scope, or add `BREAKING CHANGE:` footer:

```
feat(api)!: remove deprecated user endpoint

BREAKING CHANGE: The /v1/users endpoint has been removed.
Use /v2/users instead.
```

### 1.3 Branch Naming

```
type/short-description
```

Examples: `feat/add-dark-mode`, `fix/memory-leak`, `docs/update-api-docs`, `refactor/config-module`

Use `kebab-case` for the description. If the project has its own convention (e.g., `feature/` instead of `feat/`), follow the project's convention.

### 1.4 Source Code Comment Language

| Scenario | Language |
|----------|----------|
| Public API documentation (docstring / JSDoc) | Follow project convention; default to project's primary language |
| Internal implementation comments | Match existing comments in the same file |
| New files | Follow project convention; default to project's primary language |
| Third-party library wrappers/extensions | Match the original library's language |

**Key principle:** New comments MUST match the language of existing comments in the same file. Never mix comment languages within a single file unless the project explicitly allows it.

### 1.5 Variable and Function Naming

- Follow the project's existing naming style (`camelCase`, `snake_case`, `PascalCase`, `SCREAMING_SNAKE_CASE`, etc.)
- **Never** mix naming styles within the same file
- New code must match adjacent code's naming
- If linter/formatter config exists, follow its rules

### 1.6 PR / Issue Writing

**PR title:** Follow Conventional Commits format, use the collaboration language

**PR description template:**

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

If the project has a PR template (`.github/PULL_REQUEST_TEMPLATE*`), use that template instead.

**Issue titles:** Concise, use the collaboration language

```
[Bug] Login page occasionally shows blank screen
[Feature] Support custom theme colors
[Question] How to configure multi-instance deployment
```

### 1.7 Release Notes Language

- If the project's `CHANGES.md` / `CHANGELOG.md` uses bilingual format → Release Notes should be bilingual
- If the project uses a single language → Stay consistent
- No convention found → Use the detected collaboration language

### 1.8 Dependency Updates

When updating dependencies:

- Follow the project's lock file strategy (`package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`, `Pipfile.lock`, `Cargo.lock`, `go.sum`, etc.)
- Never commit changes to lock files that are inconsistent with manifest changes
- Use the project's existing package manager — don't introduce a new one
- Group related dependency updates into a single commit/PR (e.g., `chore(deps): update react to v19`)
- Isolate breaking dependency updates into separate PRs

---

## Part II: Behavior Specs

### 2.1 Respect Existing Code Style

**This is the most important behavioral rule.** Before modifying any code:

1. **MUST search** (see [Convention File Search](#project-convention-file-search-mandatory-first-step)) for style guides, `.editorconfig`, linter configs
2. **MUST read** existing source files in the same directory to understand the project's code style, including: indent style (tab/space, width), quote style (single/double), trailing comma policy, import ordering, error handling patterns
3. **MUST follow** the project's existing style, even if it differs from your preference
4. **Never** introduce format changes inconsistent with the project style in the same PR

### 2.2 Minimal Change Principle

- PRs should contain ONLY changes directly related to the PR's purpose
- **Never** bundle unrelated formatting, refactoring, or comment changes in a feature PR
- Large-scale formatting changes should be a separate `style:` PR
- Don't modify code you don't understand
- Don't delete code that looks "redundant" unless you're certain

### 2.3 No Unrelated Changes

The following should NOT appear in feature/fix PRs (unless they are the PR's purpose):

- Unrelated import additions/removals
- Unrelated file renames
- Formatting changes in unrelated code regions
- Personal preference style changes
- IDE-generated config changes

### 2.4 Testing and Quality

- Ensure modified code passes the project's existing tests
- New features should include corresponding test cases
- Bug fixes should preferably add regression tests
- Run the project's existing lint/format checks before committing
- Don't reduce existing test coverage

### 2.5 Communication Standards

- Critique code, not people: In code reviews, discuss code quality only
- Constructive feedback: Raise issues with suggested solutions
- Use the collaboration language for all communication
- Respect project maintainers' final decisions
- Search for similar existing issues before opening new ones
- Don't @ mention people unless their attention is genuinely needed

### 2.6 Security and Privacy

- **Never** commit secrets, passwords, tokens, or API keys in code, commit messages, or PR descriptions
- Don't disclose user privacy data in issues
- Use `.gitignore` to exclude sensitive files
- Report security vulnerabilities through private channels (Security Advisory), not public issues

### 2.7 License Compliance

- New source files must include a license header (see [License Guide](#open-source-license-guide))
- Verify third-party code license compatibility before including it
- Preserve existing copyright notices — never delete others' copyright info
- When modifying others' code, note changes in the file header as appropriate

### 2.8 No Tool Signatures in Project Output

**Strictly prohibited** to embed AI tool signatures, generation markers, or brand watermarks in any project output, including but not limited to `Generated with ...`, `Co-authored-by: ...bot`, etc.:

- Commit messages (both title and body)
- PR titles and descriptions
- Issue titles and descriptions
- Release notes / Changelogs
- Source code file contents
- Documentation files
- Configuration files

**Rationale:** An open-source project's commit history and documentation are the project's own assets. Tool signatures pollute the git history, reduce professionalism, and may cause dissatisfaction among project maintainers and the community.

### 2.9 Pre-commit Checklist

Before submitting any commit or creating a PR, confirm:

```
- [ ] Searched and read project convention files (STYLE_GUIDE.md, etc.)
- [ ] Read existing code in the same directory to understand project style
- [ ] Code style matches project's existing style
- [ ] No unrelated changes
- [ ] Tests pass
- [ ] Lint/format checks pass
- [ ] New files have license headers
- [ ] No sensitive information leaked
- [ ] Commit message follows Conventional Commits format
- [ ] No tool signatures/brand watermarks in any output
- [ ] Dependency changes follow project's package manager strategy
```

---

## Part III: PR and Release Workflow

### Standard PR Flow

```
1. Fork / Create branch → 2. Search conventions → 3. Code & test → 4. Commit → 5. Create PR → 6. Ownership check → 7. Review → 8. Merge (conditional)
```

**Detailed steps:**

1. **Create branch**: From `main` (or project's default branch), naming follows [Branch Naming](#13-branch-naming)
2. **Search conventions**: Execute [Convention File Search](#project-convention-file-search-mandatory-first-step)
3. **Code and test**: Follow project style, ensure existing tests and lint pass
4. **Commit**: Follow [Conventional Commits](#12-commit-message-format) format
5. **Create PR**:
   - Title follows [PR Writing Standards](#16-pr--issue-writing)
   - Use PR template if project has `.github/PULL_REQUEST_TEMPLATE*`
   - Use `--body-file` or `--notes-file` for multi-line content (see [CLI Text Handling](#shell-cli-multi-line-text-handling))
   - Link related issues (`Closes #xxx`)
6. **Ownership check**: Execute [Ownership Verification](#step-5-ownership-verification). If mismatch, keep PR only
7. **Code Review**: Be patient, respond promptly, describe changes clearly
8. **Merge** (only when ownership matches): Use `--merge --delete-branch`

### Release Publishing

Follow the [Release Workflow](#release-workflow-with-ownership-verification) section.

---

# Shell CLI Multi-line Text Handling

Different shells handle newline characters differently. Using the wrong syntax causes literal `\n` text in PR/Release descriptions.

## Shell Comparison

| Shell | Newline Escape | Note |
|-------|---------------|------|
| **Bash / Zsh** | `\n` | Standard escape sequence |
| **PowerShell** | `` `n `` | Backtick+n, `\n` is treated as **literal text** |
| **Fish** | `\n` | Standard escape sequence |

## Recommended Approach: File-based (All Shells)

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

## Bash / Zsh Inline

```bash
gh pr create --title "feat: new feature" --body "$(cat <<'EOF'
## Changes

Description with `code` snippets.
EOF
)"
```

## PowerShell Inline

```powershell
# Here-String
gh pr create --title "feat: new feature" --body @"
## Changes

Description...
"@

# Or backtick newlines
gh pr create --title "feat: new feature" --body "## Changes`n`nDescription"
```

## Key Rules

1. **Prefer `--body-file` / `--notes-file`** for all `gh` commands with multi-line content
2. **PowerShell: never use `\n`** in double-quoted strings — it won't be interpreted as a newline
3. **PowerShell: content containing backticks** must use `--body-file`, otherwise the shell will swallow backtick-wrapped content
4. Applies to all `gh` commands: `pr create/edit`, `release create/edit`, `issue create/edit`

---

# Release Workflow with Ownership Verification

**Core principle: Follow the project's versioning strategy. Verify ownership before merge/release.**

> **IMPORTANT: Merge and release operations are restricted by ownership verification. Only when the current git user matches the repository owner is merging and Release publishing allowed; otherwise, only create a PR.**

## Workflow Overview

```
1. Code changes → 2. Version bump → 3. Branch & commit → 4. Create PR → 5. Ownership check → 6. Merge (conditional) → 7. Release (conditional)
```

## Step 1: Code Changes

Implement fixes or features. Follow the project's line ending and encoding conventions (check `.editorconfig` and `.gitattributes`).

**Prerequisite:** Must execute [Convention File Search](#project-convention-file-search-mandatory-first-step) before coding.

## Step 2: Version Bump

**Detect the project's versioning strategy:**

| Strategy | Detection | Example |
|----------|-----------|---------|
| **SemVer** | Existing tags like `v1.2.3` or `CHANGELOG.md` with SemVer | `v1.2.3` → `v1.3.0` (minor) or `v1.2.4` (patch) |
| **CalVer** | Tags like `2024.06.1` or date-based versions | Follow the project's date format |
| **Custom** | Project-specific version files or patterns | Follow existing convention |

**Files to update** (if they exist):

- `package.json` / `pyproject.toml` / `Cargo.toml` / `go.mod` / `*.csproj` / etc.
- `CHANGELOG.md` / `CHANGES.md` / `HISTORY.md`
- `README.md` version references
- Any project-specific version files

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

## Step 3: Branch, Commit, Push

**Branch naming:** Follow [Branch Naming](#13-branch-naming).

**Commit message:** Follow [Conventional Commits](#12-commit-message-format).

## Step 4: Create PR

Use `--body-file` for multi-line content. See [CLI Text Handling](#shell-cli-multi-line-text-handling).

PR title and description follow [PR/Issue Writing Standards](#16-pr--issue-writing).

## Step 5: Ownership Verification

> **This step is MANDATORY and cannot be skipped. Must execute before merging.**

After creating the PR and before merging, verify that the current git user identity matches the remote repository owner.

### Verification Method

```bash
# 1. Get local git user name
git config user.name

# 2. Get remote repository owner (GitHub username/org)
gh repo view --json owner --jq ".owner.login"

# 3. Get currently logged-in GitHub user
gh api user --jq ".login"
```

### Decision Rules

Compare these three values:

| Info | Command | Example |
|------|---------|---------|
| Local git user | `git config user.name` | `ZhangSan` |
| GitHub logged-in user | `gh api user --jq ".login"` | `zhangsan` |
| Repository owner | `gh repo view --json owner --jq ".owner.login"` | `zhangsan` or `my-org` |

**Match:** GitHub logged-in user (case-insensitive) equals repository owner → **Continue to Step 6 and Step 7** (merge + release)

**Mismatch:** GitHub logged-in user differs from repository owner → **STOP immediately. Keep PR only. No merge, no release.** Notify the repository owner to review and merge.

### Organization Repositories

For organization-owned repositories, also check team membership:

```bash
# Check if current user is a member of the org
gh api orgs/<org-name>/members/<username> --silent 2>&1
# Exit code 0 = member, non-zero = not a member
```

Even if the user is an org member, the merge/release is gated by the project maintainer's explicit permission (via CODEOWNERS approval or direct instruction from the user running the agent).

### Actions on Mismatch

1. Add a PR comment @ the repository owner, explaining the PR is ready for review
2. Report the ownership mismatch to the user running the agent
3. **Prohibited:** Auto-executing `gh pr merge` or `gh release create`

## Step 6: PR Merge (Only When Ownership Matches)

**Prerequisite: Step 5 result is "match". Otherwise, skip this step.**

```bash
gh pr merge <number> --merge --delete-branch
git checkout main && git pull origin main
```

Merge strategy follows the project's preference:
- `--merge` — standard merge commit
- `--squash` — squash all commits into one
- `--rebase` — rebase on target branch

If the project enforces a specific merge strategy via branch protection rules, follow that.

## Step 7: Release (Only When Ownership Matches)

**Prerequisite: Step 5 result is "match" AND Step 6 has been executed. Otherwise, skip this step.**

### Release Notes Format

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

### Create Release

```bash
# Write notes to file first (recommended for all shells)
gh release create vX.Y.Z --title "vX.Y.Z — Summary" --notes-file release-notes.md
```

### Release Assets

If the project builds distributable artifacts:

1. Follow the project's build instructions (usually in `README.md`, `CONTRIBUTING.md`, or `Makefile`)
2. Build artifacts using the project's standard build toolchain
3. Upload assets:

```bash
gh release upload vX.Y.Z "artifact-file" --clobber
```

**Common build/publish scenarios:**

| Ecosystem | Build Command | Publish |
|-----------|---------------|---------|
| npm | `npm run build` | `npm publish` |
| PyPI | `python -m build` | `twine upload dist/*` |
| Cargo | `cargo build --release` | `cargo publish` |
| Go | `goreleaser release` | Automatic via goreleaser |
| Docker | `docker build` | `docker push` |
| GitHub Release | Build per project docs | `gh release upload` |

### Post-Release

- Delete local build artifacts (zip, tar.gz, etc.)
- Verify the release page on GitHub shows all expected assets
- Verify package registry (if applicable) shows the new version

## Release Checklist

```
- [ ] Searched and read project convention files
- [ ] Version bumped according to project's versioning strategy
- [ ] CHANGELOG updated (if project has one)
- [ ] README version references updated (if applicable)
- [ ] Line endings match project convention
- [ ] PR created with --body-file
- [ ] Ownership verification executed (Step 5)
- [ ] Ownership match: PR merged, Release created, assets uploaded
- [ ] Ownership mismatch: PR only, owner notified
- [ ] Local build artifacts cleaned up
```

## Common Pitfalls

| Problem | Cause | Fix |
|---------|-------|-----|
| Release notes lose code snippets | Shell swallows backticks | Use `--notes-file` |
| PR body shows literal `\n` | PowerShell doesn't parse `\n` | Use Here-String or file |
| Batch files garbled | Wrong line endings | `.gitattributes` enforce CRLF |
| Accidentally merge/release on someone else's repo | Skipped ownership check | Must execute Step 5 |
| Wrong version bump | Ignored project's versioning strategy | Detect strategy from existing tags |

---

# Open-Source License Guide

## Choosing a License

| License | Type | Key Characteristics | Best For |
|---------|------|---------------------|----------|
| **MIT** | Permissive | Very short, allows everything | Libraries, tools, most projects |
| **Apache 2.0** | Permissive | Includes patent grant | Corporate open-source, large projects |
| **BSD 2-Clause** | Permissive | Similar to MIT, slightly different wording | Academic projects |
| **BSD 3-Clause** | Permissive | BSD 2 + no-endorsement clause | Academic projects needing name protection |
| **GPL-3.0** | Copyleft | Derivatives must be GPL-3.0 | Projects wanting to ensure openness |
| **LGPL-3.0** | Weak copyleft | Linking allowed in proprietary code | Libraries wanting wide adoption + openness |
| **AGPL-3.0** | Strong copyleft | Network use = distribution | Web services, SaaS |
| **MPL-2.0** | File-level copyleft | Modified files must be open-source | Projects mixing open and proprietary |
| **Unlicense / CC0** | Public domain | No restrictions | Releasing into public domain |

## Common License Templates

### MIT License

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

### Apache License 2.0

Use the full standard Apache 2.0 text (available at https://www.apache.org/licenses/LICENSE-2.0). Add a `NOTICE` file if the project requires attribution notices.

### BSD 3-Clause

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

## Source File Header

Adapt comment syntax per language: Python `#`, JS/TS/C/C++/Java/Go `//`, Batch `REM`, HTML `<!-- -->`

**Short header (recommended for permissive licenses):**

```python
# Copyright (c) <year> <copyright holder>
# SPDX-License-Identifier: <SPDX-ID>
```

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

## Common Mistakes

1. **Pasting full legal text** for GPL (700 lines) → Use the short declaration template
2. **Missing "or later version"** for GPL → Must include `either version 3 of the License, or (at your option) any later version`
3. **Confusing GPL and CC** → GPL is for software, CC is for creative works
4. **No SPDX identifier** → Always add `SPDX-License-Identifier` in file headers for machine readability
5. **Mixing incompatible licenses** → Verify license compatibility before combining code from different projects

## References

- [SPDX License List](https://spdx.org/licenses/)
- [Choose a License](https://choosealicense.com/)
- [GPL-3.0 Official](https://www.gnu.org/licenses/gpl-3.0.html)
- [How to Apply GPL](https://www.gnu.org/licenses/gpl-howto.html)

---

# Common Development Scenarios

## Scenario 1: Fork Workflow (Contributing to Others' Projects)

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

**Key rules for forks:**
- Never push directly to the upstream repository
- Keep your fork's default branch synced with upstream
- Always create PRs from feature branches, not from your default branch

## Scenario 2: Hotfix / Urgent Fix

```
1. Create branch from the release tag or main: git checkout -b hotfix/fix-critical-bug v1.2.3
2. Fix the issue
3. Bump patch version: v1.2.3 → v1.2.4
4. Create PR, ownership check, merge if match
5. Create release with hotfix
6. Cherry-pick fix to main branch if applicable
```

## Scenario 3: Dependency Update

```
1. Create branch: chore/update-react-19
2. Update package manifest (package.json, pyproject.toml, etc.)
3. Run install to update lock file
4. Run full test suite
5. Fix any breaking changes
6. Commit: chore(deps): update react to v19
7. Create PR with detailed changelog of breaking changes
```

## Scenario 4: Monorepo Changes

```
1. Identify affected packages
2. Search conventions in each package directory
3. Create branch: feat/package-name/feature-description
4. Only modify affected packages — avoid touching others
5. Run tests for affected packages AND integration tests
6. Use scope in commit message matching the package name
7. Follow the monorepo's release tooling (changesets, lerna, nx, turborepo, etc.)
```

## Scenario 5: Documentation-Only Changes

```
1. Create branch: docs/update-readme
2. Follow project's documentation style (Markdown, reStructuredText, etc.)
3. Verify links work
4. Check spelling and grammar
5. Commit: docs: update README with new API examples
6. No version bump needed (unless project convention requires it)
```

## Scenario 6: CI/CD Configuration Changes

```
1. Create branch: ci/add-deploy-step
2. Test CI changes locally if possible (act, docker-compose, etc.)
3. Ensure changes don't break existing pipelines
4. Commit: ci: add staging deployment step
5. Create PR — note that CI changes may require admin approval
```

## Scenario 7: Large Refactoring

```
1. Discuss with maintainers first (open an issue or discussion)
2. Create branch: refactor/module-name
3. Break into small, reviewable commits
4. Each commit should keep tests passing
5. Use fixup commits for review feedback, squash before merge
6. Ensure test coverage doesn't decrease
```

## Scenario 8: Reverting a Bad Release

```
1. Create branch: revert/bad-release
2. git revert <bad-commit-hash> (or revert the merge commit)
3. If the release was published, publish a corrected version
4. For npm/PyPI: consider deprecating the bad version
5. For GitHub Release: edit or delete the bad release, create corrected one
```

## Scenario 9: Adding a New Language / Framework to a Project

```
1. Open a discussion/issue first to get maintainer buy-in
2. Add appropriate linter/formatter config for the new language
3. Add CI steps for the new language's build/test pipeline
4. Follow the existing project's directory structure conventions
5. Add documentation for the new component
```

---

# Agent Configuration Reference

This section helps AI agents configure themselves when loading this skill.

## Auto-detection Checklist

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

## Project Profile Template

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

## Universal Rules (Override Project Detection)

These rules always apply regardless of project detection:

1. **Never** output tool signatures or brand watermarks in project files
2. **Never** commit secrets or sensitive information
3. **Always** verify ownership before merge/release
4. **Always** search convention files before making changes
5. **Always** follow Conventional Commits unless the project explicitly uses a different format
6. **Always** match existing code style in the files being modified

## References

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [GitHub CLI Manual](https://cli.github.com/manual/)
- [SPDX License List](https://spdx.org/licenses/)
- [Choose a License](https://choosealicense.com/)
