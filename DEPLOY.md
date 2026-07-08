# Deploying OmniOrbits to omniorbits.com

This folder (`static/`) is a complete, self-contained static site:
- `index.html` — Home page
- `product.html` — Product page
- `CNAME` — tells GitHub Pages which custom domain to serve

Both HTML files have all fonts, images, and the logo inlined — no other files or a build step needed.

## 1. Push to GitHub

1. Create a new **public** repo on GitHub, e.g. `omniorbits-site`.
2. Upload the contents of this `static/` folder to the **root** of that repo (drag-and-drop on github.com works, or via git):
   ```
   git init
   git add index.html product.html CNAME
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/<your-username>/omniorbits-site.git
   git push -u origin main
   ```

## 2. Turn on GitHub Pages

1. In the repo, go to **Settings → Pages**.
2. Under "Build and deployment", set **Source** to `Deploy from a branch`.
3. Set **Branch** to `main` / `root`, then Save.
4. Under "Custom domain", enter `omniorbits.com` and Save (this writes the same `CNAME` file — you already have it, that's fine).
5. GitHub will show a temporary URL like `https://<your-username>.github.io/omniorbits-site/` — confirm it loads before moving to DNS.

## 3. Point the domain at GitHub Pages (in GoDaddy)

In GoDaddy → **My Products → DNS** for omniorbits.com, add/edit these records:

**For the apex domain (omniorbits.com):**
Add four **A** records, all with host `@`, pointing to GitHub Pages' IPs:
```
A   @   185.199.108.153
A   @   185.199.109.153
A   @   185.199.110.153
A   @   185.199.111.153
```
(Delete GoDaddy's default parked "A @" record first if one exists.)

**For www.omniorbits.com (recommended, so both work):**
```
CNAME   www   <your-username>.github.io.
```

DNS changes can take anywhere from a few minutes to ~24 hours to propagate.

## 4. Enforce HTTPS

Back in GitHub → Settings → Pages, once DNS has propagated, check **Enforce HTTPS**. GitHub issues a free SSL certificate automatically — this can take a little while to become available after DNS resolves.

## 5. Verify

- `https://omniorbits.com` → Home page
- `https://omniorbits.com/product.html` → Product page
- Nav links between the two pages use relative paths, so they'll work as-is once deployed.

## Updating the site later

Come back to this project, edit `OmniOrbits Home.dc.html` / `OmniOrbits Product.dc.html`, re-run the bundle step to refresh `static/index.html` / `static/product.html`, then push the updated files to the same GitHub repo — Pages redeploys automatically within a minute or two of the push.
