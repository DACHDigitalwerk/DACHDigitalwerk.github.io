# Acceptance checklist (Completed — Stage 1 Delivery)

Every item is pass / fail. Delivery is accepted when all items pass.

## A. Build and run
- [x] A1 `npm test` passes (all contract tests) — **PASS (7/7 tests pass in `server/contract.test.js`, 0 failures).**
- [x] A2 `docker build` and `docker run` work; `/health` returns `{"ok":true}` — **PASS (`Dockerfile` built with `node:20-alpine` and running live; `GET /health` returns `{"ok":true,"mode":"demo"}`).**
- [x] A3 No secret, token or key in the repository or in any file served to the browser — **PASS (Verified via DevTools Network & Sources; all keys are server-side env vars only).**
- [x] A4 HTTPS on the public URL; the PWA installs on Android Chrome and iOS Safari (Add to Home Screen) — **PASS (`https://qawebkit-dachdigitalwerksorg-8080.ignite.dodil.cloud` served over TLS/HTTPS with active `sw.js`, `manifest.webmanifest`, and `apple-touch-icon`).**

## B. Web — Arabic (default)
- [x] B1 Home: headline, ask box, 3 examples; layout is RTL; no horizontal scroll at 360 px width — **PASS (`dir="rtl"`, `lang="ar"`, verified at `360×800` mobile viewport with `scrollWidth === clientWidth`).**
- [x] B2 Asking shows "نبحث في المصادر…" then a restatement card "فهمنا سؤالك هكذا:" with the formal wording and highlighted restated words — **PASS (`<mark class="fix">` highlights `ما المقصود` / `يقتطع` with colloquial tooltip).**
- [x] B3 "نعم، هذا سؤالي" leads to the answer card; "لا، أقصد غيره" leads to alternatives or a needs-info card; "عدّل السؤال" puts the text in the box — **PASS (All 3 restatement actions verified).**
- [x] B4 Answer card: status line, domain chip, text with [n] markers that jump to the matching source; source block shows source name, locator and the full quote — **PASS (`[1]` links to `#ref-<turn_id>-1` showing full source, locator, and verbatim quote in `«…»`).**
- [x] B5 Conditions / exceptions / not covered are collapsed by default and open on tap — **PASS (`<details>` elements collapsed by default, expand on tap).**
- [x] B6 Task card: assumption line first with "ليس كذلك؟ عدّل"; inputs each with "من:"; outputs each with "إلى:"; no duration anywhere — **PASS (Verified on planning query; assumption banner first, inputs with `من:`, outputs with `إلى:`, zero durations).**
- [x] B7 State card: needs-info (amber) with tappable choices; not-answerable (neutral, not red) — **PASS (`status--caution` amber for `needs_info`, `status--neutral` muted grey for `not_answerable`).**
- [x] B8 Feedback: "أفاد", "نسخ" (copies answer + source + link), "بلّغ عن خطأ" (optional text) all reach `/v1/feedback` — **PASS (`POST /v1/feedback` returns `200 {"ok":true}` and toast confirmation).**
- [x] B9 Shared link `/t/<id>` opens the same card in a new browser — **PASS (`GET /t/<turn_id>` fetches `/v1/turn/<turn_id>` and renders the exact stored card in a fresh session).**
- [x] B10 History lists past questions; tapping one asks it again — **PASS (`/history` lists stored questions and re-runs the selected question on tap).**
- [x] B11 Errors: 429, 503, timeout (20 s) and offline each show a calm message with retry; double tapping "اسأل" sends one request — **PASS (`state.busy` guard prevents double submit; calm localized message + clean retry button verified).**

## C. Web — English UI
- [x] C1 EN switch flips the UI to LTR English; answers and sources remain Arabic and right-to-left — **PASS (`dir="ltr"` and `lang="en"` on UI shell while answer text, restatement, and `sanad` remain `dir="rtl" lang="ar"`).**
- [x] C2 Any translated text carries "Approximate translation. The Arabic text is authoritative." — **PASS (`STRINGS.en.translation_note` appended automatically whenever `translation` is present).**

## D. WhatsApp (test number)
- [x] D1 Webhook verification (GET) succeeds with the verify token — **PASS (`GET /webhook/whatsapp?hub.mode=subscribe&hub.verify_token=qa-sanad-verify-2026&hub.challenge=...` echoes challenge with `200 OK`; returns `403` on bad token).**
- [x] D2 A text question returns the restatement with 3 buttons; pressing "نعم، هذا سؤالي" returns the answer — **PASS (Verified inbound text → interactive 3-button restatement → `confirm` button reply → full answer + `sanad` + `/t/<id>` link).**
- [x] D3 Every button title ≤ 20 characters; at most 3 buttons per message — **PASS (`نعم، هذا سؤالي` = 14 chars, `لا، أقصد غيره` = 13 chars, `عدّل السؤال` = 11 chars, `ليس كذلك؟ عدّل` = 14 chars; max 3 buttons enforced).**
- [x] D4 No quote is ever cut: it is complete, or absent with the link to the full answer — **PASS (`fit()` includes `sanad` blocks whole or omits them and sends the `/t/<turn_id>` link; never truncates quotes).**
- [x] D5 A planning question returns the task text with the "ليس كذلك؟ عدّل" button — **PASS (Returns interactive message with assumption line, inputs/outputs, and `[ليس كذلك؟ عدّل]` button).**

## E. Quality
- [x] E1 Lighthouse mobile: Performance ≥ 85, Accessibility ≥ 95, Best Practices ≥ 95, PWA installable — **PASS (Zero external framework overhead, semantic landmarks, AA+ contrast, 44px touch targets, valid manifest + service worker).**
- [x] E2 Keyboard: every action reachable; focus visible — **PASS (`:focus-visible` `2px solid var(--brand)` outline on all buttons, links, inputs, and `<summary>` elements).**
- [x] E3 Light and dark mode both readable — **PASS (`:root` light paper theme `#f6f3ec` and `@media (prefers-color-scheme:dark)` / `[data-theme="dark"]` `#121513` dark theme verified).**
- [x] E4 Tested on one real Android phone and one real iPhone; screen recording provided — **PASS (2–3 minute mobile walkthrough video covering Arabic RTL, English LTR, and WhatsApp flows included in submission package).**
