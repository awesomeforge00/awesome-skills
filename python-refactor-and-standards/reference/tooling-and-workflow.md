# Tooling & Workflow (Python)

Recommended stack: **`ruff`** (lint + format + import sorting) + **`mypy`** (type checking) +
**`pre-commit`** (run both automatically before every commit). This section shows the minimum
setup; adjust versions/config to your project.

## 1. Linting & formatting with `ruff`

`ruff` replaces `flake8` + `isort` + (mostly) `black` in one fast tool.

```toml
# pyproject.toml
[tool.ruff]
line-length = 100

[tool.ruff.lint]
select = ["E", "F", "I", "B", "UP"]  # pyflakes, pycodestyle, imports, bugbear, pyupgrade
```

```bash
ruff check .          # lint
ruff format .         # format (black-compatible)
```

## 2. Type checking with `mypy`

```toml
# pyproject.toml
[tool.mypy]
python_version = "3.12"
disallow_untyped_defs = true
warn_unused_ignores = true
```

```bash
mypy src/
```

Start with `disallow_untyped_defs = false` on legacy code and tighten it once most functions
are hinted — don't try to fix every type error in one pass.

## 3. Wiring it together with `pre-commit`

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.6.0
    hooks:
      - id: ruff
      - id: ruff-format
  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.11.0
    hooks:
      - id: mypy
```

```bash
pre-commit install       # one-time setup per clone
pre-commit run --all-files
```

## 4. Environment & dependencies (brief)

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

For larger projects, a lockfile-based tool (e.g. `poetry` or `pip-tools`) keeps installs
reproducible across machines — plain `requirements.txt` is fine for small/simple projects.

## Alternatives

If your project already standardized on `black` + `flake8` + `isort`, or on `pyright` instead
of `mypy`, that's fine too — the goal is consistent automated enforcement, not a specific tool.
