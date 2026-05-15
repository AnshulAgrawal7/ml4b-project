# CLAUDE.md — Instructions for Claude Code

## Project Context
ML4B SoSe 2026 — FAU Nürnberg, Lehrstuhl für Wirtschaftsinformatik
Goal: Gym exercise recognition from Apple Watch sensor data (accelerometer, gyroscope) using Machine Learning.
Methodology: CRISP-DM (Business Understanding → Data Understanding → Data Preparation → Modeling → Evaluation → Deployment)
Stack: Python 3.11, uv, scikit-learn, Streamlit, Jupyter

## Project Structure
- `app/` → Streamlit web application
- `data/` → NOT in git. raw/ for original data, processed/ for features
- `notebooks/` → Jupyter notebooks, one per CRISP-DM phase
- `src/ml4b/` → reusable Python package
- `tests/` → unit tests
- `docs/` → all documentation
- `Course_Files/` → university materials, read-only, never modify

## Code Rules
- Always use `pathlib.Path` for file paths, never hardcode absolute paths
- All functions must have type hints and Google-style docstrings
- Formatter: ruff (`uv run ruff format .`)
- Linter: ruff (`uv run ruff check .`)
- Never import from `Course_Files/`
- No secrets or credentials in code — use `.env` and `python-dotenv`

## Git Rules
- Branch strategy: main → develop → feature/xxx
- Never commit directly to main
- Commit message format (Conventional Commits):
  - `feat:` new feature
  - `fix:` bug fix
  - `docs:` documentation
  - `data:` data changes
  - `model:` model changes
  - `refactor:` restructuring
- Never commit: data files, model binaries, .env, .venv

## Documentation Rules
- Architecture: arc42 lightweight format (see docs/architecture/)
- Docstrings: Google Style on every function and class
- Every CRISP-DM phase gets its own notebook AND a corresponding docs entry
- When writing new code, always update docs/architecture/architecture.md if the structure changes
- ADR (Architecture Decision Records) in docs/decisions/ for every major technical decision

## When Adding New Code
1. Write function signature with type hints and docstring first
2. Then implementation
3. Then at least one test in tests/
4. Run `uv run ruff format .` and `uv run ruff check .` before committing
5. Update architecture.md if project structure changed

## When Asked to Document
- Use arc42 for architecture (lightweight: only sections 1,2,3,5,6,8 are mandatory)
- Use Google Docstring style for all Python code
- Keep docs close to code — prefer markdown files next to the modules they describe
