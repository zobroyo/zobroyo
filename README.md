# Hi, I'm Zobro 👋

A developer in Dubai 🇦🇪. I build my own software and run it on my own hardware. Most of what I
make lives under one name — **The Zverse** — a family of apps I design, code, host and operate end
to end: the app, the database, the server it runs on, the domain, the deploy pipeline and the
backups.

---

## The Zverse

### 💬 Z Chat — a full chat platform, self-hosted
A real-time chat app used by a few dozen people, deployed on every push to `main`. Built
together with a collaborator; the home server, domain and deploy pipeline it runs on are mine.

- **Stack:** React 19 + TanStack Start (SSR on Nitro), TypeScript, Tailwind, Supabase
  (Postgres, Auth, Realtime, Storage), Docker, Cloudflare.
- **Features:** DMs and group rooms, voice/video calls (WebRTC + TURN), web push notifications,
  reactions, pins, media uploads, profiles, friends, an admin console, applications and appeals,
  in-app bug reports and support.
- **Security done in the database, not the UI:** approval, blocking and send-rate limits are
  enforced with Postgres row-level security and triggers, so nothing can be bypassed by calling
  the API directly. Device-fingerprint ban enforcement, security headers, HSTS, self-hosted fonts.
- **Ships everywhere:** web, installable PWA with offline page, and native shells for Windows,
  macOS, iOS and Android (Electron / WebView2 / Capacitor).
- **Zaps:** a play-money economy inside the app — wallets, streaks, a shop, a savings bank, a
  practice stock market, an arcade that pays out from in-game scores, and an AI-judged court.
- **Big refactor (Nov 2026):** split a 3,300-line chat screen and a 3,000-line call hook into
  feature modules, unified the server routes, auth and rate limiting — tests went from
  **113 → 266** with behaviour verified identical against the old build.

### 🧰 The rest of the family
| Tool | What it does |
|---|---|
| **Zogo (Z AI)** | AI assistant built into Z Chat, running on Cloudflare Workers AI |
| **Z Games / Zaps Arcade** | Browser games hub that feeds the in-app economy |
| **Zlides** | Presentation maker |
| **Z Mail** | My own mail panel — Cloudflare Email Worker inbound, Resend outbound, a dependency-free MIME parser with tests |
| **Z Console** | Admin console with OAuth sign-in and a full audit log |
| **Z Media** | Media/upload service on my own server |

All of them share one design language, **Z Ink** — a token sheet, component recipes and a
conformance checker that keeps every tool consistent while each keeps its own accent colour.

### 🚀 How it ships
- Push to `main` → my server pulls, builds clean, checks the rendered page's assets, health-checks,
  and **rolls back automatically** if anything is off. Push to live in about a minute.
- Database changes as versioned SQL migrations (90+), tested inside rolled-back transactions.
- Nightly encrypted backups (restic), full DB dumps and storage mirrors — and the restore has been
  rehearsed, including a full move of production to a new Supabase project with row-hash parity
  checks.

---

## 🖥️ Home lab
An old HP laptop turned into a headless **Debian 13** server that now hosts production.

- Docker + Compose, one folder per service, log caps, hardware watchdog, no-sleep, `ufw`.
- Cloudflare Tunnels for public apps, Tailscale for private access, and my own **Headscale**
  control server with its own DERP relay and an on-demand exit node.
- Pi-hole, Uptime Kuma with Discord alerts, Home Assistant.
- Chosen on measured numbers, not specs: ~2.7× single-thread and ~7× aggregate throughput over
  the VM it replaced.

## 🎙️ Mijo — my own voice assistant
"Hey Mijo" → the laptop does the thing → a spoken reply.

- Split across two machines: **wake word + actions** on Windows, **speech-to-text, intent routing
  and text-to-speech** on the home server.
- Python, openWakeWord, faster-whisper, Piper TTS, Chrome DevTools Protocol for browser control.
- No cloud and no LLM in the hot path: **~0.4 s** replies, 48/48 on its routing test set, and it
  verifies each action actually happened before saying so. Joins Google Meet calls on command.

## 🏠 ZAKS Home — home automation
A wall-mounted Android tablet running my own control-panel app over Home Assistant.

- Native Android app built **without Gradle** (aapt2 / javac / d8 by hand) — 32 KB APK.
- Talks to Home Assistant over WebSockets, weather from Open-Meteo, ambient night mode,
  auto-starts as the launcher and survives reboots.

## 📺 couch-tv — open-source Google TV home screen
An ad-free home screen and phone remote for Google TV. MIT licensed.
Animated scenery backgrounds with depth-layered clocks, "keep watching", on-TV settings, multiple
languages.

## 🔧 Smaller things
- **Figma plugin** that generates tinted colour ramps from the Z Ink tokens.
- **tv-debloat** — scripted, reversible clean-up of Android/Google TVs over ADB.
- **x-mass-delete** — userscript to bulk-delete your own posts on X.
- **AI spend tracker** — a Cloudflare Worker relay + dashboard that measured my own model spend.

---

## 📈 How I got here
| When | What |
|---|---|
| 2023 | First websites — HTML and CSS projects |
| 2024 | JavaScript: a 2D Minecraft-style game, a football game, word games, a voice-selfie app, image recognition |
| 2025 | Lua for Roblox, a translator for a made-up language, the first version of a chat app |
| 2026 | The Zverse: Z Chat and its siblings, a home server running production, Mijo, ZAKS Home, couch-tv |

## 🛠️ Toolbox
`TypeScript` `React` `Node.js` `Python` `SQL / Postgres` `Lua` `Bash` `PowerShell` ·
`Supabase` `Cloudflare (Workers, Tunnels, Email, Workers AI)` `Docker` `Debian` `Tailscale / Headscale` ·
`WebRTC` `PWA / Web Push` `Electron` `Capacitor` `Android` `Playwright` `Figma`
