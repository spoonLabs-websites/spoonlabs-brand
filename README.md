# @spoonlabs/brand

Shared brand system for the spoonLabs AI ecosystem. Design tokens, color scales, SVG logos, and site meta templates — all in one package.

---

## Install

```bash
npm install @spoonlabs/brand
# or
pnpm add @spoonlabs/brand
```

Or reference files directly via CDN / local path if not using npm.

---

## What's included

```
colors.css          CSS custom properties (light + dark mode)
tokens.json         Design tokens in Style Dictionary format
logos/
  wordmark.svg      Full "spoonLabs AI" horizontal wordmark
  icon.svg          Standalone spoon icon (32x32, for general use)
  favicon.svg       SVG favicon (32x32, minimal — drop straight into <head>)
meta/
  llms-template.txt Template for llms.txt per site
  og-template.html  OG social preview image generator template
```

---

## Usage

### CSS custom properties

```html
<link rel="stylesheet" href="node_modules/@spoonlabs/brand/colors.css">
```

```css
/* Or with a bundler */
@import '@spoonlabs/brand/colors.css';
```

```css
/* Then use in your styles */
.button {
  background: var(--color-primary);
  color: var(--color-primary-fg);
  font-family: var(--font-mono);
  border-radius: var(--radius-md);
  padding: var(--space-3) var(--space-6);
  transition: background var(--transition-base);
}

.button:hover {
  background: var(--color-primary-hover);
  box-shadow: var(--shadow-purple-md);
}
```

Dark mode is automatic via `prefers-color-scheme: dark`. To control it manually:

```html
<!-- Force dark -->
<html class="dark">

<!-- Force light -->
<html class="light">
```

---

### Design tokens (tokens.json)

Tokens follow the [W3C Design Token Community Group](https://design-tokens.github.io/community-group/format/) draft format and are compatible with Style Dictionary.

```js
import tokens from '@spoonlabs/brand/tokens.json';

const primary = tokens.color.purple[500].$value;  // "#534AB7"
const fontMono = tokens.typography.fontFamily.mono.$value;
const spacer4  = tokens.spacing[4].$value;         // "1rem"
```

**Style Dictionary config example:**

```js
// sd.config.js
module.exports = {
  source: ['node_modules/@spoonlabs/brand/tokens.json'],
  platforms: {
    css: {
      transformGroup: 'css',
      buildPath: 'src/styles/',
      files: [{ destination: 'tokens.css', format: 'css/variables' }]
    },
    js: {
      transformGroup: 'js',
      buildPath: 'src/',
      files: [{ destination: 'tokens.js', format: 'javascript/module' }]
    }
  }
};
```

---

### Logos

```html
<!-- Wordmark (use on dark backgrounds by default) -->
<img src="node_modules/@spoonlabs/brand/logos/wordmark.svg" alt="spoonLabs AI" height="40">

<!-- Icon -->
<img src="node_modules/@spoonlabs/brand/logos/icon.svg" alt="spoonLabs AI" width="32" height="32">

<!-- SVG favicon in <head> -->
<link rel="icon" type="image/svg+xml" href="/logos/favicon.svg">
```

Both SVGs are self-contained — no external font or image dependencies required for rendering.

---

### OG Image template

Open `meta/og-template.html`, replace the three placeholder variables, then render with a headless browser tool (Puppeteer, Playwright, or satori):

| Variable | Replace with |
|---|---|
| `[SITE_NAME]` | e.g. `spoonLabs Cards` |
| `[TAGLINE]` | e.g. `Trade. Collect. Play.` |
| `[DOMAIN]` | e.g. `spoonlabs.cards` |
| `[ACCENT_COLOR]` | Optional hex override, e.g. `#1D9E75` |

**Puppeteer render script:**

```js
import puppeteer from 'puppeteer';
import path from 'path';

const browser = await puppeteer.launch();
const page = await browser.newPage();
await page.setViewport({ width: 1200, height: 630, deviceScaleFactor: 2 });
await page.goto(`file://${path.resolve('meta/og-template.html')}`);
await page.screenshot({ path: 'og-image.png', type: 'png' });
await browser.close();
```

---

### llms.txt template

Copy `meta/llms-template.txt` to the root of each site as `llms.txt`, then fill in the bracketed fields:

```
[SITE NAME]     → The site's display name
[ONE LINE DESCRIPTION] → A single sentence about the site
[DOMAIN]        → The site's domain
[PURPOSE]       → What the site does
[STACK]         → Tech stack (e.g. "Astro, Cloudflare Pages, D1")
[CONTACT METHOD] → e.g. "daniel@spoonlabs.ai"
```

---

## Color tokens reference

### Primitive scales

| Token | Value | Notes |
|---|---|---|
| `--color-purple-500` | `#534AB7` | Brand primary |
| `--color-teal-500` | `#1D9E75` | Brand accent |
| `--color-gray-500` | `#5F5E5A` | Neutral / warm gray |
| `--color-dark-bg` | `#0D0C0F` | Dark background |
| `--color-gray-50` | `#F8F7F5` | Light background |

Each scale (purple, teal, gray) runs `50` → `900`.

### Semantic tokens

| Token | Light value | Dark value |
|---|---|---|
| `--color-bg` | `gray-50` | `dark-bg` |
| `--color-surface` | `#FFFFFF` | `#161519` |
| `--color-text` | `gray-900` | `gray-50` |
| `--color-text-muted` | `gray-500` | `gray-400` |
| `--color-border` | `gray-200` | `rgba(255,255,255,0.1)` |
| `--color-primary` | `purple-500` | `purple-400` |
| `--color-accent` | `teal-500` | `teal-400` |

### Typography tokens

| Token | Value |
|---|---|
| `--font-mono` | `'Space Mono', 'JetBrains Mono', monospace` |
| `--font-body` | `'DM Sans', 'Inter', system-ui, sans-serif` |
| `--font-display` | `'DM Sans', 'Inter', system-ui, sans-serif` |

Font sizes: `--font-size-xs` (12px) through `--font-size-6xl` (60px).

### Spacing scale

Base-4 system. `--space-1` = 4px, `--space-4` = 16px, `--space-16` = 64px.

Full scale: `px`, `0`, `0-5`, `1`, `1-5`, `2`, `2-5`, `3`, `3-5`, `4`, `5`, `6`, `7`, `8`, `9`, `10`, `11`, `12`, `14`, `16`, `20`, `24`, `32`.

### Border radius

`--radius-none` → `--radius-sm` → `--radius-base` → `--radius-md` → `--radius-lg` → `--radius-xl` → `--radius-2xl` → `--radius-3xl` → `--radius-full`

### Shadows

Standard shadow scale: `--shadow-sm` through `--shadow-2xl`, plus `--shadow-inner`.

Purple glow set for interactive states:
- `--shadow-purple-sm` — focus ring (`0 0 0 3px rgba(83,74,183,0.2)`)
- `--shadow-purple-md` — hover glow
- `--shadow-purple-lg` — active/prominent glow

### Transitions

- `--transition-fast`: `150ms ease`
- `--transition-base`: `200ms ease`
- `--transition-slow`: `300ms ease`
- `--ease-spring`: `cubic-bezier(0.175, 0.885, 0.32, 1.275)`

---

## Brand guidelines (summary)

- **Name:** spoonLabs AI — lowercase "spoon", capital "L" in Labs, "AI" as abbreviation
- **Primary:** `#534AB7` — deep purple. Use for CTAs, active states, key UI elements.
- **Accent:** `#1D9E75` — teal. Use for success states, highlights, the "AI" badge.
- **Neutral:** `#5F5E5A` — warm gray. Use for muted text, borders, subtle UI.
- **Dark bg:** `#0D0C0F` — near-black with a purple undertone. Canonical dark background.
- **Light bg:** `#F8F7F5` — warm white. Canonical light background.
- **Fonts:** Space Mono for code/UI labels/wordmark. DM Sans for body and display.
- **Vibe:** Technical but approachable. Dev-founder energy. Not corporate.

---

## Ecosystem

| Site | Purpose |
|---|---|
| spoonlabs.ai | Brand hub |
| spoonlabs.cards | Card trading & collectibles |
| spoonlabs.app | Live demos & tools |
| spoonlabs.shop | Card storefront |
| spoonlabs.store | Digital products |
| spoonlabs.info | Case studies & docs |
| connecteduniverse.ai | AI agent services |

---

## License

MIT — spoonLabs AI
