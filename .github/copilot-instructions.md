<!-- Copilot instructions for AI coding agents on this repo -->
# Copilot guidance — nas-stacks

Purpose
- Quickly orient AI coding agents to the repository's architecture, developer workflows, and local conventions so suggestions and edits are immediately useful.

Big picture
- This repo contains several lightweight Docker Compose stacks that run on a Synology NAS (Docker context named `nas`). Key stacks: `homebridge`, `infra`, and `dev` (see `compose.yaml` in each folder).
- Docker interaction is performed from the developer Mac against the NAS by setting `DOCKER_API_VERSION=1.43` and `--context nas` (example commands below).

Where to look first
- `compose.yaml` files: `homebridge/compose.yaml`, `infra/compose.yaml`, `dev/compose.yaml` — each shows service names, networking and volume conventions.
- `homebridge/config.example.json` — canonical service configuration and fields expected by Homebridge.
- `docker-control/readme.md` — documents VS Code tasks and the fact that tasks target the `nas` context and pin API version.

Important conventions and patterns
- All remote Docker commands target the `nas` context and pin `DOCKER_API_VERSION=1.43`. Use this exact pattern for CLI examples and tasks.
- Do NOT commit runtime data or secrets. The `homebridge` README explicitly says "Do not commit runtime data." Treat any `config.json` or `/volume1/docker/...` mounts as runtime state.
- `homebridge/compose.yaml` uses `network_mode: host` and binds volume paths to Synology host paths (e.g. `/volume1/docker/homebridge`). Respect host-level networking when editing service ports or bind addresses in `config.example.json`.
- `infra/compose.yaml` exposes a TCP listener to bridge the Docker socket to `127.0.0.1:23750`. Edits that change socket handling or ports must preserve the intent of local-only access.
- Local env files: `dev/README.md` indicates `.env` files are local-only and `.env.example` is the template. Never attempt to auto-commit private `.env` contents.

Developer workflows (explicit commands)
- Bring a compose stack up (detached):
  - `DOCKER_API_VERSION=1.43 docker --context nas compose up -d`
- Follow logs:
  - `DOCKER_API_VERSION=1.43 docker --context nas compose logs -f`
- Tear down:
  - `DOCKER_API_VERSION=1.43 docker --context nas compose down`
- VS Code tasks are available in the `docker-control` workspace (see `docker-control/readme.md`) and mirror the above commands; prefer using those tasks for consistent context and API pinning.

Editing guidance for AI edits
- Small changes only: prefer minimal, focused diffs (modify only the compose or config file relevant to the task). Don't refactor multiple stacks in one change.
- Respect host paths and `network_mode: host` for `homebridge` — do not replace `network_mode` with bridge networking without explicit user instruction.
- When adding or changing ports, follow existing examples (e.g. `dev` exports `8088:80`, `homebridge` expects host networking and configuration binds to the host IP shown in `config.example.json`).
- If a change requires new secrets or `.env` keys, add only a `.env.example` entry and document the required runtime env variable in the commit message; do not populate secrets.

What to call out in PRs
- Why the change is safe for NAS deployment (API pin and context preserved).
- Any host-paths or networking changes and why they don't break existing mounts or host IP bindings.
- If new env variables were introduced, include an updated `.env.example` and instructions to set them locally.

Example touchpoints
- `homebridge/compose.yaml` — host networking + volume conventions.
- `infra/compose.yaml` — docker-socket bridge pattern.
- `dev/compose.yaml` + `dev/README.md` — local stack and `.env` guidance.
- `docker-control/readme.md` — VS Code task conventions and API pinning.

If anything here is unclear or you want more detail about a specific stack, tell me which stack or file and I'll expand or adjust these instructions.
