# Hi, I'm Zobro 👋

A developer in Dubai 🇦🇪. I build my own software and run it on my own hardware — the app, the
database, the server, the domain, the deploy pipeline and the backups. Most of it lives under one
name: **The Zverse**.

```
The Zverse monorepo   ~144k lines · 1,000+ commits · 94 SQL migrations · ~100 tables · 266 tests
Runs on               my own Debian server, deployed automatically on every push
Ships to              web · PWA · Windows · macOS · iOS · Android
```

---

## 💬 Z Chat
A real-time chat platform used by a few dozen people. It started in September 2026 as a generated
scaffold and grew into a self-hosted product. Built together with a collaborator; the server,
domain and deploy pipeline it runs on are mine.

**Stack:** React 19 · TanStack Start (SSR on Nitro) · TypeScript · Tailwind v4 · shadcn/ui ·
Supabase (Postgres + RLS, Auth, Realtime, Storage) · Node · Docker · Cloudflare

**Chat**
- Public room, DMs and groups. Read receipts, typing indicators, per-chat unread counts, mentions,
  reactions, edits with history, pins, message search, slow mode, mutes and blocks.
- Discord-style markdown, rich link previews (SSRF-guarded), image lightbox, voice notes, file
  previews, cursor-paged history and day dividers.
- Phone-first layout with a real chat list and bottom dock. Halloween and Christmas animated themes.

**Calls**
- WebRTC voice and video with Supabase Realtime signalling, TURN, ringtones, active-speaker
  detection, guest invite links and a native CallKit bridge on iOS. About 55 files and 60+ tests.

**Accounts, safety and moderation**
- Email/password, Google, passkeys, password recovery and account deletion. Z Chat is also an
  **OAuth 2.1 / PKCE provider** that gives single sign-on to every other Z tool.
- AI moderation before a message is sent, with an editable prompt, link blocklists and auto
  timeouts. Device-fingerprint bans (never IP), ban reasons and appeals. Applications with an
  approval gate. A full admin audit log, backfilled with 140 past actions.
- About 18 admin pages: users, conversations, reports, applications, appeals, feedback, game
  saves, logs and settings.

**Security**
- Approval, blocking and send-rate limits (8 per 10 s, 40 per minute) are enforced in Postgres with
  RLS and triggers, so calling the API directly can't get round them. Verified by impersonating
  users inside rolled-back transactions.
- JWT signature verification, server secrets stored as SHA-256 hashes, and anonymous execute
  revoked on 12 `SECURITY DEFINER` functions.
- Uploads checked by magic bytes, with executables rejected. HSTS, security headers, self-hosted
  fonts (GDPR), legal pages that name every data processor, and a 13+ gate.

**Everywhere**
- Installable PWA with an offline page and web push. Push is sent from the app server and skips
  the device where you already have that chat open.
- Native shells: a **C# WebView2 tray app** with an NSIS installer for Windows, **Electron** for
  macOS, and **Capacitor** for iOS and Android. 7 GitHub Actions workflows build the apps.

**Zaps — the in-app economy (play money)**
- Wallets, instant transfers, per-friend streaks and a leaderboard.
- Player-run shops with escrowed payment, refunds and receipts, and a cosmetics shop for name
  colours and badges.
- A savings bank, a practice stock market, and a Showroom for collections.
- An **AI-judged court** with lawyers, witnesses and chat messages as evidence.
- Pong with 1v1 wagers, and an arcade that pays out from in-game scores.

**Performance and refactoring**
- Main bundle cut from **204 → 131 KB** gzipped. Database advisor warnings **50 → 0**: RLS
  rewritten across 48 policies plus 19 new indexes. Daily realtime egress cut from **1.8 GB to
  under 4 MB**.
- *Untangle* refactor: a 3,343-line chat screen became a 35-line route plus feature modules, and a
  3,000-line call hook was split up. Tests went **113 → 266**, with a server-render smoke test of
  59 routes returning zero 5xx and two-account browser tests matching the old build.

## 🧰 The rest of the Zverse
| Tool | What it is |
|---|---|
| **Zogo** | AI assistant in the app and in chat (`@zogo`), with saved conversations and per-user limits. Runs on Cloudflare Workers AI, with automatic fallback between providers |
| **Zaps Arcade** | Self-hosted HTML5 games portal on a zero-dependency Node server. Sign-in through Z Chat, saves synced to Postgres, scores turned into Zaps |
| **Zlides** | AI presentation maker with quotas and a budget cap |
| **Z Mail** | My own mail panel: Cloudflare Email Worker for inbound, Resend for outbound, SQLite, and a dependency-free MIME parser with tests |
| **Z Console** | Admin console with OAuth sign-in |
| **Z Media** | Upload service on my own server, with byte-range streaming |
| **Z Meet bridge** | Brings Google Meet audio into Z Chat calls using headless Chromium and PulseAudio |
| **Tools** | Expense splitter (tested) and unit converter |

**Z Ink**, the shared design language: an 847-line spec, a 656-line token sheet and `zcheck`, a
conformance checker that runs in CI. All 23 Z tools pass it, and each keeps its own accent colour.
I also built a matching Figma file (tokens, 20 components, desktop and phone screens) and a Figma
plugin that generates the tint ramps.

## 🚀 Shipping and ops
- **Push to live in about a minute.** My server polls every 30 s, does a clean build, checks the
  rendered page's assets and health-checks the app. It **rolls back automatically** if anything
  fails. The app, arcade, slides and media all deploy from one push.
- **Moved production to a new database region with zero data loss:** 40 users, 4,582 messages,
  87 storage files, 113 policies, 118 functions and 26 triggers. Checked with md5 row-hash parity
  before the old project was deleted.
- Nightly encrypted backups (restic), full database dumps and a storage mirror. Restores rehearsed.
- A two-site audit script (48/48 checks: assets, canonical/OG tags, legal pages, `security.txt`,
  headers, redirects) and a Playwright mobile-preview rig (iPhone WebKit + Android Chromium).
- Diagnosed production stalls from the evidence (database timeouts and client polling storms) and
  fixed them with throttling and heartbeat changes instead of a paid upgrade.

---

## 🖥️ Home lab — my own server
A spare laptop turned into a headless **Debian 13** server that hosts production (started on an HP i3; moved to a faster i7 laptop with 16 GB in Nov 2026, same addresses).

- I benchmarked it before moving: **~2.7× single-thread and ~7× throughput** over the VM it
  replaced.
- Docker per service with log caps, a hardware watchdog, `ufw`, unattended upgrades, and
  power-on-after-outage.
- Cloudflare Tunnels for the public apps, plus my own **Headscale** control server (Caddy + Let's
  Encrypt, its own DERP relay, an on-demand exit node) and a one-click Windows "Home VPN" toggle.
- Hardened in Nov 2026: a read-only Docker proxy instead of handing containers the Docker socket, Uptime Kuma reachable only over the tailnet, image-update alerts, services bound to localhost.
- Pi-hole, Uptime Kuma with Discord alerts, Home Assistant, a Minecraft Paper server with its own
  control panel, and Sunshine/Moonlight game streaming locked to the tailnet.
- **Home network:** found the line was GPON fibre (875/320 Mbps wired) behind a double NAT, and
  fixed it by bridging the second router instead of buying a new one.

## 🎙️ Mijo — my own voice assistant
"Hey Mijo" → the laptop does it → a spoken reply in a dry butler voice.

- Split across two machines: the **wake word and actions** run on Windows (openWakeWord); the
  **speech-to-text, intent router and text-to-speech** run on the server (faster-whisper, Piper).
- No cloud and no LLM in the hot path: **~0.4 s** replies (6 ms cached), 48/48 on its routing
  tests, and about 15% of one core.
- It opens and closes apps, types, controls volume and media, sets timers and locks the PC. It
  reads and clicks web pages over the **Chrome DevTools Protocol**, and **joins Google Meet calls
  by voice**. It checks each action actually happened before saying so.
- Also has a tray app, a control panel, a click-through mic badge that hides when muted, and a
  hold-to-talk phone page over the tailnet.

## 🏠 ZAKS Home — home automation
- A **native Android app built without Gradle** (aapt2 / javac / d8 by hand): a 32 KB APK that runs
  as the wall tablet's launcher.
- Talks to Home Assistant over WebSockets, with weather from Open-Meteo, an ambient night mode and
  boot auto-start. It re-arms wireless debugging after every reboot, so I can always deploy to it.
- I debloated the tablet over ADB (62 apps, all reversible) and planned IR control for the house's
  ACs plus a relay layout for the lights.

## 🤖 AI tooling
- Turned 213 past AI coding sessions (~18k messages) into an Obsidian "second brain" with
  project histories and decision logs, plus a search tool over the whole chat archive.
- Wrote agent skills for my own workflows: `z-api-route`, `z-migration`, and `z-ship`, which
  refuses to push unless types and tests pass.
- Built **Z Spend**: a Cloudflare Worker relay plus a dashboard that measured my AI spend, priced
  13,800+ calls exactly, and enforced a spending cap. Retired when I moved to Workers AI.
- Built a car and a boss NPC in Roblox Studio driven over MCP.

## 🔧 Smaller things
- **Windows crash forensics:** analysed 46 unexpected shutdowns from the event log. Traced three
  BSODs to the BattlEye anti-cheat driver and one to a virtual audio driver, and ruled sleep out
  with evidence.
- **tv-debloat:** scripted, reversible clean-up of a Philips Google TV over ADB, and set up an
  open-source ad-free home screen on it.
- **x-mass-delete:** a 400-line userscript that bulk-deletes your own posts on X, dry-run by
  default.

---

## 📈 How I got here
| When | What |
|---|---|
| 2023 | First websites: a Tim Berners-Lee tribute page, a games page, a Bootstrap blog |
| 2024 | JavaScript and canvas: a NASA Mars-rover photo viewer, a **2D Minecraft-style block builder**, a football game, a two-player word game, a **voice-activated selfie app**, **webcam image recognition**, an ASCII keyboard visualiser |
| 2025 | Lua for Roblox, a **translator for a made-up language**, an esports clan site, a Flask app, and **ZOBRO CHAT**, my first chat app (Firebase) |
| 2026 | Rainbow Six operator tier list → **The Zverse**, my own server, Mijo, ZAKS Home |

## 🛠️ Toolbox
**Languages:** TypeScript · JavaScript · Python · SQL · C# · Java (Android) · Lua · Bash · PowerShell
**Web:** React · TanStack Start · Tailwind · Node · WebRTC · PWA / Web Push · OAuth 2.1
**Data:** Postgres · Supabase (RLS, Realtime) · SQLite
**Infra:** Docker · Debian · Cloudflare (Workers, Tunnels, Email, Workers AI) · Tailscale / Headscale · Caddy · restic · GitHub Actions
**Apps:** Electron · Capacitor · WebView2 · Android SDK
**Testing and design:** Vitest · Playwright · Figma
