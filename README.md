# four-color-review

## What it is

`four-color-review` is a Claude skill for developers and architects who want to review whether a business requirement, workflow, or PRD tells a complete story before moving into implementation.

## When to use

Use it when you need to:
- review whether a requirement is closed-loop
- find missing responsibilities or failure handlers
- extract four-color domain entities
- produce lightweight domain suggestions without designing implementation details

## Slash command

Invoke with:

```text
/four-color-review
```

Continuation modes:

```text
/four-color-review -c <review-doc.md>
/four-color-review -a <review-doc.md>
```

- `-c` continues only unresolved issues scored below 6/10.
- `-a` re-asks all recorded issues regardless of prior score or resolution.

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
