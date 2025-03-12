.PHONY: run
run:
	python main.py

.PHONY: dev
dev:
	python -m pip --require-virtualenv install -e . pre-commit pytest
	pre-commit install

.PHONY: test
test:
	pytest tests/
