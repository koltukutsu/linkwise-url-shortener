 # Linkwise API (FastAPI)

 This is the backend service for the Linkwise URL Shortener. The structure is prepared with an industry-standard layout.

 ## Layout
 - `src/linkwise_api/` — application modules (API, core, db, schemas, services, middleware, utils)
 - `tests/` — unit and integration tests
 - `alembic/` — database migrations

 ## Quickstart
 ```bash
 cp .env.example .env
 make install
 make dev
 ```

 ## Migrations
 ```bash
 make alembic-rev   # create new revision
 make migrate       # upgrade to head
 ```


