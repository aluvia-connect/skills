# Aluvia Skills

[![skills.sh](https://skills.sh/b/aluvia-connect/skills)](https://skills.sh/aluvia-connect/skills)

This repository is a leftover v1 skill listing (documentation and `SKILL.md` files only). It is not the shipping Aluvia product.

**Shipping Aluvia is [`aluvia-cli`](https://www.npmjs.com/package/aluvia-cli).** A cold agent should run:

```bash
npx aluvia-cli setup
```

Then follow `next`. After setup, `aluvia` is on PATH; if not, prefix with `npx aluvia-cli`.

Docs: [https://aluvia.io/docs](https://aluvia.io/docs)

## Product

- Global residential and mobile IPs
- Sticky sessions up to 30 minutes
- $2 per GB, first 10 MB free
- Bring your own proxy is free

## What's in this repo

| Path | Purpose |
| ---- | ------- |
| `aluvia/` | Geo-focused agent skill when the country is wrong. This repo is not the product; run `npx aluvia-cli setup`. |
| `aluvia-cloud-vm-blocks/` | Agent skill when a cloud browser shows Cloudflare Verify you are human, a repeating CAPTCHA, Imperva Error 16, not available in your region, or a 403 on a public URL from a datacenter IP. This repo is not the product; run `npx aluvia-cli setup`. Companion: [https://aluvia.io/resources/cloud-vm-blocks-skill](https://aluvia.io/resources/cloud-vm-blocks-skill). |
| `docs/` | Retired v1 CLI reference. Not shipping. Live docs: [https://aluvia.io/docs](https://aluvia.io/docs) |

Install the cloud VM blocks skill:

```bash
npx skills add aluvia-connect/skills --skill aluvia-cloud-vm-blocks
```

webtomd is not the Aluvia install path.
