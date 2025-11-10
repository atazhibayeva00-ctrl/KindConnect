# KindConnect

KindConnect is our conversational support platform that pairs a React + ChatKit frontend with a FastAPI backend. We use it to prototype, test, and deploy AI-assisted experiences tailored to KindConnect partners, mentors, and community members.

## What’s Inside

- `frontend/`: Vite + React client that wraps the ChatKit widget, handles session state, and exposes custom UI actions.
- `backend/`: FastAPI service that orchestrates OpenAI Agents, KindConnect-specific tools, and ChatKit server-side events.
- `examples/`: Additional scenarios we use internally to explore new journeys, integrations, or verticals.

## Tech Stack

- React 18 + Vite
- TypeScript on the client, Python 3.11 on the server
- FastAPI, Uvicorn, [`uv`](https://docs.astral.sh/uv/getting-started/installation/) for dependency management
- OpenAI ChatKit SDKs (JS + Python)

## Getting Started

### Prerequisites

- Node.js 20+
- Python 3.11+
- `uv` (recommended) or `pip`
- Environment variables:
  - `OPENAI_API_KEY`
  - `VITE_CHATKIT_API_DOMAIN_KEY` (any non-empty placeholder for local dev)

### 1. Bootstrap the Backend

From the repo root:

```bash
npm run backend
```

This command installs backend dependencies with `uv` and starts Uvicorn on `http://127.0.0.1:8000`.

To manage the backend manually:

```bash
cd backend
uv sync
export OPENAI_API_KEY="sk-proj-..."
uv run uvicorn app.main:app --reload --port 8000
```

If you prefer `pip`:

```bash
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -e .
export OPENAI_API_KEY="sk-proj-..."
uvicorn app.main:app --reload --port 8000
```

### 2. Launch the Frontend

From the repo root:

```bash
npm run frontend
```

This starts Vite on `http://127.0.0.1:5170`.

Manual steps:

```bash
cd frontend
npm install
npm run dev
```

Configuration defaults live in `frontend/src/lib/config.ts`. Use that file to change API URLs, feature flags, or theme presets.

### 3. Run Everything Together

```bash
npm start
```

This helper spins up both services. Ensure `uv` is installed and the required env vars are exported before running it.

## Domain Allowlisting

For production or tunnel testing:

1. Host the frontend on an approved domain.
2. Register the domain in the OpenAI domain allowlist console.
3. Update `frontend/vite.config.ts` (`server.allowedHosts`) with the domain.
4. Set `VITE_CHATKIT_API_DOMAIN_KEY` to the allowlisted value.

## Core Flows We Test

- **Profile & Fact Capture** – verify the agent persists community context.
- **Localized Weather Check** – confirm tool execution and widget rendering.
- **Theme Switching** – ensure client-side actions sync with ChatKit state.

Use these flows as smoke tests after local changes.

## Development Tips

- Backend code lives under `backend/app/`. Key entrypoint: `backend/app/main.py`.
- Frontend entrypoint: `frontend/src/main.tsx`.
- Keep shared types or constants in `frontend/src/lib/` and `backend/app/schemas/` to avoid drift.
- Add new agent tools in `backend/app/tools/` and register them in `app/main.py`.
- When introducing new widgets, mirror the client-side handler in `frontend/src/lib/config.ts`.

## Testing

- Backend: add tests under `backend/tests/` and run with `uv run pytest`.
- Frontend: add tests under `frontend/src/__tests__/` and run `npm test`.

## Deployment

1. Build the frontend: `npm run build --prefix frontend`.
2. Package the backend with your preferred infra tooling.
3. Export production env vars (`OPENAI_API_KEY`, `VITE_CHATKIT_API_DOMAIN_KEY`, others like `KINDCONNECT_ENV` as needed).
4. Update observability dashboards and smoke the flows above post-deploy.

## Contributing

1. Create a feature branch.
2. Run lint & tests locally (`npm run lint --prefix frontend`, `uv run ruff check backend`).
3. Open a PR with context, screenshots, and regression notes.

## License

Internal KindConnect project. Distribution outside the team requires approval.
