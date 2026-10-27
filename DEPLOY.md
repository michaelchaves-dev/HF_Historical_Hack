# DEPLOY.md — HF Incident Timeline → Vercel → tokenarchitectai.tech

Exact deploy steps for Roman + Michael. Do these in order, in one sitting
(Michael needs to be available for step 2).

**Preconditions**
- [ ] Folder `~/workspace/builds/hf-timeline-deploy/` contains `index.html`, `vercel.json`, `README.md`, and this file.
- [ ] Michael has approved the exact content of `index.html` for public release.
- [ ] Spend: $0 — Vercel free tier only. No purchases, no plan upgrades.

---

## Step 1 — Get this folder into a GitHub repo / Vercel import

Pick ONE route:

**Route A — GitHub repo first (preferred, matches the proven pipeline)**
1. Michael creates a new GitHub repo under `michaelchaves-dev` for this site
   (Roman's GitHub app cannot create repos — Michael creates it, as with the
   other buildout repos).
2. Roman pushes the contents of this folder to that repo's `main` branch:
   `index.html`, `vercel.json`, `README.md`, `DEPLOY.md`.
3. Confirm on GitHub that `index.html` is at the repo root.

**Route B — Vercel import without a prior manual push**
1. In Vercel, choose Add New → Project → Import, and import the GitHub repo
   once it exists / is connected as in Route A.
   (Vercel deploys from a Git repo — there is no deploy step before this import.)

Do not use any other hosting/upload service.

## Step 2 — Create the Vercel project

1. Go to vercel.com and sign in.
   - **Sign-in requires Michael:** Vercel sign-in runs through GitHub
     (`michaelchaves-dev`), and Vercel then asks for its **own 2FA — an
     authenticator-app code (or passkey) that only Michael can supply**
     at deploy time. Have Michael and his authenticator ready before starting.
     GitHub may also send a device-verification code — handle it the same way
     as in previous deploys.
2. Add New → Project → Import the repo from Step 1.
3. Project settings:
   - Framework preset: **Other** (plain static site)
   - Build Command: **leave empty / none** — there is no build step
   - Output Directory: **leave as the project root (the folder itself)**
   - Install Command: leave default/empty — there are no dependencies, no `package.json`
4. Deploy. Vercel will give a temporary `*.vercel.app` URL.
5. Open that `*.vercel.app` URL and confirm the timeline loads and interacts
   correctly **before** touching the custom domain.

## Step 3 — Add the custom domain in Vercel

1. In the Vercel project: Settings → Domains → Add.
2. Add exactly: `tokenarchitectai.tech`
3. Also add `www.tokenarchitectai.tech` if Vercel prompts for the www variant,
   and follow Vercel's redirect recommendation shown on screen.
4. Vercel will now display the DNS records this domain needs. **Leave this
   screen open — Step 4 copies from it.**

## Step 4 — Enter Vercel's DNS records at Hostinger

> ⚠️ **Do NOT invent DNS values.** No A-record IP and no CNAME target is
> written in this document on purpose — those values change and must be
> **copied exactly from Vercel's domain screen (Step 3) at the time of the
> actual deploy.**

1. Sign in to Hostinger (Michael — his account) and open DNS for
   **tokenarchitectai.tech**.
2. For each record Vercel displays in Step 3, create/edit the matching record
   in Hostinger DNS:
   - Same record **type** Vercel shows (e.g. A or CNAME, as displayed)
   - Same **name/host** Vercel shows (e.g. `@` or `www`, as displayed)
   - Same **value/target** Vercel shows — character for character
3. Remove or replace any conflicting pre-existing record for the same
   name/type that Hostinger DNS shows, only if Vercel flags a conflict.
4. Save, then return to Vercel's domain screen and wait for it to report the
   domain as valid / DNS verified. This can take a while to propagate —
   if Vercel still shows "Invalid Configuration," re-check that the Hostinger
   entries match Vercel's screen exactly before changing anything else.

## Step 5 — Verify live + HTTPS

1. Open `https://tokenarchitectai.tech` in a fresh/private browser window.
   - Page title should read: **HF Incident — Interactive Timeline**
   - Timeline should load fully and be interactive (scroll/click through it)
   - Address bar should show a valid HTTPS lock — Vercel provisions the
     certificate automatically once DNS is verified; allow time for it to issue
2. If `www.tokenarchitectai.tech` was added, open it too and confirm it loads
   or redirects as configured in Step 3.
3. Re-open the `*.vercel.app` URL to confirm it still serves the same page.
4. Report the live URL back to Michael and record the verified live state —
   only after this check may the site be described as live/shipped.

---

## If something goes wrong

- **Vercel domain stuck on "Invalid Configuration":** compare Hostinger DNS
  against Vercel's domain screen character-by-character (Step 4); wait for
  propagation before making further changes.
- **Page loads without styling/interaction:** confirm `index.html` at the
  project root is the byte-identical copy (SHA-256
  `2a81c25c2e9e6c89874ffcc1a3b6bbf8328fd82c412ce9062b6bc019463b6986`,
  29,503 bytes) — the file is self-contained, so a broken deploy is a
  wrong-file/wrong-root problem, not a missing-assets problem.
- **2FA blocks sign-in:** stop and get Michael — do not work around it.
