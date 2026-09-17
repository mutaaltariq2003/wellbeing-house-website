# The Wellbeing House — Launch & Editing Guide

Everything you need to put the site online and change it yourself afterwards.

---

## Part 0 — What changed, and why it matters

Your site was one 2.6 MB HTML file with all 25 photos crammed inside it as base64 text (long unreadable blocks of letters and numbers). That works, but it is close to impossible to edit — the actual content was buried under 2.5 MB of gibberish.

I have restructured it into a normal website folder:

```
website-files/
├── index.html          ← the whole site, now only 92 KB and readable
├── images/             ← all 24 photos as normal .jpg / .png files
├── robots.txt          ← tells Google it may index the site
├── sitemap.xml         ← helps Google find your pages
└── _redirects          ← stops broken links showing a 404
```

Why this matters:

| | Before | After |
|---|---|---|
| Editing the text | Scroll past 2.5 MB of base64 | Open, Ctrl+F, type |
| Replacing a photo | Re-encode to base64, paste a 200,000-character string | Drop a new file in `images/` |
| Page load speed | Everything loads at once, nothing cacheable | Images cache separately, page appears instantly |
| Google indexing | No description, no location data | Full SEO + clinic structured data |

The site works exactly the same. Nothing visual changed.

**Important:** `index.html` and the `images` folder must always stay together. The HTML looks for photos at `images/filename.jpg` — separate them and every picture breaks.

---

## Part 1 — Get a domain name

You need an address people type in. Two realistic options:

### Option A — `.pk` or `.com.pk` (recommended for a Pakistani clinic)

Signals to both patients and Google that you're a Pakistan-based business.

- The registry is **PKNIC**. <cite index="8-1">As of August 2026 the price is Rs 2,100 per year for Pakistan-based registrants, up from Rs 1,800</cite>. <cite index="13-1">Because registration is billed in two-year blocks, the direct registry cost is Rs 4,200 for two years.</cite>
- <cite index="12-1">PKNIC does not sell directly to individuals — you must go through an accredited reseller</cite>, so expect to pay a markup. Resellers commonly quote somewhere in the Rs 3,300–4,500 range for two years.
- Established resellers include HosterPK, PK Domain, and Domain.pk. Compare the **renewal** price, not just the first-year price — that is where people get caught out.

### Option B — `.com`

Around $10–15/year from Namecheap, Cloudflare Registrar, or Porkbun. Cheaper, globally familiar, and Cloudflare sells domains at cost with no markup.

### Suggested names
`thewellbeinghouse.pk` · `thewellbeinghouse.com.pk` · `wellbeinghouse.pk`

> **Register the domain in the clinic's own name / Dr. Khan's name — never a developer's or an employee's.** Whoever is listed as registrant legally controls the domain. Getting it transferred later, if that relationship sours, is painful.

---

## Part 2 — Choose a host

Your site is static (no database, no server code), so hosting is free.

| Host | Free bandwidth | Best for |
|---|---|---|
| **Cloudflare Pages** | <cite index="4-1">Unlimited — the only one of the four majors with no bandwidth cap, plus a 300-city CDN and built-in DDoS protection</cite> | **Recommended** |
| GitHub Pages | <cite index="1-1">Soft cap of 100 GB/month; public repositories only on the free tier</cite> | Simplicity |
| Netlify | Was 100 GB, now credit-based — <cite index="5-1">300 credits/month at 20 credits per GB works out to roughly 15 GB</cite> | Avoid now |
| Vercel | 100 GB/month | Overkill here |

**Recommendation: Cloudflare Pages.** Unlimited bandwidth means a viral post or a spike in traffic can never knock your clinic's site offline or trigger a surprise bill. Cloudflare also gives you free DNS, free SSL, and DDoS protection in the same dashboard.

Note that Netlify's free tier has become considerably weaker — <cite index="2-1">its pricing has changed four times, most recently in April 2026 when bandwidth went from 10 to 20 credits per GB</cite>. I would not build a clinic's public presence on it today.

---

## Part 3 — Two ways to launch

Pick **one**. Method A is faster; Method B is better long-term.

### Method A — Drag and drop (5 minutes, no technical knowledge)

Good if you want to be live today and don't mind re-uploading the whole folder each time you make a change.

1. Go to **dash.cloudflare.com** and create a free account
2. In the sidebar: **Workers & Pages** → **Create** → **Pages** → **Upload assets**
3. Give the project a name, e.g. `wellbeing-house`
4. Drag the **`website-files` folder** into the upload box (the folder itself, not the individual files)
5. Click **Deploy**

You're live at `wellbeing-house.pages.dev`.

To update later: edit `index.html` on your computer → go back to the project → **Create new deployment** → drag the folder in again.

### Method B — GitHub (recommended: edit from any browser, automatic backups)

This takes 20 minutes to set up once, and afterwards you can edit the website from your phone. Every change is saved forever and reversible.

**Step 1 — Put the files on GitHub**

1. Create a free account at **github.com**
2. Click **+** (top right) → **New repository**
3. Name it `wellbeing-house-website`, set it to **Public** (required for GitHub's free tier; your site is public anyway), click **Create**
4. On the next screen click **uploading an existing file**
5. Drag in `index.html`, `robots.txt`, `sitemap.xml`, `_redirects`, **and the whole `images` folder**
6. Click **Commit changes**

**Step 2 — Connect Cloudflare Pages to GitHub**

1. **dash.cloudflare.com** → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
2. Authorise GitHub, pick `wellbeing-house-website`
3. Build settings — this is important, your site needs no build step:
   - Framework preset: **None**
   - Build command: **leave completely empty**
   - Build output directory: `/`
4. **Save and Deploy**

Done. Now **every time you change a file on GitHub, the live site updates within about a minute, automatically.**

---

## Part 4 — Connect your domain

Once the site is live on `something.pages.dev`:

1. In Cloudflare Pages → your project → **Custom domains** → **Set up a custom domain**
2. Enter `thewellbeinghouse.pk` (and repeat for `www.thewellbeinghouse.pk`)
3. Cloudflare will show you **nameservers**, something like:
   ```
   xxx.ns.cloudflare.com
   yyy.ns.cloudflare.com
   ```
4. Log in to wherever you bought the domain (PKNIC reseller panel), find **Nameservers** / **DNS**, and replace what's there with Cloudflare's two
5. Wait. Usually 1–2 hours, occasionally up to 24.

HTTPS (the padlock) is issued automatically and free. You don't need to buy an SSL certificate — if a reseller tries to sell you one, you don't need it.

**After connecting the domain,** open `index.html` and confirm the URLs near the top match your real domain:
```html
<link rel="canonical" href="https://www.thewellbeinghouse.pk/">
<meta property="og:url" content="https://www.thewellbeinghouse.pk/">
```
Also update the URL inside `sitemap.xml` and `robots.txt`.

---

## Part 5 — Editing the site afterwards

### The easiest method (Method B users): edit in your browser

1. Go to your repository on **github.com**
2. Click `index.html`
3. Click the **pencil icon** (top right)
4. Press **Ctrl+F**, search for the exact words you want to change
5. Type your change
6. Scroll down → **Commit changes**
7. Wait ~1 minute. The live site is updated.

This works from a phone browser too.

### Rules for editing safely

Only change text **between** tags. Never delete the tags.

```html
<h3>Inpatient Care</h3>
     ^^^^^^^^^^^^^^  safe to edit
^^^^                ^^^^^ leave alone
```

I have put a full editing guide **inside `index.html` itself** — open the file and it's the first thing you see, explaining where every section lives.

### Common edits

**Change the phone number or email**
These appear in several places. Use Find & Replace on all of them:
- `+92 323 6530127` — also appears inside `wa.me/923236530127` WhatsApp links
- `051 6122571`
- `thewellbeinghouse@outlook.com`
- Also update the `telephone` and `email` fields in the structured-data block near the top

**Change service descriptions**
Ctrl+F the heading, e.g. `Inpatient Care`, and edit the paragraph under it.

**Add a team member**
Find `============ TEAM`, copy a whole `<div class="team-card reveal">...</div>` block, paste below it, change the name, role, description, and image filename.

**Add or replace a photo**
1. Put the new image in `images/`
2. Update the filename in the `<img src="images/...">` line
3. Keep photos under ~200 KB — resize to about 1200 px wide first

**Change opening hours**
Ctrl+F `9:00 AM – 5:00 PM`. Update it in the Find Us page, the Find Us popup, **and** the structured-data block.

### Undoing a mistake

On GitHub: click **History** (or the clock icon) on the file → find the version before your change → **Revert**. Nothing is ever permanently lost. This is the single biggest reason to use Method B over Method A.

---

## Part 6 — After launch

**Tell Google the site exists**
1. Go to **search.google.com/search-console**
2. Add your domain, verify ownership (Cloudflare makes this one click)
3. Submit `https://www.thewellbeinghouse.pk/sitemap.xml`

Expect a few days to a few weeks before you appear in search results.

**Update your Google Business Profile**
Your clinic already has a listing (currently 4.3★ from 18 reviews). Add the website URL to it — this is one of the strongest local-SEO signals there is, and it makes the "Share Your Feedback" button on your site round-trip properly.

**Link it everywhere**
Instagram bio, Facebook page, WhatsApp Business profile, printed cards, the sign outside.

**Test before announcing**
Open the site on an actual phone and check:
- Every page in the menu opens
- The Book Appointment button opens WhatsApp correctly
- The Find Us popup shows the right address
- Photos all load
- The personality quiz runs to the end

---

## Part 7 — Running costs

| Item | Cost |
|---|---|
| Domain (`.pk`, via reseller) | ~Rs 3,300–4,500 per 2 years |
| Cloudflare Pages hosting | Free |
| SSL / HTTPS | Free |
| GitHub | Free |
| **Total** | **≈ Rs 1,700–2,300/year — just the domain** |

Set a calendar reminder about a month before the domain expires. An expired domain can be bought by someone else, and getting it back is expensive or impossible.

---

## Part 8 — Honest limitations

Worth knowing before you launch:

- **No booking system.** The "Book Appointment" button opens WhatsApp or the phone dialler. There is no calendar, no database, no automatic confirmation. If you later want real online booking with time slots, that needs a backend — a different and much larger project.
- **No contact form.** Same reason. If you want one, Cloudflare Pages and Netlify both offer free form handling that emails you submissions; it would need to be added.
- **No admin panel.** Editing means editing HTML. It's plain text and the guide inside the file makes it manageable, but it isn't WordPress. If someone non-technical will be updating it weekly, consider a CMS later.
- **The review section shows your rating, not review text.** It displays 4.3★ / 18 reviews and links to Google. I deliberately did not copy review text onto the site, since that content belongs to the people who wrote it and to Google.
- **Not formally accessibility-audited.** Alt text and semantic structure are in place, but contrast ratios and full keyboard navigation through the quiz and modals haven't been tested against WCAG.

---

## Quick reference

```
Live site (temporary):  wellbeing-house.pages.dev
Live site (final):      thewellbeinghouse.pk
Edit at:                github.com/YOUR-USERNAME/wellbeing-house-website
Host dashboard:         dash.cloudflare.com
Search Console:         search.google.com/search-console
```

**If the site ever breaks:** revert the last change on GitHub (History → Revert). Everything comes back.
