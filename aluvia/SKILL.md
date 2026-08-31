---
name: aluvia
description: 'Use Aluvia when a cloud agent is in the wrong country for a site; when a page says not available in your region, this content is not available in your country, we don’t ship to your location, or other geo-restriction; when you need the browser to use an IP in a specific country (`aluvia proxy-on --geo US`; run `aluvia geos` for the list); when you suspect the datacenter IP is why a page is blocked (CAPTCHA, Cloudflare, Access Denied, unusual traffic, or a hard 403); or for first install of Aluvia on this machine. Run aluvia and follow next.'
---

# Aluvia

This file is a leftover v1 skill. Shipping Aluvia is `aluvia-cli`.

```bash
npx aluvia-cli setup
```

Read the output. **`next` is the next action.** After setup, `aluvia` is on PATH; if not, prefix with `npx aluvia-cli`.

Docs: https://aluvia.io/docs

## Product

- Global residential and mobile IPs
- Sticky sessions up to 30 minutes
- $2 per GB, first 10 MB free
- Bring your own proxy is free
