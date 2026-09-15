# DaviddTech Auto-Trade Setup

**Take a DaviddTech strategy off a chart and have it trade your Bybit account automatically.**

Written for DaviddTech students who have never automated anything. No coding. Nothing to install
on a server. Nothing to rent.

There are two ways through, and they end in the same place.

---

## Pick your route

### 🤖 [AUTOMATED-SETUP.md](AUTOMATED-SETUP.md) — Claude does it, you answer questions

Paste one file into **Claude Code** and it interviews you: it pulls the DaviddTech strategy sheet,
shows you what is actually performing live, and once you pick a row it builds the TradingView alert
itself — condition, webhook, message, error check.

**About 10 minutes.** Your second bot takes two.

Needs Claude Code plus [tv-cdp-mcp](https://github.com/pbajkovic-hub/tv-cdp-mcp), the TradingView
driver it uses.

### ✋ [EASY-STEPS.md](EASY-STEPS.md) — 18 steps, by hand, no AI

Plain English, one instruction at a time, nothing assumed. Works no matter what software you have.

**About 25 minutes.**

### 📖 [MANUAL-SETUP.md](MANUAL-SETUP.md) — the long version

The same route with every screen explained and every trap called out. Read this when something
has gone wrong and you want to know *why*.

---

## What you need either way

- A **paid TradingView plan** — Essential or higher. Free plans cannot send webhooks, and there is
  no way around it.
- A **DaviddTech subscription**, so you have a strategy to run.
- A **Bybit account** with money in it.

---

## ⚠️ Read this before you start

**This is live trading. There is no practice mode on this route.** The moment your alert fires, a
real order goes onto your real Bybit account.

Three things that are not optional:

1. **Fund the account with an amount you would shrug at losing.** It will be traded while you are
   asleep. You can always add more later.
2. **Your Bybit key must have withdrawal permission OFF.** With it off, the worst case is unwanted
   trades. With it on, anyone who gets that key can empty the account. Nothing here needs it.
3. **Fire your first trade by hand at the smallest size you can set**, then check it on Bybit
   before letting it run. That is where setup mistakes get caught while they are still cheap.

**Never paste your API secret, or the alert message generated for you, into a chat, a screenshot,
a Discord channel or a forum post.** The keys inside that message are encrypted, but they still
authorise trades on your account.

---

## How it works

Four pieces in a line. A signal is born on a chart and ends as an order on an exchange.

```
  STRATEGY  ──▶  TRADINGVIEW  ──▶   FASTCLOUD   ──▶   BYBIT
  (chart)         ALERT            (DaviddTech)      (your account)

  decides         "POST this        decrypts your     places the
  when to         message to        key, turns it     actual buy or
  buy/sell        this URL"         into an order     sell order
```

You install nothing and you run nothing. There is no program on your computer and no server to
rent — so you can close your laptop and it keeps trading.

---

## If you want to stop it

Worth knowing **before** you need it:

- **Pause the alert** in TradingView's Alerts panel — no new trades. Anything already open stays
  open, so check your positions too.
- **Delete the API key** on Bybit — this cuts it off completely and instantly, whatever any alert
  does. This is the emergency brake.

Either way, you close open positions yourself, on Bybit.

---

## Not a DaviddTech subscriber?

This guide will not help you — the route runs through DaviddTech's service and assumes you have a
DT strategy to run.

The TradingView side is separate and open to anyone:
**[tv-cdp-mcp](https://github.com/pbajkovic-hub/tv-cdp-mcp)** drives TradingView Desktop from an AI
agent or a plain terminal — read your charts, export indicator data to CSV, manage alerts — with
no dependency on DaviddTech or StrategyFactory.

---

## Disclaimer

Not financial advice. Not affiliated with DaviddTech, StrategyFactory, TradingView or Bybit.

Automated trading can lose money faster than manual trading, because it does exactly what it was
told, thousands of times, without ever getting nervous. Past performance on a strategy card comes
from a backtest and a short live record — every strategy has losing stretches. Trade only what you
can afford to lose, and satisfy yourself that each step is right before you arm anything.
