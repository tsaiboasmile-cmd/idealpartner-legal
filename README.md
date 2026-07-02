# IdealPartner — Legal &amp; Compliance Site

A tiny, no-build static site with the three legal documents for the **IdealPartner** iOS app,
styled in a clean Apple-like aesthetic (light + dark mode, mobile-first). Ready to host on
**GitHub Pages**.

```
idealpartner-site/
├── index.html            → landing page linking to the three docs
├── privacy.html          → Privacy Policy (CCPA/CPRA + GDPR/UK GDPR)
├── terms.html            → Terms of Use
├── data-compliance.html  → Data & Compliance reference (CCPA matrix, sub-processors, transfers)
├── styles.css            → shared stylesheet (no frameworks, no build step)
└── README.md             → this file
```

The filenames are **stable and predictable** so you can link to them from inside the app
(Settings → About → "Privacy Policy" / "Terms of Use"):

- `https://<your-domain>/privacy.html`
- `https://<your-domain>/terms.html`
- `https://<your-domain>/data-compliance.html`

---

## ⚠️ Before you publish — this is a template, not legal advice

These documents are **compliance-aligned templates**, not legal advice. **Have them reviewed by
a qualified attorney licensed in your jurisdiction before launch.**

You must replace every placeholder. They are visually marked in the pages (dashed orange chips)
and written in `[BRACKETS]`. Search the files for `[` to find them all.

### 🚀 Solo developer? The 6 things you actually must fill in

If you're a one-person indie, this is the whole job. Fill these six, and you're 90% done:

| Placeholder | What to put (solo dev) |
|---|---|
| `[LEGAL ENTITY NAME]` | **Just your own name** (e.g. `Jane Doe`), or your registered business name if you have one |
| `[CONTACT EMAIL]` | One support/privacy email — the same address is fine everywhere |
| `[MAILING ADDRESS]` | A contact address. Don't want to use home? Use a **PO box** or **mail-forwarding service** |
| `[GOVERNING LAW / JURISDICTION]` | The country/state you live and operate in (e.g. `Taiwan` or `California, USA`) |
| `[EFFECTIVE DATE]` | The date you publish (e.g. `July 3, 2026`) |
| `[SUPABASE DATA REGION]` | From your Supabase dashboard → Project Settings → General (e.g. `ap-northeast-1`) |

Plus `[YEAR]` in the footers (the current year).

### What a solo developer can usually skip

- **DPO (Data Protection Officer):** you almost certainly don't need one. The pages already say
  "we have not appointed a DPO." Leave it.
- **EU/UK representative:** only required if you have no EEA/UK presence *and* actively offer the
  App to people there. If that's not you, delete the `[EU / UK REPRESENTATIVE]` lines. If it is,
  cheap representative services exist.
- **Extra sub-processors / analytics rows:** you use none, so delete those placeholder rows in
  `data-compliance.html` (or leave them — they say "if any").
- **The "Solo-developer note" grey boxes** in the pages: these are instructions *to you* — delete
  them before you publish.

### The few judgment calls (pick a value, then have a lawyer glance at it)

- **Retention periods** (Privacy §12 / Data & Compliance §8) — sensible defaults are pre-filled in
  brackets, e.g. delete account data within `30 days`.
- **Refund terms** (Terms §5) — Apple handles refunds; you can often just leave it to Apple.
- **Moderation response time** (Terms §8) — a common promise is "within 24 hours."
- **Dispute/arbitration clause** (Terms §16) — the simplest solo option is just "courts of
  [your country]." Don't add arbitration unless a lawyer advises it.

> Tip — quick find & replace across the folder:
> ```bash
> # macOS (BSD sed). Repeat per placeholder, or script a loop.
> LC_ALL=C sed -i '' 's/\[CONTACT EMAIL\]/privacy@example.com/g' *.html
> ```
> Verify nothing is left: `grep -o '\[[A-Z][^]]*\]' *.html | sort -u`

---

## Preview locally

No build step. Just open `index.html` in a browser, or serve the folder:

```bash
cd idealpartner-site
python3 -m http.server 8000
# open http://localhost:8000
```

---

## Deploy to GitHub Pages

### 1. Create a repository and push

```bash
cd idealpartner-site
git init
git add .
git commit -m "Add IdealPartner legal & compliance site"

# Create the repo on GitHub (via the website, or the gh CLI):
gh repo create idealpartner-legal --public --source=. --remote=origin --push
# — or, if you created the empty repo manually on github.com:
# git remote add origin https://github.com/<you>/idealpartner-legal.git
# git branch -M main
# git push -u origin main
```

### 2. Enable GitHub Pages

1. On GitHub, open the repo → **Settings** → **Pages**.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Set **Branch** to `main` and folder to **`/ (root)`**, then **Save**.
4. Wait ~1 minute. Your site publishes at:
   `https://<your-username>.github.io/idealpartner-legal/`

   Then your pages are, e.g.:
   `https://<your-username>.github.io/idealpartner-legal/privacy.html`

> If you prefer the files at the domain root without the repo-name subpath, name the repo
> `<your-username>.github.io` (a user/organization site). Then pages live at
> `https://<your-username>.github.io/privacy.html`.

### 3. (Optional) Custom domain

To serve at, e.g., `legal.idealpartner.app`:

1. In **Settings → Pages → Custom domain**, enter your domain and **Save** (this creates a
   `CNAME` file in the repo — keep it committed).
2. At your DNS provider, add a **CNAME** record pointing your subdomain to
   `<your-username>.github.io.` (For an apex/root domain, add GitHub's **A/AAAA** records
   instead — see GitHub's docs.)
3. Wait for DNS to propagate, then enable **Enforce HTTPS** in Settings → Pages.

Your in-app links then become the clean, stable:
`https://legal.idealpartner.app/privacy.html`, `/terms.html`, `/data-compliance.html`.

---

## Linking from the iOS app

Point Settings → About buttons at the stable URLs, for example:

```swift
Link("Privacy Policy", destination: URL(string: "https://legal.idealpartner.app/privacy.html")!)
Link("Terms of Use",   destination: URL(string: "https://legal.idealpartner.app/terms.html")!)
```

The pages set `viewport-fit=cover` and are fully responsive, so they render cleanly in Safari /
`SFSafariViewController` on iPhone, including notch/Dynamic-Island safe areas, in both light and
dark mode.

---

## Notes

- **Keep the three documents in sync** with each other and with your **App Store privacy
  labels** whenever your data practices change.
- Update the **"Last updated"** date whenever you edit a document.
- No analytics or trackers are included by design. If you add any, disclose them in all three
  documents and your App Store labels.
