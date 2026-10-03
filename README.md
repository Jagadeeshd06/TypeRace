th# TypeRace

Typing game with solo races vs AI, 2–5 player realtime rooms, server-authoritative rewards, and an immutable coin ledger.

## Run locally
    npm install
    cp .env.example .env   # set SECRET
    SECRET=$(openssl rand -hex 32) npm start     # http://localhost:3000
    npm test                                       # with the server running

## Deploy (Docker)
    docker build -t typerace .
    docker run -d -p 3000:3000 -e SECRET=<long-random> -v typerace-data:/data typerace
Or without Docker: Node 20+, `npm ci --omit=dev`, `NODE_ENV=production SECRET=... npm start`, behind nginx/Caddy with HTTPS
(WebSocket upgrade must be enabled; cookies are `Secure` in production, so HTTPS is required). Run ONE instance
(rooms live in memory; SQLite file in DATA_DIR). To scale out, move rooms to Redis and data to PostgreSQL (see docs/schema-design.prisma).

## What works
Register/login/guest (scrypt hashing, signed httpOnly cookie, rate limits) · solo race vs AI · server issues text and replays your
keystrokes to compute WPM/accuracy/score/rewards · replay and instant-submit protection · anti-cheat flags (impossible WPM/intervals,
robotic timing) · coins via append-only ledger with idempotency keys · XP/levels · leaderboard · multiplayer rooms (code like TYP7XK,
2–5 players, ready, host start, shared server-timestamp countdown, live positions, server-side rate cap, rankings, rewards, 30s
reconnect grace) · accessible keyboard/screen-reader-friendly UI.

**v1.1:** 16-lesson curriculum (5 levels, sequential unlock, WPM+accuracy targets enforced server-side) · practice modes (weak keys generated from your own error history, words, numbers, symbols, code) · statistics (7d/30d/all, WPM chart, practice time) · per-key accuracy keyboard heatmap (symbols + colors, not color alone).

## Not built yet
Shop/garage/customization, achievements, daily challenges, friends, AI coach, admin panel, Google login, matchmaking. Engine hooks exist in `server/engine.ts`; `docs/schema-design.prisma` is the target data model.
