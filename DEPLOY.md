# Deploy the Rooftoprob site to GitHub Pages

This folder is a **self-contained static site** (`index.html` + `CNAME`). It is its own thing — keep it in its own GitHub repo, separate from the TDC CRM.

## Files here
| File | Purpose |
|---|---|
| `index.html` | The whole website (GitHub Pages serves this at the root URL) |
| `CNAME` | Custom domain for Pages — currently `www.rooftoprobroofing.com` |
| `.nojekyll` | Tells Pages to serve files as-is (no Jekyll processing) |

---

## One-time setup (create the repo)

You don't have the GitHub CLI (`gh`) installed, so create the repo in the browser:

1. Go to <https://github.com/new>
2. Name it e.g. **`rooftoprob-site`**, set it **Public**, do **not** add a README/.gitignore (this folder already has files).
3. Click **Create repository** and copy the repo URL it shows you.

Then, from this folder, run (replace the URL with yours):

```bash
cd "rooftoprob-site"
git init
git add index.html CNAME .nojekyll
git commit -m "Rooftoprob Roofing website"
git branch -M main
git remote add origin https://github.com/<your-user>/rooftoprob-site.git
git push -u origin main
```

## Turn on Pages
1. In the new repo → **Settings → Pages**.
2. **Source:** Deploy from a branch → Branch **`main`** / folder **`/ (root)`** → Save.
3. Wait ~1 min. Your site goes live at `https://<your-user>.github.io/rooftoprob-site/`.

## Custom domain (www.rooftoprobroofing.com)
The `CNAME` file already requests this domain. To make it resolve, add DNS at your domain registrar:
- **CNAME** record: `www` → `<your-user>.github.io`
- (Optional apex) **A** records for `rooftoprobroofing.com` → GitHub's IPs: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
- Back in **Settings → Pages**, confirm the custom domain and tick **Enforce HTTPS**.

> If you don't have the domain yet, delete the `CNAME` file and the site will just live at the `github.io` URL until you're ready.

---

## Connecting the AI assistant (chat + roof photo check)
GitHub Pages can't run the Claude backend (it serves static files only). Deploy `../rooftoprob-backend` to Vercel (see its README), then tell the page where the backend lives by adding ONE line just before `</head>` in `index.html`:

```html
<script>window.ROOFTOPROB_API_BASE = "https://your-backend.vercel.app";</script>
```

Until you set that, the chat and roof-check widgets work in **offline mode** — they politely direct visitors to call (346) 826-4424. No errors, nothing broken.
