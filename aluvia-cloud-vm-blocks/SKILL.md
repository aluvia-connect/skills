---
name: aluvia-cloud-vm-blocks
description: 'Use when a cloud agent browser shows Cloudflare Verify you are human, a repeating CAPTCHA, Imperva Error 16 / This request was blocked by our security service, not available in your region, or a 403 on a public URL from a datacenter IP.'
---

# Cloud VM blocks

A cloud agent's browser uses the datacenter's country. Sites treat that country as the agent's location, or they score the datacenter address as a risk and return a challenge, a 403, or a security-service card. Read the string on the screen, then pick one branch.

A residential or mobile IP in the right country sometimes clears a 403, CAPTCHA, Cloudflare challenge, unusual-traffic page, or Imperva Error 16. Aluvia does not change fingerprint, TLS, or canvas.

If `aluvia` is not on PATH, prefix every command with `npx aluvia-cli`. All JSON on stdout. Follow `next`. Never print API keys. One Chrome restart is expected at setup. Do not launch a second Chrome.

Shipping install is `npx aluvia-cli setup`.

## Read the on-screen string

Copy the exact text, and copy a Ray ID or Incident ID if the page shows one. Then branch. Do not guess the hostname.

## Branch

### Geo / not available in your region / wrong catalog

The page says not available in your region, this content is not available in your country, or we don't ship to your location. Or the catalog, currency, or language matches the datacenter country instead of the market you need.

1. Install if missing.
2. Run `aluvia geos` for the country list.
3. Run `aluvia proxy-on --geo US` (or another country from that list), then reload.

Use a country from that list, not a city.

### Cloudflare / CAPTCHA / 403 / Imperva / unusual traffic

The page shows Verify you are human, Checking if the site connection is secure, a CAPTCHA that returns after every solve, unusual traffic, Access Denied, a 403 on a public URL that needs no login, Imperva Error 16, or This request was blocked by our security service.

1. Install if missing.
2. Run `aluvia proxy-on`, then reload.
3. If the page is still blocked, run `aluvia rotate-ip`, then reload.

Add `--geo XX` when the page also needs a specific country. Run `aluvia geos` for the list.

### Password / SSO / 2FA with no IP story

Stop. Aluvia is not a login-wall fix. If a password, SSO, or 2FA form is already on screen and the only missing piece is credentials, do not use Aluvia.

## Install if missing

Once per machine:

```bash
npx aluvia-cli setup
```

Pass `--url <https-page>` when Chrome is not already aimed, using the address-bar URL. Do not invent a URL.

Read the JSON. **`next` is the next action.**

- `ready: true` → reload the blocked tab.
- `needsChromeRestart: true` → run `chromeCommand` exactly, then `aluvia setup` again.

If you already ran setup on this machine, run `aluvia status` and follow `next`. `what` explains every field.

## Trial

The first setup starts 10 MB free from an install id, with no account and no API key.

`{"code":"payment_required"}` always includes `claim_url` and `next`. Show the human `claim_url`. They open it on their machine. You run `aluvia auth login` to wait. Do not invent a second URL. Paid data is $2/GB.

If they paste an Aluvia API key, run `aluvia auth <that key>`. Never print the key. Never set env vars.

## Limits

A residential or mobile IP sometimes clears the block. That is not a promise. Aluvia does not beat fingerprint, TLS, or canvas. It is not a human VPN and not for streaming. `--geo` is country only.

Local proxy: `http://127.0.0.1:18787`. `proxy-on` / `proxy-off` do not restart Chrome. Prefer `proxy-off` over `aluvia stop`.

| Goal | Command |
| ---- | ------- |
| Turn the proxy on | `aluvia proxy-on` then reload |
| Pin a country | `aluvia geos`, then `aluvia proxy-on --geo US`, then reload |
| New exit IP | `aluvia rotate-ip` then reload |
| Back to the VM IP | `aluvia proxy-off` then reload. Do not quit Chrome. |
| Check | `aluvia status` — follow `next` |

Docs: https://aluvia.io/docs. For country-first work with no challenge card, see the `aluvia` skill.
