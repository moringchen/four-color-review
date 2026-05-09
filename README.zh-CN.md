# four-color-review

English documentation: [README.md](README.md)

## 这是什么

`four-color-review` 是一个通过显式 slash 命令使用的需求评审技能，用于结合讲故事法和四色建模法审查业务需求、记录风险点并输出 Markdown 评审文档。

## 安装方式

请先进入 Claude Code 会话，再使用插件安装命令：

```text
/plugin marketplace add moringchen/four-color-review
/plugin marketplace install moringchen/four-color-review
```

## Slash 命令

```text
/four-color-review
/four-color-review:continue <review-doc.md>
/four-color-review:all <review-doc.md>
```

- `/four-color-review`：发起一次新的评审
- `/four-color-review:continue`：继续处理 `unresolved` 且低于 `6/10` 的风险项
- `/four-color-review:all`：对全部问题重新回复和重评分

## 输出文档说明

评审文档通常包含：
- Requirement Summary
- Storyline
- Closure Check
- Four-Color Extraction
- Risk Register
- Health Score Summary
- Domain Suggestions

## 风险分与健康分

- 每个问题都有 `0-10` 的 `Health Score`
- 最终会汇总成 `0-100` 的 `Final Health Score`
- 分数越低，风险越高

## 优化后的需求文档

评审文档输出后，skill 会询问是否基于当前评审结果生成一份优化后的需求文档。

## 已知限制

- 某些 `claude -p` 场景会先拦截 slash 前缀
- print-mode 评测不能完全代表真实交互会话效果
- 该 skill 通过显式 slash 命令使用
