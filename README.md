# Pontoon Nginx Smoke Test

This repository is a tiny Dockerized Nginx application intended to test Pontoon's GitHub-to-container deployment flow.

It is intentionally simple: Pontoon should be able to clone the repository, build the included Dockerfile, run the container on port `80`, attach it to the generated tenant network, and route traffic to it through Traefik.

## What this app validates

- Dockerfile-based builds from a GitHub repository
- Static file serving through Nginx
- Container port detection/routing on port `80`
- Health checks using `/health`
- Dynamic routes and SPA-style fallback behavior
- Static asset caching headers

## Deploy with Pontoon

1. Push this repository to GitHub.
2. In Pontoon, create a new project that points at this repository.
3. Use Dockerfile deployment with container port `80`.
4. Trigger a deployment.
5. Open the generated Pontoon subdomain.

## Useful test paths

- `/` renders the smoke-test landing page.
- `/health` returns `ok` for platform health checks.
- `/pontoon/status` returns a small JSON response from Nginx.
- `/any/random/path` falls back to the landing page so route rewrites can be tested.

## Local verification

```bash
docker build -t pontoon-nginx-smoke-test .
docker run --rm -p 8080:80 pontoon-nginx-smoke-test
```

Then open <http://localhost:8080> or run:

```bash
curl http://localhost:8080/health
curl http://localhost:8080/pontoon/status
```
