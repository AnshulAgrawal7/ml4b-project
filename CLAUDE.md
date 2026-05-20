# CLAUDE.md — Master Instructions for Claude Code

## Project Context

- **University:** FAU Nürnberg, Lehrstuhl für Wirtschaftsinformatik — ML4B SoSe 2026
- **Goal:** Gym exercise recognition from Apple Watch sensor data (accelerometer, gyroscope, 50Hz) using Machine Learning
- **Methodology:** CRISP-DM (Business Understanding → Data Understanding → Data Preparation → Modeling → Evaluation → Deployment)
- **Stack:** Python 3.11, uv, scikit-learn, Streamlit, Jupyter
- **Dataset:** RecoFit (Microsoft), MATLAB .mat format, 50Hz, wrist-worn, loaded with `scipy.io.loadmat`

## Handover Requirement
This project must be fully handover-ready at all times. A new team with no prior knowledge must be able to:
1. Understand what the project does → README.md + docs/business_understanding/
2. Set up the environment on any OS → Setup_WSL_Windows.md, Setup_macOS.md, Setup_Windows.md
3. Understand every decision made → docs/decisions/ (ADRs)
4. Reproduce all results → notebooks/ are fully runnable top-to-bottom
5. Run the Streamlit app → README.md Quickstart section

## Available Specialist Agents
For focused tasks, use the specialist agents in `agents/`:
- `agents/data_scientist.md` → ML work: feature engineering, modeling, notebooks, evaluation
- `agents/documenter.md`     → Documentation only: arc42, ADRs, CRISP-DM log, Setup guides
- `agents/reviewer.md`       → Code + doc review before every push

When to use which agent:
- Writing ML code or notebooks → data_scientist agent
- Updating any .md file → documenter agent
- Before git push → reviewer agent
- General tasks → use this CLAUDE.md directly

## Decision Documentation Rule (CRITICAL)
Every time a choice is made between two options, an ADR must be created immediately.
Format: `docs/decisions/ADR-XXX-short-title.md`

Template:

```
# ADR-XXX: [Short Title]
**Status:** Accepted
**Date:** YYYY-MM-DD

## Context
[Why was this decision needed?]

## Decision
[What was decided?]

## Alternatives Considered
[What other options were evaluated?]

## Rationale
[Why was this option chosen over the alternatives?]

## Consequences
[What are the positive and negative consequences?]
```

This applies to: algorithm choices, feature engineering decisions, dataset decisions,
library choices, architecture decisions, preprocessing choices.

## Code Comment Standard
All Python code must be self-explanatory to a new team member:

- Module-level docstring at the top of every `.py` file explaining what the file does and its role in the project
- Google-style docstring on every function and class
- Inline comments on every non-trivial block explaining WHAT it does and WHY
- Notebook markdown cells in plain language explaining what the next code cell does and why

## Project Structure

```
app/            → Streamlit web application (entry point: app/streamlit_app.py)
agents/         → AI agent instruction files for Claude Code
data/           → NOT in git. raw/ for original data, processed/ for features
notebooks/      → Jupyter notebooks, one per CRISP-DM phase, numbered 01-06
src/ml4b/       → Reusable Python package (importable as from ml4b import ...)
tests/          → Unit tests mirroring src/ml4b/ structure
docs/           → All documentation (arc42, ADRs, CRISP-DM log, data dictionary)
reports/        → Generated figures and result summaries (not in git)
```

## Code Rules

- Always use `pathlib.Path` for file paths, never hardcode absolute paths
- Use `src/ml4b/utils/config.py` for all project-wide path constants
- All functions must have type hints and Google-style docstrings
- Formatter: ruff (`uv run ruff format .`)
- Linter: ruff (`uv run ruff check .`)
- Random seeds: always set `random_state=42` for reproducibility
- No secrets or credentials in code — use `.env` and `python-dotenv`

## Git Rules

- Branch strategy: main → develop → feature/xxx
- Never commit directly to main
- Commit message format (Conventional Commits):
  - `feat:`     new feature or functionality
  - `fix:`      bug fix
  - `docs:`     documentation only
  - `data:`     data changes
  - `model:`    model changes
  - `refactor:` restructuring without behavior change
  - `test:`     adding or fixing tests
- Never commit: data files, model binaries, `.env`, `.venv`, `reports/figures/`
- Always commit code and documentation together (atomic commits)

## Standard Workflow — Activated by "follow standard workflow"
This workflow is mandatory after every task.
Writing "follow standard workflow" in any prompt activates all steps below.

### Step 1 — Code Quality
- Run `uv run ruff format .`
- Run `uv run ruff check .`
- Fix all errors before proceeding

### Step 2 — Documentation Consistency (use documenter agent)
Update every file affected by the change:
- `docs/architecture/architecture.md` → if project structure or data flow changed
- `docs/project/crisp_dm_log.md` → if a CRISP-DM phase progressed or completed
- `STRUCTURE.md` → if any folder or file was added, removed, or renamed
- `docs/data/data_dictionary.md` → if new features or datasets were added
- `docs/business_understanding/business_understanding.md` → if goals or scope changed
- `docs/setup/Setup_WSL_Windows.md`, `Setup_macOS.md`, `Setup_Windows.md` → if dependencies or setup steps changed

### Step 3 — Decision Documentation
If any choice was made between two or more options during this task:
- Create a new ADR in `docs/decisions/ADR-XXX-short-title.md`
- No decision — technical or methodological — should ever be undocumented

### Step 4 — Review (use reviewer agent)
Run through the full reviewer agent checklist in `agents/reviewer.md`.
Do not commit if any ❌ items remain unresolved.

### Step 5 — Atomic Commit
Commit code AND documentation changes together in one single commit.
Never commit code without documentation, and vice versa.

### Step 6 — Output
After completing, show only:
- List of files changed
- Commit message used
- Any ⚠️ or ❌ items found during review
- Do NOT output full file contents unless explicitly requested.
