# Cafeteria Monitor — Setup Guide

## Project structure

```
deploy/
├── src/
│   ├── server.js          ← main server
│   ├── admin.js           ← admin panel routes
│   ├── biostar.js         ← BioStar 2 WebSocket client
│   ├── filter.js          ← event filter
│   ├── auth-store.js      ← session auth
│   ├── config-store.js    ← config management
│   └── db.js              ← database
├── public/
│   ├── index.html         ← dashboard (served at /)
│   └── admin-panel.html   ← admin panel (served at /admin-panel)
├── data/
│   └── admin.json         ← auto-created on first boot
├── test-biostar.js        ← BioStar 2 connection test CLI
├── package.json
└── INSTALL.md
```

## Step 1 — configure

Create `config/config.json`:
```json
{
  "biostar": {
    "host": "192.168.0.20",
    "port": 443,
    "username": "admin",
    "password": "yourpassword"
  },
  "devices": [],
  "timeSlots": []
}
```

## Step 2 — install dependencies

```bash
npm install
```

## Step 3 — run

```bash
npm start
```

First boot output:
```
[Admin] ⚠  Created default admin — username: admin  password: admin — CHANGE THIS!
[Admin] ⚠  Created default unlock PIN: 123456 — CHANGE THIS via /admin-panel!
[Server] Cafeteria Monitor running on http://0.0.0.0:3000
[Server] Admin panel → http://0.0.0.0:3000/admin-panel
[BioStar] Logged in, session: xxxxx
[BioStar] WebSocket open — authenticating socket...
[BioStar] Event stream started
```

## Step 4 — change defaults (important!)

1. Open `http://server-ip:3000/admin-panel`
2. Log in: `admin` / `admin`
3. Go to **Admins** → create a real account → delete `admin`
4. Go to **Unlock PIN** → set a proper 6-digit PIN

## Test BioStar connection

Run the test CLI to verify BioStar 2 is reachable and streaming events:
```bash
node test-biostar.js 192.168.0.20 admin yourpassword
```

## Optional: SESSION_SECRET env var

Set in a `.env` file so sessions survive restarts:
```
SESSION_SECRET=some-long-random-string-here
```

Without this, every server restart logs everyone out.
