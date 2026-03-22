# Hosting शुभारंभ Shop (public URL + remote updates)

## How it works

1. **Website** (`index.html`) loads **`site-data.json`** on every visit (if the file exists on the server). That JSON is the same format as **Admin → Export**.
2. **Changes visible to everyone**: edit data in **Admin**, export, save the file as **`site-data.json`** in this project folder, then **deploy again** (or push to Git if you use Git-based hosting).
3. **Admin in the browser** (`admin.html`) still uses **localStorage** for quick edits on one device. For **one shared source of truth** on the internet, use **`site-data.json`** + deploy.

---

## Option A — Netlify (fastest)

1. Go to [https://app.netlify.com/drop](https://app.netlify.com/drop) (or sign up → Add new site → Deploy manually).
2. **Drag this entire project folder** (zip is fine).
3. You get a URL like `https://random-name.netlify.app`.
4. After you change **`site-data.json`**, drag the folder again **or** connect GitHub and turn on auto-deploy on push.

**Custom domain**: Site settings → Domain management.

---

## Option B — Cloudflare Pages

1. [Cloudflare Dashboard](https://dash.cloudflare.com/) → Workers & Pages → Create → Pages → Connect to Git **or** Upload assets.
2. **Build command**: leave empty. **Build output directory**: `/` (root) or `.`
3. Deploy. Your site is on `*.pages.dev`.

---

## Option C — GitHub Pages

1. Create a GitHub repository and push this project.
2. Repo **Settings → Pages → Build and deployment**: source **GitHub Actions**.
3. The workflow **`.github/workflows/pages.yml`** deploys the repo root on every push to `main` or `master`.
4. Site URL: `https://<username>.github.io/<repo>/` (or custom domain in Pages settings).

---

## Option D — Vercel

1. [vercel.com](https://vercel.com) → Import project → select this folder or Git repo.
2. Framework: **Other**, output: **.** (root).
3. Deploy.

---

## Remote editors (outside your Wi‑Fi)

- Anyone with **Netlify / Cloudflare / GitHub** access can update the site by replacing **`site-data.json`** and redeploying (or pushing to Git).
- **There is no password** on `admin.html` when hosted publicly — **do not share your admin URL** unless you add protection (e.g. Netlify password, Cloudflare Access, or host admin only locally).

---

## Optional: custom JSON URL

Add **before** the main `<script>` in `index.html`:

```html
<script>window.SITE_DATA_URL = "https://your-cdn.com/site-data.json";</script>
```

---

## Local test

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000` — `site-data.json` must be in the **same folder** as `index.html`.

---

## Tunnel (temporary public URL from your PC)

```bash
npx cloudflared tunnel --url http://localhost:8000
```

Useful for demos; not for production.
