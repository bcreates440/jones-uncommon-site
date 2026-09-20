# Jones Uncommon Website — Runbook

Project handoff / reference sheet. Last verified 2026-09-19.

Jones Uncommon — DBA under Jones Uncommon LLC. The provider's own site,
built on the same stack sold to clients, and used as the flagship demo.
Static site on GitHub Pages, edited through a Decap CMS panel, signed in
via a Cloudflare Worker shared across every client site's editor — see
[client-sites-auth](https://github.com/bcreates440/client-sites-auth).
Built from [client-site-template](https://github.com/bcreates440/client-site-template).

**Status:** Scaffolded locally, not yet pushed to GitHub · Domain not
registered · Placeholder content pending Blake's photo/About paragraph/WWCA
testimonial · Intake-form backend not chosen yet

---

## Quick reference

Everything needed to find or touch any part of the system. Nothing secret is
written down here — where a real secret exists, this says where it lives
instead.

| | |
|---|---|
| **Live site (current)** | Not live yet — remote repo not created |
| **Live site (future)** | jonesuncommon.com — not registered yet |
| **Editor** | (site)/admin/ — won't work until the repo exists on GitHub, see `admin/config.yml`'s `backend.repo` TODO |
| **GitHub repo** | https://github.com/bcreates440/jones-uncommon-site — planned name, **not created yet** |
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
(`services.html`), Start (`start.html`) — see `_data/nav.yml`.

---

## Editing — two ways in

### Content changes
Wording, the About paragraph, the headshot, the testimonial — anything a
non-technical editor should be able to do alone, once the repo exists and
the editor is wired up.

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
- **The intake form (Start page) has no backend yet.** This stack is
  deliberately server-free (GitHub Pages + a login-only Cloudflare Worker),
  so the form in `_content/start.html` (a `raw` HTML block) currently posts
  to a placeholder Formspree URL that doesn't exist. Pick a static-form
  service and swap in the real endpoint before this page goes live — see
  the `TODO(Blake)` comment right in that file.
- **`_data/site.yml` has placeholder contact info** (`phone`, `address`) —
  marked `PLACEHOLDER` in the file. Fill these in before launch or the
  footer and phone links will show placeholder text.
- **Color palette is still WWCA's** (`css/styles.css`'s `:root` navy/red/gold)
  — inherited from `client-site-template`, not a considered brand choice for
  Jones Uncommon yet.
- **Shortcuts** — typing `[[org]]`, `[[phone]]`, `[[email]]`, `[[address]]`
  inside body text pulls the real value from `_data/site.yml`.

---

## Pending tasks

- **Repo doesn't exist yet** — create `bcreates440/jones-uncommon-site` on
  GitHub, then update `admin/config.yml`'s `backend.repo` if the name
  changes, and `_config.yml`'s `url` / `admin/config.yml`'s `site_url` /
  `display_url` to match.
- **Domain** — jonesuncommon.com needs registering, then pointing at GitHub
  Pages once the repo is live.
- **Content from Blake** — headshot/warm photo, the About paragraph (Home
  page, currently a `PLACEHOLDER` block), and the WWCA testimonial (after
  his Monday meeting, also currently a `PLACEHOLDER`).
- **Intake-form backend** — pick a static-form service (Formspree or
  similar) and wire the real endpoint into `_content/start.html`.
- **Contact info** — real phone number and address (or a service-area line)
  in `_data/site.yml`.
- **Stripe** — Payment Links for the build deposit and each monthly tier,
  once there's a pricing structure to attach them to. Keys never go in this
  repo.
- **Demo sites** — the two demo cards on the Work page ("mock club",
  "mock local business") are placeholders; not built yet.
- **Brand pass** — swap the inherited WWCA color palette in
  `css/styles.css` for Jones Uncommon's own, if there is one.

---

## Build history

- Scaffolded 2026-09-19 via Claude Code from
  [client-site-template](https://github.com/bcreates440/client-site-template),
  per `jones-uncommon-claude-code-brief.md`. 4-page sitemap built with
  placeholder content where Blake's voice/photo/testimonial go. Not yet
  committed to a remote — local git repo only.

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
