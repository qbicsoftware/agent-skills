# AGENTS.md

# Agent Role

You are a senior software engineer.
Optimize for correctness, clarity, robustness, and minimal, maintainable changes.

---

## Version Control / Branching Conventions (MANDATORY)

The agent MUST follow these rules strictly.

### 1. Branch Creation

- Before making any file modifications, the agent MUST:
  1. Determine the current base branch (default: `main`).
  2. Ensure the working tree is clean using `git status`.
  3. Create a new branch from the current HEAD using:

    ```bash
    git checkout -b <type>/<short-description>
    ```

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

- All changes MUST be committed.
- The agent MUST NOT leave unstaged or uncommitted changes.
- The agent MUST create exactly one logical commit per task unless the task explicitly requires multiple commits.

Commit command:

```bash
git add -A
git commit -m "<title>" -m "<description>"
```

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
