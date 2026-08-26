# Left for Today

A one-number daily spending tracker. Tell it what comes in and what's already
spoken for; it works out what's actually yours each day, and shrinks the number
when you overspend.

Single static page — no build step, no dependencies, no server.

## Running it

Open `index.html`, or serve the folder:

    python3 -m http.server 8765

Live at **https://biancabodo.github.io/left-for-today/**

## Where your data lives

Entries are kept in your browser's `localStorage` under `leftfortoday.v1`, plus
a `lastgood` spare copy so a half-cleared browser can't leave you staring at an
empty app. Nothing in this repo contains your spending.

Tap any row in Today or History to edit it — amount, what it was, category or
destination, date and currency are all changeable, with a two-tap delete. The
entry keeps its id, so an edit syncs like any other change instead of forking the
copy on another device.

### Syncing across devices

Setup → *Your data* → *Sync across devices* connects the app to a **private**
GitHub repo of your own. It writes a `store.json` there through the GitHub
Contents API and merges on read, so two devices editing the same history end up
with the union of both — entries are keyed by id and nothing gets overwritten.

You need a [fine-grained token](https://github.com/settings/personal-access-tokens/new)
scoped to that one repo with **Contents: Read and write**. The token is stored
in that browser's `localStorage` only and is sent nowhere but `api.github.com`.
Anyone with access to the browser can read it, so use a token limited to the
data repo and revoke it if the device is lost.

Without a token the app still works — history just stays on that one device, and
*Copy my data* / *Back up to a file* are the way out.

## The savings balance

The number on the Month tab is a **balance**, not an estimate: the starting
balance you set in Setup, plus every movement you've logged, plus any money-in
you routed straight to savings. It should reconcile against your actual account.

*Put money in* / *Take money out* record movements, which are listed under the
balance and can be tapped to edit or delete. Foreign-currency movements work the
same way as spends.

What the app thinks you're *on track* to save — the pro-rata set-aside plus
however far under pace you're running — is shown as a separate line, clearly
marked as not counted in the balance. Money you merely haven't spent yet isn't
money in the account.

## Currencies

Setup → **Your currency** sets what everything is displayed and stored in. Changing
it later asks whether to convert the figures you already have at today's rate, or
to relabel and leave the numbers alone.

When you're travelling, the add sheet has its own currency picker. Log `mega image
250` in RON and it stores the entry in your base currency, keeping what you actually
paid alongside it — the log row reads *Groceries · RON 250.00*.

Rates come from two keyless, CORS-open sources:

- [Frankfurter](https://frankfurter.dev) for the ~30 currencies the ECB publishes
  daily. It answers for a **past** date, so a backdated entry converts at the rate
  that applied on the day it happened.
- [open.er-api.com](https://open.er-api.com) for everything else, which only knows
  today's rate. The app says which one it used.

The rate is always an editable field. If the network is down, or your bank charged
something different, type the real rate and the entry stores correctly. Fetched
rates are cached in `localStorage` so the same day and pair aren't looked up twice.

## Editing it

Everything is in `index.html`: styles at the top, then one IIFE holding state,
sync, dates, budget maths, parsing and rendering. Push to `main` and GitHub
Pages redeploys.
