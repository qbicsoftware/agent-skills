---
name: build-skill
description: Guide for building valid Agent Skills. Use when creating new skills, validating existing ones, or when you need to understand the agentskills specification structure.
---

# Skill Builder Guide

## Purpose

This skill provides step-by-step guidance for building valid Agent Skills that conform to the agentskills.io specification. Use it when creating a new skill from scratch or validating an existing one.

## When to use

Use this skill when:
- Creating a new Agent Skill
- Validating an existing skill's structure
- Understanding the agentskills format requirements
- Debugging skill-related issues

## Step-by-Step Guide

### 1. Choose a Skill Name

The skill name must:
- Be 1-64 characters
- Contain only lowercase letters (a-z) and hyphens (-)
- NOT start or end with a hyphen
- NOT contain consecutive hyphens (--)
- Match the parent directory name

**Valid names:**
- `pdf-processing`
- `data-analysis`
- `code-review`

**Invalid names:**
- `PDF-Processing` (uppercase not allowed)
- `-pdf` (cannot start with hyphen)
- `pdf--processing` (consecutive hyphens)
- `pdf_processing` (underscores not allowed)

### 2. Create Directory Structure

```
my-skill/
├── SKILL.md          # Required
├── scripts/          # Optional - executable code
├── references/      # Optional - documentation
└── assets/          # Optional - templates, images, data
```

### 3. Write SKILL.md Frontmatter

Your SKILL.md must start with YAML frontmatter:

```yaml
---
name: my-skill
description: A clear description of what this skill does and when to use it.
---
```

**Required fields:**
- `name`: Skill identifier (1-64 chars, lowercase + hyphens)
- `description`: What the skill does AND when to use it (1-1024 chars)

**Optional fields:**
- `license`: License name or reference to LICENSE file
- `compatibility`: Environment requirements (1-500 chars)
- `metadata`: Key-value pairs (author, version, etc.)
- `allowed-tools`: Space-delimited list of pre-approved tools

### 4. Write the Body Content

After frontmatter, write Markdown with skill instructions. Include:

- **Purpose**: What the skill does
- **When to use**: Situations where this skill applies
- **When NOT to use**: Cases to avoid this skill
- **Step-by-step instructions**: Clear numbered steps
- **Examples**: Input/output examples
- **Edge cases**: Common failure modes

Keep the main SKILL.md under 500 lines. Move detailed reference material to `references/` files.

### 5. Add Supporting Files (Optional)

**scripts/**: Executable code agents can run
- Self-contained scripts
- Include error messages
- Handle edge cases

**references/**: Additional documentation
- REFERENCE.md for technical details
- FORMS.md for templates
- Domain-specific files

**assets/**: Static resources
- Templates
- Images/diagrams
- Data files

### 6. Validate Your Skill

Run the validation using `sklint` in strict mode:

```bash
sklint --strict ./your-skill
```

For installation instructions, see the [sklint repository](https://github.com/sven1103-agent/sklint).

## Validation Checklist

Before considering a skill complete, verify:

- [ ] Directory name matches `name` in frontmatter
- [ ] Name uses only lowercase letters and hyphens
- [ ] Name does not start or end with hyphen
- [ ] Name has no consecutive hyphens
- [ ] Description is 1-1024 characters
- [ ] Description explains WHAT and WHEN TO USE
- [ ] SKILL.md has valid YAML frontmatter
- [ ] Body content is well-structured with clear instructions
- [ ] Main SKILL.md is under 500 lines
- [ ] File references use relative paths from skill root

## Common Pitfalls

1. **Poor descriptions**: Don't just say "helps with X". Explain what the skill does AND when an agent should use it.

2. **Too long**: Keep SKILL.md under 500 lines. Use `references/` for detailed documentation.

3. **Wrong name format**: Always use lowercase and hyphens. Test with the validation script.

4. **Missing progressive disclosure**: Put essential instructions in SKILL.md, detailed reference in separate files.

5. **Deep nesting**: Keep file references one level deep. Avoid `references/subdir/deep.md`.

## Example: Creating a New Skill

1. Create directory: `mkdir skills/my-new-skill`

2. Create `SKILL.md`:
```yaml
---
name: my-new-skill
description: Does something useful. Use when the user needs X or mentions Y.
---
```

3. Add body content with instructions

4. Run validation:
```bash
sklint --strict skills/my-new-skill
```

5. Fix any errors and validate again

## Reference

See [AGENTSKILLS_SPEC.md](references/AGENTSKILLS_SPEC.md) for the complete specification.
