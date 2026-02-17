# QBiC × BMBS OpenSkills Collection (University of Tübingen)

This repository is a curated collection of **self-made,
OpenSkills.io--compliant skills** developed at the University of
Tübingen as a joint effort of the **Quantitative Biology Center (QBiC)**
and the **Chair for Biomedical Biosciences (BMBS)**.

The primary purpose of this repository is to provide reusable, versioned
skills that can be embedded into other Git projects (e.g., agent
projects, automation repositories, internal tooling) while keeping
validation and maintenance centralized.

------------------------------------------------------------------------

## Scope

-   A collection of skills stored under `./skills/`
-   Skills intended to be fully **OpenSkills.io compliant**
-   Continuous validation via GitHub CI using `sklint` in strict mode
-   Optional agent governance via `AGENTS.md` (see section below)

This repository is a **skills library**, not a full agent project.

------------------------------------------------------------------------

## Repository Structure

    .
    ├── skills/
    │   ├── <skill-name>/
    │   │   ├── SKILL.md
    │   │   └── ...
    ├── .github/workflows/
    ├── AGENTS.md
    └── README.md

------------------------------------------------------------------------

## Validation (CI)

This repository runs `sklint` in CI to ensure all skills conform to the
OpenSkills.io specification.

Typical CI logic:

-   Iterate over directories in `skills/*`
-   Run `sklint --strict <skill-dir>`

You can validate locally (if installed):

    sklint --strict skills/<skill-name>

------------------------------------------------------------------------

## Recommended Integration into Other Git Projects

### Option A: Git Submodule (Recommended)

    git submodule add <REPO_URL> vendor/tue-skills
    git submodule update --init --recursive

Reference skills from:

    vendor/tue-skills/skills/<skill-name>

Update later:

    git submodule update --remote --merge

------------------------------------------------------------------------

### Option B: Git Subtree

    git subtree add --prefix vendor/tue-skills <REPO_URL> main --squash

Update:

    git subtree pull --prefix vendor/tue-skills <REPO_URL> main --squash

------------------------------------------------------------------------

### Option C: Manual Clone

    git clone <REPO_URL>

Copy or sync the `skills/` directory as needed.

------------------------------------------------------------------------

## Using the Skills

Depending on your tooling, you may:

-   Point your agent tooling to the `skills/` directory
-   Symlink selected skills into a local skills folder

Example:

    mkdir -p .agents/skills
    ln -s ../../vendor/tue-skills/skills/<skill-name> .agents/skills/<skill-name>

------------------------------------------------------------------------

## About `AGENTS.md` and How to Disable It

This repository contains an `AGENTS.md` to guide agents working **inside
this repository**.

When vendoring this repo (submodule/subtree), you may not want these
instructions to apply to your project.

### Options to Avoid Inheriting AGENTS.md

1.  Run agents from your project root (your own `AGENTS.md` takes
    precedence).
2.  Place the submodule in a directory like `vendor/` or `third_party/`
    and exclude it from agent scanning.
3.  Override with your own root-level `AGENTS.md`.
4.  If supported by your tooling, explicitly disable agent instruction
    scanning for the vendored path.

------------------------------------------------------------------------

## Contributing

-   Add new skills under `skills/<skill-name>/`
-   Ensure OpenSkills.io compliance
-   Run `sklint --strict` locally
-   CI must pass before merging

------------------------------------------------------------------------

## Maintainers

Maintained as a joint effort of:

-   Quantitative Biology Center (QBiC), University of Tübingen
-   Chair for Biomedical Biosciences (BMBS), University of Tübingen

------------------------------------------------------------------------

## License

Specify your license here (e.g., MIT, Apache-2.0, or internal usage
terms).
