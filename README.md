## 1) linkwise-url-shortener

````md
# Linkwise URL Shortener

### Why it matters
- Builds core CRUD, database migrations, and API testing skills.
- Teaches rate limiting and lightweight analytics that show up in real services.
- Gets you comfortable with Dockerized development and CI basics.

### Project description and features
A tiny, extensible URL shortener that is fast to run locally and easy to evolve.
- Create short links with optional custom aliases
- Redirect handling with click analytics: user agent, referrer, rough geo
- IP rate limiting and soft delete
- OpenAPI docs, unit and E2E tests, optional Next.js dashboard

### Possible stack
- FastAPI, PostgreSQL, Docker, GitHub Actions, optional Redis for rate limits

## Why
Practice CRUD, migrations, API design, testing, and delivery. Add analytics and perf hardening in small steps.

## Stack
FastAPI • PostgreSQL • Docker • GitHub Actions

## Architecture
- API service exposes link CRUD and redirect
- PostgreSQL stores links and click events
- Optional Redis for rate limiting and caching

## Quickstart
```bash
cp .env.example .env
docker compose up -d
make seed
make test
````

## API

- Create link
    
    ```bash
    curl -X POST http://localhost:8080/links \
      -H "Content-Type: application/json" \
      -d '{"url":"https://example.com","alias":"ex"}'
    ```
    
- Redirect  
    GET /ex → 302 to target
    
- Stats
    
    ```bash
    curl http://localhost:8080/links/{id}/stats?from=2025-08-01&to=2025-08-11
    ```
    

## Roadmap

- M0 Setup: compose, migrations, health, Makefile
    
- M1 MVP: create and redirect, tests, OpenAPI
    
- M2 Analytics: click table, referrer, UA, country
    
- M3 Safety: rate limit per IP, E2E tests
    
- Stretch: Next.js dashboard, QR codes
    

## Trade-offs

- Simple IP based rate limit first
    
- Soft delete to avoid breaking shared links
    

## Repo

app/ infra/ scripts/ tests/ README.md

## Implementation guide

1. Data model
    
    - tables: links(id, alias unique, target_url, deleted_at), clicks(id, link_id, ts, ua, ref, country).
        
    - add indexes on alias and clicks(ts, link_id).
        
2. Bootstrap project
    
    - scaffold API, env config, health endpoint, error handler.
        
    - Alembic or Prisma migrations. Seed script for sample links.
        
3. Docker Compose
    
    - services: api, db, optional redis.
        
    - healthchecks and wait-for-db script.
        
4. Create and redirect
    
    - POST /links validates URL, reserves alias via unique index.
        
    - GET /:alias looks up link and issues 302. Handle soft delete.
        
5. Analytics
    
    - middleware on redirect writes a click row with ua, ref, country (GeoLite stub or simple IP map).
        
    - GET /links/{id}/stats aggregates daily counts in SQL.
        
6. Rate limiting
    
    - per IP sliding window using Redis or Postgres advisory locks.
        
    - return 429 with Retry-After.
        
7. Docs and tests
    
    - OpenAPI from code.
        
    - unit tests for validators, integration tests for redirect and stats.
        
8. Security and hygiene
    
    - input validation, safe redirects only to http(s), strip control chars.
        
    - logging with correlation id.
        
9. CI
    
    - lint, test, build image, run container smoke test.
        
    - cache deps to speed up runs.
        
10. Optional dashboard
    

- Next.js page for link list and simple charts.
    
- screenshots for README.
    

**Milestones**: M0 scaffold, M1 MVP, M2 analytics, M3 rate limit and E2E.  
**Acceptance checks**: unique alias enforced, 429 on over limit, stats match inserted clicks.
