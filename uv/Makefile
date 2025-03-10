RUN=uv run

# User

run:
	$(RUN) main.py

# Dev

dev:
	$(RUN) --group dev pre-commit install

test:
	$(RUN) --group dev pytest --cov=main tests/

.PHONY: run dev test
