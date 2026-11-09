# Concept Wealth Woodhill — Design System Brief
> Drag this file into any new Claude project or chat to instantly apply the same style.

---

## Core stack

- **Font:** Plus Jakarta Sans (Google Fonts) — weights 400/500/600/700/800
- **Shared stylesheet:** `css/glass.css` — all tools link to this one file; page-specific CSS goes in a small inline `<style>` block only
- **Charts:** Chart.js 4.4.x (CDN)
- **Data persistence:** Firebase Firestore + `js/cw-profile.js` (saves per-calculator results to localStorage + Firestore)

---

## Page shell (every tool uses this structure)

```
brand-strip        dark slate (#404f62) bar — hexagonal constellation logo + "Authorised by SANLAM" blue pill
breadcrumb         frosted glass bar — ← All Tools  ›  Current Page Name
calc-hdr           frosted glass header — eyebrow tag + h1 + subtitle + icon PNG (top right)
page-bg            padding: 24px 24px 40px
  content-wrap     max-width: 820px; margin: 0 auto
    glass-card     main calculator inputs + results
    glass-card     secondary explainer / comparison card (if needed)
footer             frosted, border-top, disclaimer + "Concept Wealth Woodhill · SARS 2026/27"
```

---

## Colour tokens

```css
--page-bg:      #f5f7ff
--blue:         #3a78c9    /* primary accent — CTAs, highlighted values, links */
--blue-2:       #5b8fd9    /* lighter accent */
--indigo:       #5b5fc7    /* gradient partner to blue */
--slate:        #404f62    /* brand strip / dark nav */
--green:        #16a34a    /* positive values, gains */
--teal:         #0e8a6e    /* growth alternative */
--red:          #dc2626    /* tax paid, losses, negative */
--amber:        #f59e0b    /* warnings */
--text:         #0f172a    /* primary text */
--text-2:       #475569    /* secondary / body */
--text-3:       #94a3b8    /* hints / muted labels */
--glass:        rgba(255,255,255,0.72)
--glass-heavy:  rgba(255,255,255,0.88)   /* card backgrounds */
--glass-border: rgba(255,255,255,0.82)
--glass-shadow: 0 8px 32px rgba(58,120,201,0.10), 0 1px 4px rgba(58,120,201,0.06)
--blur:         blur(20px)
--border:       rgba(100,116,139,0.14)
--border-s:     rgba(100,116,139,0.08)
```

**Page background (fixed radial gradients):**
```css
background: #f5f7ff;
background-image:
  radial-gradient(ellipse 80% 60% at 15% 20%, rgba(91,95,199,0.08) 0%, transparent 70%),
  radial-gradient(ellipse 60% 50% at 85% 70%, rgba(58,120,201,0.07) 0%, transparent 70%),
  radial-gradient(ellipse 50% 50% at 50% 0%,  rgba(14,138,110,0.04) 0%, transparent 60%);
background-attachment: fixed;
```

---

## Typography

| Element | Size | Weight | Other |
|---------|------|--------|-------|
| Form labels | 11px | 700 | UPPERCASE, letter-spacing 0.06em |
| Hint / caption | 12px | 400 | `--text-3` |
| Body / description | 13–14px | 400–500 | `--text-2` |
| Card title | 15–16px | 700 | `--text` |
| Section title | 17–18px | 700–800 | `--text` |
| Hero result | 24–28px | 700–800 | `--blue` |
| Brand name (strip) | 13px | 700 | white |
| Eyebrow tag | 9px | 700 | UPPERCASE, `--blue`, letter-spacing 0.1em |

All numeric outputs: `font-variant-numeric: tabular-nums`

---

## Glass card

```css
background: var(--glass-heavy);          /* rgba(255,255,255,0.88) */
backdrop-filter: blur(20px);
-webkit-backdrop-filter: blur(20px);
border: 1px solid var(--glass-border);
border-radius: 18px;
box-shadow: var(--glass-shadow);
padding: 28px 32px;
margin-bottom: 20px;
```

---

## Input: standard

```html
<label class="form-label" for="id">Label</label>
<input type="text" id="id" class="calc-input" inputmode="numeric">
```

- Height: 46px
- Border-radius: 10px
- Border: 1.5px solid rgba(100,116,139,0.18)
- Background: rgba(255,255,255,0.95)
- Focus: border `--blue`, box-shadow `0 0 0 3px rgba(58,120,201,0.12)`

## Input: currency with R prefix

```html
<div class="input-prefix-wrap">
  <span class="input-prefix">R</span>
  <input type="text" id="id" class="calc-input input-has-prefix" inputmode="numeric">
</div>
```

- `.input-prefix`: 38px wide, right-border separator (1.5px), background rgba(248,250,252,0.6)
- `.input-has-prefix`: padding-left: 48px
- **Never use floating/overlapping prefix text** — always use the separator pattern

---

## Mode toggle (pill selector)

Use instead of dropdowns for 2–3 mutually exclusive options.

```html
<div class="mode-toggle">
  <button class="mode-btn active" onclick="setMode(this)">Option A</button>
  <button class="mode-btn" onclick="setMode(this)">Option B</button>
</div>
```

```javascript
function setMode(btn) {
  btn.closest('.mode-toggle').querySelectorAll('.mode-btn')
    .forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
}
```

- Container: `gap:4px; padding:4px; border-radius:12px` glass background
- Active button: blue gradient + `box-shadow: 0 3px 10px rgba(58,120,201,0.28)`

---

## Collapsible tab pattern

Default: **closed**. Use for calculation workings, methodology, reference tables, tax brackets — anything that's useful but not needed at a glance.

```html
<button id="myBtn" onclick="togglePanel('myPanel','myChevron','myBtn','Show detail','Hide detail')"
  style="display:flex;align-items:center;gap:6px;background:none;border:none;cursor:pointer;
         font-size:12px;font-weight:600;color:var(--text-3);letter-spacing:0.04em;
         text-transform:uppercase;padding:0;font-family:'Plus Jakarta Sans',sans-serif;">
  <svg id="myChevron" width="13" height="13" viewBox="0 0 24 24" fill="none"
    stroke="currentColor" stroke-width="2.5" style="transition:transform 0.2s;">
    <polyline points="9 18 15 12 9 6"/>
  </svg>
  Show detail
</button>
<div id="myPanel" style="display:none;margin-top:12px;border:1px solid rgba(100,116,139,0.13);
  border-radius:12px;overflow:hidden;">
  <!-- content -->
</div>
```

```javascript
function togglePanel(panelId, chevronId, btnId, openLabel, closeLabel) {
  const panel   = document.getElementById(panelId);
  const chevron = document.getElementById(chevronId);
  const btn     = document.getElementById(btnId);
  const open    = panel.style.display === 'none';
  panel.style.display     = open ? 'block' : 'none';
  chevron.style.transform = open ? 'rotate(90deg)' : '';
  btn.lastChild.textContent = ' ' + (open ? closeLabel : openLabel);
}
```

---

## Derived indicator pill

When a value can be calculated from user inputs, show the result as a pill instead of making the user select it from a dropdown.

```html
<div id="indicator" style="display:none;padding:10px 14px;background:rgba(58,120,201,0.06);
  border-radius:8px;border:1px solid rgba(58,120,201,0.14);">
  <div style="display:flex;align-items:center;justify-content:space-between;">
    <span style="font-size:12px;color:var(--text-3);">Your marginal rate</span>
    <strong id="indValue" style="font-size:15px;color:var(--blue);">—</strong>
  </div>
  <div id="indLabel" style="font-size:11px;color:var(--text-3);margin-top:3px;">—</div>
</div>
```

---

## Projection comparison cards

**Rule: conservative / bank scenario LEFT (plain), growth / balanced scenario RIGHT (highlighted)**

```html
<div style="display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:20px;">
  <div class="result-summary-card">           <!-- conservative: LEFT, no highlight -->
    <p class="label">Just in the Bank after 30 yrs</p>
    <p id="summaryBank" class="value">R0</p>
  </div>
  <div class="result-summary-card highlight"> <!-- growth: RIGHT, blue tint -->
    <p class="label">Balanced Investment after 30 yrs</p>
    <p id="summaryInvested" class="value">R0</p>
  </div>
</div>
```

---

## Year 1 snapshot card (what to show)

For each scenario, show all four of these rows:
1. **Tax Paid** (R) — `.val-red`
2. **Effective Tax Rate** (%) — `.val-red` — tax as % of gross return
3. **After-Tax Return** (%) — `.val-blue` or `.val-green`
4. **After-Tax Return (R)** — `.val-blue` or `.val-green` — the actual rand amount

---

## Card icons

Use real PNG files — **never generated SVG illustrations**.

```html
<div class="iso-wrap">
  <img src="icons/filename.png" alt="">
</div>
```

```css
.iso-wrap { width:56px; height:56px; margin-bottom:14px; }
.iso-wrap img { width:100%; height:100%; object-fit:contain;
  filter:drop-shadow(0 3px 8px rgba(0,0,0,0.18)); }
```

---

## Brand logo (hexagonal constellation SVG)

```html
<svg width="26" height="26" viewBox="0 0 26 26" fill="none">
  <polygon points="13,1 23,7 23,19 13,25 3,19 3,7"
    fill="none" stroke="rgba(255,255,255,0.22)" stroke-width="1"/>
  <circle cx="13" cy="1"  r="1.8" fill="#5b9ef4"/>
  <circle cx="23" cy="7"  r="1.8" fill="#f47c5b"/>
  <circle cx="23" cy="19" r="1.8" fill="#5b9ef4"/>
  <circle cx="13" cy="25" r="1.8" fill="#5b9ef4"/>
  <circle cx="3"  cy="19" r="1.8" fill="#f47c5b"/>
  <circle cx="3"  cy="7"  r="1.8" fill="#5b9ef4"/>
  <line x1="13" y1="1"  x2="23" y2="7"  stroke="rgba(255,255,255,0.18)" stroke-width="0.8"/>
  <line x1="23" y1="7"  x2="23" y2="19" stroke="rgba(255,255,255,0.18)" stroke-width="0.8"/>
  <line x1="23" y1="19" x2="13" y2="25" stroke="rgba(255,255,255,0.18)" stroke-width="0.8"/>
  <line x1="13" y1="25" x2="3"  y2="19" stroke="rgba(255,255,255,0.18)" stroke-width="0.8"/>
  <line x1="3"  y1="19" x2="3"  y2="7"  stroke="rgba(255,255,255,0.18)" stroke-width="0.8"/>
  <line x1="3"  y1="7"  x2="13" y2="1"  stroke="rgba(255,255,255,0.18)" stroke-width="0.8"/>
</svg>
```

---

## SA-specific constants (2026/2027 tax year)

```
Interest exemption (under 65):   R23,800 p.a.
Interest exemption (65+):         R34,500 p.a.
CGT annual exclusion:             R40,000
CGT inclusion rate:               40% of gain included in taxable income

Tax brackets:
  R1 – R245,100          18%
  R245,101 – R383,100    26%
  R383,101 – R530,200    31%
  R530,201 – R695,800    36%
  R695,801 – R887,000    39%
  R887,001 – R1,878,600  41%
  R1,878,601+            45%

Return assumptions used in calculators:
  Interest / bank / bonds:   7% p.a.
  Equity / growth:           10% p.a.
```

---

## UX principles (hard rules, not suggestions)

1. **Compact by default — detail on demand.** Main view is clean. Calculations, bracket tables, methodology go behind collapsible tabs. Default: closed.

2. **Natural inputs, not technical dropdowns.** Ask "what's your annual income + age?" — derive the marginal rate programmatically. Show the derived value as a confirmed indicator pill. Never make the user know their own tax bracket.

3. **Real icon PNGs, never generated SVG illustrations.** Isometric SVG icons look generated. Use actual image assets.

4. **Left = worse, right = better.** Projection comparisons always: conservative/bank LEFT (plain), growth/balanced RIGHT (blue-tinted highlight).

5. **Show both rands and percentages for returns.** Tax paid + effective tax rate. After-tax return % + after-tax return R. Both together tell the story.

6. **Toggles over selects for 2–3 options.** Mode toggle pill for binary/trinary choices. Feels more immediate, no dropdown overhead.

7. **Format numbers consistently.** SA locale (`en-ZA`): `R 1 234 567` with spaces as thousand separators. Percentages: `12,34%` with comma decimal. Use `Intl.NumberFormat('en-ZA', ...)` always.

---

## Minimal new-page template

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title | Concept Wealth Woodhill</title>
  <link rel="stylesheet" href="css/glass.css">
  <style>
    .content-wrap { max-width: 820px; }
    .page-bg { padding: 24px 24px 40px; }
    /* page-specific overrides only */
  </style>
</head>
<body>

<!-- Brand Strip -->
<div class="brand-strip">
  <div class="brand-left">
    <!-- hexagonal constellation SVG here -->
    <div>
      <div class="brand-name">Concept Wealth Woodhill</div>
      <div class="brand-sub">Financial Tools Suite</div>
    </div>
  </div>
  <div class="brand-right">Authorised by&nbsp;<span class="sanlam-pill">SANLAM</span></div>
</div>

<!-- Breadcrumb -->
<div class="breadcrumb">
  <a href="index.html" class="bc-back">
    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><polyline points="15 18 9 12 15 6"/></svg>
    All Tools
  </a>
  <span class="bc-sep">›</span>
  <span class="bc-cur">Page Name</span>
</div>

<!-- Page Header -->
<div class="calc-hdr">
  <div>
    <div class="calc-eyebrow"><span class="calc-dot"></span>CATEGORY</div>
    <h1 class="calc-h1">Tool Name</h1>
    <p class="calc-sub">One-line description of what this tool does.</p>
  </div>
  <img src="icons/icon.png" alt="" class="hdr-icon" onerror="this.style.display='none'">
</div>

<!-- Main Content -->
<div class="page-bg">
<div class="content-wrap">

  <div class="glass-card">
    <!-- inputs and results here -->
  </div>

</div>
</div>

<!-- Footer -->
<footer style="background:rgba(255,255,255,0.7);backdrop-filter:blur(20px);border-top:1px solid rgba(100,116,139,0.14);padding:16px 40px;text-align:center;margin-top:20px;">
  <p style="font-size:11px;color:#94a3b8;">For illustrative and educational purposes only. Not financial advice.</p>
  <p style="font-size:10px;color:#94a3b8;margin-top:4px;font-weight:700;letter-spacing:0.05em;text-transform:uppercase;">Concept Wealth Woodhill · SARS 2026/27</p>
</footer>

</body>
</html>
```
