venv:
	python -m venv .venv

install-dev: venv
	.venv/bin/python -m pip install -e .[dev]

dev: install-dev
	.venv/bin/pre-commit install

lint:
	.venv/bin/ruff check .

type:
	.venv/bin/mypy .

.PHONY: venv install-dev dev lint type
