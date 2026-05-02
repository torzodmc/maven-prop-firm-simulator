# 🏢 Maven Prop Firm Challenge Simulator

> **A free, browser-based prop firm challenge simulator built as an addon for [LocalBarReplay](https://github.com/torzodmc/LocalBarReplay) — the open-source bar replay trading tool.**

Practice passing real Maven Trading challenges without risking a single dollar. Load historical market data, replay it bar-by-bar, and let the simulator enforce real Maven rules in real time.

---

## 🎯 What This Simulates

Full rule enforcement for all 4 Maven Trading account types:

| Plan | Challenge Phases | Daily Drawdown | Max Drawdown | Profit Target |
|---|---|---|---|---|
| **1-Step** | 1 → Funded | 3% (equity-based) | 5% trailing | 8% |
| **2-Step** | 2 → Funded | 4% (equity-based) | 8% static | Phase 1: 8% / Phase 2: 5% |
| **3-Step** | 3 → Funded | 2% (equity-based) | 3% static | 3% per phase |
| **Instant Funding** | Direct Funded | 2% (equity-based) | 3% trailing | None |

### Rules Enforced in Real Time:
- ✅ **Daily drawdown** — equity-based, resets each UTC day
- ✅ **Max drawdown** — static (based on initial balance) or trailing (based on peak equity)
- ✅ **Profit target** — per-phase, must reach before advancing
- ✅ **Minimum profitable trading days** — 3 days with ≥ 0.5% gain required
- ✅ **Consistency score** — biggest winning day can't exceed 20% of total profit (Instant Funding)
- ✅ **Max floating loss** — open unrealized loss capped at 1% of balance (Instant Funding)
- ✅ **Leverage lock** — crypto leverage fixed to 2× per Maven rules
- ✅ **Phase progression** — automatically advances Phase 1 → Phase 2 → Funded when targets are hit
- ✅ **Breach detection** — instant lockout with a clear reason displayed

---

## 📸 What It Looks Like

When active, the addon replaces the Account panel with a live **Challenge HUD**:

- 📊 **Profit progress bar** — how close you are to the phase target
- 📉 **Daily drawdown meter** — turns orange → red as you approach the limit
- 📉 **Max drawdown meter** — same, with static/trailing label
- 🟢 **Profitable days dots** — fills up as you hit qualifying days
- 💰 **Live equity** — updates every replay tick
- ❌ **Breach screen** — clear reason why the account was failed
- 🔄 **Restart button** — restart the challenge from scratch

---

## 🚀 Installation

This addon runs inside **[LocalBarReplay](https://github.com/torzodmc/LocalBarReplay)** — you need that first.

### Step 1 — Get LocalBarReplay

```bash
git clone https://github.com/torzodmc/LocalBarReplay.git
```

Or [download the ZIP](https://github.com/torzodmc/LocalBarReplay/archive/refs/heads/main.zip) and extract it.

### Step 2 — Add This Addon

The addon is already included in LocalBarReplay. Just:

1. Open `index.html` in your browser (no server needed — it runs locally)
2. Click the **📊 Indicators** button in the toolbar
3. Scroll to **🧩 Trade Addons** at the bottom
4. Check **🏢 Maven Prop Firm**
5. The Account section transforms into the Challenge HUD

> **Alternatively**, if you want to use the latest version from this repo, copy `maven_prop_firm.js` into the `addons/` folder of your LocalBarReplay clone — it auto-detects.

### Step 3 — Load Your Data & Start

1. Load a CSV of historical price data via the **Load Data** button
2. Pick your **Maven plan** and **account size** from the HUD
3. Click **Start Challenge**
4. Press **▶ Play** (or Space) and start trading

---

## 🎮 How to Use

### During a Challenge

- Trade using the Buy/Long and Sell/Short buttons in the panel
- The HUD updates **every tick** — drawdown meters move as price moves
- If you breach a rule, trading locks **immediately** and the reason is shown

### Speed Control

- **Above 1×** — normal candle-by-candle replay
- **Below 1×** — **sub-candle mode**: price updates within each candle, so you see the candle build bar by bar (great for studying exact drawdown moments)

### Advancing Phases

When you hit the profit target with enough profitable days, the HUD automatically advances you to the next phase (or marks you as **Funded**). Your balance resets to the original account size to simulate a fresh funded account.

---

## 📋 Account Sizes Available

$5,000 · $10,000 · $25,000 · $50,000 · $100,000 · $200,000

---

## ⚙️ Technical Details

This is a single JavaScript file addon that hooks into LocalBarReplay's addon system using these lifecycle callbacks:

| Hook | Purpose |
|---|---|
| `onActivate()` | Injects the HUD, locks leverage/lots |
| `onDeactivate()` | Restores original UI |
| `onBeforeTrade()` | Blocks trades when challenge is failed or rules would be violated |
| `onEveryTick()` | Checks all drawdown rules on every replay frame |
| `onTradeClose()` | Records P&L for profitable day tracking |

No backend, no server, no accounts. Everything runs in your browser tab with your own data.

---

## 🔗 Main Project

This addon is part of **[LocalBarReplay](https://github.com/torzodmc/LocalBarReplay)** — a free, open-source bar replay tool for discretionary traders.

Features of the main project:
- Load any CSV of OHLCV data
- Replay bar-by-bar with speed control (0.1× to 20×)
- Sub-candle mode for granular replay
- Full trading simulation (long/short, TP/SL, leverage)
- Built-in indicators: EMA, SMA, RSI, MACD, Bollinger Bands
- Trade history with MFE/MAE tracking
- Extensible addon system for custom data recording and rule enforcement

---

## 📝 License

MIT — free to use, modify, and distribute.

---

*Not affiliated with Maven Trading. This is an unofficial simulator for educational and practice purposes only.*
