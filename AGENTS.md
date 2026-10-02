# Base44 Dev Environment

## Overview
Single-file Flask API (`api.py`) with one endpoint: `GET /shopify?site=<domain>&cc=<CC|MM|YYYY|CVV>&proxy=<optional>&variant=<optional>`. No database, no env vars, no secrets.

## Running
```bash
docker compose -f docker-compose.base44.yml up -d --build
```
- Python 3.12-slim base, dependencies installed at container start from `requirements.txt` (Flask + aiohttp).
- Runs via `flask run --debug` with live reload enabled (Werkzeug reloader).
- App listens on port 5000 inside the container, mapped to host port 3000.

## Health
The `/shopify` endpoint returns HTTP 400 (missing params) when called without arguments — this is the expected healthy response. The healthcheck treats any HTTP response as success.

## Verifying
```bash
curl "http://localhost:3000/shopify?site=test.myshopify.com&cc=4242424242424242|12|2027|123"
```
Returns JSON with Gateway, Price, Currency, Response, Status, and Time fields.
