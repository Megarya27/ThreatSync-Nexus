<!--
  Purpose: Provide concise, actionable guidance for AI coding agents working in this repository.
  Note: A quick scan found no existing COPILOT/AGENT instruction files or manifest files in the workspace.
  This file therefore focuses on HOW to discover the project's structure, what to look for, and
  concrete checks an agent should perform before making edits. Replace placeholders with repo
  specific examples once the repository manifests are present.
-->

# Copilot / AI agent instructions (concise)

1. Repo discovery (run immediately on checkout)
   - Search for manifests and CI files (PowerShell):
     - Get-ChildItem -Recurse -Force -Include package.json,pyproject.toml,requirements.txt,setup.py,pom.xml,build.gradle,Dockerfile,Makefile,README.md
   - Look for entry points: `src/`, `cmd/`, `app/`, `main.go`, `index.js`, `app.py`, `server.ts`, `bin/`.
   - Check infra and integration files: `docker-compose.yml`, `Dockerfile`, `.env`, `config/*.yml`, `terraform/`, `k8s/`, `migrations/`.

2. Identify project type & build/test commands
   - If `package.json` exists: prefer `npm ci` then `npm run test` and `npm run build` (or use `pnpm`/`yarn` if present).
   - If Python: look for `pyproject.toml` or `requirements.txt`. Use virtualenv, then `pytest` for tests.
   - If Java/Maven/Gradle: discover `pom.xml` or `build.gradle` and use `mvn -q -DskipTests=false test` or `./gradlew test`.
   - If none of the above are found, inspect top-level folders and `README.md` for custom build steps.

3. Architecture reconnaissance (what to extract and record)
   - Major components: list directories that look like services (e.g., `api/`, `web/`, `worker/`, `infra/`).
   - Service boundaries: for each service, record its transport (HTTP, gRPC, message queue) by scanning for frameworks (`express`, `fastapi`, `spring`, etc.).
   - Data flows: search for DB clients (psycopg2, sqlalchemy, mongoose, jdbc) and message brokers (kafka, rabbitmq, redis).
   - External integrations: find env names or config keys referencing third-party APIs (AWS, Sentry, Datadog, Stripe).

4. Project-specific conventions to look for
   - Filename/casing conventions in `src/` (kebab-case vs snake_case vs camelCase).
   - Service-test pairing (e.g., tests live under `tests/` next to service folder or under `__tests__/`).
   - Configuration pattern: single `config/` vs env-driven `.env` usage; prefer examples in `config/*.example`.

5. Making edits: small, verifiable changes only
   - Always run the project's fast test suite locally after changes (unit tests + linter). If there is a slow integration suite, run only when necessary and report time/cost.
   - When adding or changing behavior, include a minimal test that demonstrates the change in the same test layout used by the project.

6. Integration & debugging hints
   - If a `Dockerfile` or `docker-compose.yml` is present, prefer reproducing the dev environment with Compose for integration testing.
   - Check for `Makefile` or `scripts/` that wrap common tasks; use them instead of reinventing commands.

7. Merging with existing agent guidance
   - If the repo already contains `.github/copilot-instructions.md` or `AGENT.md`, preserve any repository-specific sections and add incremental updates below a clearly labeled block `<!-- added-by-ai -->`.

8. If you (the agent) cannot find expected files
   - Create a short diagnostic report listing which files were searched and not found, and propose the next steps for the human maintainer (examples: "no package.json found — is this repo a mono-repo submodule?", "no tests — prefer to add smoke tests first").

9. Examples (replace when repo files are discovered)
   - Example: "Service `api/` uses `fastapi` (look for `uvicorn` or `fastapi`) and exposes `/health` — run `uvicorn api.main:app --reload` to start locally."
   - Example: "UI lives in `frontend/` with `package.json` — `npm ci && npm run start` launches dev server."

Please ping the repo owner if anything in this file is unclear or if you want me to merge actual live examples from discovered files.

-- End of instructions --
