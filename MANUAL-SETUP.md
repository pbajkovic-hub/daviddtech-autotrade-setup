# From Strategy to Auto-Trade — A Beginner's Onboarding Guide

**Who this is for:** complete newcomers to automated trading. No coding experience assumed.

**What you'll have at the end:** a strategy running on a TradingView chart that automatically
sends its buy/sell signals to your exchange account — placing **real orders with real money**.

> ### ⚠️ This is live trading. There is no practice mode on this route.
>
> The moment your alert fires, a real order goes onto your real Bybit account. That is the
> point of the setup — but it means two things are not optional:
>
> - **Fund the account with an amount you would genuinely shrug at losing.** You can always
>   add more later.
> - **Your first trade is fired deliberately, at the smallest size you can set** (Section 6).
>   You watch that one land and check it on the exchange before letting it run.
>
> **Read Section 8 before you connect anything.** Section 4 step 3 — withdrawal permission
> **off** — is the protection that applies no matter what else you do.

---

## Section 0 — The big picture

Four pieces, in a line. A signal is born on a chart and ends as an order on an exchange.

```
  ┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
  │  1. STRATEGY     │     │  2. TRADINGVIEW  │     │  3. FASTCLOUD    │     │  4. EXCHANGE     │
  │  (DaviddTech     │ ──▶ │     ALERT        │ ──▶ │   (DaviddTech)   │ ──▶ │   (Bybit LIVE)   │
  │   chart + rules) │     │                  │     │                  │     │                  │
  │                  │     │ "when the strategy│     │ decrypts your key│     │ places the actual│
  │ decides WHEN to  │     │  fires, POST this │     │ and turns it into│     │ buy / sell order │
  │ buy and sell     │     │  message to a URL"│     │ an exchange order│     │ on your account  │
  └──────────────────┘     └──────────────────┘     └──────────────────┘     └──────────────────┘
        you pick it            you set it up          already running          you connect it
```

**In one paragraph:** you pick a DaviddTech strategy and open its chart in TradingView. On that
chart you create an **alert** — TradingView's "tell me when this happens" feature. Instead of
emailing you, the alert POSTs a small text message to a **webhook URL** belonging to DaviddTech's
FastCloud service. That message carries your own exchange key, encrypted, so FastCloud can place
the matching order on your account. Nothing is manual after setup — but nothing is risky either,
because the API key you create can trade and *cannot withdraw* — so the worst thing a leak or a
bug can do is place trades you did not want, never move money off the exchange.

> **You install nothing and you run nothing.** There is no program on your computer, no server to
> rent, no terminal. Everything below happens in a browser and in TradingView. You can close your
> laptop and the strategy keeps trading.

---

## Section 1 — Before you start

Four things. Get all four before Section 2, or you'll get stuck halfway.

- [ ] **A TradingView account** — free tier works to learn the flow. (Note: free accounts are
      limited to a small number of active alerts, and webhook alerts require a paid plan. You can
      follow every step below and see every screen on a free account; sending the webhook is the
      one part that needs an upgrade.)
- [ ] **A StrategyFactory account** — this is where the ready-made strategies live.
- [ ] **An exchange account with API access** — this guide uses **Bybit**. Create it and fund it
      with an amount you have decided in advance you can afford to lose. This account will be
      traded automatically, including while you are asleep.
- [ ] **A DaviddTech subscription**, with a DT strategy you can add to a chart. This guide is
      written for DaviddTech students and assumes throughout that you are running a DT strategy —
      that is what lets you skip most of the setup later on.

> **Time:** about 20–30 minutes the first time, most of it reading. There is nothing to
> install and nothing to compile.

---

## Section 2 — Get your strategy (StrategyFactory)

StrategyFactory is a library of pre-built, pre-tested strategies. You don't write anything — you
pick one and take its chart link over to TradingView.

**1. Open the Strategy Library.** Log in to StrategyFactory and click **Strategy Library** in the
left sidebar. Every row is one strategy, on one trading pair, on one timeframe. The columns show
how it has performed: 7-day / 30-day / 90-day return, win rate, and profit factor.

> **[Annotation: red box around the left-sidebar "Strategy Library" item, and a second box around
> the table's "Win rate" / "Profit factor" columns]**
>
> Don't agonise over which strategy to pick right now — you are learning the plumbing. Pick any
> row. You can swap the strategy later without redoing any of the setup.

**2. Open that strategy's actions menu.** At the right-hand end of the row, next to the pink
**Copy bot** button, click the small **chevron (⌄)**. A menu appears with four choices.

> **[Annotation: red box around the "Open in TradingView" menu item; red arrow pointing at the
> chevron button that opened the menu]**
>
> **Click "Open in TradingView"** — *not* "Copy this bot". "Copy this bot" is StrategyFactory's
> own hosted-bot product and is a different path from the one this guide teaches. "Open in
> TradingView" hands you the chart, which is what you want.

**3. You land on a TradingView chart.** The link opens a **view-only** copy of the strategy's
chart — the indicator is already applied and already configured with the exact settings that
produced the numbers you saw in the table. Keep this tab open; Section 3 starts here.

---

## Section 3 — Set up TradingView

The chart you just opened belongs to someone else, so you can't add an alert to it yet. First you
make your own copy.

**1. Notice "View Only Mode".** Top-left of the chart says **View Only Mode**, and top-right
there's a **Copy** button. This is the giveaway that you're looking at someone else's chart.

> **[Annotation: red box around the "View Only Mode" label top-left, and a red box + arrow on the
> "Copy" button top-right]**
>
> The panel on the left and the table bottom-right are the strategy's own backtest results, drawn
> by the indicator. They describe the *strategy's* historical performance — not your account.

**2. Click Copy, and confirm.** A dialog appears: **"Make copy of chart layout"**, with the new
layout's name pre-filled. You can rename it to something you'll recognise later (e.g.
`MY FIRST BOT — AAVE 45m`). Click **Make copy**.

> **[Annotation: red box around the "New layout name" text field; red box + arrow on the
> "Make copy" button]**
>
> Give it a real name now. Once you have several bots, `...copy copy` tells you nothing.

**3. You now own this chart.** "View Only Mode" is gone and the toolbar is fully editable. Every
remaining step happens on *your* copy.

**4. Open the alert dialog.** With your copy open, click the **Alert** button in the top toolbar
(the small clock icon labelled "Alert"). Keyboard shortcut: **Alt + A**.

**5. Set the Condition — this is what makes it fire.** On the **Settings** tab, the **Condition**
row has two dropdowns. Set the first to the **strategy's name** (e.g. *AI vs. Ninja Turtle by
@DaviddTech*) — **not** the symbol name. Leave the second on the strategy's own alert option
(usually *alert() function calls only* or *Order fills only*).

> ⚠️ This is the single most common place beginners go wrong. If the first dropdown says
> `AAVEUSDT.P` instead of the strategy's name, the alert will fire on a *price* condition and your
> strategy's logic is doing nothing.

**6. Fill the Message box.** Still on the **Settings** tab, scroll to **Message**. This is *what*
gets sent. Paste the message you generate in Section 5 — it carries your own encrypted API key, so
it is personal to you and should not be shared.

> If you are wiring a DaviddTech Pine strategy the recommended way, this box contains exactly one
> line and nothing else:
> ```
> {{strategy.order.alert_message}}
> ```
> The strategy fills in the direction, size and levels itself each time it fires. Section 5
> explains where that comes from.

**7. Switch to the Notifications tab and add the Webhook URL.** Click the **Notifications** tab at
the top of the dialog. Tick the **Webhook URL** checkbox, and paste this address into the field
beside it:

```
https://fastcloud.daviddtech.com/webhooks/bybit
```

> **This same address is correct for everybody.** It is not personal and you do not need your own.
> What *is* personal is the Message box above, because your encrypted key lives in there.

> ⚠️ **`/webhooks/` is plural. Check it character by character.** `/webhook` (singular) and
> `/bybit` are both real addresses that accept your alert, return success, and then **do nothing**
> — no order, no error, no warning anywhere. If your alerts fire and nothing ever trades, this is
> the first thing to check.

> **The two boxes are not interchangeable.** The **Message** box (Settings tab) is *what* is sent.
> The **Webhook URL** box (Notifications tab) is *where* it is sent. Swapping them is mistake #3
> in Section 7.

**8. Click Create.** The alert is now live and will fire the next time your strategy signals.
You can see it, pause it, or delete it in the **Alerts** panel on the right-hand side of the
chart.

> **Free-plan note:** if the Webhook URL checkbox is greyed out, your TradingView plan doesn't
> include webhooks. Everything up to this point still worked — you just can't send the signal out
> until you upgrade.

---

## Section 4 — Connect your exchange

This is where you create the credentials FastCloud uses to place orders on your behalf. **Read
this section completely before clicking anything.** Step 3 is the most important step in this
entire guide.

> **Have Section 5 open in another tab before you start.** Bybit shows your API secret **once**,
> and the next thing you do with it is paste it into trigger.trade. Doing both in one sitting
> saves you deleting the key and starting over.

**1. Decide the balance first.** Log in to Bybit and settle, before you go any further, on how
much sits in this account. It is about to be traded automatically, so the number you put in is
the number you are putting at risk. Pick one you would shrug at losing — you can always top it
up later, and getting money back out of a bad start is much harder.

**2. Go to API Management and create a new key.** From the account menu choose **API** (or go to
**Account & Security → API Management**), then click **Create New Key**. Choose the
**System-generated API Keys** option, and give it a name you'll recognise, like `daviddtech-live`.

**3. ⛔ Set the permissions — trade ON, withdraw OFF.** This is the step that protects you.

| Permission | Set it to | Why |
|---|---|---|
| **Contract — Orders / Positions** (or *Unified Trading — Trade*) | ✅ **ON** | FastCloud needs this to open and close your trades |
| **Withdrawal** | ❌ **OFF** — leave unchecked | nothing in this setup ever moves money off the exchange, so never grant the ability to |
| **Read-only** / Wallet read | ✅ optional | lets the balance check in Section 5 work; harmless |

**4. IP restriction — leave this one alone.** The form offers to restrict the key to specific IP
addresses. **Skip it here.** On this setup the orders are placed by DaviddTech's servers, not by
your computer, so the address that matters is theirs and not one you can look up. Restricting the
key to your own home IP would block the very thing you are setting up.

> This is a genuine lock you are choosing not to use, and on this route there is no way around
> that — the address that would need allowing is DaviddTech's, not yours. It is exactly why
> step 3 matters so much: the withdrawal toggle is the protection you *do* control.

**5. Copy the key and secret — once.** When you submit, Bybit shows the **API Key** and **API
Secret**. The **secret is shown exactly once and never again.** Copy both straight into
trigger.trade (Section 5, step 2), then close the dialog.

If you lose the secret, nothing is broken and nothing is at risk — you simply delete that key in
Bybit and create another. It takes a minute.

> ### ⚠️ Treat the secret like a password
> Paste it into trigger.trade and nowhere else. Not into a chat, not into a screenshot, not into a
> support ticket. If you ever think someone has seen it, delete that key in Bybit and create a new
> one — it takes a minute and costs nothing.

**6. That's it — there is nothing to configure.** There is no endpoint to point at and no
settings file to edit anywhere. trigger.trade's **Test connection** button in Section 5 is where
you find out the key is good.

---

## Section 5 — Generate your alert files

Your strategy knows *when* to trade. Bybit knows *how* to hold a position. This section connects
the two, and it happens entirely in a browser.

The tool is **[trigger.trade](https://trigger.trade/app.html)** — StrategyFactory's own generator.
It is free, needs no login, and takes your Bybit key and encrypts it so that the alert message can
carry it safely.

> **Why it is safe to paste a key into a web page here.** The page says it plainly: *"Your API
> keys are AES-256 encrypted server-side the moment you continue. This page stores nothing."* The
> encrypted version is what ends up in your alert. Your plaintext key is never stored, and never
> appears in TradingView.
>
> That said — this only holds because of what you did in Section 4. The key you paste here must
> have **withdrawal disabled**. trigger.trade will not remind you of that; it does not check, and
> this key trades real money.

Open `https://trigger.trade/app.html`. It walks through five steps.

### Step 1 — Exchange

Pick **Bybit**, then **Continue →**.

### Step 2 — Connect

Paste the **API key** and **API secret** from Section 4. Bybit does not use a passphrase, so leave
that empty. Ignore the Telegram field for now.

Click **Test connection**. You want it to confirm before you go on — if it fails here, it will
fail as a live alert too, and it is far easier to fix now.

> **If the test fails**, it is almost always one of: the key has no trading permission, the key
> was IP-restricted after all, or your computer's clock has drifted far enough that the signed
> request expired. Go back to Section 4 rather than pressing on.

### Step 3 — Trade setup → **skip it**

This is where DaviddTech students save the most time. The page offers a **"Skip setup for
DaviddTech strategies"** option — take it.

**Why:** a DT strategy already decides its own direction, position size, leverage, take-profit and
stop-loss. Setting them again here would mean two things fighting over the same trade. Let the
strategy own it.

### Step 4 — Take-profit & Stop-loss → **skip it too**

Same reason. Your DT strategy carries its own exits.

### Step 5 — Your files

Click **Generate my files →**. You get two things, each with a **Copy** button:

| What | Where it goes |
|---|---|
| **The webhook URL** | The **Notifications** tab of your alert (Section 3, step 7) |
| **The Pine snippet** | Into your strategy's code — see below |

Use the **one-alert setup**, which the page itself recommends. Instead of building four separate
alerts, you paste the generated `alert_message` arguments into the strategy's own
`strategy.entry(...)` and `strategy.close(...)` calls, then create **one** alert whose Message box
is exactly:

```
{{strategy.order.alert_message}}
```

The strategy then supplies the right message for each signal — long entry, short entry, or a
close — and you never touch it again.

### What the generated message actually looks like

You do not need to write this or understand every field — trigger.trade produces it for you. It is
here so it is not a black box. The shape is roughly:

```
    "simplified":"1",
    "signalbot":"1",
    "inverse": "0",
    "unified":"0",
    "encryptor": "2",
    "log": "1",
    "api_key":"y",
    "api_secret":"",
    "secret_key":"",
    "passphrase":"",
    "email_id":"",
    {{strategy.order.alert_message}}
    "action":"{{strategy.order.action}}",
    "qty":"{{strategy.order.contracts}}",
    "position_size":"{{strategy.position_size}}",
    "price":"{{close}}",
    "signal_param":"{}",
    "signal_type":"",
    "coin_pair":"{{ticker}}",
    "time":"{{timenow}}"
}
```

Reading it:

- `api_key` / `api_secret` / `secret_key` — **your encrypted credentials.** Shown blank here.
  trigger.trade fills these in, and this is the reason the message is personal to you.
- `encryptor: "2"` — tells FastCloud which encryption the keys use. Never change it.
- `{{strategy.order.alert_message}}` — **the slot your DT strategy injects into.** Direction,
  size and levels arrive here at fire time. This is why the line has no comma after it: the
  injected text brings its own punctuation.
- `{{ticker}}`, `{{close}}`, `{{timenow}}` — TradingView placeholders, filled in when it fires.

> **Treat your generated message like a password.** The keys in it are encrypted, but they still
> authorise trades on your account. Do not paste it into a chat, a screenshot or a forum post.

---

## Section 6 — Your first live trade, end to end

Everything is wired. Now prove it — **once, small, and watched.**

The moment your alert fires, FastCloud places a real order with real money. That is precisely why
the first one is deliberate and tiny, fired while you are sitting in front of it, instead of
waiting for the strategy to surprise you at three in the morning.

### Step 1 — Fire one test alert

On your chart, open the alert you created in Section 3 and trigger it once by hand. The
most controllable way is to temporarily set the alert condition to something that must happen
immediately — a price crossing just above the current price, for example — let it fire, and then
set the condition back to the strategy.

**Turn the size down to the minimum first.** In your DT strategy's settings, find the risk or
margin input and set it as low as it will go *before* you fire. You are testing plumbing, not the
strategy, and at minimum size a setup mistake you haven't found yet is the cheapest lesson you
will ever buy.

### Step 2 — Check Bybit, not TradingView

TradingView will tell you the alert fired. That only proves it sent the message — it says nothing
about whether the order was placed.

Open Bybit and look at Positions. Check four things:

| Check | What you want |
|---|---|
| **A position exists** | On the symbol the chart was on |
| **Direction** | Long where the strategy said long |
| **Size** | In the ballpark you expected. If it is roughly your leverage multiple too big, see Section 7 |
| **TP and SL attached** | The take-profit and stop-loss are actually on the position |

> **That last one matters most.** A live position with no stop is the one outcome you never want
> to discover later. If TP/SL did not attach, **close the position by hand right now** and fix it
> before firing anything else.

### Step 3 — If nothing happened

Work down this list in order — it is roughly how often each one is the cause:

1. **The webhook URL is `/webhook` instead of `/webhooks`.** Singular silently does nothing.
2. **The Message box is empty, or holds the wrong thing.** For the one-alert setup it must be
   exactly `{{strategy.order.alert_message}}` and nothing else.
3. **The Webhook URL checkbox is not ticked** on the Notifications tab. Pasting the address
   without ticking the box sends nothing.
4. **Your TradingView plan does not include webhooks.** If the checkbox is greyed out, that is
   the answer — the free plan cannot send them.
5. **The alert Condition is set to the price, not your strategy.** In the Settings tab the first
   dropdown must name your strategy. If it shows something like `BTCUSDT.P`, the alert is
   watching the price and your strategy never fires it.

### Step 4 — Then leave it alone

Once one trade has gone through cleanly, put the strategy's normal size back and let it run.

Watch the next few trades against the chart: did each position match what the strategy drew? Did
each one close the way you expected? **Check it daily for the first week.** Once that is boring,
it is working.

> **Scale up slowly, and only after it has been boring.** The first week is when you find out
> whether you set this up the way you think you did. Raising the size is a decision to take after
> you have watched it open *and close* several trades without touching it — not on the strength of
> one good day. Read Section 8 before you raise anything.

---

## Section 7 — Common mistakes

The five below account for most first-time failures. None of them are your fault — every one is
an easy thing to get wrong and a two-minute thing to fix.

### ❌ Firing the first test at full size

**What it looks like:** the very first alert you fire by hand opens a full-size position on a
real account — and if anything in the wiring is wrong, you find out with money on the line.

**Why it happens:** the setup feels finished by the end of Section 5, and the obvious next move
is to let the strategy run. But nothing has been proven yet: the message, the webhook, the key
and the leverage maths have never once made it all the way to the exchange together.

**How to avoid it:** turn the strategy's risk or margin input to its minimum *before* you fire
the test in Section 6, check the position on Bybit, and only then put your normal size back. A
mistake found at minimum size costs almost nothing. The same mistake at full size does not.

### ❌ Forgetting to disable withdrawal permission

**What it looks like:** nothing. It works fine. That's exactly the problem — you won't find out
until something goes wrong.

**Why it matters:** an API key is a password that software uses. If your key can withdraw and it
leaks — a pasted screenshot, a shared alert message, a compromised machine — whoever has it can
empty the account. If the key *cannot* withdraw, the worst case is unwanted trades. Bad, but
recoverable.

**How to avoid it:** Withdraw permission **OFF**, every time, no exceptions. There is no setup in
this guide that needs it. See Section 4, step 3.

### ❌ Pasting the webhook URL into the wrong TradingView field

**What it looks like:** the alert fires — you see it in TradingView's alert log — but nothing ever
reaches FastCloud, and no order appears on the exchange.

**Why it happens:** the alert dialog has two tabs, and each has a big text box:
- the **Settings** tab has the **Message** box — this is the *content* that gets sent
- the **Notifications** tab has the **Webhook URL** box — this is the *address* it gets sent to

Putting the URL in the Message box means TradingView cheerfully sends the text "https://..." to
nobody. Putting the message JSON in the URL box means the alert can't be saved at all.

**How to avoid it:** URL goes on the **Notifications** tab, in the field next to the **Webhook
URL** checkbox — and the checkbox must be **ticked**. The message/JSON goes on the **Settings**
tab. See Section 3.

### ❌ Writing `/webhook` instead of `/webhooks`

**What it looks like:** alerts fire, TradingView says they were sent, and nothing ever trades. No
error appears anywhere.

**Why it happens:** `/webhooks/bybit` is the address that places orders. `/webhook/bybit` and
`/bybit` are also real addresses — they accept your message and return success, then drop it.
There is no failure to notice.

**How to avoid it:** copy the URL, never retype it. If nothing trades, check this first.

---

### ❌ Multiplying the leverage into your size yourself

**What it looks like:** a position roughly your leverage multiple too large — 10× bigger than
intended, at 10× leverage.

**Why it happens:** FastCloud applies leverage itself. Size means **margin**, not notional. "$100
at 10×" is one hundred, not one thousand.

**How to avoid it:** on a DT strategy you skipped this setup entirely, so it should not arise. If
you ever do set size by hand, enter the margin and let the service do the multiplication.

---

## Section 8 — Safety reminders

> ### ⚠️ The three rules
>
> **1. Start at the smallest size. Scale up slowly, and only after it has been boring.**
> This route trades real money from the first signal, so size is the only dial that decides what
> a mistake costs. Fire the first trade at the minimum (Section 6), then run at a size you would
> shrug at while you watch it open *and close* several trades without touching it. Days, not
> minutes. There is no prize for starting big.
>
> **2. Trade-only keys. Withdrawal permission disabled. Always.**
> Nothing in this setup ever needs to move money off the exchange, so never give it the ability
> to. This single toggle is the difference between "a bug cost me a bad trade" and "a leak cost me
> the account".
>
> **3. Risk caps exist for a reason — set them before you need them.**
> Decide your maximum position size and the total loss at which you switch the whole thing off,
> and decide them today, while you're calm and nothing is happening. Write the number down. On this setup the size comes from the strategy's own
> settings, so that is where you turn it down — and the daily stop is a line you hold yourself. An
> automated system will do exactly what it's told, thousands of times, without ever getting
> nervous. That is its strength and its danger. The caps are what make an automated strategy a
> tool instead of a liability.

**Two more things worth internalising:**

- **Past performance on a strategy card is not a promise.** Those 90-day numbers come from a
  backtest and a short live record. Markets change. A strategy that looks excellent this month can
  be flat or negative next month. Assume every strategy will have losing stretches, because they
  all do.
- **Know how to stop it, before you need to.** Two ways: **pause the alert** in TradingView's
  Alerts panel (no new trades — anything already open stays open, so check Positions too), or
  **delete the API key** on Bybit, which cuts FastCloud off instantly no matter what any alert
  does. That second one is the emergency brake. Either way you close open positions yourself.
- **Never share your API secret.** Not in a screenshot, not in a Discord message, not "just to
  check something". Anyone who genuinely needs to help you can do so without it. If you think a
  secret has been seen by anyone, delete that key on the exchange and make a new one — it takes
  a minute and costs nothing.

---
