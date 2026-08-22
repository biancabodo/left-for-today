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

## Editing it

Everything is in `index.html`: styles at the top, then one IIFE holding state,
sync, dates, budget maths, parsing and rendering. Push to `main` and GitHub
Pages redeploys.
