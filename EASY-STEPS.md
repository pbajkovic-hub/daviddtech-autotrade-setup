# Easy Steps — Do This, Then This

No jargon. No software to install. Just do each step in order and check the box.

**Time:** about 25 minutes.

> ### ⚠️ This trades real money, from the very first signal
>
> There is no practice mode on this setup. The moment your alert fires, a real order goes onto
> your real Bybit account.
>
> That's the point — it's what you're here to do. But it means two things:
>
> - **Only fund the account with money you'd be genuinely OK losing.** Not money you need.
> - **Your first trade is at the smallest size you can set** (Step 15). You watch that one land,
>   check it looks right, and only then let it run normally.
>
> The one protection you set up regardless is in Step 6: your key will be able to **trade but not
> withdraw**. Do not skip it.

---

## What you need first

Get these four before you start, or you'll get stuck in the middle.

- [ ] A **TradingView** account — and it must be a **paid** one (Essential or higher).
      The free plan cannot send signals out. That's the one thing you can't work around.
- [ ] A **DaviddTech** subscription, so you have a strategy to use.
- [ ] A **Bybit** account with money in it.
- [ ] About 25 minutes where nobody interrupts you.

---

# PART 1 — Get the strategy onto your own chart

### Step 1. Open your strategy

Log in to **StrategyFactory**. Click **Strategy Library**. Find the strategy you want.

### Step 2. Open it in TradingView

At the right-hand end of that strategy's row, click the **actions menu** (the little button at the
end). Choose **Open in TradingView**.

A TradingView chart opens.

### Step 3. Make it yours

Look at the **top-left** of the chart. It says **View Only Mode**.

That means it isn't yours yet — you're looking at someone else's. You can't add an alert to it.

Look at the **top-right**. Click **Copy**.

A box appears: *"Make copy of chart layout"*. Give it a name you'll recognise. Click to confirm.

> ✅ **You should now see:** the words "View Only Mode" are **gone**. The chart is yours.

---

# PART 2 — Make your Bybit key

A "key" is just a password that lets software trade for you. You're going to make one that **can
trade but cannot take money out**.

### Step 4. Decide what you're putting in

This account is going to be traded automatically. Before you go further, settle on the amount and
put only that in.

**Pick a number you would shrug at losing.** Not your rent. Not "I'll just start big to make it
worth it." Your strategy will place trades while you're asleep, and the first week is when you
find out whether you've set it up the way you think you have.

You can always add more later. Getting money back out of a bad start is harder.

### Step 5. Create the key

Log in to Bybit. Account menu → **API** (or *Account & Security → API Management*).

Click **Create New Key**. Choose **System-generated API Keys**.

Name it something you'll recognise, like `daviddtech`.

### Step 6. ⛔ The most important click in this whole guide

You'll see a list of permissions with checkboxes. Set them like this:

| Permission | Do this |
|---|---|
| **Contract — Orders / Positions** (may be called *Unified Trading — Trade*) | ✅ **TICK IT** |
| **Withdrawal** | ❌ **LEAVE IT UNTICKED** |
| **Read-only** / Wallet read | ✅ tick it, it's harmless |

> ⛔ **Withdrawal must stay OFF.**
>
> With it off, the worst thing that can happen is a bad trade.
> With it on, someone who gets hold of your key can empty your account.
>
> Nothing in this guide needs it. **Check it twice.** This is the single most important
> thing on this page.

**Skip the IP restriction box.** Leave it empty. (Your computer isn't the one placing the orders,
so locking it to your address would break things.)

### Step 7. Copy both codes NOW

Bybit shows you an **API Key** and an **API Secret**.

> ⚠️ **The Secret is shown once and never again.** Not "hard to find later" — *gone*.

Copy both somewhere safe for the next five minutes. You'll paste them in Step 8.

Lost it? Delete the key on Bybit and make a new one. Takes a minute, costs nothing.

---

# PART 3 — Connect the two

### Step 8. Go to trigger.trade

Open this in your browser:

```
https://trigger.trade/app.html
```

It's free. No account, no login. It turns your Bybit key into a scrambled version that's safe to
put inside an alert.

Now work through its five steps:

**Its Step 1 — Exchange.** Click **Bybit**. Click **Continue →**.

**Its Step 2 — Connect.** Paste your **API Key** and **API Secret** from Step 7.
Leave "passphrase" empty — Bybit doesn't use one. Ignore Telegram.

Click **Test connection**.

> ✅ **Wait for it to say the connection worked.** Don't move on until it does.
>
> If it fails, it's almost always one of these: you forgot to tick the trading permission, the key
> has expired, or your computer's clock is wrong by a few minutes.

**Its Step 3 — Trade setup.** Look for **"Skip setup for DaviddTech strategies"**. Click it.

> **Why skip?** Your DaviddTech strategy already decides how big to trade, which direction, and
> where to take profit. If you set those here too, two things would be arguing over your trade.
> Let the strategy do its job.

**Its Step 4 — Take-profit & Stop-loss.** Skip this too. Same reason.

**Its Step 5 — Your files.** Click **Generate my files →**.

You now get two things, each with a **Copy** button:
1. a **webhook URL**
2. a **Pine snippet** (a small piece of code)

Keep this browser tab open. You need both in a moment.

> ⚠️ **What it just generated is as good as your password.** The key inside it is scrambled, but
> it can still place trades on your account. Don't paste it into a chat, a screenshot, or a
> Discord channel.

---

# PART 4 — Set up the alert

### Step 9. Add the code to your strategy

Take the **Pine snippet** trigger.trade just gave you and add it to your strategy's code, on its
`strategy.entry(...)` and `strategy.close(...)` lines.

This is what lets one single alert handle everything — buys, sells and exits — instead of you
building four separate ones.

### Step 10. Create the alert

Go back to **your copy** of the chart. Press **Alt + A** (or click the **Alert** button in the top
toolbar).

### Step 11. Set what makes it fire

On the **Settings** tab, find **Condition**. It has two dropdowns.

Set the first one to **your strategy's name**.

> ⛔ **This is the number one mistake.** If that dropdown shows something like `BTCUSDT.P`, it's
> set to the *price*, not your strategy. Your strategy would then do nothing at all.
>
> It must say the strategy's name.

Set it to fire **Once per bar close**.

> **Why?** While a candle is still forming, a signal can appear and then vanish again. If you fire
> on every tick you'll trade signals that were never really there — with real money.

### Step 12. The Message box

Still on the **Settings** tab, scroll down to **Message**.

Delete whatever is in there. Put in exactly this, and nothing else:

```
{{strategy.order.alert_message}}
```

That's the whole box. One line.

### Step 13. The Webhook URL box

Click the **Notifications** tab at the top of the same window.

Find the checkbox called **Webhook URL**. **Tick it.**

Paste this address into the box next to it:

```
https://fastcloud.daviddtech.com/webhooks/bybit
```

> ⛔ **Two things people get wrong here. Both fail silently — nothing tells you.**
>
> **1. `webhooks` has an S on the end.** `webhook` (no S) is also a real address. It accepts your
> alert, says "success", and then does absolutely nothing. Copy the address, never type it.
>
> **2. You must actually TICK the checkbox.** Pasting the address without ticking the box sends
> nothing anywhere.

> **Don't mix up the two boxes.** They're on different tabs and they do different jobs:
> - **Message** (Settings tab) = *what* gets sent
> - **Webhook URL** (Notifications tab) = *where* it gets sent

> **Is the Webhook URL checkbox greyed out?** Your TradingView plan is the free one. This is the
> step that needs a paid plan. Everything you've done so far is fine and saved.

### Step 14. Click Create

Done. The alert is live.

---

# PART 5 — Your first real trade

**Do not walk away now.** This is the part that catches problems while they're still cheap.

### Step 15. Turn the size right down, then fire once

**First, turn your strategy's trade size down to the smallest it will go.** In its settings, find
the risk or position size and set it to the minimum.

Then make the alert fire once on purpose: open it, temporarily change the condition to something
that must happen right now — a price crossing just above the current price — let it fire, then
**change the condition back to your strategy**.

> **This places a real order.** At minimum size, that's the cheapest lesson you'll ever buy. At
> full size, a setup mistake you haven't found yet costs real money.

### Step 16. Look at Bybit — not TradingView

TradingView will tell you the alert fired. **That only means it sent the message.** It does not
mean anything was bought.

Open Bybit. Go to **Positions**. Check four things:

- [ ] A position actually exists
- [ ] It's the right direction (bought when the strategy said buy)
- [ ] The size is roughly what you expected — **if it's about 10× too big, see below**
- [ ] **A stop-loss is attached to it**

> ⛔ **The stop-loss matters most.** A live position with no stop is the one thing you never want
> to find out about later. If there's no stop attached, **close the position by hand right now**
> and fix the setup before firing anything else.

> **Position way bigger than expected?** Your strategy is probably multiplying the leverage in
> itself. FastCloud applies leverage on its own — the size you set means *margin*, not the full
> position. Turn the size down and fire again at minimum.

### Step 17. Nothing happened?

Go down this list in order. It's roughly how often each one is the real cause:

1. The webhook address says `webhook` instead of `webhooks`.
2. The Message box isn't exactly `{{strategy.order.alert_message}}`.
3. You didn't tick the **Webhook URL** checkbox.
4. Your TradingView plan doesn't include webhooks (the box was greyed out).
5. The Condition dropdown is set to the price, not your strategy.

### Step 18. Now let it run — and keep watching for a while

Once that first trade looked right, put your normal size back.

Then watch the next few trades against the chart. Did each position match what the strategy drew?
Did each one close the way you expected? **Check it daily for the first week.**

Once that's boring, it's working.

---

# The three rules

**1. Start small. Scale up slowly, and only after it's been boring.**
The first week is when you find out whether you set it up the way you think you did. Small size
turns that into a cheap lesson instead of an expensive one. There is no prize for starting big.

**2. Withdrawal permission stays off. Always.**
Nothing in this setup needs it. That one checkbox is the difference between "a bug cost me a
trade" and "a leak cost me everything."

**3. Decide your stop-loss point while you're calm.**
Pick the total loss where you switch the whole thing off — and pick it now, today, before there's
money on the line. Write it down. An automated system does exactly what it's told, thousands of
times, and never gets nervous. That's its strength and its danger.

---

## If you want to stop it

Two ways, and it's worth knowing both **before** you need them:

- **Pause the alert** in TradingView's Alerts panel. No new trades. Anything already open stays
  open, so check your positions too.
- **Delete the API key** on Bybit. This cuts it off completely and instantly, no matter what any
  alert does. This is the emergency brake.

Any position that's already open you close yourself, on Bybit.
