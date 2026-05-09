---
name: four-color-review-continue
description: Use only when the user explicitly invokes `/four-color-review-continue <document-path>` to continue a previously generated four-color review score document.
---

# four-color-review-continue

## Overview

This command-only entrypoint continues a previously generated four-color review score document. It must accept only valid score documents and continue only unresolved items below 6/10.

## When to Use

Use this skill only through this explicit slash command:
- `/four-color-review-continue <document-path>` — continue unresolved items below 6/10 from a previously generated score document

Do not use fuzzy semantic triggering for this skill. It is a command-driven review tool.

## Command Requirements

1. The input document must be a previously generated four-color review score document.
2. The input document must carry the fixed score-document marker at the beginning of the file.
3. Only unresolved items scored below 6/10 may be continued.
4. The continued review must ask every required risky follow-up one by one, instead of stopping after only the first risky question.
5. The continued review must generate a new review document file rather than overwrite the old one.
6. The new review document filename must use the source requirement document name plus `-评审-日期-版本`.
7. All user questions must follow the same `AskUserQuestion` single-question serial loop used by the main four-color-review skill.
8. Each asked question must identify the current story-flow step and explain what closure or story completeness is blocked if the answer stays unclear.
9. The output document must preserve source document path, inherited optimization record if present, question and answer log, risk register, per-item scores, and final score.

## Delegation

Apply the same review workflow, blocking rules, output structure, document lineage rules, and Chinese heading rules defined in the main `four-color-review` skill.
