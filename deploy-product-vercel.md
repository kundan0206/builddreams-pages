# Deploy a Product Showcase Page to Vercel and Map a Subdomain

This guide explains how to deploy a single product showcase folder (for example, `products/flexi-rag`) to Vercel and map it to a custom subdomain (for example, `flexirag.builddreams.com`). It includes prep, Git steps, Vercel Web + CLI flows, DNS/subdomain setup, common pitfalls and verification steps.

1. Overview

---

- Goal: host one product page (static HTML/CSS/JS) independently on Vercel and attach a custom subdomain.
- Two recommended flows:
  - Web UI: Import repo into Vercel and set the project's Root Directory to the product folder.
  - CLI: Use `vercel` to deploy a specific folder directly.

1. Prerequisites

---

- You control the DNS for the parent domain (e.g., `builddreams.com`).
- The repository contains the product folder (example: `products/flexi-rag`).
- You have a Vercel account and access to the Git provider (GitHub/GitLab) used by the repo.
- Optional: `vercel` CLI (`npm i -g vercel`) for CLI deploys.

1. Make the product folder self-contained (recommended)

---

Why: Vercel serves a project from its root. Make sure the product folder contains everything the page needs and uses relative paths.

- Recommended folder layout:

  products/flexi-rag/
  - index.html # rename the page to index.html if possible
  - assets/
    - hero.png
    - 1.png
    - builddreams.png # local copy of company logo if referenced

1. Optional `vercel.json` (simple static project)

---

Place this in `products/flexi-rag/vercel.json` if you need explicit rewrites or project-level config.

```json
{
  "rewrites": [{ "source": "/", "destination": "/index.html" }]
}
```

1. Deploying

Vercel CLI (fast, advanced)

```bash
npx vercel login
# from repo root:
npx vercel --prod ./products/flexi-rag
```

```bash
# to re-deploy
cd /products/<product-name>
npx vercel --prod
```

The CLI will ask whether to link to an existing project or create a new one; answer as appropriate.

1. Map a custom subdomain (example: `flexirag.builddreams.com`)

---

1. In Vercel Dashboard → Projects → select your project → Settings → Domains → Add.
2. Enter the subdomain `flexirag.builddreams.com` and click Add.
3. Vercel will show DNS records to add (usually a CNAME for subdomains). Example instruction:
   - Type: CNAME
   - Name/Host: `flexirag`
   - Value: `cname.vercel-dns.com` (or the exact value Vercel shows you)
4. Add the provided record at your DNS provider (Cloudflare, Route53, Namecheap, etc.).
5. Wait for Vercel to verify the domain; once verified Vercel will provision TLS automatically.

CLI alternative to add the domain:

```bash
# run from the repo directory and follow prompts
npx vercel domains add <project-name> flexirag.builddreams.com
```

1. Quick checklist before requesting DNS changes

---

- [ ] `products/<slug>/index.html` exists and references `assets/*` relatively
- [ ] All images referenced are present in `products/<slug>/assets/`
- [ ] `git push` of the feature branch is complete and accessible to Vercel
- [ ] Vercel project root is set to `products/<slug>` (if using dashboard import)

1. Example: small README to include inside the product folder

---

Add `products/flexi-rag/README.md` so future contributors can deploy in one step:Add `products/flexi-rag/README.md` so future contributors can deploy in one step:

## Deploying this product to Vercel

1. Ensure assets are in `assets/` and `index.html` is present.
2. From repo root run:

```bash
git checkout -b feat/deploy-flexi-rag
git add products/flexi-rag
git commit -m "prepare flexi-rag for Vercel"
git push origin feat/deploy-flexi-rag
```
