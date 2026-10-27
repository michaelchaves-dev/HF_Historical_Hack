# HF_Historical_Hack

Home of the interactive **HF Incident Timeline**. Public home (agreed Oct 26, 2026): **tokenarchitectai.tech** — deployed on Vercel, DNS pointed from Hostinger.

## Contents

| File | Purpose |
|---|---|
| `index.html` | The timeline itself — single self-contained file |
| `vercel.json` | Minimal Vercel config for a plain static site (no build command; repo root is the output; `cleanUrls` off) |
| `DEPLOY.md` | Step-by-step deploy + DNS procedure |

## Source & integrity

- `index.html` is a byte-identical copy of the exported timeline (29,503 bytes).
- SHA-256: `2a81c25c2e9e6c89874ffcc1a3b6bbf8328fd82c412ce9062b6bc019463b6986`
- Title: `HF Incident — Interactive Timeline`
- Fully self-contained: all CSS/JS inline; only external reference is Google Fonts (graceful fallback). No missing assets.
