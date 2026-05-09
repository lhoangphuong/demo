# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A minimal Python/Flask web application deployed on Fly.io via Docker.

## Commands

```bash
# Install dependencies
pip install -r requirements.txt

# Run locally (dev server on port 8080)
python app.py

# Run with gunicorn (matches production)
gunicorn --bind 0.0.0.0:8080 --workers 2 app:app
```

### Fly.io deployment

```bash
# First-time setup (creates the app on Fly.io)
fly launch --no-deploy

# Deploy
fly deploy

# Check status / logs
fly status
fly logs
```

## Architecture

- `app.py` — Flask application entry point; all routes live here.
- `Dockerfile` — Multi-stage-free slim Python image; gunicorn serves on port 8080.
- `fly.toml` — Fly.io config: `sin` (Singapore) primary region, `shared-cpu-1x` / 256 MB VM, scale-to-zero enabled.

The health check endpoint (`GET /health`) returns `{"status": "ok"}` and is used by Fly.io to verify the machine is healthy after deploy.

## Fly.io conventions

- Internal port is **8080**; Fly terminates TLS externally (`force_https = true`).
- The app name in `fly.toml` (`app = "demo"`) must match the app created in your Fly.io account — rename it before the first `fly launch`.
- `auto_stop_machines = "stop"` and `min_machines_running = 0` enable scale-to-zero for cost savings.
