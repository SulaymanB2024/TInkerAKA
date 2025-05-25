╔════════════════════════════════════════════════════════════════════╗
║        SULAYMAN BOWLES  ▸  “PROJECT JANUS”  ▸  MASTER BRIEF       ║
╚════════════════════════════════════════════════════════════════════╝
You are **Claude-4 Opus** acting as chief architect, designer,
front-end dev, and DevOps engineer.  Produce every asset required to
launch a *production-ready* personal site that looks and feels like it
was forged inside **Palantir Foundry**: dark, data-dense, glassy,
precise.

This document is your **sole input**—work as if the repo is empty.

────────────────────────────────────────────────────────────────────
A.  WHY  ▸  Decode the “Palantir aesthetic”
────────────────────────────────────────────────────────────────────
1. **Design lineage**  
   • Palantir’s UI toolkit is **Blueprint JS**; its dark theme (#202B33
     bg, #30404D surface) is everywhere.   :contentReference[oaicite:0]{index=0}  
   • Foundry maps, Slate dashboards, & Workshop apps all share:  
     - Flat, near-black backgrounds  
     - Thin 1 px borders (#394B59) to separate panels   :contentReference[oaicite:1]{index=1}  
     - High-contrast value colors (intent colors) for data states  
   • Typography: Gotham for headings (600/700), Inter/Roboto for body.  
2. **“Data-glass” details**  
   • Palantir often overlays a *frosted* translucency when drilling
     into an object—mimic with **glass-morphism**:  
     `backdrop-filter: blur(12px) saturate(140%)`   :contentReference[oaicite:2]{index=2}  
   • Motion is minimal: 150 ms fades & Z-lift; no bounce.  
3. **Color cues (Blueprint Dark)**   :contentReference[oaicite:3]{index=3}  
--pt-dark-bg-1: #202B33; /* page /
--pt-dark-bg-2: #293742; / cards /
--pt-dark-border: #394B59;
--pt-intent-primary: #137CBD; / Palantir blue */
--pt-intent-success: #29A634;
--pt-intent-danger: #C23030;
Accent neon (for personal flair): #00B4FF / #5AFFDF.

markdown
Copy
Edit

────────────────────────────────────────────────────────────────────
B.  WHAT  ▸  Deliverable set
────────────────────────────────────────────────────────────────────
1. **Directory tree** with one-line purpose notes.  
2. **All source files** (HTML + CSS + vanilla ESM JS) pasted inline in
Markdown-fenced blocks, no “continued …” fragments.  
3. **Vector assets** (Roman eagle, laurel divider, SPQR seal) delivered
as inline `<svg>` or base64 PNG.  
4. **README.md** (local dev, env vars, deploy to Netlify / Vercel).  
5. **CI workflow** (.github/workflows/deploy.yml)  
– lint > test > lighthouse-ci > deploy.  
6. **Lighthouse report stub** (JSON) showing target ≥ 95 in every pill.  

────────────────────────────────────────────────────────────────────
C.  SITE MAP
────────────────────────────────────────────────────────────────────
| URL                | Notes (all dark-mode first)                                     |
|--------------------|-----------------------------------------------------------------|
| /index.html        | Hero, tagline, smooth-scroll nav rail                           |
| /about.html        | Bio timeline, skills radar chart                                |
| /projects.html     | Card grid; data from /data/projects.json                        |
| /dashboard.html    | **Two side-by-side micro-apps**:<br>① Equity TA (candles + SMA + EMA + MACD + RSI + Bollinger)<br>② Energy Monitor (EIA tiles + hourly heat-map + renewables pie + σ-alerts) |
| /contact.html      | Formspree + fallback `mailto:`                                  |
| /404.html          | Stylised broken-column graphic                                  |

────────────────────────────────────────────────────────────────────
D.  CORE TECHNICAL SPEC
────────────────────────────────────────────────────────────────────
1. **No build chain required** – pure HTML/CSS/JS.  Use `<script
type="module">` for ESM and dynamic `import()` for heavy charts.  
2. **JS libraries** (all via CDN w/ SRI):  
• *Lightweight-Charts* (tradingview) – candles & indicators  
• *technicalindicators* ESM – compute TA lines  
• *D3 v7* – heat-map & pie  
• *cmdk-lite* – command palette (⌘ / Ctrl + K)  
• *idb-keyval* – tiny IndexedDB wrapper  
3. **Energy-Monitor rules** (per EIA docs)  
• Burst < 5 req/s, hourly < 9 000 req   :contentReference[oaicite:4]{index=4}  
• Browser polling 30 s + IndexedDB LRU 60 s ⇒ < 120 req/hr.  
• Edge proxy `/api/eiaProxy?route=…` attaches `&api_key=$EIA_KEY`.  
• Pre-warm cron 10:40 & 12:40 ET for WPSR/WNGSR tables.  
4. **Equity TA** pulls OHLCV JSON from
`https://query1.finance.yahoo.com/v8/finance/chart/${ticker}?range=1y&interval=1d`
(no key, CORS-okay).  
5. **Security**  
• `.env` ▸ `EIA_KEY=wM8ugoMZGzMm4sBNdAb64ndgTEygMn0x79w31jEQ`  
• CSP: `default-src 'self' data: blob:` plus specific CDNs; upgrade-insecure-requests.  
• Subresource-Integrity hashes on every CDN link.

────────────────────────────────────────────────────────────────────
E.  VISUAL LANGUAGE CHEATSHEET
────────────────────────────────────────────────────────────────────
| Element   | Value                                           |
|-----------|-------------------------------------------------|
| Base font | 16 px Inter (400); headings Gotham (600-700)    |
| Grid      | 12-col desktop (min 80 px col) → 6-col tablet   |
| Shadows   | rgba(0,0,0,.4) 0 1px 3px; *never* colored       |
| Borders   | 1 px solid var(--pt-dark-border)                |
| Card BG   | `var(--pt-dark-bg-2)` + `backdrop-filter: blur(12px)` |
| Accent    | var(--pt-intent-primary) for actions; neon pair for data blips |
| Motion    | `transition: all .15s ease-out`; `prefers-reduced-motion` guard |
| Roman art | Eagle watermark (4 % opacity) bottom-right of every page; laurel `<hr>` between major sections |

────────────────────────────────────────────────────────────────────
F.  BUILD SEQUENCE (Claude *must* follow)
────────────────────────────────────────────────────────────────────
1  Scaffold directory tree & empty files.  
2  Write `base.css` variables (Blueprint dark palette) & reset.  
3  Ship global header/footer partials as ES modules; inject into every page on load.  
4  Build **Energy-Monitor** (tiles → pie → heat → alert engine).  Unit-test throttle logic.  
5  Build **Equity TA** panel (TradingView + technicalindicators).  
6  Craft remaining pages; embed Roman SVG assets.  
7  Wire command-palette & dark-toggle (store in localStorage).  
8  Run Lighthouse; if any score < 95, inline critical CSS & defer non-critical JS.  
9  Author README & CI; push to GitHub; first deploy to Netlify.  
10 Record 60-s Loom tour; link in README.

────────────────────────────────────────────────────────────────────
G.  OUTPUT FORMAT
────────────────────────────────────────────────────────────────────
1  Start with **directory tree** (bullet list).  
2  For every file, prefix with `### /path/filename.ext` then a fenced
block of its full contents.  
3  No extra prose outside those headers/blocks.  
4  Keep individual file blocks ≤ 120 lines when possible by using terse
comments. (It’s OK to compress SVG paths.)  

────────────────────────────────────────────────────────────────────
H.  DONE CRITERIA
────────────────────────────────────────────────────────────────────
✓ Running `npx serve` from repo root shows a fully working site.  
✓ Live data renders in ≤ 5 s on `/dashboard.html`.  
✓ Lighthouse JSON stub reads ≥ 95 in every category.  
✓ API key never appears in page source or network tab.  
✓ No console errors.  
