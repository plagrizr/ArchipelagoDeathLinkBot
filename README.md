# StreamReaper

**GitHub:** https://github.com/plagrizr/ArchipelagoDeathLinkBot  
**Creator:** [plagrizr](https://www.plagrizr.com)  
**Version:** 1.2.0

---

## What Is StreamReaper?

StreamReaper is a Windows desktop application that connects your **Twitch** and **TikTok LIVE** audiences directly into your **Archipelago multiworld randomizer** sessions via the DeathLink system. It turns viewer support — bits, subscriptions, gifts — into a coin currency that any viewer can spend to trigger a death in your game in real time.

It sits invisibly between your game client and the Archipelago server, proxying the connection and injecting DeathLink bounce packets on demand. No game modification required. No manual triggers. Viewers earn, viewers spend, you die.

---

## What Is Archipelago DeathLink?

[Archipelago](https://archipelago.gg) is a free, open-source multiworld randomizer platform that lets multiple players play different games simultaneously, sharing items and progression. **DeathLink** is an optional Archipelago feature where a death in one player's game propagates to every other player who has DeathLink enabled — creating a shared consequence system across the entire session.

StreamReaper plugs your stream audience into this system. Instead of deaths only happening between players, your viewers become participants.

> **Huge shoutout to the Archipelago team and community.** StreamReaper wouldn't exist without the incredible work they put into building and maintaining the Archipelago platform and the DeathLink protocol. If you play randomizers and haven't tried Archipelago, go check it out — [archipelago.gg](https://archipelago.gg) — and consider supporting the project.

---

## How It Works

```
[Twitch Chat / TikTok LIVE]
          │
          │  bits · subs · gifts → coins credited to viewer bank
          ▼
    [StreamReaper]   ────────  [Railway API + PostgreSQL]
          │                     (coin balances, per room)
          │  !dlink command → spends coins → injects DeathLink bounce
          ▼
    [Local Proxy Port]  ◄────  [Archipelago Client]
          │
          ▼
    [archipelago.gg]  →  death propagates to all DeathLink players
```

1. Viewers earn coins by cheering bits, subscribing, gifting subs, sending TikTok gifts, or receiving a gift from another viewer
2. Any viewer with enough coins types `!dlink` in chat
3. The bot verifies the balance, deducts the cost, and fires a DeathLink bounce packet through the proxy
4. The death hits every player in the Archipelago session who has DeathLink enabled
5. The coin cost is configurable per streamer — crank it up for harder games, lower it for chaos

---

## Features

### Platform Support
- **Twitch** — full two-way chat integration (reads commands, posts responses)
- **TikTok LIVE** — read-only monitoring via EulerStream API (earns coins from gifts and SuperFan subscriptions, reads `!dlink` and `!dlbank` commands, cannot reply in TikTok chat)
- Multiple channels on both platforms simultaneously

### Coin Economy
- **Twitch Bits** — 1 bit = 1 coin (fixed)
- **Twitch Subs** — configurable coin award; T2/T3 tier multipliers supported
- **Twitch Gift Subs** — coins awarded to the gifter; gift bombs aggregated before crediting
- **TikTok Gifts** — TikTok coin value × configurable multiplier
- **TikTok SuperFan** — configurable flat coin award
- **!dlgive** — viewers can transfer coins to each other directly from their own balance
- Balances are persistent, shared across all streams using the same API key

### Coin Rooms
Choose which coin bank ("room") a session runs against **before you connect** — right from a bar on the dashboard.
- **Default (persistent) room** — your normal, always-on economy where regulars build up coins over time. Can never be deleted or renamed.
- **Session rooms** — spin up a named room (e.g. `julyevent`) and everyone starts from zero and earns fresh during the event. Perfect for multi-streamer sessions where new viewers shouldn't be competing against months of banked coins.
- Up to **5 rooms per host**; create, select, change, and delete from the dashboard
- The room bar **locks while any connection is active**, so a session can't switch mid-stream
- Deleting a room wipes only that room's coins; your default bank is never touched

### Viewer Commands
| Command | Works In | Who Can Use | Effect |
|---|---|---|---|
| `!dlink` | Twitch + TikTok | Anyone | Spend coins to trigger a DeathLink |
| `!dlbank` | Twitch + TikTok | Anyone | Check your current coin balance and the current death cost |
| `!dlgive <user> <amount>` | Twitch | Anyone | Transfer coins from your own balance to another viewer |
| `!dlcommands` | Twitch | Anyone | List the available commands in chat |
| `!archigames` | Twitch | Anyone | List every game in the multiworld (**bold** = DeathLink-enabled) |
| `!dlgames` | Twitch | Anyone | List only the DeathLink-enabled games |
| `!dlgame <slot>` | Twitch | Anyone | Show what game a specific slot is playing |
| `!refund` | Twitch | Host only | Refund the last death to the viewer who triggered it |

### Archipelago Proxy
- Transparent TCP proxy — your Archipelago client connects to a local port instead of directly to `archipelago.gg`
- Bot intercepts the connection and injects DeathLink bounce packets on spend
- Works with any game that supports Archipelago DeathLink
- AP connection state shown live on the dashboard

### Multiworld Game List
- **Automatic capture** — the moment your Archipelago client connects through the proxy, StreamReaper reads the room's players and games. No credentials, no setup.
- **DeathLink Games picker** — the protocol doesn't tell observers which slots have DeathLink enabled, so you tick the real ones in a simple picker. Any game that's actually *seen dying* is auto-confirmed live — which even catches client-toggled games like Ocarina of Time.
- Powers the `!archigames`, `!dlgames`, and `!dlgame` viewer commands
- Long lists auto-split across multiple chat messages to stay under Twitch's 500-character cap

### Dashboard
- **Room bar** at the top — pick your coin room before connecting (locks while live)
- Live connection diagram showing Twitch, TikTok, Bot, Client, and Archipelago connection states
- Color-coded lines: green = connected, amber = partial/waiting, dim = offline
- **Viewer-commands reference panel** — so you always know what to tell chat; game commands light up once a multiworld list is captured
- Death event log table with timestamp, viewer, channel, and coins spent
- Real-time status log with highlighted coin events
- Always-on-top pin button
- Click the plagrizr logo to open the About panel

### Settings
- **Twitch** — OAuth login flow (browser-based), channel list
- **TikTok** — EulerStream API key, account handles to watch
- **Archipelago** — AP server port and local proxy port
- **API** — Railway API key
- **Economy** — death cost, cooldown, sub coins, gift sub coins, T2/T3 multipliers, chat announcement toggle, TikTok gift multiplier, opt-in transaction log
- **Updates** — check for updates, download and install in-app with one click

### Auto-Update
Built-in update checker — Settings → Updates fetches the latest version from GitHub, shows the changelog, and downloads and installs the new exe automatically. The app closes, updates itself, and relaunches.

### Security
- Twitch OAuth token masked in Settings UI
- API key auth on all Railway endpoints
- Coin transfers are atomic database transactions — no double-spend or race conditions possible

---

## Getting Started (End Users)

1. Download the latest `StreamReaper-vX.X.X.exe` from the [GitHub releases page](https://github.com/plagrizr/ArchipelagoDeathLinkBot/releases/latest)
2. Run the exe — no installation required, no Python needed
3. Open **Settings (⚙)**:
   - **Twitch tab** — click Login, authorize your bot account in the browser, add channel names
   - **API tab** — enter your DeathLink API key (provided by the host)
   - **Archipelago tab** — set AP port (from your Archipelago client) and Client Listen Port (default 42069)
   - **Economy tab** — configure death cost, cooldowns, and coin rates
4. *(Optional)* In the **Room bar** at the top, pick your coin bank — leave it on **default** for your normal economy, or create a **session room** for an event. This must be set *before* you connect.
5. In your Archipelago client, change the server from `archipelago.gg:38281` to `localhost:42069`
6. Click **Open Port**, then **Connect Twitch**
7. Your viewers can now start earning and spending coins

---

## Getting Started (Running From Source)

**Requirements:** Python 3.11+

```bash
cd app
pip install -r requirements.txt
python main.py
```

**Build exe:**
```bash
build.bat
```
Produces `app/dist/StreamReaper.exe` and `app/dist/StreamReaper-vX.X.X.exe`.

**Release to GitHub:**
```bash
release.bat
```
Requires [git](https://git-scm.com) and [gh CLI](https://cli.github.com) installed and authenticated.

---

## API Backend

Coin balances are stored in a shared [Railway](https://railway.app) PostgreSQL database via a FastAPI backend. The API handles:
- Credit (earning) and spend (deducting) with atomic transactions
- Balance lookups
- Viewer-to-viewer transfers
- Room registry — list/create/delete coin rooms, capped at 5 per host
- Leaderboard and death event history
- Automated daily database backups retained for 14 days

The API is self-hostable — the source lives in the `api/` folder.

---

## Keywords

StreamReaper, Archipelago multiworld randomizer, DeathLink, Twitch bot, TikTok LIVE bot, stream interaction, viewer participation, coin economy, coin rooms, session rooms, DeathLink games, multiworld game list, bits to coins, sub rewards, gift sub rewards, Twitch chat commands, TikTok chat commands, EulerStream, game death trigger, streamer tool, Windows desktop app, PyQt6, FastAPI, Railway, PostgreSQL, live stream game integration, randomizer tools, streaming tools, plagrizr

---

## Creator

Built by **[plagrizr](https://www.plagrizr.com)**  
GitHub: [plagrizr/ArchipelagoDeathLinkBot](https://github.com/plagrizr/ArchipelagoDeathLinkBot)
