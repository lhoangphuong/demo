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
- `templates/index.html` — SaaS landing page; single-file HTML/CSS/JS, no build step.
- `Dockerfile` — Multi-stage-free slim Python image; gunicorn serves on port 8080.
- `fly.toml` — Fly.io config: `sin` (Singapore) primary region, `shared-cpu-1x` / 256 MB VM, scale-to-zero enabled.

The health check endpoint (`GET /health`) returns `{"status": "ok"}` and is used by Fly.io to verify the machine is healthy after deploy.

## Fly.io conventions

- Internal port is **8080**; Fly terminates TLS externally (`force_https = true`).
- The app name in `fly.toml` is `demo-8xqghg`. The live URL is `https://demo-8xqghg.fly.dev`.
- `auto_stop_machines = "stop"` and `min_machines_running = 0` enable scale-to-zero for cost savings.

## Skills

Skills are in `.claude/skills/`. Before starting any task that matches a skill name, read its `SKILL.md` first. Available skills:

- `brainstorming` — refine ideas before writing code
- `writing-plans` / `executing-plans` — break work into small tasks and execute them
- `test-driven-development` — RED-GREEN-REFACTOR cycle
- `systematic-debugging` — structured approach to finding bugs
- `requesting-code-review` / `receiving-code-review` — code review workflow
- `verification-before-completion` — checks before marking a task done
- `using-git-worktrees` — isolated branches for parallel work
- `dispatching-parallel-agents` / `subagent-driven-development` — multi-agent patterns
- `finishing-a-development-branch` — merge and cleanup process
- `frontend-design` — distinctive, production-grade UI with strong aesthetic direction

## Keeping CLAUDE.md current

Update this file whenever you make a change that affects how someone would develop or deploy this project:

- New routes, services, or modules added to `app.py` — update **Architecture**
- Changes to port, region, VM size, or scaling in `fly.toml` — update **Fly.io conventions**
- New dependencies or changed run commands — update **Commands**
- Changes to the GitHub Actions workflow (new secrets, triggers, steps) — add a **CI/CD** section

Do this in the same commit as the code change, not as a follow-up.
