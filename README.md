# DataPress landing site (datap-rs.org)

A self-contained [reveal.js](https://revealjs.com/) presentation served at the
apex domain **datap-rs.org** via GitHub Pages.

The site is fully static and has **no external runtime dependencies** —
reveal.js (v5.1.0) is vendored under `vendor/`, so it works offline and never
calls out to a CDN. Plus a `CNAME` for the custom domain and a GitHub Actions
workflow that deploys to Pages.

```
.
├── index.html                  # the presentation (open locally in a browser)
├── vendor/reveal/              # vendored reveal.js 5.1.0 (css, js, plugins)
├── CNAME                       # datap-rs.org
├── .nojekyll                   # serve files as-is (no Jekyll processing)
└── .github/workflows/pages.yml # build-free deploy to GitHub Pages
```

## Preview locally

Just open `index.html` in a browser. Navigation: arrow keys / space to advance,
`F` fullscreen, `S` speaker notes, `O` slide overview.

## One-time setup

> This folder is meant to be the **root of a new, separate repository** — kept
> apart from the main `datapress` repo so the existing `docs.datap-rs.org`
> Pages site is untouched.

### 1. Create the repo and push

From inside this `presentation/` folder:

```bash
git init -b main
git add .
git commit -m "Initial DataPress landing site"
git remote add origin git@github.com:<you>/<landing-repo>.git
git push -u origin main
```

### 2. Enable GitHub Pages

In the new repo: **Settings → Pages → Build and deployment → Source =
GitHub Actions**. The included `pages.yml` workflow runs on every push to
`main` and publishes the site. (The `CNAME` file is uploaded with the
artifact, so Pages keeps the custom domain configured.)

### 3. Point the apex domain at GitHub Pages

At your DNS provider for `datap-rs.org`, add the GitHub Pages apex records:

**A records** (`@`):

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**AAAA records** (`@`):

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

If your provider supports `ALIAS`/`ANAME` flattening at the apex, you may
instead point `@` to `<you>.github.io`.

> `docs.datap-rs.org` (the MkDocs site) is a separate CNAME-style record and
> is unaffected by these apex records.

### 4. Verify

In **Settings → Pages**, set the custom domain to `datap-rs.org` (it should
auto-populate from the `CNAME` file), wait for the DNS check to pass, then
enable **Enforce HTTPS**. The site is live at <https://datap-rs.org>.
