# tycewebsite-star.github.io

Personal landing page for **Tim Yilmaz**, Mortgage Agent (Level 1) with Tyce Group,
serving downtown Toronto and clients across Ontario.

**Live site:** https://tycewebsite-star.github.io

---

## Overview

A single-page static website — no build step, no dependencies, no framework.
Everything is contained in one HTML file with inline CSS and a small amount of
vanilla JavaScript, which keeps hosting on GitHub Pages effortless and page
loads near-instant.

### Sections

| Section | Purpose |
|---|---|
| Hero | Headline, portrait, and primary call-to-action (email / phone) |
| Ledger strip | At-a-glance stats — years of experience and market served |
| About | Background, career history, and licence credentials |
| How It Works | Three-step client process, from first contact to closing |
| Contact | Direct contact details, social links, and an enquiry form |

---

## Local development

No tooling required. Serve the directory over HTTP:

```
python3 -m http.server 8000
```

Then open http://localhost:8000. Opening `index.html` via `file://` also works,
but HTTP is closer to production behaviour.

---

## Contact form

The form posts to [FormSubmit](https://formsubmit.co), a free relay that forwards
submissions as email. GitHub Pages serves static files only and cannot send mail
itself, so a relay of some kind is required.

Fields: first name, last name, email, phone, enquiry type, and message.

### Configuration

Hidden fields on the form control its behaviour:

| Field | Purpose |
|---|---|
| `_subject` | Subject line of the forwarded email |
| `_template` | `table` — formats the email as a readable table |
| `_captcha` | `false` — skips the interstitial captcha page |
| `_honey` | Hidden honeypot; bots that fill it are silently dropped |
| `_next` | Redirect target after a successful send |

`_next` is hardcoded to the production URL as a fallback, then overwritten by
JavaScript from `location.origin + location.pathname` at page load — so it
points at localhost during development and at the live site in production
without any manual edits. On return the script detects `?sent=1`, swaps the form
for a confirmation panel, and strips the query parameter from the URL.

If JavaScript is disabled the hardcoded production URL is used, so visitors
still return to the site. Only the localhost override is lost.

> **Note:** if `_next` is ever submitted empty, FormSubmit silently redirects to
> its own page instead of erroring. When testing locally, hard-reload
> (`Cmd+Shift+R`) after editing — `http.server` sends no cache headers, so a
> stale page will post without the field and land you on formsubmit.co.

### Before deploying

- **The form currently points at a test address.** Change the `action` URL on the
  `<form>` in `index.html` to Tim's real address. It is marked `TESTING ONLY`.
- **Each address needs one-time activation.** The first submission to a new
  address is consumed by FormSubmit's confirmation step and is *not* forwarded —
  click the activation link it emails, then submit again to verify.
- Consider swapping the plain address for FormSubmit's random alias endpoint so
  it is not exposed in public page source.
- **Social links are placeholders.** Instagram and Facebook use `href="#"`. Add
  the real URLs along with `target="_blank" rel="noopener"`.
- If spam becomes a problem, set `_captcha` to `true`.

---

## Contact

Tim Yilmaz — Mortgage Agent, Level 1
Tyce Group · Toronto, ON
416.508.6002 · tim.yilmaz@vinegroup.ca
ON Licence 13511
