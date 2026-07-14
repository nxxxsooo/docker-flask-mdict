# MdictLive

## Overview
Self-hosted dictionary server that serves MDict (.mdx/.mdd) files over HTTP with a web UI. Written in Python (Flask). Deployed on Unraid Docker.

## Architecture
- Flask web server + mdict-utils for .mdx parsing
- Server-side caching: in-memory LRU + optional Valkey/Redis backend
- Docker image: multi-arch (amd64/arm64)
- Public at dict.mjshao.fun via NPM reverse proxy

## Key Files
- `app.py` — Flask server, routes, caching logic
- `Dockerfile` — Multi-stage build
- `docker-compose.yml` — Dev/prod compose
- `requirements.txt` — Python deps

## Patterns & Conventions
- Dictionary files mounted via Docker volume
- CSS/JS served from static/ directory
- Landing page at mjshao.fun/mdict-live/

## Current Status
- Dormant: the Unraid container and `dict.mjshao.fun` public endpoint are retired
- Dictionary source data remains at `/mnt/user/media/Dicts/`
- Server-side caching with LRU + optional Redis is implemented
- Published source, container images, and portfolio landing page are retained

## Next Steps
- Revisit as an AI-assisted dictionary rather than restoring the old public reader as-is
- Preserve the existing MDict corpus and parsing support as reusable inputs

## Resolved Issues
- Added server-side caching to reduce repeated .mdx lookups
