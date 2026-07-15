<div align="center">

```
 █████╗ ██╗     ██╗      █████╗ ██╗   ██╗██████╗  ██████╗ ██████╗ ███████╗███╗   ██╗
██╔══██╗██║     ██║     ██╔══██╗╚██╗ ██╔╝██╔══██╗██╔═══██╗██╔══██╗██╔════╝████╗  ██║
███████║██║     ██║     ███████║ ╚████╔╝ ██║  ██║██║   ██║██║  ██║█████╗  ██╔██╗ ██║
██╔══██║██║     ██║     ██╔══██║  ╚██╔╝  ██║  ██║██║   ██║██║  ██║██╔══╝  ██║╚██╗██║
██║  ██║███████╗███████╗██║  ██║   ██║   ██████╔╝╚██████╔╝██████╔╝███████╗██║ ╚████║
╚═╝  ╚═╝╚══════╝╚══════╝╚═╝  ╚═╝   ╚═╝   ╚═════╝  ╚═════╝ ╚═════╝ ╚══════╝╚═╝  ╚═══╝
```

**A complete Sic Bo (Tai/Xiu) dice gambling plugin for Spigot**  
with Discord integration, jackpot system, MySQL support, and rich embed notifications.

[![Version](https://img.shields.io/badge/version-2.0.1-brightgreen)](#)
[![Spigot](https://img.shields.io/badge/Spigot-1.20.4-orange)](#)
[![Java](https://img.shields.io/badge/Java-17%2B-blue)](#)
[![License](https://img.shields.io/badge/license-MIT-lightgrey)](#)

</div>

---

## Table of Contents

1. [Overview](#overview)
2. [Features](#features)
3. [Requirements](#requirements)
4. [Installation](#installation)
5. [Configuration](#configuration)
   - [config.yml](#configyml)
   - [messages.yml](#messagesyml)
6. [Commands & Permissions](#commands--permissions)
   - [Player Commands](#player-commands)
   - [Admin Commands](#admin-commands)
   - [Discord Commands](#discord-commands)
7. [Discord Bot Setup](#discord-bot-setup)
8. [Account Linking](#account-linking)
9. [MySQL Setup](#mysql-setup)
10. [The Jackpot System](#the-jackpot-system)
11. [How the Game Works](#how-the-game-works)
12. [Upgrading from 1.0.0](#upgrading-from-100)
13. [Troubleshooting](#troubleshooting)
14. [FAQ](#faq)

---

## Overview

**AllayDoden** brings the classic Sic Bo dice game to your Minecraft server as a fully automated, always-running mini-game. Every session, players place bets on whether the sum of three dice will be High (**TAI**, 11–17) or Low (**XIU**, 4–10). A live BossBar counts down the session timer, the dice roll automatically, and winnings are paid out instantly through your Vault economy.

What makes AllayDoden stand out is its **Discord integration**. A built-in bot ("Jack") connects your Discord channel directly to the game — players can link their accounts, place bets, check their balance, and watch every result unfold in rich colour-coded embeds, all without leaving Discord. The **jackpot system** adds an extra layer of excitement: losing bets feed a shared pool, and every triple dice result triggers a random winner who walks away with the entire pot.

---

## Features

### Core Game
- 🎲 **Continuous session loop** — the game runs 24/7 with no admin intervention required
- 🔴🔵 **TAI / XIU betting** — bet on High (sum 11–17) or Low (sum 4–10)
- 🟣 **SPECIAL rule** — a sum of exactly 3 or 18 (triple 1s or triple 6s) means the house wins all bets
- 📊 **Live BossBar** — shows session number, time remaining, and total bets on each side
- 🔊 **Configurable sounds** — distinct sounds for wins, losses, specials, and jackpots
- 💰 **Vault economy** — works with any Vault-compatible economy plugin

### Discord Integration
- 🤖 **Built-in Discord bot** ("Jack") — no external application needed
- 💬 **Rich embed notifications** — all announcements use colour-coded Discord embeds
- 🔗 **Account linking** — link Discord to Minecraft with a simple in-game code
- 🎰 **Bet from Discord** — linked players use `!tai` / `!xiu` without being online
- 💳 **Balance check** — `!money` command shows in-game balance from Discord
- 📣 **Universal bet announcements** — every bet (in-game or Discord) appears in the channel

### Jackpot System
- ⭐ **Auto-accumulating pool** — a configurable % of every losing bet feeds the pot
- 🎯 **Triple-trigger** — any triple dice result (1-1-1 through 6-6-6) triggers the jackpot
- 🏆 **Random winner** — one lucky bettor from that session wins the entire pool
- 💾 **Persistent** — the pool survives server restarts (saved to `jackpot.yml`)
- 🛠️ **Admin control** — manually add to or set the pool at any time

### Storage & Performance
- 🗄️ **Optional MySQL storage** — store account links in a database with HikariCP pooling
- 📁 **YAML fallback** — MySQL connection failure automatically falls back to `links.yml`
- ⚡ **Async I/O** — all file/database writes happen off the main thread
- 🎯 **Optimised random** — shared `Random` instance replaces per-roll allocation
- ⚙️ **BossBar interval** — configurable update frequency to reduce load on weak servers

---

## Requirements

| Requirement | Version |
|---|---|
| Java | 17 or higher |
| Spigot / Paper | 1.20.4 or compatible |
| Vault | Any current release |
| Economy plugin | Any Vault-compatible (EssentialsX, CMI, etc.) |
| MySQL *(optional)* | 5.7+ or MariaDB 10.3+ |
| Discord bot token *(optional)* | Any valid bot token |

> **Note:** The plugin ships as a fat jar — MySQL connector (8.3.0) and HikariCP (5.1.0) are bundled inside. You do **not** need to install them separately.

---

## Installation

1. **Download** `allaydoden-2.0.1.jar` from the releases page.
2. **Place** the jar in your server's `plugins/` directory.
3. **Ensure** Vault and an economy plugin (e.g. EssentialsX) are installed and loaded.
4. **Start** your server. AllayDoden generates its default configuration files under `plugins/AllayDoden/`.
5. **Stop** the server and edit `config.yml` and `messages.yml` to your preferences.
6. **Restart** the server. The game starts automatically.

The plugin will log a line like:

```
[AllayDoden] AllayDoden 2.0.1 enabled. Session #1 started.
```

If you see this, the game is running. Skip to [Configuration](#configuration) to tune it.

---

## Configuration

### config.yml

Below is a fully annotated reference of every setting. Default values are shown.

```yaml
# ─────────────────────────────────────────────────────────────────
#  AllayDoden  ·  config.yml  ·  v2.0.1
# ─────────────────────────────────────────────────────────────────

# Format string used when displaying money amounts.
# Uses Java DecimalFormat syntax.
format-money: "#,###"


# ── Task ────────────────────────────────────────────────────────
task:
  # Duration of each session in seconds.
  time-per-session: 125

  # How often the BossBar title is refreshed (in ticks, 1 tick = 1 second).
  # Increase to 2 or 4 on weak servers to reduce main-thread load.
  bossbar-update-interval: 1


# ── Bet Settings ────────────────────────────────────────────────
bet-settings:
  # Minimum bet amount (in Vault currency units).
  min-bet: 500

  # Maximum bet amount.
  max-bet: 1000000

  # Tax percentage applied to profit on a winning bet.
  # E.g. tax: 10 means 10% of profit is deducted.  0 = no tax.
  tax: 0

  # Payout multiplier. 2.0 means a winning bet of 1000 pays back 2000.
  payout-ratio: 2.0

  # Betting is locked this many seconds before the session ends.
  # Prevents last-second bets after the dice intention is predictable.
  disable-while-remaining: 15

  # If true, the plugin re-rolls when the result would be 3 or 18
  # (eliminating the SPECIAL outcome entirely).
  disable-special: false


# ── BossBar ─────────────────────────────────────────────────────
boss-bar:
  enabled: true
  reloading:
    # Seconds between a session ending and the next one starting.
    time: 5


# ── Sound ───────────────────────────────────────────────────────
sound:
  win:
    enabled: true
    sound-name: ENTITY_PLAYER_LEVELUP
    volume: 1.0
    pitch: 1.0
  lose:
    enabled: true
    sound-name: ENTITY_VILLAGER_NO
    volume: 1.0
    pitch: 1.0
  jackpot:
    enabled: true
    sound-name: UI_TOAST_CHALLENGE_COMPLETE
    volume: 1.0
    pitch: 1.0


# ── Jackpot ─────────────────────────────────────────────────────
jackpot:
  enabled: true

  # Amount the pool is seeded with after each payout.
  seed-amount: 10000

  # Percentage of each losing bet contributed to the pool.
  # 5 means 5% of each loss feeds the pot.
  contribute-percent: 5

  # true  = any triple (1-1-1, 2-2-2 … 6-6-6) triggers the jackpot.
  # false = only the SPECIAL sums (3 and 18) trigger the jackpot.
  trigger-on-any-triple: true


# ── MySQL ────────────────────────────────────────────────────────
mysql:
  # Set to true to use MySQL instead of links.yml.
  # Falls back to links.yml automatically if the connection fails.
  enabled: false
  host: localhost
  port: 3306
  database: allaydoden
  username: root
  password: ""
  # HikariCP maximum connection pool size.
  pool-size: 5


# ── Discord ──────────────────────────────────────────────────────
discord:
  # Set to true to enable the Discord bot.
  enabled: false

  # Your bot's token from the Discord Developer Portal.
  bot-token: "YOUR_BOT_TOKEN_HERE"

  # The ID of the channel where the bot should listen and announce.
  channel-id: "YOUR_CHANNEL_ID_HERE"

  # Display name used in embed footers.
  bot-name: "Jack"

  # Seconds before a generated link code expires.
  link-code-expiry-seconds: 300

  embed:
    # Whether to announce every successful bet placement.
    announce-bets: true
    # Whether to announce win/lose/special results.
    announce-results: true

    # Embed accent colours as decimal RGB integers.
    # Tip: convert hex to decimal — e.g. #2ECC71 → 3066993
    color-win:     3066993   # Green
    color-lose:    15158332  # Red
    color-special: 10181046  # Purple
    color-jackpot: 16766720  # Gold
    color-session: 3447003   # Blue
    color-bet:     16776960  # Yellow
```

---

### messages.yml

`messages.yml` controls every piece of text the plugin sends to players.  
All strings support `&` colour codes (e.g. `&a` = green, `&c` = red, `&l` = bold).

**Available placeholders per message:**

| Message key | Placeholders |
|---|---|
| `session-start` | `{session}` |
| `bet-success` | `{side}`, `{amount}` |
| `win-message` | `{payout}` |
| `lose-message` | `{bet}` |
| `special-message` | `{bet}` |
| `insufficient-funds` | `{balance}` |
| `invalid-amount` | `{minBet}`, `{maxBet}` |
| `min-bet-exceeded` | `{minBet}` |
| `max-bet-exceeded` | `{maxBet}` |
| `jackpot-win` | `{player}`, `{amount}` |
| `bossbar-betting` | `{session}`, `{time}`, `{xiuBet}`, `{taiBet}` |
| `bossbar-result-xiu` | `{total}` |
| `bossbar-result-tai` | `{total}` |
| `bossbar-result-special` | `{total}` |
| `bossbar-jackpot` | `{player}`, `{amount}` |

---

## Commands & Permissions

### Player Commands

| Command | Description | Permission |
|---|---|---|
| `/tai <amount>` | Bet on TAI (High, sum 11–17) | `doden.use` |
| `/xiu <amount>` | Bet on XIU (Low, sum 4–10) | `doden.use` |
| `/asb` | Show current session info, your bet, and jackpot pool | `doden.use` |
| `/asb rules` | Show payout rules, SPECIAL rule, and jackpot trigger | `doden.use` |
| `/asb link` | Generate a 6-digit code to link your Discord account | `doden.use` |

**Example session workflow:**

```
You: /asb
> Session #14 | 78s remaining | TAI: $12,000 | XIU: $9,500
> Jackpot pool: $47,200
> You have not placed a bet yet.

You: /tai 5000
> ✅ Bet placed: TAI (High) — $5,000

[78 seconds pass — dice roll]
> 🎲 Result: 4 — 5 — 6  (sum 15) → TAI wins!
> 💰 You won $10,000!
```

---

### Admin Commands

All admin commands require the `doden.admin` permission.

| Command | Description |
|---|---|
| `/asbadmin pause` | Toggle the session between PLAYING and PAUSED states. |
| `/asbadmin force <d1> <d2> <d3>` | Force the next dice result. Values must be 1–6. Takes effect at the next session end. |
| `/asbadmin settime <seconds>` | Jump the countdown to a specific number of remaining seconds. |
| `/asbadmin bets [sort]` | List all active bets. Sort modes: `tai`, `xiu`, `highest`, `lowest`. |
| `/asbadmin jackpot add <amount>` | Add funds to the jackpot pool manually. |
| `/asbadmin jackpot set <amount>` | Set the jackpot pool to an exact amount. |
| `/asbadmin jackpot info` | Display current pool size, seed amount, and trigger setting. |
| `/asbadmin reload` | Hot-reload `config.yml` and `messages.yml` without restarting. |
| `/asbadmin info` | Show plugin diagnostics — session state, session number, economy status. |

---

### Discord Commands

These commands are typed in the configured Discord channel. Most require an account link first.

| Command | Requires Link | Description |
|---|---|---|
| `!link <code>` | No | Confirm an account link using the code from `/asb link`. |
| `!unlink` | Yes | Remove the link between your Discord and Minecraft accounts. |
| `!tai <amount>` | Yes | Place a TAI (High) bet from Discord. |
| `!xiu <amount>` | Yes | Place a XIU (Low) bet from Discord. |
| `!money` | Yes | Check your current in-game Vault balance. |

---

## Discord Bot Setup

### Step 1 — Create a bot application

1. Go to [discord.com/developers/applications](https://discord.com/developers/applications).
2. Click **New Application**, give it a name (e.g. "Jack"), and confirm.
3. In the left sidebar, click **Bot**.
4. Click **Add Bot**, then **Yes, do it!**
5. Under the bot's username, click **Reset Token**, then copy the token. Keep this private.

### Step 2 — Enable required Gateway Intents

Still on the **Bot** page, scroll down to **Privileged Gateway Intents** and enable:

- ✅ **Message Content Intent** — required for Jack to read `!tai`, `!xiu`, etc.
- ✅ **Server Members Intent** — recommended for presence features

Click **Save Changes**.

### Step 3 — Invite the bot to your server

1. In the left sidebar, click **OAuth2 → URL Generator**.
2. Under **Scopes**, check `bot`.
3. Under **Bot Permissions**, check: `Send Messages`, `Embed Links`, `Read Message History`, `View Channels`.
4. Copy the generated URL, open it in a browser, and invite Jack to your server.

### Step 4 — Get the channel ID

1. In Discord, enable Developer Mode: **User Settings → Advanced → Developer Mode**.
2. Right-click the channel where you want the bot to operate.
3. Click **Copy ID**. This is your `channel-id`.

### Step 5 — Configure AllayDoden

In `plugins/AllayDoden/config.yml`:

```yaml
discord:
  enabled: true
  bot-token: "paste-your-token-here"
  channel-id: "paste-your-channel-id-here"
  bot-name: "Jack"
```

Restart the server. You should see:

```
[AllayDoden] Jack connected to Discord.
```

---

## Account Linking

Account linking connects a Discord user to their Minecraft account so they can bet from Discord and have their balance tracked correctly.

### Linking an account

**Step 1** — In-game, generate a link code:

```
/asb link
> Your link code is: 483921  (expires in 5 minutes)
```

**Step 2** — In the Discord bot channel, confirm the link:

```
!link 483921
> ✅ Successfully linked to Steve!
```

The link is now active. The player can use `!tai`, `!xiu`, and `!money` from Discord.

### Unlinking an account

From Discord:

```
!unlink
> 🔓 Unlinked from Steve.
```

### Notes

- Each Minecraft account can be linked to exactly one Discord account, and vice versa.
- Link codes expire after the duration set in `discord.link-code-expiry-seconds` (default: 5 minutes).
- If using MySQL, links persist in the `allaydoden_links` table and survive server restarts.
- If using YAML, links are stored in `plugins/AllayDoden/links.yml`.

---

## MySQL Setup

MySQL storage is optional. The plugin defaults to `links.yml` and only switches to MySQL when `mysql.enabled: true` is set. If the database is unreachable at startup, it falls back to YAML automatically with a warning in the console.

### Step 1 — Create the database

```sql
CREATE DATABASE allaydoden CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'allaydoden'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON allaydoden.* TO 'allaydoden'@'localhost';
FLUSH PRIVILEGES;
```

### Step 2 — Configure AllayDoden

```yaml
mysql:
  enabled: true
  host: localhost
  port: 3306
  database: allaydoden
  username: allaydoden
  password: "your_password"
  pool-size: 5
```

### Step 3 — Restart the server

The plugin creates the `allaydoden_links` table on first start if it does not exist:

```sql
CREATE TABLE IF NOT EXISTS allaydoden_links (
    discord_id    VARCHAR(64)  NOT NULL,
    discord_name  VARCHAR(64)  NOT NULL,
    minecraft_uuid VARCHAR(36) NOT NULL,
    PRIMARY KEY (minecraft_uuid)
);
```

### Migrating from YAML to MySQL

Existing `links.yml` entries are **not** migrated automatically. To carry them over:

1. Note each player's Discord ID and Minecraft UUID from `links.yml`.
2. Insert rows into `allaydoden_links` manually via SQL, or let players re-link.

---

## The Jackpot System

The jackpot adds a progressive prize pool on top of regular winnings.

### How it works

1. **Feeding the pool** — every bet that does not win (loss or SPECIAL) donates a percentage of its value to the jackpot pool. This percentage is controlled by `jackpot.contribute-percent` (default: 5%).

   > A player loses a $10,000 bet → $500 goes to the jackpot pool.

2. **Triggering the jackpot** — when all three dice show the same number (e.g. 3-3-3), the jackpot fires. The trigger can be configured:
   - `trigger-on-any-triple: true` — any of the six triples triggers it.
   - `trigger-on-any-triple: false` — only sum 3 (1-1-1) and sum 18 (6-6-6) trigger it.

3. **Selecting the winner** — one player from all bets placed in that session is chosen randomly. All bettors have an equal chance regardless of bet size or side.

4. **Paying out** — the winner receives the entire pool. The pool then resets to `jackpot.seed-amount` (default: $10,000) ready for the next accumulation cycle.

5. **No bettors** — if a triple rolls but nobody has bet that session, a "near miss" message is broadcast and the pool is kept intact.

### Pool persistence

The current jackpot pool amount is saved to `plugins/AllayDoden/jackpot.yml` after every change. It is loaded on plugin startup, so the pool survives server restarts.

### Admin controls

```
/asbadmin jackpot info          → Current pool: $47,200  Seed: $10,000
/asbadmin jackpot add 50000     → Added $50,000 to jackpot pool.
/asbadmin jackpot set 100000    → Jackpot pool set to $100,000.
```

---

## How the Game Works

### Session lifecycle

```
┌─────────────────────────────────────────────────────────────┐
│  Session starts                                             │
│   • BossBar appears for all online players                  │
│   • "Session #N started" message broadcast                  │
│   • Discord: 🎲 Session #N Started embed                    │
│                                                             │
│  Betting phase  (configurable duration, default: 125s)      │
│   • Players use /tai or /xiu to bet                         │
│   • Discord-linked players use !tai / !xiu                  │
│   • Each player can place exactly one bet per session        │
│   • BossBar updates every second (configurable)             │
│   • Betting locks when time-remaining ≤ disable-while-      │
│     remaining (default: last 15 seconds)                    │
│                                                             │
│  Dice roll                                                  │
│   • Three dice rolled (1–6 each)                            │
│   • Sum calculated: 3–10 = XIU, 11–18 = TAI                 │
│   • Sum 3 or 18 = SPECIAL (house wins all, unless           │
│     disable-special: true)                                  │
│   • All three dice equal = potential jackpot trigger        │
│                                                             │
│  Payout                                                     │
│   • Winners: deposited (bet × payout-ratio) minus tax       │
│   • Losers / SPECIAL: bet is lost; % feeds jackpot pool     │
│   • Jackpot: if triggered, random bettor wins entire pool   │
│   • Discord embed: result, dice, sum                        │
│                                                             │
│  Cooldown  (default: 5 seconds)                             │
│   • BossBar shows result colour                             │
│   • Next session starts automatically                       │
└─────────────────────────────────────────────────────────────┘
```

### Payout formula

```
gross_return = bet_amount × payout_ratio
profit       = gross_return − bet_amount
tax_deducted = profit × (tax_percent / 100)
final_payout = round(gross_return − tax_deducted)
```

With the defaults (`payout_ratio: 2.0`, `tax: 0`):  
A winning bet of **$5,000** returns **$10,000** (profit of $5,000, no tax).

### SPECIAL rule

When the sum is exactly 3 (1+1+1) or 18 (6+6+6), the SPECIAL rule applies:
- All bets are lost — no winners, regardless of which side was bet.
- All lost bet amounts contribute to the jackpot pool.
- A purple SPECIAL embed is sent to Discord.

The SPECIAL rule can be disabled entirely with `bet-settings.disable-special: true`.

---

## Upgrading from 1.0.0

1. Stop the server.
2. Replace the old jar with `allaydoden-2.0.1.jar` in `plugins/`.
3. **Do not delete** the existing `plugins/AllayDoden/` folder — `links.yml` is preserved.
4. Start the server once to generate the new config sections.
5. Stop the server and configure the additions in `config.yml`:
   - `jackpot` section
   - `mysql` section (if desired)
   - `discord.embed` section
   - `task.bossbar-update-interval`
   - `sound.jackpot`
6. Restart the server.

> **MySQL migration note:** Existing `links.yml` entries are not migrated automatically to MySQL. Players can re-link, or entries can be inserted manually — see [MySQL Setup](#mysql-setup).

---

## Troubleshooting

### Bot does not connect to Discord

- Verify `discord.enabled: true` in `config.yml`.
- Confirm the bot token is correct and has not been regenerated in the Developer Portal.
- Ensure the **Message Content Intent** is enabled in the Developer Portal under Bot → Privileged Gateway Intents.
- Check the console for a specific error message from JDA.

### `!tai` returns "You need to link your account first" even after linking

- Confirm the link was successful — you should have seen "✅ Successfully linked to `<name>`" after `!link <code>`.
- Make sure you are typing in the correct channel (the one matching `channel-id`).
- If the issue persists, type `!unlink` and re-link. This was a known race condition fixed in **2.0.1**.

### BossBar is not visible to some players

- This is the join-mid-session issue fixed in **2.0.0**. Ensure you are on 2.0.0 or later.
- If the issue still occurs, confirm the `boss-bar.enabled: true` setting.

### The game stopped progressing after a session (no new session starts)

- This was caused by a `ClassCastException` in the payout loop, fixed in **2.0.1**.
- Use `/asbadmin info` to check the session state. If it shows `PLAYING` with 0 time remaining, a `/asbadmin settime 1` can nudge the session to end and restart.
- Upgrade to 2.0.1 to prevent recurrence.

### MySQL connection fails at startup

- Check your host, port, database name, username, and password.
- Ensure the MySQL server is running and reachable from the Minecraft server host.
- Verify the database user has `SELECT`, `INSERT`, `DELETE` privileges on the configured database.
- The plugin will fall back to `links.yml` automatically — check the console for the fallback warning.

### No win/lose sounds play

- Confirm `sound.win.enabled: true` and `sound.lose.enabled: true`.
- Verify the sound names are valid Bukkit `Sound` enum values for your server's Minecraft version. A full list is available in the Spigot Javadocs.

### Vault is not found

- Ensure the Vault plugin jar is in your `plugins/` folder and loads before AllayDoden.
- Ensure a Vault-compatible economy plugin is also present and enabled.

---

## FAQ

**Q: Can players bet both TAI and XIU in the same session?**  
A: No. Each player may place exactly one bet per session.

**Q: What happens to a Discord-linked player's bet if they are not online when the session ends?**  
A: AllayDoden uses Vault's offline player support. Winnings are deposited into (and losses deducted from) the player's balance even if they are not connected. They will see their updated balance the next time they log in.

**Q: Can I use AllayDoden without a Discord bot?**  
A: Yes. Set `discord.enabled: false` in `config.yml`. All in-game functionality works independently. The Discord integration is entirely optional.

**Q: Can I run multiple Discord bots in different channels?**  
A: The current implementation supports a single channel. To target a different channel, change `channel-id` in `config.yml` and reload or restart.

**Q: How do I disable the SPECIAL rule?**  
A: Set `bet-settings.disable-special: true`. The plugin will re-roll any result that would be a sum of 3 or 18.

**Q: Is the jackpot pool shared across server restarts?**  
A: Yes. The pool is written to `plugins/AllayDoden/jackpot.yml` asynchronously after every change and loaded again on plugin startup.

**Q: What if no one bets during a jackpot session?**  
A: If a triple dice result occurs but no bets were placed that session, a "no winner" message is broadcast and the pool remains intact for the next trigger.

**Q: Can I import existing 1.0.0 YAML links into MySQL?**  
A: Not automatically. You can INSERT rows manually into `allaydoden_links` using the Discord ID, Discord name, and Minecraft UUID from `links.yml`, or have players re-link after enabling MySQL.

**Q: Does AllayDoden work on Paper or Folia?**  
A: AllayDoden is built and tested against the Spigot API. It should work on Paper (which is Spigot-compatible). Folia uses a different scheduler model and is not officially supported.

**Q: Can two players link to the same Discord account?**  
A: No. Each Discord account can be linked to exactly one Minecraft account. Attempting to re-link a Discord account that is already linked returns "Account already linked to `<player>`."

---

<div align="center">

---

*AllayDoden is released under the MIT License.*  
*Built with ❤️ for Minecraft server communities.*

</div>
