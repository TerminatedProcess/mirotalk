# Project: MiroTalk P2P

**Last Updated:** 2026-03-09

## Overview

Free WebRTC P2P video conferencing app. Browser-based, no download or login required. Supports up to 8k/60fps, chat, screen sharing. Open source (AGPLv3), with commercial licensing available.

## Technology Stack

- **Language:** JavaScript (Node.js backend, vanilla JS frontend)
- **Framework:** Express 5, Socket.io 4 (signaling server)
- **WebRTC:** Browser native P2P, no media server (pure peer-to-peer)
- **Testing:** Mocha + should + sinon + proxyquire
- **Package Manager:** npm
- **Dev Tools:** nodemon, prettier

## Key Dependencies

- `express` + `helmet` + `cors` — HTTP server
- `socket.io` — WebRTC signaling
- `httpolyglot` — HTTP/HTTPS on same port
- `express-openid-connect` — OIDC/Auth0 auth
- `jsonwebtoken` + `crypto-js` — JWT/token auth
- `openai` — ChatGPT integration
- `nodemailer` — Email notifications
- `@mattermost/client` — Mattermost integration
- `@ngrok/ngrok` — Tunnel support
- `@sentry/node` — Error monitoring
- `dompurify` + `he` — XSS sanitization

## Directory Structure

```
app/
  src/
    server.js          # Main server entry point (Express + Socket.io)
    api.js             # REST API class (ServerApi)
    config.template.js # Brand/UI config template (copy to config.js)
    validate.js        # Room name / input validation
    xss.js             # XSS checking
    host.js            # Host protection logic
    tokenManager.js    # JWT token management
    htmlInjector.js    # Dynamic HTML injection
    mattermost.js      # Mattermost integration
    logs.js            # Logger (colors, JSON)
    lib/
      nodemailer.js    # Email alerts
  api/                 # REST API docs (swagger.yaml, README)
  ssl/                 # SSL certs (cert.pem, key.pem)
public/
  js/                  # Frontend JS (client.js, landing.js, etc.)
  css/                 # Stylesheets
  images/              # Static assets
tests/
  test-api.js          # API unit tests
  test-validate.js     # Validator unit tests
  test-xss.js          # XSS unit tests
```

## Configuration

- **Runtime env:** `.env` file (copy from `.env.template`)
- **Brand/UI config:** `app/src/config.js` (copy from `config.template.js`, gitignored)
- Both files are gitignored — use templates as source of truth

Key env vars: `HOST`, `PORT`, `NODE_ENV`, `HOST_PROTECTED`, `HOST_USERS`, `API_KEY_SECRET`, `JWT_KEY`, `OIDC_ENABLED`, `NGROK_ENABLED`, `SENTRY_DSN`

## Development Commands

- **Install:** `npm install`
- **Dev:** `npm run start-dev` (nodemon)
- **Start:** `npm start`
- **Test:** `npm test` (mocha tests/)
- **Lint/Format:** `npm run lint` (prettier)
- **Docker build:** `npm run docker-build`

## Architecture Notes

- Pure P2P: clients connect directly after signaling via Socket.io
- Signaling server routes: join, relay ICE candidates, SDP offers/answers
- Host protection: optional username/password gate (or OIDC)
- REST API: protected by `API_KEY_SECRET`, JWT token generation for joining
- Frontend: vanilla JS, no build step — files served statically
- Noise suppression: WebAssembly (rnnoise) via AudioWorklet
- XSS: server-side (`xss.js`) and client-side (`dompurify`)
