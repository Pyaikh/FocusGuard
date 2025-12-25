# FocusGuard – Codex Instructions (AGENTS.md)

You are working in a monorepo:

- backend/  (Laravel 12 API)
- frontend/ (React + Vite)
- extension/ (Chrome Extension MV3)

Primary goal: build MVP in small, safe, reviewable steps. Do NOT implement everything at once.

## Core rules
1) Make changes in small batches (one feature per task). Avoid big refactors.
2) Always show a concise plan before editing files.
3) After edits, provide:
    - file list changed
    - key decisions
    - how to run/verify
4) Prefer minimal dependencies. If adding a library, explain why.
5) Never modify anything outside this repo.
6) Never run destructive commands without asking (rm -rf, reset --hard, dropping DB).

## Project environment (local, no Docker)
- MySQL is provided by MAMP.
- Backend runs via: `php artisan serve --port=8000`
- Frontend runs via: `npm run dev` (Vite default port 5173)

### MySQL (MAMP) defaults (confirm in .env)
- host: 127.0.0.1
- port: 8889
- user: root
- pass: root
- database: focusguard

## Commands you may run (allowed)
Backend:
- composer install
- php artisan key:generate
- php artisan migrate
- php artisan make:* (model, migration, controller, request, etc.)
- php artisan test (if tests exist)

Frontend:
- npm install
- npm run dev
- npm run build
- npm run lint (if configured)

Extension:
- no build step required for MVP
- keep it plain JS/HTML
- provide instructions to load unpacked extension in Chrome

## Backend architecture
- Use Laravel 12.
- API auth: Sanctum token-based.
- Use Form Requests for validation (Store/Update).
- Authorization: user can access only their own resources.

### API conventions
- Routes in routes/api.php
- Controllers: app/Http/Controllers/Api
- Requests: app/Http/Requests
- Responses: JSON only

## FocusGuard business rules (MVP)
- Mode is selectable: soft|hard (stored in users.mode, also snapshot into session.mode at start).
- Blocking precedence:
    1) allowlist match => allow
    2) else blocklist match => block
    3) else allow

## Extension behavior (MVP)
- Poll `GET /api/runtime` every 5–10 seconds.
- If current session is running => evaluate current tab URL.
- If blocked => redirect to `blocked.html?url=...`
- Send events to backend:
    - tab_blocked {url, host}
    - tab_allowed (optional)

## Definition of Done for any task
- Code compiles / migrations run.
- No placeholder TODOs unless explicitly requested.
- Provide manual test steps.

## When uncertain
- Ask a short question OR pick the simplest reasonable default and state it.
