# AGENTS.md

# Agent Role

You are a senior software engineer.
Optimize for correctness, clarity, robustness, and minimal, maintainable changes.

---

## Build / Lint / Test Commands

This repository uses [`sklint`](https://github.com/sven1103-agent/sklint) for validating skills against the [Agent Skills](https://agentskills.io) specification.

### Validation (Lint)

```bash
# Install sklint
go install github.com/sven1103-agent/sklint/cmd/sklint@latest

# Validate a single skill (strict mode)
sklint --strict skills/<skill-name>

# Validate all skills
for dir in skills/*; do
  if [ -d "$dir" ]; then
    sklint --strict "$dir"
  fi
done
```

### Running CI Locally

The CI workflow (`.github/workflows/sklint.yml`) runs the same validation. Ensure all skills pass `sklint --strict` before committing.

---

## Code Style Guidelines

### Skill Structure

Each skill lives in `skills/<skill-name>/` and must include:

- `SKILL.md` - The main skill definition file
- Additional files allowed (examples, configs) as needed

### SKILL.md Format

Use YAML frontmatter with these required fields:

```yaml
---
name: <skill-name-in-kebab-case>
description: <clear description of what the skill does>
license: <SPDX license identifier>
metadata:
  author: <author name>
  version: "<semver>"
---
```

**Requirements:**
- `name`: kebab-case, must match directory name
- `description`: Concise, max 2 sentences
- `license`: Use SPDX identifiers (e.g., `AGPL-3.0-only`, `MIT`, `Apache-2.0`)
- `metadata.author`: Full name or GitHub username
- `metadata.version`: Semantic version string (quoted)

### Content Style

- Use clear, imperative language
- Use markdown headings (##, ###) for section hierarchy
- Include "When to use" and "When NOT to use" sections
- Provide concrete examples
- Keep sections focused and concise

### Naming Conventions

- Skill directories: `kebab-case` (e.g., `linear-thinking`, `code-review`)
- Skill names in frontmatter: must match directory name
- Headings: Title Case or Sentence case consistently

### Markdown Guidelines

- Use `---` for frontmatter separator (must be first content in file)
- Use fenced code blocks with language hints: ```bash, ```yaml, etc.
- Use bullet points for lists
- Keep line length reasonable (max ~100 chars when practical)

---

## Version Control / Branching Conventions (MANDATORY)

The agent MUST follow these rules strictly.

### 1. Branch Creation

- Before making any file modifications, the agent MUST:

    1. Verify the working tree is clean:
    - Run `git status --porcelain`
    - If output is not empty → STOP and report.

    2. Determine whether this is a NEW TASK or a FOLLOW-UP:

    - NEW TASK if the user explicitly asks to "start a new branch" / "new task" / "new feature".

    - Otherwise treat as FOLLOW-UP and continue on the current branch.

    3. If NEW TASK:
    - `git fetch origin`
    - Create and switch to a new branch based on `origin/main`:
        `git checkout -b <type>/<short-description> origin/main`

    4. If FOLLOW-UP:
    - Continue working on the current branch.
    - Do NOT create a new branch unless explicitly instructed.

    5. Confirm:
    - `git status` shows the intended branch
    - Working tree is clean

- Allowed branch types:
  - `feature`
  - `fix`
  - `chore`
  - `refactor`
  - `test`

- Branch name format:
  ```
  <type>/<kebab-case-short-description>
  ```

  Example:
  ```
  feature/duration-cli-parsing
  ```

- The agent MUST NOT modify or commit directly to:
  - `main`
  - `master`
  - Any protected branch

---

### 2. Committing Changes

- All task-related changes MUST be committed.
- The agent MUST NOT leave task-related changes unstaged or uncommitted.
- The agent MUST create exactly one logical commit per task unless the task explicitly requires multiple commits.
- The agent MUST stage ONLY files that are part of the task (files it created OR modified for the task).
- The agent MUST NOT stage unrelated files (including generated artifacts, caches, build outputs, or editor temp files).

#### Staging and commit commands (use explicit paths)

```bash
git add <path1> <path2> ...
git commit -m "<type>: <short title>" -m "<description>"
```

Before committing, the agent MUST:
- run `git status`
- verify only intended files are staged

The agent MUST NOT use `git add -A` unless explicitly instructed.

---

### 3. Commit Message Format (STRICT)

Commit messages MUST follow this template:

```
<type>: <short descriptive title>

<clear explanation of what was changed and why>
```

Example:

```
feature: implement duration parsing CLI

Adds parsing logic for serialized time format.
Includes input validation and end-to-end test coverage.
```

- The title MUST:
  - Be <= 72 characters
  - Use imperative mood ("add", "fix", "refactor")
  - Not end with a period

- The body MUST:
  - Explain reasoning, not just what changed
  - Mention tests added or updated

---

### 4. Validation Before Completion

Before finishing, the agent MUST:

1. Run `git status` to confirm:
   - The repository is on the newly created branch
   - The working tree is clean

2. Output:
   - Branch name
   - Commit hash

Failure to comply with these rules is considered incomplete task execution.


### 5. Pull Request Workflow (MANDATORY)

After committing changes, the agent MUST create a Pull Request in **DRAFT** state.

The Draft PR MUST:

- Clearly describe the intended change
- Not assume final approval
- Be concise and review-oriented
- Reflect exactly one logical unit of work (unless explicitly instructed otherwise)

---

#### Draft PR Template

```
## Summary
<What this PR introduces or changes>

## Scope
- <Major change or affected component>
- <Major change or affected component>

## Rationale
<Why this change is necessary>

## Status
Draft – awaiting validation and review.
```

---

#### Rules

- The PR MUST be created as a Draft.
- The agent MUST NOT mark the PR as “Ready for Review” unless explicitly instructed.
- The PR MUST remain concise and factual.
- The PR MUST NOT include internal reasoning traces or irrelevant logs.
- The PR MUST align with the associated task or issue reference, if available.

Failure to comply with this policy is considered incomplete task execution.

---

## 6. Skills Section Management in README.md

### Purpose

The `## Skills` section in `README.md` provides a human-readable listing of all available skills in the `./skills/` directory. This section is maintained using the template at `./template/skill_entry.md`.

### When to Update

Update the `## Skills` section in `README.md` when explicitly instructed to do so (e.g., "update the skill section" or "add skill documentation").

### How to Update

1. **List Available Skills**
   - Scan the `./skills/` directory for all skill directories
   - Each skill directory should contain a `SKILL.md` file

2. **Use the Template**
   - Reference the template at `./template/skill_entry.md`
   - The template defines the structure for each skill entry:
     - Skill name (as a level 3 heading)
     - Description
     - License
     - Metadata

3. **Populate the Section**
   - For each skill in `./skills/`, create an entry using the template
   - Extract skill information from the skill's `SKILL.md` file
   - Follow the template format exactly to ensure consistency

4. **Validation**
   - Ensure all skills in `./skills/` are documented in the section
   - Verify each entry contains required fields: name, description, and license
   - Check that the formatting matches the template structure

### Example Entry

Using the template structure:

```markdown
### `linear-thinking`

#### Description
A skill providing structured linear reasoning capabilities for agents.

#### License
AGPL-3.0

#### Metadata
- version: `1.0.0`
```

### Notes

- Maintain alphabetical order or organize by category as appropriate
- Do not manually remove entries when skills are deleted; update the listing only when explicitly asked
- This section serves as documentation for users browsing the repository—keep entries clear and concise
