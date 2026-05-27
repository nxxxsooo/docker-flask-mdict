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
- ✅ Running on Unraid (dict.mjshao.fun)
- ✅ Server-side caching with LRU + optional Redis
- ✅ Published on GitHub with badges
- ✅ Landing page on portfolio

## Next Steps
- Performance tuning for large dictionaries
- Consider full-text search index

## Resolved Issues
- Added server-side caching to reduce repeated .mdx lookups
