# Paintball Arena Bern — Homepage Clone (Chat Widget Review Site)

A **static, single-page replica** of the [paintballarena-bern.ch](https://paintballarena-bern.ch)
homepage, built only to let the client review the **PBA chat widget** in a realistic
context before it goes on the live WordPress site.

- **Not the live site.** A visible staging banner and `noindex,nofollow` keep it out of
  search and make its purpose obvious. Marketing content © Paintball Arena Bern; images
  are referenced from the client's own CDN.
- **Tech:** plain HTML + CSS. No build step, no dependencies — Vercel serves it as-is.

## The widget

The chat widget is embedded at the bottom of `index.html`:

```html
<script src="https://api-v2.bernarenaapps.net/rest/widget.js"
        data-api="https://api-v2.bernarenaapps.net"
        data-lang="de"
        defer></script>
```

It loads from and talks to the **production** chatbot backend
(`https://api-v2.bernarenaapps.net`). Leads are flagged as test until the backend's
`chatbot.lead.test-mode` is turned off, so nothing here reaches the real customer-service
queue as a genuine enquiry.

## Deploy (Vercel)

Import the repo in Vercel — it auto-detects a static site (no framework, output = repo
root). No environment variables or build command needed.

## ⚠️ After deploying: whitelist the origin

The widget's browser calls to `/rest/chat/stream` are CORS-checked by the backend. The
Vercel origin (e.g. `https://<project>.vercel.app`) must be added to
`ChatController.@CrossOrigin` in the API repo, then the API redeployed — otherwise the
widget loads but every chat turn is blocked by the browser. Send the deployed URL over to
have it added.

## Local preview

```bash
npx serve .      # or: python3 -m http.server 8080
```
