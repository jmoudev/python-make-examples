# pip

**make** examples for a Python project utilising pip for packaging.

## dev

Make .PHONY target dev, setting up the project for development:

```bash
$ make dev
python -m venv .venv
.venv/bin/python -m pip install -e .[dev]
...
Successfully installed cfgv-3.4.0 distlib-0.3.9 filelock-3.17.0 ...
.venv/bin/pre-commit install
pre-commit installed at .git/hooks/pre-commit
```

## lint

Make .PHONY target lint, running the ruff linter:

```bash
$ make lint
.venv/bin/ruff check .
All checks passed!
```

## type

Make .PHONY target type, running mypy for type checking:

```bash
$ make type
.venv/bin/mypy .
Success: no issues found in 1 source file
```
