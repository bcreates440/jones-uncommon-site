# Jones Uncommon Website — Runbook

Project handoff / reference sheet. Last verified 2026-09-20.

Jones Uncommon — DBA under Jones Uncommon LLC. The provider's own site,
built on the same stack sold to clients, and used as the flagship demo.
Static site on GitHub Pages, edited through a Decap CMS panel, signed in
via a Cloudflare Worker shared across every client site's editor — see
[client-sites-auth](https://github.com/bcreates440/client-sites-auth).
Built from [client-site-template](https://github.com/bcreates440/client-site-template).

**Status:** Live on GitHub Pages · Domain not registered · Awaiting a headshot
and the WWCA testimonial · Copy is written but unreviewed by Blake

---

## Quick reference

Everything needed to find or touch any part of the system. Nothing secret is
written down here — where a real secret exists, this says where it lives
instead.

| | |
|---|---|
| **Live site (current)** | https://bcreates440.github.io/jones-uncommon-site/ |
| **Live site (future)** | jonesuncommon.com — not registered yet |
| **Editor** | https://bcreates440.github.io/jones-uncommon-site/admin/ |
| **GitHub repo** | https://github.com/bcreates440/jones-uncommon-site — public, branch `main` |
| **OAuth worker** | https://client-sites-auth.bcreates440.workers.dev — shared with other client sites, source at [bcreates440/client-sites-auth](https://github.com/bcreates440/client-sites-auth), not in this repo |
| **GitHub account** | bcreates440 |
| **Cloudflare account** | bcreates440@gmail.com — subdomain `bcreates440.workers.dev` |
| **GitHub OAuth App** | "Client Sites Editor" — shared, not site-specific |
| **Callback URL on file** | `https://client-sites-auth.bcreates440.workers.dev/callback` — must match the worker exactly or login breaks |
| **Client secret** | Not recorded anywhere. Lives only as a secret on the `client-sites-auth` Cloudflare Worker. |

---

## How it fits together

GitHub Pages hosts and builds the site for free but can't run any code of its
own — that's the one gap the Cloudflare Worker exists to fill. Everything
else is standard Jekyll.

**Signing in to the editor:**

```
Editor (/admin/)  →  Worker (client-sites-auth.workers.dev)  →  GitHub (login + approve app)  →  Editor (signed in)
```

**Publishing a change:**

```
Editor ("Publish")  →  GitHub repo (new commit on main)  →  GitHub Pages (rebuilds, ~1 min)
```

Same path whether the commit comes from the editor or from a manual
`git push`. There is no separate deploy step — a push to `main` is a publish.

---

## File map

```
jones-uncommon-site/
├── _content/          the 4 pages — front matter + section "blocks"
├── _data/              site.yml, team.yml, nav.yml — shared facts
├── _includes/         header, footer, block templates, jsonld
├── _layouts/          page.html — the one shell every page uses
├── css/styles.css     the whole site's styling, one file
├── images/            web-optimised photos — tracked in git
├── admin/             config.yml (CMS schema) + index.html
├── check.rb           *** run this before trusting any manual edit ***
├── README-EDITING.md  the in-repo how-to
├── RUNBOOK.md          this file — in git, excluded from the built site
```

The 4 pages: Home (`index.html`), Work (`work.html`), Services
(`services.html`), Contact (`contact.html`) — see `_data/nav.yml`.
`fonts/` holds the self-hosted heading font and its licence.

---

## Editing — two ways in

### Content changes
Wording, the About paragraph, the headshot, the testimonial — anything a
non-technical editor should be able to do alone.

1. Open `/admin/`, sign in with GitHub
2. Open a page, edit the section
3. Click **Publish**
4. Live in about a minute

### Structural changes
A new kind of section, a template change, anything that touches
`_includes/` or `admin/config.yml`.

1. Edit the files directly
2. Run `ruby check.rb` — must say `ALL CHECKS PASSED`
3. `git commit` & `git push`
4. GitHub Pages rebuilds automatically

**Local preview:**

```
bundle exec jekyll serve
# then open http://localhost:4000
```

---

## Things that will bite you

Same rules as every site built on this stack:

- **Rule #1 — Decap deletes what its schema doesn't know about.** The editor
  rewrites a whole page from `admin/config.yml`'s field list every save. Add
  a field by hand without adding it there too, and it gets silently thrown
  away on the next editor save. `check.rb` catches this — run it after any
  manual template change.
- **There is deliberately no contact form.** Decided 2026-09-20: communication
  should feel personal, and a form puts a queue between the customer and the
  business — off-brand for "a real person, not an agency". `contact.html`
  publishes the phone and email directly instead. Client sites are a different
  decision (Formspree free tier) — see `ops/design/form-backend-decision.md`.
- **The phone number lives in THREE fields** in `_data/site.yml` — `phone`
  (readable), `phone_dial` (`+1…`) and `phone_href` (`tel:+1…`). Change them
  together or the tap-to-call button and the printed number drift apart.
  Currently Blake's personal line; a Google Voice number is the eventual plan.
  `email`/`email_href` pair the same way and still point at a domain that
  does not exist yet.
- **Buttons can pull their address from `_data/site.yml`** via `link:`
  (e.g. `link: phone_href`) instead of a hardcoded `url:`. Keeps the number in
  one place. (`link` was supported by `buttons.html` but missing from
  `admin/config.yml`, so Decap silently deleted it on save — fixed here and
  upstream in `client-site-template` on 2026-09-20.)
- **Every colour comes from a `:root` token, and `check.rb` enforces it.**
  Write a colour anywhere else — including an inline `style=` in a page — and
  the check fails. To rebrand, edit `:root` and nothing else. The one
  exception is `theme_color` in `_data/site.yml`, because a `<meta>` tag
  cannot read a CSS variable.
- **Two type tokens, not one.** `--display` is the serif used for headings and
  the wordmark; `--ui` is the sans used for buttons, eyebrows and table
  headers. Keep them separate — uppercase serif at label size reads dated.
  `--display-case` flips headings between sentence case and uppercase.
- **The heading font is self-hosted** (`fonts/lora-var.woff2`, OFL). It is
  preloaded in `head.html` and uses `font-display:swap`. The `@font-face`
  `src:` path is `../fonts/…` because stylesheet paths resolve from the
  stylesheet, not the page.
- **Shortcuts** — typing `[[org]]`, `[[phone]]`, `[[email]]`, `[[address]]`
  inside body text pulls the real value from `_data/site.yml`.

---

## Pending tasks

- **Domain** — jonesuncommon.com needs registering, then pointing at GitHub
  Pages. Until then `_config.yml`'s `url` and `admin/config.yml`'s
  `site_url`/`display_url` stay on the github.io address.
- **Headshot** — the About section on the home page is a text-only `prose`
  block on purpose, because a `split` block with an empty `image:` renders a
  broken image. Once there's a real photo: change that block's type to
  `split`, add `image:`, `image_alt:`, `image_width:`/`image_height:`, and
  drop the photo in `images/`. Nothing else needs touching.
- **WWCA testimonial** — the "Recent work" section currently carries
  verifiable facts (Lighthouse scores, page count, running cost) rather than
  a quote. Add a real quote from the board once they've given one; do not
  write one on their behalf.
- **Copy review** — the About paragraph and the rest of the site copy were
  drafted for Blake, not by him. Everything in it is factually true as far as
  it goes, but it has not been checked against how he'd actually say it.
- **Email address** — `hello@jonesuncommon.com` does not exist until the
  domain is registered. It is published on the contact page and in the footer,
  so mail sent there currently goes nowhere.
- ~~Intake-form backend~~ — **resolved 2026-09-20**: no form on this site by
  design. Client sites use Formspree's free tier, client-owned. See
  `ops/design/form-backend-decision.md`.
- **Stripe** — Payment Links for the build deposit and each monthly tier,
  once there's a pricing structure to attach them to. Keys never go in this
  repo.
- **Demo sites** — the two demo cards on the Work page ("mock club",
  "mock local business") are placeholders; not built yet.
- **Brand pass** — swap the inherited WWCA color palette in
  `css/styles.css` for Jones Uncommon's own, if there is one.

---

## Build history

- **2026-09-19** — Scaffolded from
  [client-site-template](https://github.com/bcreates440/client-site-template),
  per `jones-uncommon-claude-code-brief.md`. 4-page sitemap, placeholder copy,
  local git only.
- **2026-09-20** — Pushed to `bcreates440/jones-uncommon-site` (public) and
  GitHub Pages enabled. Live.
- **2026-09-20** — `start.html` (which held a non-functional form) replaced by
  `contact.html`: phone and email published directly, no form, by decision —
  see `ops/design/form-backend-decision.md`. Real phone number added.
- **2026-09-20** — Retheme. Colour tokens renamed for their role, ~12
  brand-derived hex literals removed from the stylesheet, and colour swept out
  of `cta.html`, `footer.html`, `head.html` (`<meta theme-color>` had been
  shipping WWCA navy to every page) and the editor's boot screen. New palette
  applied. `check.rb` gained a guard that fails on any colour outside `:root`.
  Fixed three invisible buttons found along the way.
- **2026-09-20** — Typography. Headings moved to self-hosted Lora (OFL) in
  sentence case; `--display`/`--ui` split so labels stay sans.
- **2026-09-20** — Site copy drafted (About, proof section) and the voice
  changed from "we" to "I" throughout, since a one-person business writing
  "we" undercuts the "a real person, not an agency" positioning. **Not yet
  reviewed by Blake.**

---

## If it breaks

**Editor login fails**
- Confirm the shared worker's still deployed: `npx wrangler deployments list --name client-sites-auth`
- Confirm both secrets are set: `npx wrangler secret list --name client-sites-auth`
- Confirm the OAuth App's callback URL still exactly matches `.../callback`

**Site won't update**
- Check the build log: repo → Actions or Deployments tab
- A red build usually means a Jekyll/Liquid syntax error in the last commit

**A field vanished**
- Almost always: it's missing from `admin/config.yml`
- Run `ruby check.rb` locally — it names the exact field

---

The private ops repo for this business lives at
`C:\Users\Blake Jones\jones-uncommon\ops` — log any work done on this site
to `clients/jones-uncommon/request-log.md` when done.

Bring this file to a fresh Claude session along with whatever's going
wrong — it has everything needed to pick the project back up cold.
