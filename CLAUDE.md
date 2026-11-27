# CLAUDE.md

## Project overview

Spendly is a lightweight personal expense tracker built with Flask and SQLite.

---

## Architecture

spendly/
├── app.py              # All routes — single file, no blueprints
├── database/
│   └── db.py           # SQLite helpers: get_db(), init_db(), seed_db()
├── templates/
│   ├── base.html       # Shared layout — all templates must extend this
│   └── *.html          # One template per page
├── static/
│   ├── css/
│   │   ├── style.css       # Global styles
│   │   └── landing.css     # Landing-page-only styles
│   └── js/
│       └── main.js         # Vanilla JS only
└── requirements.txt

**Where things belong:**

- New routes → `app.py` only, no blueprints
- DB logic → `database/db.py` only, never inline in routes
- New pages → new `.html` file extending `base.html`
- Page-specific styles → new `.css` file, not inline `<style>` tags

---

## Code style

- Python: PEP 8, snake_case for all variables and functions
- Templates: Jinja2 with `url_for()` for every internal link — never hardcode URLs
- Route functions: one responsibility only — fetch data, render template, done
- DB queries: always use parameterized queries (`?` placeholders) — never f-strings in SQL
- Error handling: use `abort()` for HTTP errors, not bare `return "error string"`

---

## Tech constraints

- **Flask only** — no FastAPI, no Django, no other web frameworks
- **SQLite only** — no PostgreSQL, no SQLAlchemy ORM, no external DB
- **Vanilla JS only** — no React, no jQuery, no npm packages
- **No new pip packages** — work within `requirements.txt` as-is unless explicitly told otherwise
- Python 3.10+ assumed — f-strings and `match` statements are fine

---

## Commands

- **Run app**: `python app.py` (runs on port 5001, debug mode on)
- **Install dependencies**: `uv sync` (preferred; creates `.venv/`) or `pip install -r requirements.txt`
- **Run app via uv**: `uv run python app.py` (no activation needed); otherwise `source .venv/bin/activate`
- **Run tests**: `pytest` (uses `pytest-flask`; no `tests/` directory exists yet)
- **Run a single test**: `pytest <path_to_test_file>::<test_name>`

---

## Implemented vs stub routes

| Route                         | Status                                   |
| ----------------------------- | ---------------------------------------- |
| `GET /`                     | Implemented — renders `landing.html`  |
| `GET /register`             | Implemented — renders `register.html` |
| `GET /login`                | Implemented — renders `login.html`    |
| `GET /logout`               | Stub — Step 3                           |
| `GET /profile`              | Stub — Step 4                           |
| `GET /expenses/add`         | Stub — Step 7                           |
| `GET /expenses/<id>/edit`   | Stub — Step 8                           |
| `GET /expenses/<id>/delete` | Stub — Step 9                           |

**Do not implement a stub route unless the active task explicitly targets that step.**

---

## Warnings and things to avoid

- **Never use raw string returns for stub routes** once a step is implemented — always render a template
- **Never hardcode URLs** in templates — always use `url_for()`
- **Never put DB logic in route functions** — it belongs in `database/db.py`
- **Never install new packages** mid-feature without flagging it — keep `requirements.txt` in sync
- **Never use JS frameworks** — the frontend is intentionally vanilla
- **`database/db.py` is currently empty** — do not assume helpers exist until the step that implements them
- **FK enforcement is manual** — SQLite foreign keys are off by default; `get_db()` must run `PRAGMA foreign_keys = ON` on every connection
- The app runs on **port 5001**, not the Flask default 5000 — don't change this

---

## Project context

This is a learning project for building a Flask expense tracker step-by-step. Many pieces are intentionally
unfinished placeholders (e.g. `database/db.py` is an empty stub with comments describing what to implement in
"Step 1", and routes in `app.py` like `/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, and
`/expenses/<id>/delete` return literal "coming in Step N" strings). When asked to implement one of these steps,
follow the guidance already left in comments rather than redesigning the intended structure.

## Architecture

- `app.py`: Flask app instance and all route definitions (both implemented pages and the placeholder routes
  awaiting implementation).
- `database/db.py`: Intended to hold SQLite connection/init logic — `get_db()` (connection with `row_factory`
  and foreign keys enabled), `init_db()` (create tables with `CREATE TABLE IF NOT EXISTS`), and `seed_db()`
  (insert sample dev data). Not yet implemented.
- `templates/`: Jinja templates, extending `base.html`. Currently: `landing.html`, `login.html`, `register.html`,
  `terms.html`, `privacy.html`.
- `static/css/style.css` and `static/js/main.js`: Frontend styling and logic (JS file currently empty).
- `pyproject.toml` / `uv.lock` / `requirements.txt`: Dependency management. Keep `requirements.txt` in sync with
  `pyproject.toml` if editing dependencies, since both are present.

## Notes

- Python 3.10 (`.python-version`, `requires-python >=3.10`). The virtualenv is `.venv/` (created by `uv sync`, gitignored).
- The SQLite database file (`expense_tracker.db`) is gitignored — expect to create the DB fresh via
  `init_db()`/`seed_db()` rather than finding one checked in.
