---
name: four-color-review
description: Use only when the user explicitly invokes one of these slash commands: `/four-color-review`, `/four-color-review:continue <document-path>`, or `/four-color-review:all <document-path>`.
---

# four-color-review

## Overview

Use storytelling first and four-color modeling second. The goal is to verify that the business narrative is coherent and closed-loop before extracting domain entities and giving lightweight domain suggestions.

## When to Use

Use this skill only through these explicit slash commands:
- `/four-color-review` — start a new review
- `/four-color-review:continue <document-path>` — continue unresolved items below 6/10
- `/four-color-review:all <document-path>` — re-ask and re-score all recorded issues

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
12. After the review document is complete, ask whether the user wants an optimized requirements document generated from the current review using `AskUserQuestion`. If the user declines, end the turn.

## Question Interaction Rule

Whenever the skill needs the user to answer a question, it must use `AskUserQuestion` in cursor-style interactive selection form. Free-text questioning is not allowed by default and may only be used if the user explicitly selects `Other` in that interaction.

The skill must never ask a plain-text question directly in the response body when user input is required.

Each `AskUserQuestion` interaction must contain exactly one question. Do not batch multiple questions in a single interaction, even if they are related.

This rule applies to:
- blocking clarifying questions
- follow-up questions in `/four-color-review:continue` mode
- follow-up questions in `/four-color-review:all` mode
- the final confirmation about whether to generate an optimized requirements document

## Blocking Rules

Stop and ask before continuing if any of these are unclear:
- who owns the responsibility
- what event or state transition happens
- what result completes the flow
- what rule is carried by which object
- who handles failure, timeout, or manual intervention

When asking follow-up questions:
- ask one question at a time
- ask the smallest question that unblocks closure
- if multiple gaps exist, choose only the single most blocking gap first
- do not ask the second question until the first one has been answered
- continue from the blocked step after the answer arrives
- do not invent assumptions and continue silently

## Output Structure

Always structure the output in this order:
1. Requirement Summary
2. Storyline
3. Closure Check
4. Missing Information Questions (only if blocked)
5. Four-Color Extraction
6. Responsibility and Relationship Analysis
7. Risk Register
8. Health Score Summary
9. Domain Suggestions

## Continuation Modes

- `/four-color-review:continue <document-path>` continues only unresolved items scored below 6/10.
- `/four-color-review:all <document-path>` re-asks all issues regardless of prior score or resolution state.

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
