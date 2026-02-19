---
name: linear-thinking
description: Enforce clear, step-by-step, cause→effect reasoning and a single ordered plan. Use for tasks that benefit from structured analysis (requirements, debugging, implementation planning, checklists).
license: AGPL-3.0-only
metadata: 
  author: Sven Fillinger
  version: "1.0"
---

# Linear Thinking Skill

## Purpose
Produce outputs that follow a **single, ordered sequence** of steps where each step logically follows from the previous one (A → B → C). This skill prioritizes clarity, predictability, and completeness over creativity or exploring many alternatives.

## When to use
Use this skill when the user asks for, or the task benefits from:
- A plan, checklist, or procedure
- Debugging or root-cause analysis
- Implementation steps or migration steps
- Requirements breakdown / acceptance criteria
- Any task where missing a step would cause failure

## When NOT to use
Avoid this skill when:
- The user explicitly wants brainstorming, lateral thinking, or many alternatives
- The problem is inherently exploratory (open-ended ideation)
- The user asks for a short answer with no explanation

## Output rules (mandatory)
1. **Single path:** Provide one primary approach unless the user asks for options.
2. **Strict order:** Use numbered steps. Each step must be actionable and build on prior steps.
3. **No jumps:** Do not skip from problem statement to final answer without intermediate steps.
4. **Explicit assumptions:** If anything is ambiguous, list assumptions before the plan.
5. **Edge cases:** Include a small “Edge cases / failure modes” section for non-trivial tasks.
6. **Verification:** End with a “Validation” section describing how to confirm correctness (tests, checks, examples).
7. **Concise reasoning:** Keep explanations tight; focus on the chain of logic rather than long prose.

## Required response structure
Use this structure whenever applicable:

1. **Restate the goal**
2. **Inputs / constraints**
3. **Assumptions (if needed)**
4. **Plan (numbered steps)**
5. **Edge cases / failure modes**
6. **Validation (how to verify it worked)**

## Style guidance
- Prefer concrete nouns and verbs (“Parse tokens”, “Reject invalid input”, “Add end-to-end test”).
- Prefer deterministic language (“Do X”, “Then Y”), avoid vague language (“maybe”, “kinda”, “should probably”).
- If you must present alternatives, limit to **2**, label one as **recommended**, and keep it brief.

## Example behavior (illustrative, not code)
If asked “How do we add a CLI to accept input and print output?”:
1) Restate goal  
2) Identify inputs (args, stdin) and constraints  
3) Plan steps (parse args → validate → compute → print → exit codes)  
4) Edge cases (missing args, invalid format)  
5) Validation (run sample commands, add test)

## Safety / quality guardrails
- Do not invent requirements not present in the task.
- If a step depends on missing information, explicitly note it and proceed with a reasonable default assumption.
- Prefer minimal, maintainable changes over large refactors unless asked.