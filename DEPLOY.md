# Deploying the Infasia website

Your site is plain HTML/CSS/JS (a "static" site). That means you do **not** need
WordPress or any site builder. The cheapest and most reliable way to host it is
**GitHub Pages** (free, includes HTTPS), pointed at your existing **infasia.net**
domain. This folder is already prepared for that.

---

## What's already done in this folder

- Real Infasia logo wired into the header and the browser tab icon (favicon) on every page.
- Social-share image and tags on the homepage.
- `CNAME` file containing `infasia.net` — tells GitHub Pages your custom domain.
- `.nojekyll`, `robots.txt`, `sitemap.xml`, and a branded `404.html`.

You only need to (1) put these files on GitHub and turn on Pages, and
(2) point the domain. Steps below.

---

## Part 1 — Put the site on GitHub Pages (free)

### Option A — No tools, just the browser (easiest)
1. Create a free account at https://github.com (if you don't have one).
2. Click **+ → New repository**. Name it `infasia-site`. Set it **Public**. Create it.
3. On the new repo page, click **uploading an existing file**.
4. Open this `Website` folder on your Mac, select **everything inside it**
   (including the `assets` folder and the `CNAME` / `.nojekyll` files — turn on
   "Show hidden files" in Finder with **Cmd+Shift+.** so you can see `.nojekyll`),
   and drag it all into the browser. Click **Commit changes**.
5. Go to the repo's **Settings → Pages**.
   - Under "Build and deployment", Source = **Deploy from a branch**.
   - Branch = **main**, folder = **/ (root)**. Click **Save**.
6. Wait ~1 minute. Your site is live at `https://infasiallc.github.io/infasia-site/`.
   (The custom domain in Part 2 replaces this URL.)

### Option B — With git (if you prefer the terminal)
A repo is already initialized in this folder. After creating the empty GitHub repo
(step 2 above, but do **not** add a README), run:
```bash
cd "/Users/salman.mik.us/Desktop/AI Services Business/Website"
git branch -M main
git remote add origin https://github.com/infasiallc/infasia-site.git
git push -u origin main
```
Then do step 5 above to enable Pages.

---

## Part 2 — Point infasia.net at GitHub Pages (using Northwest's DNS)

Northwest gives you DNS control in your account, so you can keep the free
infasia.net domain and just aim it at GitHub.

1. Log in to your Northwest Registered Agent account → your domain `infasia.net`
   → the **DNS** (or "DNS records" / "Manage DNS") section.
2. Add these records. If a default "parking"/placeholder A record exists, delete it first.

   **Four A records** (for the bare domain `infasia.net`). Host/Name = `@` (or leave blank):
   | Type | Host | Value           |
   |------|------|-----------------|
   | A    | @    | 185.199.108.153 |
   | A    | @    | 185.199.109.153 |
   | A    | @    | 185.199.110.153 |
   | A    | @    | 185.199.111.153 |

   **One CNAME record** (for `www.infasia.net`):
   | Type  | Host | Value                       |
   |-------|------|-----------------------------|
   | CNAME | www  | `infasiallc.github.io.` |

   (`infasiallc` is your GitHub username. Keep the trailing dot if the panel requires it.)

3. Save. DNS changes take a few minutes to a few hours to take effect.
4. Back in GitHub → **Settings → Pages → Custom domain**, it should already show
   `infasia.net` (from the `CNAME` file). If not, type `infasia.net` and Save.
5. Once the green check appears, tick **Enforce HTTPS**. Done — https://infasia.net is live.

---

## Part 2 (RECOMMENDED) — Use Cloudflare for DNS + free, reliable email

Northwest's built-in email forwarding has been unreliable (late / missed). Moving
DNS to **Cloudflare** (free) fixes that AND hosts all the website records in one
place. Northwest stays the registrar — only the nameservers change.

GitHub username: **infasiallc**  •  Forward email to: **infasia26@gmail.com**

1. Create a free account at https://dash.cloudflare.com → **Add a site** → enter
   `infasia.net` → choose the **Free** plan. (Do NOT buy anything or transfer the domain.)
2. Cloudflare scans your current records and shows **2 nameservers** (e.g.
   `xxx.ns.cloudflare.com`). Copy them.
3. In **Northwest** → domain `infasia.net` → **Nameservers** → replace Northwest's
   nameservers with the 2 Cloudflare ones → Save. (This switches DNS control to
   Cloudflare; Northwest's old forwarding stops — Cloudflare replaces it in step 5.)
4. In **Cloudflare → DNS → Records**, make sure these website records exist
   (delete any leftover "parking" A record first):

   | Type  | Name | Value                | Proxy status |
   |-------|------|----------------------|--------------|
   | A     | @    | 185.199.108.153      | DNS only (grey cloud) |
   | A     | @    | 185.199.109.153      | DNS only (grey cloud) |
   | A     | @    | 185.199.110.153      | DNS only (grey cloud) |
   | A     | @    | 185.199.111.153      | DNS only (grey cloud) |
   | CNAME | www  | infasiallc.github.io | DNS only (grey cloud) |

   > Set these to **DNS only** (grey cloud, not orange) so GitHub Pages can issue HTTPS.
5. In **Cloudflare → Email → Email Routing** → **Enable**. Cloudflare auto-adds the
   needed MX/TXT records. Add a route: `info@infasia.net` → `infasia26@gmail.com`,
   then click the verification link Cloudflare emails to that Gmail. Done — email now
   forwards reliably through Cloudflare.
6. Finish GitHub Pages: **Settings → Pages → Custom domain** shows `infasia.net`
   (from the `CNAME` file). Once the check is green, tick **Enforce HTTPS**.

### If you'd rather skip Cloudflare
You can instead add the four A records + the `www` CNAME directly in Northwest's DNS
panel (the table in the section above) and keep Northwest's email forwarding. The
website still works on free GitHub Pages — you just don't get the email reliability fix.

---

## About "buying a domain one time" (important)

Domain names cannot be bought once and owned forever — every domain in the world is
**rented annually** (this is how the global domain system works, not a Northwest
upsell). Your infasia.net is **free for the first year**, then renews for a small
yearly fee. The most you can pre-pay is usually **up to 10 years** at once, which is
the closest thing to "one and done." So:

- **Recommended:** keep infasia.net (already yours, free year one). Optionally pre-pay
  several years to avoid thinking about it.
- If you ever want a different domain, `.com`/`.net`/`.io` typically run ~$10–15/year
  (.io is pricier). Cheap registrars: Cloudflare Registrar (at-cost, no markup),
  Porkbun, Namecheap. Whatever you pick, the GitHub Pages steps above are identical —
  just change the domain in the DNS records and the `CNAME` file.

---

## Updating the site later
Edit the files, then either re-upload them on GitHub (Option A) or run
`git add -A && git commit -m "update" && git push` (Option B). Pages redeploys automatically.

## Things still marked as placeholder in the site
Search the `.html` files for the word `placeholder` and for `hello@infasia.example`
to find contact details and a few "aspirational" notes to finalize before launch.
