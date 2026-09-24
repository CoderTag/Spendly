# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- **Run app**: `python app.py` (runs on port 5001, debug mode on)
- **Install dependencies**: `uv sync` (preferred) or `pip install -r requirements.txt`
- **Run tests**: `pytest`
- **Run a single test**: `pytest <path_to_test_file>::<test_name>`

## Project context

This is a learning project for building a Flask expense tracker step-by-step. Many pieces are intentionally
unfinished placeholders (e.g. `database/db.py` is an empty stub with comments describing what to implement in
"Step 1", and routes in `app.py` like `/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, and
`/expenses/<id>/delete` return literal "coming in Step N" strings). When asked to implement one of these steps,
follow the guidance already left in comments rather than redesigning the intended structure.

## Architecture

- `app.py`: Flask app instance and all route definitions (both implemented pages and the placeholder routes
  awaiting implementation).
- `main.py`: Unrelated `uv`-generated entry point stub (prints a hello message); not part of the Flask app.
- `database/db.py`: Intended to hold SQLite connection/init logic — `get_db()` (connection with `row_factory`
  and foreign keys enabled), `init_db()` (create tables with `CREATE TABLE IF NOT EXISTS`), and `seed_db()`
  (insert sample dev data). Not yet implemented.
- `templates/`: Jinja templates, extending `base.html`. Currently: `landing.html`, `login.html`, `register.html`,
  `terms.html`, `privacy.html`.
- `static/css/style.css` and `static/js/main.js`: Frontend styling and logic (JS file currently empty).
- `pyproject.toml` / `uv.lock` / `requirements.txt`: Dependency management. Keep `requirements.txt` in sync with
  `pyproject.toml` if editing dependencies, since both are present.

## Notes

- The SQLite database file (`expense_tracker.db`) and `venv/` are gitignored — expect to create the DB fresh via
  `init_db()`/`seed_db()` rather than finding one checked in.
