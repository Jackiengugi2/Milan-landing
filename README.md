# Milan Academies — Admissions Landing Page

Static landing page (`index.html`) and post-enquiry page (`thank-you.html`) for the
Milan Academies admissions campaign. No build step; deploys to Netlify as-is.

```
index.html          landing page + enquiry form
thank-you.html      shown after a successful enquiry (fires the Meta Pixel Lead event)
assets/landing.css  shared styles (palette tokens live in each page's <style> block)
assets/landing.js   form validation, Netlify submission, WhatsApp hand-off, Pixel, UTM capture
images/             photos: see images/README.md
netlify.toml        publish settings
```

## Deploying on Netlify

1. Netlify → **Add new site → Import an existing project → GitHub** → pick this repo.
2. Branch: the branch holding this code. Build command: *(empty)*. Publish directory: `.`
   (both already set in `netlify.toml`).
3. **Site configuration → Forms → Enable form detection**, then **Deploys → Trigger deploy**
   so Netlify registers the form. Netlify only scans forms on deploys made after detection is on.
4. Check **Forms** shows `milan-admissions`. Under **Forms → Form notifications**, add an email
   notification so admissions gets each enquiry in their inbox.
5. Add the custom domain under **Domain management**.

## Form

The form is a Netlify form named `milan-admissions` (`data-netlify="true"`, honeypot field `website`).
JavaScript validates it, posts it to Netlify, then sends the parent to `thank-you.html`, where
the WhatsApp button is pre-filled with their enquiry. Without JavaScript the form still posts
straight to Netlify and lands on `/thank-you.html`.

Each submission also records `campaign`, `page_url` and any `utm_*` / `fbclid` parameters
from the ad link.

## Settings (top of `index.html`, `window.LANDING`)

| Key | Purpose |
|---|---|
| `metaPixelId` | Meta Pixel ID. Set it in **both** `index.html` and `thank-you.html`. |
| `formEndpoint` | Optional second destination (Google Sheet / Privyr webhook). Netlify already stores every enquiry. |
| `whatsapp`, `waIntro` | Number and opening line for the WhatsApp hand-off. |
| `campaign` | Campaign code saved with every enquiry. |

The palette (crimson, gold, cream) and `images/logo.png` were taken from the school's
Mashujaa Day poster. The Mashujaa Day section in `index.html` is seasonal; delete the
`<!-- MASHUJAA DAY -->` section after October. The school's name is
**Milan Academies**.

Local preview: `python3 -m http.server 8080` (form submissions only work once deployed on Netlify).
