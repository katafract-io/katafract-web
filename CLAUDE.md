# Katafract Website — Claude Instructions

## Project Overview
Static website for Katafract LLC, an independent iOS app studio. Hosted at katafract.com,
served by nginx from `/var/www/katafract.com/public_html/`.

## Directory Structure
```
public_html/
├── index.html          # Main landing page (standalone styles, accordion app cards, Enclave section)
├── about.html          # Studio overview
├── pricing.html        # Enclave pricing (Haven, NetArmor, Apps one-time)
├── enclave.html        # Enclave platform product page (Haven + NetArmor)
├── dns.html            # Haven DNS free setup guide
├── canary.html         # Warrant canary — updated quarterly
├── privacy.html        # Privacy hub (links to per-app policies)
├── support.html        # Support hub (links to per-app support)
├── terms.html          # Terms hub (links to per-app terms)
├── style.css           # Shared stylesheet for all non-index pages
├── katafract-mark.svg  # Shield logo (navbar brand mark)
├── favicon.svg
├── apps/
│   ├── exifarmor.html  # ExifArmor product page
│   ├── parkarmor.html  # ParkArmor product page
│   └── docarmor.html   # DocArmor product page
├── privacy/
│   ├── exifarmor.html
│   ├── parkarmor.html
│   └── docarmor.html
├── terms/
│   ├── exifarmor.html
│   ├── parkarmor.html
│   └── docarmor.html
└── support/
    ├── exifarmor.html
    ├── parkarmor.html
    └── docarmor.html
```

## Apps
- **ExifArmor** — Photo metadata stripping. Color: `#00F0FF` (cyan). Live on App Store.
- **ParkArmor** — Parking spot saver. Color: `#4ADE80` (green). Coming soon.
- **DocArmor** — Encrypted document vault. Color: `#E879F9` (purple). Planned.

## Enclave Platform (Network Services)
- **Haven** — Private DNS (AdGuard-based). Color: `#4ADE80` (green). Pricing: $3.99/mo or $31.99/yr. Free tier available.
- **NetArmor** — WireGuard VPN app for iOS. Color: `#FF006E` (magenta). Pricing: $9.99/mo or $79.99/yr (includes Haven).
- **Enclave** — Bundle brand for Haven + NetArmor together.

Stripe payment link placeholders in `pricing.html` use `https://buy.stripe.com/HAVEN_MONTHLY` etc. — replace with real links when Stripe products are created.

## URL Structure
nginx serves clean URLs via `try_files $uri $uri.html $uri/ =404`.
Canonical URLs use no `.html` extension (e.g. `https://katafract.com/terms/exifarmor`).

Correct internal link format: `/privacy/exifarmor.html`, `/terms/parkarmor.html`, etc.
(`.html` extension required in `href` attributes even though URLs are clean in browser).

## Style Conventions
- `index.html` has its own self-contained `<style>` block; it does **not** use `style.css`
- All other pages load `/style.css` with `<link rel="stylesheet" href="/style.css">`
- Subdirectory pages must use **absolute paths** (`/style.css`, `/katafract-mark.svg`, etc.)
- Root-level pages (`about.html`, `privacy.html`, etc.) may use relative paths
- Brand colors: cyan `#00F0FF`, green `#4ADE80`, purple `#E879F9`, dim text `#8C94A6`
- Fonts: Cormorant Garamond (headings), DM Sans (body), JetBrains Mono (labels/mono)
- Dark background: `#05050F`

## Nav Pattern
All pages use `<nav class="site-nav">` except `index.html` which uses `<nav>` with inline styles.
Per-app pages (under `apps/`, `privacy/`, `terms/`, `support/`) link their nav Support and Privacy
items to the **per-app** versions (e.g. `/support/exifarmor.html`), not the hub pages.

```html
<nav class="site-nav">
  <a href="/" class="nav-brand">
    <img src="/katafract-mark.svg" class="nav-mark" alt="">
    <span class="nav-brand-name">Katafract</span>
  </a>
  <ul class="nav-links">
    <li><a href="/">Home</a></li>
    <li class="hide-sm"><a href="/support/[app].html">Support</a></li>
    <li><a href="/privacy/[app].html">Privacy</a></li>
    <li><a href="/about.html">About</a></li>
  </ul>
</nav>
```

## Footer Pattern
All pages share an identical footer structure with absolute links:
```html
<footer class="site-footer">
  <div class="footer-inner">
    <div class="footer-brand">
      <img src="/katafract-mark.svg" class="nav-mark" alt=""> Katafract LLC
    </div>
    <nav class="footer-nav">
      <a href="/privacy.html">Privacy Policies</a>
      <a href="/terms.html">Terms of Service</a>
      <a href="/support.html">Support</a>
      <a href="/about.html">About</a>
      <a href="mailto:hello@katafract.com">Contact</a>
    </nav>
    <p class="footer-motto">Teneo &middot; Effrenus &middot; Katafractarius</p>
    <p class="footer-copy">&copy; 2026 Katafract LLC. All rights reserved.</p>
  </div>
</footer>
```

## Canonical URL / Meta Tag Pattern
Per-app pages must use the **subdirectory** canonical form:
- Correct: `https://katafract.com/terms/exifarmor`
- Wrong: `https://katafract.com/terms-exifarmor` (old flat format — do not use)

## What NOT to Do
- Do not use relative paths in subdirectory pages (will break CSS/assets)
- Do not modify `index.html`'s inline `<style>` block to reference `style.css` — it is intentionally standalone
- Do not add iCloud sync, cloud storage, accounts, analytics, or network calls to any **iOS app** description
- Do not change the "Data Not Collected" Apple Privacy Nutrition Label claims — they must remain accurate
- iOS apps (ExifArmor, ParkArmor, DocArmor) are one-time $2.99 purchases — do not add subscription pricing to them
- Enclave network services (Haven, NetArmor) ARE subscription-based — pricing is established above
- Do not change canary.html dates without verifying the actual current date and next-due date

## Deployment
Files are served directly from `/var/www/katafract.com/public_html/` on the production server.
To deploy: push to `main` on `katafractured/website`, then on the server run:
```bash
cd /var/www/katafract.com/public_html && git pull origin main
```
Or set up a post-receive hook / webhook for automatic deploys.
