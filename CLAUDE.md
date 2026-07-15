# Dream Weaver Racing — Project Context

Team site for HRL hydroplane boat **F-26 "Dream Weaver"**, driver **Kevin Smith** (Peabody, MA).
Formula F class, Hydroplane Racing League. Built July 2026, modeled on the Herrin Choker Racing
site (`Documents\HerrinChoker\HerrinChokerRacing`) but with its own identity.

**Domain: dreamweaverracing.com** — purchased July 12 2026 via Cloudflare Registrar
(same Cloudflare account as herrinchoker.ca).

## Deployment — LIVE

Live at **https://dreamweaverracing.com** since July 15 2026. Cloudflare Worker
**dreamweaverracing** (assets-only static site, no build step), custom domain attached.
Auto-deploy is connected: **push to `main` → Workers Builds runs `npx wrangler deploy` →
live in about a minute.** GitHub repo: `jordancmoore-create/DreamWeaverRacing`
(branch `main`, build command empty). `wrangler.jsonc` is the assets-only config
(no `main` field — don't add one unless adding Worker code); `.assetsignore` keeps
CLAUDE.md and config files out of the served site. The Worker name in wrangler.jsonc
must stay `dreamweaverracing` or git builds break.

---

## Design system (from the boat's livery)

```css
--orange:#f0821e         /* primary — hull orange */
--orange-bright:#ff9435  /* hovers, highlights */
--blue:#2f62d8           /* royal blue — the "26" on the cowling */
--blue-deep:#1c3a8e
--blue-lt:#7da2f0        /* dates, secondary links */
--black:#0a0b0e          /* page bg (cool, vs Herrin's warm black) */
--ink:#12141a            /* panels, alt sections */
--white:#f7f6f2
--muted:#8f96a3
```

Fonts: **Saira Condensed** (headings, numbers) + **Saira** (body). Deliberately upright/no
italics — Herrin Choker is the heavy-italic maroon/gold site; Dream Weaver is upright
orange/blue with **checkered-flag motifs** (from the checkered cowling): `.flag-strip` bars
top + above footer, checkered kicker bullets, checkered photo placeholders.

Structural borders are blue-tinted `rgba(47,98,216,…)`; orange is reserved for CTAs,
hovers, and accents. Next-race bar is royal blue (Herrin's is red).

---

## File structure

```
index.html          single-page site
css/style.css       all styles
js/main.js          hamburger menu + race badge auto-compute (same logic as Herrin site)
images/             empty — boat photo is currently HOTLINKED from the HRL site (see below)
```

**Photo is hotlinked, not local.** The hero bg (css/style.css) and gallery big frame (index.html)
point at `https://hrlhydroplane.com/wp-content/uploads/2022/04/F-26-scaled.jpg` because the build
session couldn't write binaries outside its sandbox. To localize, run:

```powershell
curl.exe -L -o images\f26-boat.jpg https://hrlhydroplane.com/wp-content/uploads/2022/04/F-26-scaled.jpg
```

then swap both references to `images/f26-boat.jpg` (TODO comments mark the two spots).

## Facts used on the site (source: hrlhydroplane.com/en/les-pilotes/kevin-smith/)

- Kevin Smith, born April 11 1992, Peabody MA, racing since 2007, Ford engine
  (only Ford in the field vs the new Hondas — 2026 season goal quote on site)
- Father piloted the **S-26 Dreamweaver** — the Legacy section quote comes from his HRL bio
- Crew: Charles Smith (Crew Chief), Justin Polinski (Chief Engineer), Mary Smith
  (Team Coordinator), Courtney Smith (Communications), Mike Ward (Team Member)
- Sponsors: Charles Smith Plumbing & Heating, Dez's Paint Shop, M & N Competition Consultants
- Socials: facebook.com/Dream26weaver · instagram.com/dreamweaverracing
- 2026 schedule identical to Herrin Choker's (league-wide HRL calendar)

## What's not done yet

- **Localize the boat photo** — see hotlink note above; do this before promoting the site,
  ideally with real photos from Kevin's crew (confirm rights on the HRL shot)
- **www subdomain** — add www.dreamweaverracing.com as a second custom domain if wanted
  (Worker → Settings → Domains & Routes)
- **Photos** — only the one HRL photo; get real team photos + confirm usage rights,
  update the hero `credit` span with the photographer's name
- **Driver headshot** — replace the "KS" `.driver-avatar` div with an `<img>`
- **Race results section** — not built (Herrin site doesn't have one either)
- **Videos** — generic HRL embeds (2026 livestream promo + 2025 recap); swap in
  F-26 footage when it exists

## Local dev notes (this machine)

- **Preview:** `.claude/launch.json` config **dreamweaver** — python http.server, port 2626.
- **Windows Defender Controlled Folder Access** protects `Documents`: apps not on its
  allowlist fail writes here with misleading errors (git: `.git: No such file or directory`).
  `git.exe` was allowlisted July 15 2026 (`C:\Program Files\Git\mingw64\bin\git.exe` and
  `...\cmd\git.exe`) — a Git upgrade can silently invalidate this; if git write errors
  return, re-add it via Windows Security → Ransomware protection → Allow an app.
- For Claude sessions: the Write/Edit tools work here; if a *shell* write (mkdir,
  redirects, curl -o) fails oddly, suspect CFA and hand the command to the user.
- Sister site: Herrin Choker Racing (#38 / F-38, herrinchoker.ca) lives at
  `Documents\HerrinChoker\HerrinChokerRacing` — same HRL schedule, shared design DNA.
