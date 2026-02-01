# dev stack

Put `compose.yaml` here.
Keep `.env` local only (use `.env.example` as a template).

## Run on NAS
From Mac / VS Code:

- Up: `DOCKER_API_VERSION=1.43 docker --context nas compose up -d`
- Logs: `DOCKER_API_VERSION=1.43 docker --context nas compose logs -f`
- Down: `DOCKER_API_VERSION=1.43 docker --context nas compose down`

Or use VS Code tasks in docker-control.