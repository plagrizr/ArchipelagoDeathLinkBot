# DeathLink Bot

A desktop controller that routes your **Twitch** and **TikTok** audience's events into [Archipelago](https://archipelago.gg)'s **DeathLink** system — letting viewers spend earned coins to trigger deaths in your game.

---

## Features

- **Twitch Integration** — earns coins from bits, subs (T1/T2/T3 multipliers), gift subs, and resubs
- **TikTok LIVE Integration** — earns coins from gifts and SuperFan subscriptions via EulerStream
- **Archipelago DeathLink Relay** — proxies your local AP client through a configurable port; injects DeathLink bounces on demand
- **Coin Economy** — per-viewer coin balances backed by a shared API; `!dlink` spends coins, `!dlbank` shows balance
- **Chat Commands** — `!dlink` (trigger death), `!dlbank` (check balance) on both Twitch and TikTok
- **Auto-Update** — Settings → Updates checks GitHub and downloads the latest exe in-app

---

## Quick Start

1. Download `DeathLink-vX.X.X.exe` from the [latest release](https://github.com/plagrizr/ArchipelagoDeathLinkBot/releases/latest)
2. Run it — no install needed
3. Open **Settings (⚙)** and configure:
   - **Twitch** — log in with your bot account, add channels to watch
   - **TikTok** — add your EulerStream key and account handles (optional)
   - **Archipelago** — set your AP port and local proxy port
   - **API** — enter your DeathLink server API key
   - **Economy** — tune coin costs, multipliers, cooldowns

---

## How It Works

```
[Twitch / TikTok]
        │  events (bits, subs, gifts)
        ▼
   [DeathLink Bot]  ──── coin API (Railway) ────
        │
        │  DeathLink Bounce packet
        ▼
   local proxy port  ◄──  Archipelago client
        │
        ▼
   archipelago.gg
```

Viewers earn coins by supporting the stream. Any viewer with enough coins can type `!dlink` in chat to spend them and send a DeathLink bounce to the game.

---

## Requirements (running from source)

```
Python 3.11+
PyQt6
twitchAPI>=4.2.0
TikTokLive>=6.0
websockets
requests
```

Install: `pip install -r app/requirements.txt`  
Run: `python app/main.py`

---

## Building the exe

```bat
build.bat
```

Produces `app/dist/DeathLink.exe` and `app/dist/DeathLink-vX.X.X.exe`.

---

## Releasing

```bat
release.bat
```

Reads `version.json`, commits `README.md` + `version.json` to GitHub, creates a release, and attaches the versioned exe.

---

## Creator

Made by **[plagrizr](https://www.plagrizr.com)**
