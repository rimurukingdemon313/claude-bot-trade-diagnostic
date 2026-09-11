# Paper Trading Bot Dashboard (TradeLocker)

A trading dashboard with a React front end, an Express API server, and a Python
analysis/trading engine that talks to TradeLocker.

## Stack

- **Frontend:** React 19, Vite 7, Tailwind CSS v4, TanStack Query, Recharts, wouter
- **Server:** Express 5 (TypeScript, bundled at startup with esbuild), pino logging
- **Engine:** Python 3 (standard library only) — market data, risk engine, SMC analysis, TradeLocker client
- **Deploy:** Dockerfile included

## Getting started

```sh
npm install
cp .env.example .env   # fill in your own credentials
npm run build
npm start              # serves the API and the built dashboard
```

The server listens on `PORT` (default `5000`).

## Configuration

All credentials come from environment variables — see `.env.example`. Nothing
secret is committed to this repository.

Key variables:

- `TRADELOCKER_EMAIL`, `TRADELOCKER_PASSWORD`, `TRADELOCKER_SERVER`, `TRADELOCKER_URL`
- AI provider key(s) used by the analysis engine
- `PORT`

## Tests

```sh
pytest          # 35 Python tests
npm run typecheck
npm run build
```

`pytest.ini` puts the project root on the import path, so `pytest` works from
the repository root with no extra setup.

## Connection diagnostics

`diagnose_tradelocker_connection.py` is a standalone helper (not part of the
live trading path). Run it with your real credentials in the environment to see
which HTTP approach successfully authenticates against TradeLocker:

```sh
python3 diagnose_tradelocker_connection.py
```

It prints PASS/FAIL per approach plus the raw response body on failure.

## Demo

```sh
python3 run_smc_demo.py
```

## Docker

```sh
docker build -t trading-dashboard .
docker run -p 5000:5000 --env-file .env trading-dashboard
```

## Layout

```
analysis_engine/    Python analysis modules (SMC, signals)
server/             Express API (TypeScript)
src/                React dashboard
tests/              Python test suite
public/             Static assets (favicon, robots.txt)
*.py                TradeLocker client, market data, risk engine, diagnostics
```
