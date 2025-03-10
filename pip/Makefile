PYTHON=.venv/bin/python

venv:
	python3 -m venv .venv

# User

install: venv
	$(PYTHON) -m pip install .

run:
	$(PYTHON) main.py

# Dev

install-dev: venv
	$(PYTHON) -m pip install -e .[dev]

dev: install-dev
	$(PYTHON) -m pre-commit install

test:
	$(PYTHON) -m pytest --cov=main tests/

# CI/CD - for docker install

install-ci:
	python3 -m pip install .[ci]

test-all:
	tox run-parallel

lint:
	ruff check .

type:
	mypy .

.PHONY: docs
docs:
	sphinx-build .
