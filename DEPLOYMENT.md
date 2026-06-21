# Deployment Guide — ChurchHUB Investor Site

This guide is written for a **non-technical founder**. Follow it top to bottom.
You do not need to understand code. You will copy a file, click a few buttons,
and paste two DNS records.

The whole site is a **single file**: `index.html`. Everything — the financial
model, the stress tester, the investor deck, the embedded PDF prospectus, the
leadership photos, every icon — lives inside that one file. There are no other
moving parts to break.

**Target live address:** `https://investors.churchhub.ai`

---

## What's in this folder (file structure)

| File | What it is | Do you touch it? |
|------|------------|------------------|
| `index.html` | **The entire website.** Open it in any browser to preview. | No — deploy as-is |
| `DEPLOYMENT.md` | This guide. | No |
| `README.md` | Short project description. | No |
| `.nojekyll` | An empty file that tells GitHub Pages "serve my file exactly as-is, don't process it." Prevents a class of silent failures. | No — just keep it |
| `.gitignore` | Tells Git to ignore junk files (e.g. `.DS_Store` on Mac). | No |

That's the complete set. **Five files. Nothing else is required.** If all five are
present, you have everything you need to deploy.

> **Note on file size:** `index.html` is about **7.3 MB**. That is normal and
> expected — most of it is the 32-page PDF prospectus embedded directly inside,
> so the site works even with no internet connection to a separate file. It is
> well within every host's limits.

---

## The big picture (how the three pieces fit)

1. **GitHub** = where the file is stored (the filing cabinet).
2. **Vercel** = the service that takes the file from GitHub and serves it to the
   public on the internet (the storefront). It also gives you free HTTPS (the
   padlock).
3. **Custom domain** = your branded web address, `investors.churchhub.ai`,
   pointed at Vercel so visitors see your name, not a random URL.

You do them in that order: GitHub → Vercel → Domain.

---

## PART 1 — GitHub Setup

**Goal:** get `index.html` (and the other four files) into the repository
`https://github.com/Churchhub2026/churchhub-investor-site`.

### Easiest path: upload through the website (no commands)

1. Go to **https://github.com/Churchhub2026/churchhub-investor-site**.
   (Sign in as the `Churchhub2026` account.)
2. If the repository is empty, click **"uploading an existing file"** in the
   middle of the page. If it already has files, click **Add file → Upload files**.
3. Drag **all five files** from this folder into the browser window:
   `index.html`, `DEPLOYMENT.md`, `README.md`, `.nojekyll`, `.gitignore`.
   - **Important:** `.nojekyll` and `.gitignore` start with a dot. On Mac, press
     **Cmd+Shift+.** (period) in the file picker to make dot-files visible. On
     Windows they show by default. If the drag doesn't pick them up, that
     keyboard shortcut reveals them.
4. In the **"Commit changes"** box at the bottom, type a short message like
   `Add investor site` and click **Commit changes**.
5. Refresh the page. You should see all five files listed. ✅ GitHub is done.

### Alternative: command line (only if you prefer it)

A ready-made repository **bundle** was provided (`churchhub-investor-site.bundle`)
that contains the files already committed. From a terminal:

```bash
git clone churchhub-investor-site.bundle churchhub-investor-site
cd churchhub-investor-site
git remote set-url origin https://github.com/Churchhub2026/churchhub-investor-site.git
git push -u origin main
```

You'll be asked to sign in to GitHub (use a Personal Access Token as the
password if prompted). When it finishes without errors, GitHub is done.

---

## PART 2 — Vercel Deployment

**Goal:** put the GitHub file on the public internet with HTTPS.

1. Go to **https://vercel.com** and click **Sign Up** (or Log In).
   Choose **"Continue with GitHub"** and sign in as `Churchhub2026`. This links
   the two accounts so Vercel can see your repository.
2. On the Vercel dashboard, click **Add New… → Project**.
3. Find **`churchhub-investor-site`** in the list and click **Import**.
4. On the configuration screen, **change nothing.** This is a plain HTML site,
   not an app, so:
   - **Framework Preset:** leave as **Other** (Vercel usually auto-detects this).
   - **Build Command:** leave **empty**.
   - **Output Directory:** leave **empty / default**.
   - **Root Directory:** leave as **`./`**.
5. Click **Deploy**.
6. Wait about 30–60 seconds. You'll see a **"Congratulations"** screen with a
   preview. Click the preview, or the **`.vercel.app`** link, to see your live
   site (e.g. `churchhub-investor-site.vercel.app`).
7. Click through it: tabs, the **Financial Model** sliders, the **Investor Deck**
   button, and the **Investor Prospectus** button (the PDF should open in a
   pop-up that fills the screen). ✅ Vercel is done.

> Every time you upload a new `index.html` to GitHub in future, Vercel
> **automatically re-deploys** it within a minute. You don't repeat Part 2.

---

## PART 3 — Custom Domain: `investors.churchhub.ai`

**Goal:** make the site load at `https://investors.churchhub.ai` instead of the
`.vercel.app` address.

Because `investors.` is a **subdomain** of `churchhub.ai`, this uses a **CNAME**
record (one line you paste at whoever manages your `churchhub.ai` DNS — e.g.
GoDaddy, Namecheap, Cloudflare, Google Domains).

### Step A — Tell Vercel about the domain

1. In Vercel, open your project → **Settings → Domains**.
2. Type `investors.churchhub.ai` and click **Add**.
3. Vercel will show you the **exact DNS record** to create. It will look like
   the table below. **Always use the value Vercel shows you on screen** — each
   project can be assigned its own CNAME target, so copy from the dashboard
   rather than assuming.

### Step B — Add the DNS record at your domain provider

Log in wherever `churchhub.ai` is managed, find **DNS** / **DNS Records** /
**Zone Editor**, and add **one** record:

| Field | What to enter |
|-------|---------------|
| **Type** | `CNAME` |
| **Name / Host** | `investors` (just the word — your provider adds `.churchhub.ai` automatically) |
| **Value / Target / Points to** | `cname.vercel-dns.com` *(or the exact value Vercel shows you — copy it verbatim, including any trailing period)* |
| **TTL** | `3600` or **Auto** (either is fine) |

Save it.

> **If you ever point the bare root `churchhub.ai` at Vercel instead** (not
> needed for the investor subdomain), the root uses an **A record** with value
> **`76.76.21.21`** — root domains can't use CNAME. For `investors.churchhub.ai`,
> the CNAME above is correct.

### Step C — Verify

1. Back in Vercel's **Domains** screen, wait a few minutes and click **Refresh**
   (or it updates on its own). The status changes from "Invalid Configuration"
   to a green **"Valid Configuration."**
2. Vercel then automatically issues a free SSL certificate (the padlock). This
   can take a few minutes after the record verifies.
3. Open **https://investors.churchhub.ai**. You should see the site with a
   padlock in the address bar. ✅ Domain is done.

> **Patience note:** DNS changes can take anywhere from a few minutes to a few
> hours to spread across the internet (occasionally up to 24–48 hours). If it
> isn't live immediately, that's normal — wait and refresh.

---

## DNS Records — Quick Reference

For `investors.churchhub.ai` (the only record you need):

```
Type:  CNAME
Host:  investors
Value: cname.vercel-dns.com      ← or the exact target Vercel displays
TTL:   3600 (or Auto)
```

Optional, only if you also point the root domain `churchhub.ai` at Vercel:

```
Type:  A
Host:  @
Value: 76.76.21.21
TTL:   3600 (or Auto)
```

---

## Troubleshooting

**"The PDF prospectus pop-up is blank / gray."**
The pop-up always shows a **Download** button and an **Open in new tab** button at
the top — those work in every browser. The inline preview shows blank only in the
rare browser with no built-in PDF viewer. Visitors can still get the PDF via those
two buttons. (Chrome, Safari, Edge, and Firefox all preview it inline normally.)

**"My domain says Invalid Configuration in Vercel."**
The DNS record either hasn't propagated yet (wait) or has a typo. Double-check
the **Host** is exactly `investors` and the **Value** matches what Vercel shows.
Remove any *other* old record on `investors` that might conflict.

**"Site loads at .vercel.app but not at my custom domain."**
That means Vercel works and the issue is purely DNS. Recheck the CNAME, and give
it time. You can test propagation at **https://whatsmydns.net** (enter
`investors.churchhub.ai`, choose CNAME).

**"I see no padlock / 'Not Secure'."**
The SSL certificate is still being issued. Wait a few minutes after the domain
shows "Valid," then reload. Vercel provisions HTTPS automatically — you never buy
or install a certificate.

**"Cloudflare-specific: the site won't get a padlock."**
If `churchhub.ai`'s DNS is on Cloudflare, set the record's proxy status to
**"DNS only"** (the gray cloud, not orange). The orange-cloud proxy blocks
Vercel from issuing the certificate.

**"GitHub didn't pick up my `.nojekyll` file."**
On Mac, dot-files are hidden. In the upload window press **Cmd+Shift+.** to reveal
them, then drag it in. The file is intentionally empty — that's correct.

**"I changed the site — how do I publish the update?"**
Upload the new `index.html` to GitHub (Part 1). Vercel redeploys automatically in
about a minute. No other steps.

**"Page looks broken / numbers don't update when I move sliders."**
This only happens if JavaScript is disabled in the browser. The financial model
needs JavaScript. All mainstream browsers have it on by default.

---

## Browser Compatibility

The site is built with standard HTML, CSS, and JavaScript — no special plugins.

| Browser | Status |
|---------|--------|
| Google Chrome (desktop & mobile) | ✅ Fully supported, inline PDF preview works |
| Safari (Mac & iPhone/iPad) | ✅ Fully supported, inline PDF preview works |
| Microsoft Edge | ✅ Fully supported, inline PDF preview works |
| Firefox | ✅ Fully supported, inline PDF preview works |
| Older browsers (Internet Explorer 11) | ⚠️ Not supported — visitors should use a modern browser |

Notes:
- **JavaScript must be on** (it is, by default) for the financial model and stress
  tester to calculate.
- The layout is **responsive** — it adapts to phones, tablets, and desktops.
- The site is **fully self-contained**: no external fonts, no third-party scripts,
  no tracking. It even works offline once loaded.

---

## Final Pre-Launch Checklist

Before sharing the link with investors, open the live site and confirm:

- [ ] All five tabs switch (The Opportunity, Why ChurchHUB Wins, Financial Model,
      Leadership, Formulas)
- [ ] Financial Model sliders move and the numbers recalculate
- [ ] The stress-test preset buttons (Base case, CAC, Churn, Token, ARPC, Stacked)
      change the outputs
- [ ] **Investor Deck** button opens the slide pop-up; arrows/Escape work
- [ ] **Investor Prospectus** button opens the PDF pop-up; **Download** and
      **Open in new tab** both work
- [ ] The three leadership photos load
- [ ] The padlock (HTTPS) shows in the address bar
- [ ] The site loads at **https://investors.churchhub.ai**

When every box is checked, you're live. 🚀
