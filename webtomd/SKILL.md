---
name: webtomd
description: Scrapes a webpage and saves it as a clean Markdown file with frontmatter. Use when the user provides a URL and wants to scrape, save, archive, or convert a page to Markdown. Triggers on "scrape this", "save this page", "convert to markdown", "download docs from", or any URL with intent to save.
---

# Web to Markdown

Two modes:
- **fast** — `WebFetch`. Quick, may summarize. Best for articles and blog posts.
- **precise** — `urllib` + `markdownify`. Verbatim content. Best for technical docs and API references.

Mode is **auto-detected** in Step 2. User can override anytime by saying "use fast" or "use precise".

## Workflow

Copy and track progress:

```
[ ] 1. Fetch raw HTML + JS-render check
[ ] 2. Auto-detect mode
[ ] 3. Detect and filter nav links
[ ] 4. Ask user which pages to scrape
[ ] 5. Convert and save each page
[ ] 6. Print summary
```

---

### 1. Fetch raw HTML

Use `python3` + `urllib` with a browser User-Agent. Extract `<title>`, strip trailing `| site name`.

**JS-render check** — flag only if BOTH are true:
- Raw HTML < 2000 chars, OR `<body>` has < 3 block tags (`p`, `h1-h6`, `li`, `section`, `div`)
- AND content is clearly empty (not just a Next.js/Nuxt SSR wrapper)

If flagged, warn and offer:
> "⚠️ Appears JS-rendered. Options: (1) continue anyway (2) retry via Jina Reader — `https://r.jina.ai/<url>` (3) cancel"

If user picks Jina, replace URL and use fast mode. Skip nav detection.

---

### 2. Auto-detect mode

Using the URL and raw HTML already fetched, pick the mode silently then inform the user:

**Use `precise` if any of these match:**
- URL path contains `/docs/`, `/api/`, `/reference/`, `/guide/`, `/manual/`, `/sdk/`
- Page has high code density: more than 5 `<code>` or `<pre>` blocks in the HTML
- Domain is a known dev-docs site (e.g. `platform.claude.com`, `docs.`, `developer.`)

**Use `fast` otherwise** (blogs, marketing, articles, landing pages).

Inform the user:
> "Auto-selected **[mode]** mode based on [reason: URL pattern / code density / domain]. Reply 'fast' or 'precise' to override, or press Enter to continue."

Wait for reply. If Enter or no override, proceed.

---

### 3. Nav links

Try in order, stop at first hit. Links must be same-domain, non-anchor:
1. Inside `<nav>` or `<aside>`
2. Elements with class/id containing `sidebar`, `toc`, `menu`, `nav`
3. None found → scrape main URL only

Filter out: login, signup, home, about, contact, changelog, status, tags, external domains.

---

### 4. Ask user

If nav links found:
```
Found N related pages. Which to scrape?
1. Title — url
2. Title — url
Reply: "all", "none", or "1 3 5"
```
If "all" and N > 10: confirm — "That's N pages with ~1s delays between each. Proceed?"

---

### 5. Convert and save

For each URL — check if file exists first:
> "⚠️ `./scraped/file.md` exists. Overwrite? (yes / no / skip)"

**Rate limit:** 1–2s delay between requests when scraping multiple URLs.

**fast mode:**
Use `WebFetch`. If response says "Output too large... saved to: /path/file.txt", read it with `bash` (`cat`).

If result < 500 chars or has no headings and no lists, suggest:
> "⚠️ Fast mode returned thin content. Retry with precise? (yes / no)"

**precise mode:**
Reuse HTML from step 1 for the main URL. For nav pages, fetch fresh with `urllib`.
- Check `markdownify`: `python3 -c "import markdownify" 2>/dev/null || pip install markdownify -q --break-system-packages`
- Strip `<script>`, `<style>`, `<nav>`, `<footer>`, `<header>`, `<aside>`
- Isolate content: `<main>` → `<article>` → element with class/id `content|docs|prose` → largest `<div>` → `<body>`
- Convert with `markdownify` (ATX headings, tables, no images), clean blank lines

**Quality check (both modes):**
- No `#` heading → warn `⚠️ No headings`
- No blank lines → warn `⚠️ Single text block`
- Both warnings → prepend `> ⚠️ JS-rendered page — structure may be lost`

Save to `./scraped/<slug>.md`:
```markdown
---
title: "Page Title"
source: "https://..."
scraped: "YYYY-MM-DD"
mode: fast | precise
---
```

---

### 6. Summary

```
Done. Saved N file(s) [mode: fast|precise]:
  - ./scraped/page.md (4,200 chars)
  - ./scraped/other.md (skipped — already exists)
  - ./scraped/broken.md ⚠️ No headings
```

## Errors
- WebFetch fails or empty → skip, report
- markdownify install fails → regex tag-strip fallback, warn user
- Non-200 or timeout → skip, report
