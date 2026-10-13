# Disposable Notes — PR840 staging E2E

Disposable public end-to-end staging application. **Not a customer product.**
This is a real, self-hostable notes app: browser form submissions are stored in
SQLite and displayed on subsequent visits, including after process restarts.

## Runtime contract

- Python **3.10+**, standard library only; no pip dependencies or build step.
- Start command: `python3 app.py`.
- Listens on **`0.0.0.0:8080`** by default; optional `PORT` overrides the port.
- `GET /` serves the browser UI and saved notes.
- `POST /notes` accepts a URL-encoded `body` field (1–4000 characters), stores
  the note, and redirects to `/`.
- Persistent SQLite file: **`/data/notes.sqlite3`**. Mount persistent storage at
  **`/data`**, writable by the runtime user. The app creates the directory and
  schema on startup. Optional `NOTES_DB` overrides the database file path.
- No authentication, external services, secrets, or outbound requests.
- This public demo is intentionally unauthenticated: anyone with access can
  read and add notes. Use only disposable, non-sensitive data. It is not
  hardened for production or hostile traffic.

## Run locally

```sh
mkdir -p /tmp/disposable-notes
NOTES_DB=/tmp/disposable-notes/notes.sqlite3 python3 app.py
```

Open <http://localhost:8080>, add a note, and reload to read it. Stop with Ctrl-C.
Restart with the same database path to retain notes. Delete that temporary
folder when finished if you no longer need its data.

The initial repository intentionally contains no Dockerfile, Compose file,
GitHub workflow, or unit-test suite: deployment preparation belongs to the
staging agent being exercised. MIT licensed; see `LICENSE`.
