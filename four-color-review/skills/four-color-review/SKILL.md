---
name: four-color-review
description: Use only when the user explicitly invokes one of these slash commands: `/four-color-review`, `/four-color-review-continue <document-path>`, or `/four-color-review-all <document-path>`.
---

# four-color-review

## Overview

Use storytelling first and four-color modeling second. The goal is to verify that the business narrative is coherent and closed-loop before extracting domain entities and giving lightweight domain suggestions.

## When to Use

Use this skill only through these explicit slash commands:
- `/four-color-review` — start a new review
- `/four-color-review-continue <document-path>` — continue unresolved items below 6/10
- `/four-color-review-all <document-path>` — re-ask and re-score all recorded issues

Do not use fuzzy semantic triggering for this skill. It is a command-driven review tool.

## Workflow

1. Restate the requirement in 3-5 sentences.
2. Reconstruct the business story in this order:
   - who initiates
   - under what trigger or context
   - what key actions happen
   - how the system or business responds
   - what result is produced
   - where the loop closes
3. After story reconstruction, output a dedicated `Storyline` section before any four-color grouping.
4. Check whether the story closes.
5. If a blocking gap exists, stop and ask exactly one clarifying question with `AskUserQuestion`.
6. If the user answers `pass`, keep going but record the issue in `Risk Register` with:
   - `User Action: pass`
   - `Status: unresolved`
   - `Health Score: x/10`
7. Score each open issue on a 0-10 health scale, where lower scores mean higher risk and weaker business closure.
8. Only after the story closes, output a dedicated `Four-Color Extraction` section using exactly these four subgroup labels:
   - Role objects
   - Core business objects
   - Rule/description objects
   - Time/event objects
9. Analyze responsibility and relationship gaps.
10. Output `Risk Register` and `Health Score Summary`, and report the final overall score on a 0-100 scale.
11. End with lightweight domain suggestions only.
12. Every four-color review run must create a new review document file rather than overwrite a prior one.
13. The review document filename must use the user-provided source requirement document name plus `-评审-日期-版本`.
14. Every review document must include a fixed score-document marker at the beginning of the file so it can be recognized as a valid four-color review score document.
15. The review document must record the source requirement document path.
16. If the source requirement document is itself an optimized requirements document, detect that from its optimization record section and copy that optimization record section into the new review document before adding the current review results.
17. After the review document is complete, ask whether the user wants an optimized requirements document generated from the current review using `AskUserQuestion`. If the user declines, end the turn.
18. If the user confirms generation of the optimized requirements document, save the generated document as a file in the current working directory before ending the turn.
19. The optimized requirements document filename must use the user-provided source requirement document name plus `-优化-日期-版本`.
20. The optimized requirements document must append an optimization record entry for the current generation, including timestamp, source review document path, and output document path.

## Question Interaction Rule

Whenever the skill needs the user to answer a question, it must use `AskUserQuestion` in cursor-style interactive selection form. Free-text questioning is not allowed by default and may only be used if the user explicitly selects `Other` in that interaction.

The skill must never ask a plain-text question directly in the response body when user input is required.

Each `AskUserQuestion` interaction must contain exactly one question. Do not batch multiple questions in a single interaction, even if they are related.

This rule applies to:
- blocking clarifying questions
- follow-up questions in `/four-color-review-continue` mode
- follow-up questions in `/four-color-review-all` mode
- the final confirmation about whether to generate an optimized requirements document
- any follow-up needed before saving the optimized requirements document

## Blocking Rules

Stop and ask before continuing if any of these are unclear:
- who owns the responsibility
- what event or state transition happens
- what result completes the flow
- what rule is carried by which object
- who handles failure, timeout, or manual intervention

When asking follow-up questions:
- ask one question at a time
- ask the smallest question that unblocks the current risk item
- if multiple risky gaps exist, ask all required risk questions one by one in blocking-priority order
- do not ask the next question until the current one has been answered
- continue this serial follow-up loop until all required risk questions for the current review mode have been covered
- each question must state the current story-flow step where the gap appears
- each question must explicitly explain what part of closure or story completeness is blocked by the missing information
- continue from the blocked step after each answer arrives
- do not invent assumptions and continue silently

## Output Structure

All section headings in both the review document and the optimized requirements document must use Chinese `##` headings.

Always structure the review output in this order:
1. 需求摘要
2. 源文档信息
3. 优化记录继承（仅当源文档是优化需求文档时）
4. 故事线
5. 闭环检查
6. 缺失信息问题（仅阻塞时出现）
7. 四色提取
8. 职责与关系分析
9. 问答记录
10. 风险登记
11. 健康分汇总
12. 领域建议

## Review Record Requirements

Each review document must explicitly record:
- the fixed score-document marker at the start of the file
- the source requirement document path
- whether the source document is an original requirement or an optimized requirements document
- every blocking question asked to the user
- every user answer received
- each recorded risk item
- each per-item health score
- the final overall health score

Each optimized requirements document must explicitly record:
- its own output file path
- the source review document path
- the current document version
- the current generation timestamp
- cumulative optimization history entries from prior optimized versions, if any
- the newly appended optimization history entry for the current generation

## Continuation Modes

- `/four-color-review-continue <document-path>` accepts only a previously generated four-color review score document carrying the fixed score-document marker, and continues only unresolved items scored below 6/10.
- `/four-color-review-all <document-path>` accepts only a previously generated four-color review score document carrying the fixed score-document marker, and re-asks all issues regardless of prior score or resolution state.

## Domain Suggestion Boundary

Stay at the concept and responsibility level. Good suggestions include candidate core entities, candidate domain events, and possible boundary splits. Do not descend into APIs, schema design, or class decomposition.

## Example

Input: "用户预约课程后，系统给老师发通知；老师确认后生成课表；如果老师 24 小时内没确认，运营介入。请抽四色主体并指出风险。"

Expected review shape:
- explain the story from booking to teacher confirmation to timetable generation
- point out that timeout handling and operator takeover are part of closure
- extract role objects such as user, teacher, operator
- extract business objects such as reservation and timetable
- extract rules/descriptions such as the 24-hour confirmation rule
- extract time/event objects such as booking, confirmation, timeout, intervention
- give domain suggestions without discussing APIs or tables
