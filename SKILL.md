---
name: four-color-review
description: Use when reviewing a business requirement, PRD, user story, or workflow from a developer or architect perspective and you need to check whether the story closes, whether responsibility breaks exist, or whether four-color domain entities should be extracted before discussing implementation.
---

# four-color-review

## Overview

Use storytelling first and four-color modeling second. The goal is to verify that the business narrative is coherent and closed-loop before extracting domain entities and giving lightweight domain suggestions.

## When to Use

Use this skill when the user asks to:
- review a business requirement or PRD for closure
- check whether a workflow or user story makes sense
- extract business roles, entities, rules, and events
- inspect responsibility gaps from a developer or architect perspective

Do not use this skill to produce API design, database tables, class diagrams, or implementation plans.

## Workflow

1. Restate the requirement in 3-5 sentences.
2. Reconstruct the business story in this order:
   - who initiates
   - under what trigger or context
   - what key actions happen
   - how the system or business responds
   - what result is produced
   - where the loop closes
3. Check whether the story closes.
4. If a blocking gap exists, stop and ask exactly one clarifying question with `AskUserQuestion`.
5. Only after the story closes, extract four-color groups:
   - role objects
   - core business objects
   - rule/description objects
   - time/event objects
6. Analyze responsibility and relationship gaps.
7. End with lightweight domain suggestions only.

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
7. Domain Suggestions

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
