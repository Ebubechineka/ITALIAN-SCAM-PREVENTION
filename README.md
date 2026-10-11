# Non Farti Ingannare — static site (GitHub Pages ready)

A **100% static** bilingual (Italian default + English toggle) book sales site:
plain HTML + CSS + JavaScript with **local image assets**, no build step, no server-side
code, no framework, no database. That means it can be hosted **anywhere** that serves
files — GitHub Pages, Netlify, Cloudflare Pages, any web host.

This folder is the **repository root**. Pushing these files to GitHub and switching on
Pages publishes the site as-is.

---

## 1. What is in here

```
/                        <- this is your repository root
├─ index.html            <- SALES LANDING PAGE  (the site home page)
├─ .nojekyll             <- tells GitHub Pages not to run Jekyll (REQUIRED)
├─ CNAME                 <- custom-domain placeholder (see §5; DELETE it if you have no domain yet)
├─ README.md             <- this file (not part of the site, safe to keep public)
├─ images/               <- hero covers (IT/EN), author portrait, inside/problem shots
│   └─ vidthumb/         <- video thumbnails
├─ sample/               <- FREE-SAMPLE OPT-IN funnel
│   ├─ index.html        <- email + phone opt-in form
│   ├─ sample_it.html    <- the Italian sample reader page
│   ├─ sample_en.html    <- the English sample reader page
│   ├─ images/           <- author portrait
│   └─ pdf/              <- the two downloadable sample PDFs (IT + EN)
├─ testimonials/
│   └─ index.html        <- "Cosa dicono i lettori" — the 5 reader stories
└─ checklist/
    └─ index.html        <- free anti-scam checklist page
```

**Page map / how everything links**

| From | To |
|---|---|
| `index.html` — hero "Anteprima gratuita", lead button, footer | `sample/` |
| `index.html` — "Leggi tutte e cinque le storie" | `testimonials/` |
| `index.html` — footer "Checklist gratuita" | `checklist/` |
| `sample/` — "Leggi il campione adesso" / "Scarica il campione" | `sample/sample_it.html` / `sample_en.html` |
| `sample/` — buy buttons, "Torna alla pagina del libro" | `../` (the sales page) |
| `testimonials/` — nav + all back-links | `../index.html#section` |
| `sample/sample_it.html` · `sample_en.html` — buy CTAs | `../` |

Everything uses **relative links**, so it works on any domain without editing.

---

## 2. Before you publish — three placeholders still to fill

These are deliberately left as clearly-marked placeholders. The site works without
them, but you should set them before a real launch.

| What | Where | Current value |
|---|---|---|
| Contact email | `index.html` and `sample/index.html` — the `CONTACT_EMAIL` variable near the end of each file | `info@goldenvistaglobal.com` |
| Checkout URLs | `index.html` — `BUY_URL_EBOOK`, `BUY_URL_PAPERBACK` | `REPLACE_WITH_YOUR_EBOOK_CHECKOUT_URL` … |
| Email form endpoint | `index.html` — `FORM_ENDPOINT`; `sample/index.html` — `FORM_ENDPOINT` in `CFG` | `https://formspree.io/f/REPLACE_WITH_YOUR_FORM_ID` |

While the checkout URLs are still placeholders the buy buttons stay **inert** and show
a short bilingual "checkout not connected yet" message instead of jumping nowhere.
Fill in a real Gumroad / Payhip / KDP URL and the buttons activate automatically.

> The example testimonials on the site are **illustrative** and carry a visible
> disclaimer (Italian consumer law). Replace them with genuine, consented reader
> reviews before promoting the site.

---

## 3. Deploy on GitHub Pages

### 3a. Create the repository and upload

**Option A — via the GitHub website (no tools needed)**

1. Sign in at <https://github.com> → **New repository**.
2. **Repository name:** e.g. `book-site` (any name; it appears in your URL).
3. **Visibility:** Public *or* Private — GitHub Pages works with both
   (private needs a paid plan; public is free).
4. **Do not** tick "Add a README file" — you already have one.
5. Click **Create repository**.
6. On the empty repo page click **uploading an existing file**.
7. Drag in **everything from this folder** — `index.html`, `.nojekyll`, `CNAME`,
   `README.md` and the four sub-folders (`images`, `sample`, `testimonials`,
   `checklist`). GitHub keeps the folder structure when you drag folders.
   > Uploading a folder that contains a dot-file (`.nojekyll`): drag it from your file
   > manager with hidden files visible, or use Option B below.
8. Commit message e.g. `Initial site`, then **Commit changes**.

**Option B — command line (recommended, handles `.nojekyll` and thousands of files)**

```bash
cd path/to/this/folder

git init
git add -A
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO.git
git push -u origin main
```

(If you already created the repo with a README, run
`git pull --rebase origin main` before the push.)

### 3b. Turn on GitHub Pages

1. In the repository go to **Settings** (top bar).
2. Left sidebar → **Pages**.
3. Under **Build and deployment → Source**, choose **Deploy from a branch**.
4. **Branch:** `main` — **Folder:** `/ (root)` → click **Save**.
5. Wait 1–2 minutes, then reload the page. A banner shows your live address.

### 3c. Your resulting URL

```
https://YOUR-USERNAME.github.io/YOUR-REPO/
```

Examples:

| Repository | Live URL |
|---|---|
| `yourname.github.io` | `https://yourname.github.io/` |
| `yourname/book-site` | `https://yourname.github.io/book-site/` |

> 💡 Tip: if you name the repository **exactly** `YOUR-USERNAME.github.io`, the site is
> served at your root `https://YOUR-USERNAME.github.io/` with **no** sub-path. That is
> the cleanest option, and the one that pairs best with a custom domain.

Every push to `main` re-publishes the site automatically (usually within a minute).

---

## 4. Where the site's assets and links stand (audit summary)

Confirmed when this package was built:

* **No build step, no server code.** Pure static HTML/CSS/JS. Zero `back-end`,
  zero templating, zero environment variables needed.
* **All images are local** (`images/…`, `sample/images/…`) — 9 assets + 4 video
  thumbnails. Nothing is hot-linked from the previous host.
* **PDFs are local** (`sample/pdf/…`).
* **All internal links are relative** — no absolute host names remain anywhere.
* **No access tokens are baked into any file** (`tk=` count = 0). The previous host
  served these pages behind short-lived signed tokens; that is **not** present here.
* **External dependencies are only:** Google Fonts (stylesheet) and YouTube embeds
  (`youtube-nocookie.com`) for the video section. Both work on any host; if you want a
  fully self-contained site, self-host the fonts and swap the embeds for direct links.

### ⚠️ Important: do not use the old `static.teamily.ai` URLs as your public address

Those URLs are **token-gated** — each visit mints a signed link that **expires in about
an hour** and then returns a `403` error page. They are fine for previewing, but they
are **not** a permanent public address and must never be used in ads, emails, business
cards or social posts. The GitHub Pages URL (or your custom domain) is your permanent
address. This is exactly why this package exists.

---

## 5. Add your own custom domain

You need a domain you own (registered at any registrar — Namecheap, GoDaddy,
Cloudflare Registrar, Aruba, IONOS, …). A `.it` or `.com` domain typically costs
€8–15 per year.

### 5a. The `CNAME` file

A file named `CNAME` (no extension, **only** the domain on its own line) tells GitHub
Pages which domain to serve. This package ships one as a **placeholder**:

```
www.your-domain.com
```

* **If you have a domain** → edit this file and put **exactly** your domain on one line,
  then commit. Use the **`www` form** (`www.example.com`) — GitHub recommends the
  `www` variant because it is the one that can be secured with HTTPS cleanly.
* **If you do not have a domain yet** → **delete this file.** A CNAME file pointing at a
  domain you do not own will make GitHub Pages show a "not properly formatted / DNS
  check failed" warning.

Alternatively, skip the file entirely and set the domain in the GitHub UI
(**Settings → Pages → Custom domain → Save**) — GitHub then writes the `CNAME` file for
you.

### 5b. Configure DNS at your registrar

**For the apex / root domain** (`example.com`) — create **four A records**:

| Type | Name / Host | Value |
|---|---|---|
| `A` | `@` | `185.199.108.153` |
| `A` | `@` | `185.199.109.153` |
| `A` | `@` | `185.199.110.153` |
| `A` | `@` | `185.199.111.153` |

Optional IPv6 — four `AAAA` records, name `@`:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

> Add **all four** A records — GitHub load-balances across them, and using only one
> removes the redundancy.

**For the `www` subdomain** — one CNAME record:

| Type | Name / Host | Value |
|---|---|---|
| `CNAME` | `www` | `YOUR-USERNAME.github.io` |

Note the trailing dot is optional; the value must be your **GitHub username** (or the
organisation name), **not** the repository name and **not** the apex domain.

> If your DNS provider offers an `ALIAS` / `ANAME` record you may point the apex at
> `YOUR-USERNAME.github.io` instead of the four IPs — but **never** combine an `ALIAS`
> at the apex with the four A records, and never CNAME `www` to the apex domain; both
> break the HTTPS certificate.

### 5c. Enable and secure it in GitHub

1. **Settings → Pages → Custom domain** → type your domain → **Save**.
   GitHub writes/refreshes the `CNAME` file in your repo.
   *(It may show "DNS check in progress" — normal.)*
2. Wait for the DNS check to go green (minutes to a few hours).
3. Tick **Enforce HTTPS** — this forces every visitor onto `https://`.
   * This box can be **greyed out for up to 24 hours** while GitHub issues the
     Let's Encrypt certificate. It becomes available on its own; just come back later.
4. Optionally tick **Enforce HTTPS** only after the certificate is issued. Do not
   disable Cloudflare "orange cloud" mid-setup, and if you proxy through Cloudflare set
   the SSL/TLS mode to **Full** (not Flexible) to avoid a redirect loop.

### 5d. Choose one hostname and redirect the other

Decide whether `example.com` or `www.example.com` is canonical. GitHub serves the one
in your `CNAME` file and redirects the other automatically — but the redirect only
works once **both** sets of DNS records above exist. Set both up.

### 5e. How long does DNS propagation take?

* Typical: **10 minutes – 2 hours** for the records to propagate.
* Worst case: **24–48 hours**, governed by the TTL on your old records.
* **Enforce HTTPS** may take up to **24 hours** after DNS is correct.
* Check progress with:

```bash
dig YOUR-DOMAIN.com +noall +answer -t A
dig www.YOUR-DOMAIN.com +noall +answer -t CNAME
```

You should see the four `185.199.*` addresses and the `…github.io` CNAME.

---

## 6. Easier alternative: Netlify or Cloudflare Pages (drag & drop)

If you would rather not touch git or DNS, both of these publish this exact folder in
about a minute and give you free HTTPS automatically:

* **Netlify** — <https://app.netlify.com/drop> → drag this folder onto the page →
  instant URL like `random-name.netlify.app`. Then **Domain settings → Add custom
  domain** and follow the on-screen DNS instructions (Netlify tells you the exact
  records).
* **Cloudflare Pages** — dashboard → **Workers & Pages → Create → Pages →
  Upload assets** → upload this folder. Then **Custom domains → Set up a domain**;
  because Cloudflare usually already hosts your DNS, it configures the records for you
  with one click.

Both support free HTTPS, automatic redeploys, and are equally good homes for this
static site. GitHub Pages is the better choice if you want the site versioned in a
repository alongside the source.

---

## 7. Quick troubleshooting

| Symptom | Cause / fix |
|---|---|
| 404 at the GitHub Pages URL | Pages not enabled, or the wrong folder. Use branch `main`, folder `/ (root)`. |
| Site shows but images are broken | The `images/` and `sample/images/` folders did not upload. Re-upload them — keep the folder structure. |
| `_`-prefixed files missing / CSS behaving oddly | `.nojekyll` was not uploaded. Add an empty file named `.nojekyll` at the repo root. |
| "Domain is not properly formatted" | The `CNAME` file has extra lines, `https://`, or a path. It must contain **only** the domain, one line. |
| "DNS check in progress" for a long time | One of the four A records (or the `www` CNAME) is missing or misspelled. Re-check with `dig`. |
| `Enforce HTTPS` greyed out | Normal for up to 24 h after DNS resolves correctly. Wait and retry. |
| Buy buttons do nothing | Expected until you set `BUY_URL_EBOOK` / `BUY_URL_PAPERBACK` (see §2). |


*Built as a static export of the delivered bilingual landing pages — all five pages
(sales, opt-in, sample IT, sample EN, testimonials) plus the checklist page, with every
platform-specific link rewritten to a plain relative path.*
