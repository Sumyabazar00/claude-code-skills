---
name: insurance-preview
description: Converts insurance product risk/coverage data into a clean, modern, card-based HTML preview that is responsive on both desktop and mobile. Accepts .docx files, .html files, or pasted HTML/table content. Output is ready to copy into the ins_products.preview column.
argument-hint: "[path-to-file] or paste content directly"
user-invocable: true
allowed-tools: Bash Read Write Edit Glob Grep Agent
---

# Insurance Product Preview Generator

You convert insurance product risk/coverage data into driver-friendly HTML previews that render cleanly on both desktop and mobile. The output HTML is self-contained (no external dependencies), designed to be stored in a database column and rendered inline.

## Context

- Insurance companies send .docx files or raw HTML tables describing their product risks and coverage tiers
- We store the HTML preview in the `ins_products.preview` column in our database as raw HTML -- so the output must be compact, self-contained, and safe to embed
- The preview is fetched and rendered inline for employees and drivers on both desktop and mobile screens
- The audience is everyday drivers -- not insurance professionals. Clarity is paramount.

## Size and compactness constraints

Because the HTML is stored as raw text in `ins_products.preview` and fetched on every product view:

- **Target total output size: under ~35 KB** for a typical 12-risk product. Stay well under 60 KB even for large products.
- **No blank-line padding between CSS rules.** Group related rules tightly.
- **No commented-out code, no TODOs, no placeholder sections.** Ship only what renders.
- **Do not duplicate CSS between desktop and mobile blocks.** Only override properties that actually change.
- **No redundant wrapper divs.** Every element must serve layout or semantics.
- **Inline SVGs only where they appear once** (in each card icon and the chevron). Do not attempt `<defs>`/`<use>` sharing -- it breaks in some rich-text renderers.
- **Do not minify** (the user may hand-edit the HTML later), but do keep whitespace tight.

## Step 1: Get the Source Data

There are three ways the user can provide input. Detect which one:

### Option A: .docx file path

The user provides a path to a .docx file via `$ARGUMENTS` or in their message.

Use `python3` with the `python-docx` library to extract the document content:

```python
from docx import Document
doc = Document("path/to/file.docx")

for table in doc.tables:
    for row in table.rows:
        for cell in row.cells:
            print(cell.text)

for para in doc.paragraphs:
    print(para.text)
```

If `python-docx` is not installed, install it: `pip install python-docx`

### Option B: .html file path

The user provides a path to an .html file via `$ARGUMENTS` or in their message. Read the file using the Read tool. Parse the `<table>` elements to extract risk categories, details, and coverage percentages.

### Option C: Pasted HTML or table content

The user pastes raw HTML (typically a `<table>`) directly in their message. This is the most common case for managers who already have the converter output. Parse the pasted content directly -- no file reading needed.

### Auto-detection rules

- If `$ARGUMENTS` ends in `.docx` -- use Option A
- If `$ARGUMENTS` ends in `.html` or `.htm` -- use Option B
- If no arguments provided, check the user's message for pasted HTML (`<table`, `<tr`, `<td` tags). If found -- use Option C
- If nothing is found, look for `.docx` and `.html` files in the current directory and ask the user which one to process

## Step 2: Analyze the Document Structure

Insurance product documents typically contain:

1. **Risk categories** (column 1): e.g., "Байгалийн эрсдэл", "Галын эрсдэл", "Замын хөдөлгөөний эрсдэл"
2. **Risk details** (column 2): Specific scenarios covered under each risk
3. **Coverage tiers** (columns 3+): Percentage of compensation per plan tier (e.g., "Үндсэн" 70%, "Нэмэлт" 80%, "Бүрэн" 100%)

The document may be in Mongolian or English. Preserve the original language.

After extracting, identify:
- How many coverage tiers exist and their names
- All risk categories and their sub-items
- Coverage percentages per tier per risk
- Any footnotes, conditions, or special notes
- "-" or empty cells mean "not covered" by that tier

## Step 3: Pick Icons for Each Risk Category

Map each risk category to an appropriate inline SVG icon. Common mappings:

| Risk Category (MN) | Icon Concept | Stroke Color |
|---|---|---|
| Байгалийн эрсдэл (Nature) | Sun/weather | #2563eb |
| Галын эрсдэл (Fire) | Flame | #dc2626 |
| Замын хөдөлгөөний эрсдэл (Traffic) | Vehicle | #d97706 |
| Зогсоолд байрлуулах/гаргах (Parking entry/exit) | P-sign | #4f46e5 |
| Гуравдагч этгээд (Third party) | People | #db2777 |
| Дугуйн эрсдэл (Tire) | Circle/wheel | #7c3aed |
| Жолоо шилжүүлсэн (Driver transfer) | Person | #0d9488 |
| Зогсоолд байх үеийн (While parked) | Building | #0891b2 |
| Салхины шил/цонх (Windshield/glass) | Window/grid | #059669 |
| Толины хулгай (Mirror theft) | Eye | #a855f7 |
| Эд ангийн хулгай (Parts theft) | Wrench | #ea580c |
| Их биеийн хулгай (Full vehicle theft) | Shield-alert | #dc2626 |
| Жолоочийн гэмтэл (Driver injury) | Heart/medical | #e11d48 |
| Зорчигчийн (Passenger) | Users | #6366f1 |
| Хариуцлага (Liability) | Scale/balance | #0284c7 |

For categories not listed above, pick the closest matching Lucide-style SVG icon. Use `stroke` SVGs (not filled), `stroke-width="2"`, `stroke-linecap="round"`, `stroke-linejoin="round"`, 22x22 viewBox in 24x24.

## Step 4: Generate the HTML

Generate a single self-contained HTML string. The output must:

### Structure

```
- Page header: product title + subtitle explaining the tiers
- Tier legend: colored badges showing each plan name
- Legend note ("Тэмдэглэгээний тайлбар"): explaining "--" and percentage/check meaning -- MUST come above the first risk section, not at the bottom
- Risk cards: one expandable card per risk category
```

**The "Тэмдэглэгээний тайлбар" note always goes above the first risk section** (above "Даатгалын эрсдэлүүд"), never as a footer at the bottom. Drivers need the key to the symbols before they read the cards. Use the same `.footer-note` styling -- only its position changes.

### Design Rules

1. **Self-contained**: All CSS inline in a `<style>` tag. No external CSS, no JS frameworks, no CDN links, no Google Fonts. Use system font stack only.
2. **No emojis**: Use inline SVG icons only. Never use emoji characters.
3. **Expandable cards (NO JavaScript)**: Each risk category is a card. Clicking the header toggles detail visibility. This MUST work without any JavaScript, because the preview is rendered inside a sandboxed `<iframe sandbox="allow-same-origin">` (no `allow-scripts`) for security -- any `<script>` is silently dead. Use the native `<details>`/`<summary>` elements: `<details class="risk-card">` wraps the card, `<summary class="risk-header">` is the clickable header row, and the detail list follows. Never emit a `<script>` tag or `onclick`/`toggleCard` handler.
4. **Color-coded tiers**: Each tier gets a distinct color pair (background + text). Use these defaults unless the document specifies branding:
   - Tier 1 (basic): blue `#e8f0fe` / `#1a56db`
   - Tier 2 (additional): orange `#fef3e2` / `#b45309`
   - Tier 3 (full): green `#e6f9ed` / `#047857`
   - If there are more tiers, add: purple `#f3e8ff` / `#7c3aed`, pink `#fce7f3` / `#db2777`
5. **"Not covered" indicator**: When a cell is "-" or empty, show a grey pill with "--" text.
6. **Coverage pills**: Show percentage in a colored pill matching the tier color.
7. **Responsive on desktop and mobile**: Use flexbox, never tables. Must include the exact breakpoint rules in the "Responsive rules" section below.
8. **Light background**: `#f5f7fa` body background, `#fff` card background.
9. **Subtle shadows**: `box-shadow: 0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04)`
10. **Border radius**: 12px for cards, 8px for badges, 6px for pills.
11. **Viewport meta tag required**: `<meta name="viewport" content="width=device-width, initial-scale=1.0">` inside `<head>`.

### Card Layout

Each card has:
- **Left**: 40x40 rounded icon with tinted background
- **Center**: Risk title (bold 16px) + subtitle (grey 13px, summarizing what this covers in plain language)
- **Right**: Coverage pills for each tier (e.g., `70%` `80%` `100%`)
- **Far right**: Chevron arrow (toggle indicator)
- **Expandable section**: Bullet list of specific scenarios covered

### CSS Class Naming

```
.container, .page-header, .plan-legend, .plan-badge, .plan-dot,
.risk-card, .risk-header, .risk-icon, .risk-title-area, .risk-title,
.risk-subtitle, .coverage-pills, .coverage-pill, .toggle-arrow,
.risk-details, .risk-details-inner, .detail-list
```

### Responsive rules (MUST include verbatim)

The `<style>` block must end with these two media queries so the preview works on phones. Do not omit, re-order, or "simplify" them -- they were tuned against real layouts. Desktop defaults stay above; these only override what changes on mobile.

```css
@media (max-width: 640px) {
    body { padding: 16px 12px; }
    .page-header { margin-bottom: 24px; }
    .page-header h1 { font-size: 20px; }
    .page-header p { font-size: 14px; }
    .plan-legend { gap: 8px; margin-bottom: 20px; }
    .plan-badge { font-size: 12px; padding: 6px 10px; gap: 6px; }
    .plan-dot { width: 8px; height: 8px; }
    .risk-header { flex-wrap: wrap; padding: 14px 16px; gap: 10px; }
    .risk-icon { width: 36px; height: 36px; }
    .risk-icon svg { width: 20px; height: 20px; }
    .risk-title { font-size: 15px; }
    .risk-subtitle { font-size: 12px; }
    .toggle-arrow { order: 2; }
    .coverage-pills { order: 3; width: 100%; justify-content: flex-start; padding-left: 46px; margin-top: 2px; }
    .coverage-pill { font-size: 12px; padding: 3px 8px; min-width: 44px; }
    .risk-details-inner { padding: 0 16px 14px 16px; }
    .detail-list li { font-size: 13px; padding-left: 18px; }
    .footer-note { margin-top: 24px; padding: 14px; font-size: 12px; }
}

@media (max-width: 380px) {
    body { padding: 14px 10px; }
    .plan-legend { flex-direction: column; align-items: center; gap: 6px; }
    .risk-header { padding: 12px 14px; gap: 8px; }
    .risk-icon { width: 32px; height: 32px; }
    .risk-title { font-size: 14px; }
    .coverage-pills { padding-left: 40px; gap: 4px; }
    .coverage-pill { font-size: 11px; min-width: 40px; padding: 3px 6px; }
}
```

Why these matter:
- `.toggle-arrow { order: 2 }` + `.coverage-pills { order: 3; width: 100% }` keeps the chevron on the first row with the title while pills wrap to their own row on narrow screens. Without the `order` overrides, the chevron gets pushed below the pills.
- `padding-left: 46px` (mobile) / `40px` (tiny) aligns pills under the title, matching the shrunk icon column width.
- The 380px breakpoint is for small Android phones where three-pill rows start to feel cramped.

### Expand/collapse (pure CSS, no JavaScript)

The preview runs in a sandboxed iframe without `allow-scripts`, so the toggle uses native `<details>`/`<summary>` -- there is NO `<script>` anywhere in the output.

Markup per card:

```html
<details class="risk-card">
  <summary class="risk-header">
    <div class="risk-icon">...</div>
    <div class="risk-title-area">...</div>
    <div class="coverage-pills">...</div>
    <svg class="toggle-arrow">...</svg>
  </summary>
  <div class="risk-details-inner">
    <ul class="detail-list">...</ul>
  </div>
</details>
```

Required CSS so it behaves and looks right (the default disclosure triangle is removed; the chevron rotates on open; the panel fades in):

```css
.risk-header { cursor: pointer; list-style: none; }
.risk-header::-webkit-details-marker { display: none; }
.toggle-arrow { transition: transform .25s ease; }
.risk-card[open] .toggle-arrow { transform: rotate(180deg); }
.risk-card[open] .risk-details-inner { animation: reveal .25s ease; }
@keyframes reveal { from { opacity: 0; transform: translateY(-6px); } to { opacity: 1; transform: none; } }
```

Cards are collapsed by default (omit the `open` attribute). `<summary>` is natively keyboard-focusable and toggles on Enter/Space, so this is more accessible than a JS click handler -- not just a fallback.

## Step 5: Output

1. Write the generated HTML to a **new, uniquely-named file** in the current directory -- **never overwrite a previous output**. Use a descriptive, collision-free name such as `preview-<product-slug>.html` (e.g. `preview-tee-vrijn-hereg sel.html`) or, if no good slug is available, append an incrementing suffix (`preview-output-2.html`, `preview-output-3.html`). Before writing, check the directory for existing `preview-*.html` files and pick a name that does not already exist, so each run preserves the earlier previews.
2. Report the output file size to the user (run `wc -c` on the file you just wrote). If it exceeds 60 KB, audit for bloat (duplicate CSS between breakpoints, oversized SVG paths, redundant wrappers) and trim before finishing.
3. Verify the two media queries (`@media (max-width: 640px)` and `@media (max-width: 380px)`) are present exactly as specified in "Responsive rules". If not, add them. Also verify the "Тэмдэглэгээний тайлбар" note sits above the first risk section.
4. Tell the user:
   - Open the generated file in a browser and test both desktop width and a narrow mobile width (≤380px) to confirm the toggle arrow stays next to the title and pills wrap to a second row
   - Copy the HTML content to paste into the `ins_products.preview` database column
   - The HTML is fully self-contained -- no external dependencies needed
   - Report the final size in the summary

## Important Rules

- **Preserve original language**: If the document is in Mongolian, the HTML must be in Mongolian. Do not translate.
- **Preserve all data**: Every risk, every detail, every percentage must appear in the output. Do not summarize or omit rows.
- **Never alter source wording -- reproduce it verbatim**: This skill only ever redesigns the *presentation*. Every piece of text that comes from the source -- risk category names, named-risk/detail rows, tier names, section headers, percentages, deductible labels, conditions, footnotes -- must be copied character-for-character: do not reword, paraphrase, pluralize, re-tense, "clean up", translate, or change punctuation/percentage formatting (e.g. keep `2%` as `2%`, never normalize to `2.0%`; keep `тогтоосон хэмжээнд хүртэл олгоно` exactly, do not rewrite as `...олгох зардлууд`). Collapsing runs of whitespace from docx artifacts and pure letter-casing changes done via CSS are the only allowed transforms. The *only* text you may author yourself is clearly-additive helper copy that does not exist in the source -- card subtitles, the legend/symbol key, and the page subtitle -- and even those must never restate a source phrase in altered form. When in doubt, quote the source exactly rather than improving it.
- **No external dependencies**: The HTML must work when pasted into a database column and rendered in a browser with zero external resources.
- **No emojis**: Use SVG icons only.
- **Responsive by default**: Always emit the 640px and 380px media queries from the "Responsive rules" section. A preview without them is incomplete.
- **Compact output**: The HTML lives in a DB column and is fetched on every product view. Keep it tight -- no blank-line padding between CSS rules, no dead code, no comments beyond section markers.
- **Driver-friendly subtitles**: For each risk card, write a short plain-language subtitle that helps a non-expert understand what this risk category means. Keep it under 10 words.
- **Consistent icon style**: All icons must be Lucide-style outlined SVGs with `stroke-width="2"`.
- **If the document has additional sections** (exclusions, conditions, notes, deductibles), add them as a separate section below the risk cards with appropriate styling (e.g., a bordered note box). Any custom styling added for these sections must also be responsive -- add matching overrides inside the 640px media query.
