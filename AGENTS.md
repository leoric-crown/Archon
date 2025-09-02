# Repository Guidelines

## Project Structure & Module Organization
- `archon-ui-main/`: React + Vite UI (TypeScript, Tailwind). Tests with Vitest.
- `python/src/server/`: FastAPI API, Socket.IO, services (crawling, storage, embeddings).
- `python/src/mcp_server/`: MCP interface and tools for AI clients.
- `python/src/agents/`: PydanticAI-based agents and orchestration.
- `python/tests/`: Backend unit tests (pytest).  
- `migration/`: SQL setup and reset scripts.  
- `docker-compose.yml`, `Makefile`, `.env(.example)`: local/dev orchestration and config.

## Build, Test, and Development Commands
- `make dev`: Backend in Docker, frontend runs locally with HMR.
- `make dev-docker`: All services in Docker (UI 3737, API 8181, MCP 8051).
- `make test` | `make test-fe` | `make test-be`: Run all, frontend, or backend tests.
- `make lint` | `make lint-fe` | `make lint-be`: Lint all/FE/BE. Auto-fix Python via Ruff.
- `make install`: Install UI (npm) and Python (uv) dependencies.
- `docker compose up -d --build`: Full stack in Docker.
  - Logs: `docker compose logs -f` (e.g., `archon-server`, `archon-mcp`).

## Coding Style & Naming Conventions
- Python: `ruff` enforced (120 cols, spaces, double quotes). Modules `snake_case.py`, classes `PascalCase`, functions/vars `snake_case`.
- TypeScript/React: ESLint + TS. Components `PascalCase.tsx`, hooks `useX.ts`, utility files `camelCase.ts`.
- Keep functions small, pure where possible; prefer dependency injection in services.

## Alpha Development & Error Handling
- Fail fast on startup/config/auth/DB issues; crash with clear errors.
- For batch/async work, continue processing but log per-item failures with stack traces.
- Never persist corrupted/placeholder data (e.g., zeroed embeddings) on failure—skip the item.
- Include IDs/URLs in error logs; use specific exceptions; report success/failure counts.

## Testing Guidelines
- Backend: `pytest` in `python/tests/` (files `test_*.py`). Run with `make test-be` or `uv run pytest` in `python/`.
- Frontend: `vitest` (`archon-ui-main/`). `npm test` or `npm run test:coverage` for coverage.
- Aim for meaningful unit tests around crawling, storage, and API routes; add UI tests for critical flows.

## Commit & Pull Request Guidelines
- Commits: Short, imperative subjects; optionally scope (e.g., "server:"), reference issues/PRs.
- PRs: Include purpose, high-level changes, screenshots for UI, and test notes. Link issues/PRPs. Ensure `make lint` and `make test` pass.

## Security & Configuration Tips
- Copy `.env.example` → `.env` and set Supabase + model keys. Avoid committing secrets.
- Use `make check` to validate local setup. Prefer CPU-only defaults unless you need GPU.
- For ports, sync `ARCHON_SERVER_PORT` and Vite `VITE_*` vars when running hybrid dev.

## Environment Essentials
- Required: `SUPABASE_URL`, `SUPABASE_SERVICE_KEY` in `.env`.
- Optional: `OPENAI_API_KEY`, `LOG_LEVEL`, `LOGFIRE_TOKEN` (observability).
