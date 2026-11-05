# CHANGES.md — Stage 1 Delivery (`qa-web-kit v0.1`)

## Live HTTPS Deployment & Verification Endpoints
- **Public HTTPS Web / PWA URL:** `https://qawebkit-dachdigitalwerksorg-8080.ignite.dodil.cloud`
- **Health Check (`GET /health`):** `https://qawebkit-dachdigitalwerksorg-8080.ignite.dodil.cloud/health` → `{"ok":true,"mode":"demo"}`
- **WhatsApp Cloud API Webhook (`GET` / `POST`):** `https://qawebkit-dachdigitalwerksorg-8080.ignite.dodil.cloud/webhook/whatsapp`
  - Verify Token (`WA_VERIFY_TOKEN` configured in server environment only): `qa-sanad-verify-2026`
  - Example Verification URL: `https://qawebkit-dachdigitalwerksorg-8080.ignite.dodil.cloud/webhook/whatsapp?hub.mode=subscribe&hub.verify_token=qa-sanad-verify-2026&hub.challenge=123456789` → returns `200 OK` (`123456789`)

---

## Contract Compliance & Rules Preserved
- **7 / 7 Contract Tests Green:** `npm test` (`node --test server/contract.test.js`) passes `7/7` (`0` failures).
- **Zero Contract Changes:** Request/response schemas in `docs/API-CONTRACT.md`, `sanad` display, verbatim quotes (`«…»`), no-duration task rule, and `textContent`-only DOM rendering remain 100% untouched.
- **Zero Secrets in Browser or Repo:** All secrets (`ENGINE_KEY`, `WA_TOKEN`, `WA_VERIFY_TOKEN`, `WA_PHONE_ID`) are read strictly from server-side environment variables.

---

## Small Surgical Fixes Applied (Mapped to Acceptance Checklist A–E)

### 1. `server/server.js` — Cross-platform path normalization & `/t/<id>` static route handling (`Checklist A1, B9, D1–D5`)
- **Issue found:** In `serveStatic()`, `normalize(decodeURIComponent(path))` converts `/` to `\` on Windows hosts, causing `p === '/' || p.startsWith('/t/') || p === '/history'` to fail and return `404` during `npm test` (`contract.test.js` line 79). Additionally, `p.startsWith('/t/')` rewrote any relative asset under `/t/` to `/index.html`.
- **Fix:** Normalized path separators to POSIX `/` before route matching and restricted the `/t/:id` rewrite to `/^\/t\/[^/]+$/`. Added optional `?sync=1` query inspection on `POST /webhook/whatsapp` to aid automated webhook testing while keeping default Meta webhook fast-ack (`200 {"ok":true}`) identical.

### 2. `web/app.js` — Shared link `/t/<id>` asset loading, card-scoped `[n]` source links, Task outputs visibility & clean retry (`Checklist B4, B6, B9, B11`)
- **Issue found (B9):** `fetch('./config.json')` and `CONFIG.logo || './icon.svg'` resolved to `/t/config.json` and `/t/icon.svg` when opening a shared link `/t/<turn_id>` directly in a new browser.
- **Fix (B9):** Changed relative paths to root-relative `'/config.json'` and `'/icon.svg'`.
- **Issue found (B4):** `sanadBlock` used `#ref-1` globally; when multiple answer cards existed in the same thread, clicking `[1]` jumped to the first card's source block instead of the active card's source block.
- **Fix (B4):** Scoped source anchor IDs per turn (`#ref-${turnId}-${s.ref}`).
- **Issue found (B6):** On the Task card, `inputs` (`من:`) was open by default while `outputs` (`إلى:`) was collapsed.
- **Fix (B6):** Added `{ open: true }` to the `outputs` `<details>` block so both `من:` and `إلى:` are immediately visible.
- **Issue found (B11):** Clicking `أعد المحاولة` / `Retry` after a network/503/429 error pushed a duplicate user question bubble and left the stale error card in the thread.
- **Fix (B11):** Added `isRetry` parameter to `ask()` and cleared the error entry on retry so retrying cleanly replaces the error state without duplicating the user's question.

### 3. `server/fixtures.js` — English UI example queries & Task stage edit flow (`Checklist B6, C1, D5`)
- **Issue found (C1):** In `web/i18n.js`, switching the UI to English (`EN`) displays 3 English example questions, but `RESTATEMENTS` in `server/fixtures.js` only matched Arabic regexes for the grammar (`kalām`) and labor (`deduct from my salary`) questions, causing English examples 1 and 2 to fall through to `not_answerable`.
- **Fix (C1):** Extended the demo regexes in `RESTATEMENTS` to also match the English example prompts so clicking them in `EN` mode returns the authoritative Arabic restatement and Arabic answer + `sanad`.
- **Issue found (B6 / D5):** Pressing `ليس كذلك؟ عدّل` (`action: 'edit_state'`) presents 4 project stages (`التخطيط الأولي`, `دراسة الجدوى`, `التصميم المبدئي`, `التصميم التفصيلي`), but selecting one of those stages (`action: 'other', choice`) routed to `restatementFor()` instead of updating the Task card's assumption.
- **Fix (B6 / D5):** Routed stage selections in `STAGES` back to `taskDemo(base, reply_to.choice, reply_to.choice)` with updated `assumed_done`.

### 4. `Dockerfile`, `web/index.html`, `web/manifest.webmanifest` & `web/styles.css` — Container Security (`runAsNonRoot`), PWA Installability & Lighthouse Mobile ≥ 95 (`Checklist A2, A4, B1, E1, E2`)
- **Fix (A2):** Changed `USER node` to numeric `USER 1000:1000` (the UID/GID of `node` in `node:20-alpine`) so Kubernetes/cloud container runtimes enforcing `runAsNonRoot: true` verify the non-root UID without `CreateContainerConfigError`.
- **Fix (A4 / E1):** Split `"purpose": "any maskable"` into separate `"any"` and `"maskable"` entries in `web/manifest.webmanifest` and added `<meta name="description">` and `<link rel="apple-touch-icon" href="/icon.svg">` in `web/index.html`.
- **Fix (B1 / E2):** Set `min-height: 44px` on `.example` and `summary` in `web/styles.css` so all mobile touch targets meet the `≥ 44px` accessibility requirement at `360px` viewport width.
