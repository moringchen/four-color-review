# four-color-review

## What it is

`four-color-review` is a Claude skill for developers and architects who want to review whether a business requirement, workflow, or PRD tells a complete story before moving into implementation.

## Installation

### Claude Code local skills directory

Install by placing this repo at:

```text
~/.claude/skills/four-color-review
```

Or symlink it from your working copy:

```bash
ln -s /absolute/path/to/four-color-review ~/.claude/skills/four-color-review
```

After that, restart Claude Code or start a new session so the skill is discovered.

## When to use

Use this skill only through explicit slash commands. It is a command-driven review tool and no longer relies on fuzzy semantic triggering.

## Slash commands

```text
/four-color-review
/four-color-review:continue <review-doc.md>
/four-color-review:all <review-doc.md>
```

- `/four-color-review` starts a new review
- `/four-color-review:continue` continues unresolved issues below 6/10
- `/four-color-review:all` re-asks all recorded issues

## Risk scoring

- Each recorded issue gets a `Health Score` from `0-10`.
- Lower scores mean higher risk and weaker business closure.
- The review document ends with a final `0-100` health score.

## Optional optimized requirements document

After the review document is complete, the skill asks whether to generate an optimized requirements document from the reviewed content.

## Example input

```text
/four-color-review 帮我看这个退款流程是否闭环：商家提交退款申请，财务审批通过后退款原路返回；如果原路返回失败，需要人工介入。
```

## Example output shape

1. Requirement Summary
2. Storyline
3. Closure Check
4. Missing Information Questions
5. Four-Color Extraction
6. Responsibility and Relationship Analysis
7. Risk Register
8. Health Score Summary
9. Domain Suggestions
10. Optional optimized requirements document prompt

## Review document behavior

The skill is designed to produce a Markdown review document that can be saved directly.

Typical review records include:
- unresolved risks
- per-question health scores
- a final overall health score
- follow-up prompts for missing information

## Known limitations

- Slash-prefixed prompts in some `claude -p` evaluation flows may be intercepted by the CLI before reaching the model.
- The skill works best in normal interactive Claude Code sessions.
- Continuation flows such as `:continue` and `:all` are implemented, but automated print-mode benchmarking may underrepresent their real interactive behavior.

## 中文说明

### 这是什么

`four-color-review` 是一个通过显式 slash 命令使用的需求评审技能，用于结合讲故事法和四色建模法审查业务需求、记录风险点并输出 Markdown 评审文档。

### 安装方式

将仓库放到：

```text
~/.claude/skills/four-color-review
```

或者使用软链接：

```bash
ln -s /absolute/path/to/four-color-review ~/.claude/skills/four-color-review
```

安装后请重启 Claude Code 或开启新会话。

### Slash 命令

```text
/four-color-review
/four-color-review:continue <review-doc.md>
/four-color-review:all <review-doc.md>
```

- `/four-color-review`：发起一次新的评审
- `/four-color-review:continue`：继续处理 `unresolved` 且低于 `6/10` 的风险项
- `/four-color-review:all`：对全部问题重新回复和重评分

### 输出文档说明

评审文档通常包含：
- Requirement Summary
- Storyline
- Closure Check
- Four-Color Extraction
- Risk Register
- Health Score Summary
- Domain Suggestions

### 风险分与健康分

- 每个问题都有 `0-10` 的 `Health Score`
- 最终会汇总成 `0-100` 的 `Final Health Score`
- 分数越低，风险越高

### 优化后的需求文档

评审文档输出后，skill 会询问是否基于当前评审结果生成一份优化后的需求文档。

### 已知限制

- 某些 `claude -p` 场景会先拦截 slash 前缀
- print-mode 评测不能完全代表真实交互会话效果
- 该 skill 以显式 slash 命令为唯一入口，不再依赖模糊语义触发
