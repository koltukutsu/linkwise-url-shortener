 .PHONY: install dev lint format test run migrate alembic-rev

 VENV?=.venv

 install:
 	python -m venv $(VENV)
 	$(VENV)/bin/pip install --upgrade pip
 	$(VENV)/bin/pip install -e .[dev]

 dev:
 	$(VENV)/bin/uvicorn linkwise_api.main:app --reload --host 0.0.0.0 --port 8000

 lint:
 	$(VENV)/bin/ruff check .

 format:
 	$(VENV)/bin/ruff check --select I --fix .
 	$(VENV)/bin/black .
 	$(VENV)/bin/isort .

 test:
 	$(VENV)/bin/pytest

 alembic-rev:
 	$(VENV)/bin/alembic revision -m "message"

 migrate:
 	$(VENV)/bin/alembic upgrade head


