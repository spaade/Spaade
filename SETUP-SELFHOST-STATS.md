# 🔧 Self-host GitHub Readme Stats (rate-limit proof)

The `github-readme-stats.vercel.app` public instance is shared by thousands of
profiles and constantly hits GitHub's API rate limit, so the cards flicker as
"broken image". Hosting your own copy on Vercel (free) fixes this permanently —
it uses **your** token, so it never runs out of quota, and `count_private=true`
will actually count your private repos.

Takes ~3 minutes.

---

## 1. Create a GitHub token (PAT)

1. Go to **https://github.com/settings/tokens** → **Generate new token** → **classic**.
2. Name: `readme-stats`.
3. Expiration: `No expiration` (or 1 year).
4. Scopes: check **`repo`** (needed so private-repo contributions are counted).
5. Click **Generate token** and **copy it** (you only see it once).

> Keep this token secret. You'll paste it into Vercel in the next step — not into
> any file in this repo.

---

## 2. Deploy to your own Vercel

One-click deploy (signs you in with GitHub and asks for the token):

**https://vercel.com/new/clone?repository-url=https://github.com/anuraghazra/github-readme-stats&env=PAT_1**

1. Sign in to Vercel with GitHub.
2. When it asks for **Environment Variables**, add:
   - **Name:** `PAT_1`
   - **Value:** the token you copied in step 1
3. Click **Deploy** and wait ~1 min.
4. Copy your deployment domain — it looks like
   `github-readme-stats-xxxxxx.vercel.app`.

(If the button doesn't prompt for the env var: open the project → **Settings →
Environment Variables** → add `PAT_1` → **Deployments → Redeploy**.)

---

## 3. Point the README at your instance

Once you have your domain, run this in PowerShell from the repo folder — it swaps
every `github-readme-stats.vercel.app` for yours:

```powershell
cd "C:\Users\gustt\OneDrive\Documentos\Claude\Projects\Github\Spaade"

# 👇 paste YOUR Vercel domain here (no https://, no trailing slash)
$domain = "github-readme-stats-xxxxxx.vercel.app"

(Get-Content README.md -Raw) `
  -replace 'github-readme-stats\.vercel\.app', $domain |
  Set-Content README.md -NoNewline -Encoding utf8

git add README.md
git commit -m "Use self-hosted github-readme-stats instance"
git push origin master
```

That's it — the Stats and Top-Languages cards will now render every time.

---

## (Optional) The Trophies card

The 🏆 trophies come from a **different** project
(`github-profile-trophy.vercel.app` → `ryo-ma/github-profile-trophy`), which is
usually more stable than the stats one. If it keeps flickering, you can either:

- **Self-host it too** — same flow:
  `https://vercel.com/new/clone?repository-url=https://github.com/ryo-ma/github-profile-trophy`
  (no token required for public data), then swap
  `github-profile-trophy.vercel.app` → your domain in the README; **or**
- **Drop it** — just delete the trophies `<img>` line in the README.

---

### Reference — the exact card URLs (already themed in your Lucy palette)

After self-hosting, these are what the lines become (with `YOUR-DOMAIN` replaced):

```
Stats:
https://YOUR-DOMAIN/api?username=spaade&show_icons=true&count_private=true&hide_border=true&bg_color=0D0B1A&title_color=5EF6FF&icon_color=FF2E97&text_color=F2D74E&ring_color=9D4EDD

Top languages:
https://YOUR-DOMAIN/api/top-langs/?username=spaade&layout=compact&hide_border=true&bg_color=0D0B1A&title_color=5EF6FF&text_color=F2D74E&icon_color=FF2E97&langs_count=8
```

> This file is just a setup reference — it does **not** show on your GitHub
> profile (only `README.md` does). Delete it whenever you're done.
