---
name: web-ui-design
description: Sumiya's web design system + taste. Use when designing, building, or restyling ANY web UI (Nuxt/Vue, React/Next, Tailwind, NuxtUI/shadcn, plain HTML/CSS) — for layout, color, type, components, spacing, depth, or "make this screen look good / less generic / less AI-default." Applies a calm, premium fintech aesthetic (depth-not-flat, strict token discipline, tight geometry, color as accent not flood, honest states, light+dark). Consult BEFORE building or redesigning a screen, and when judging whether a web UI looks generic. NOT for native mobile (use mobile-ui-design) or backend.
---

# Web UI Design — Sumiya's system

A living, portable design skill: my taste + rules for **web** UI, reusable across every web project I build (the insure mini-app, the web-admin refactor, future products). The web sibling of `mobile-ui-design`. **Update it whenever a new rule, preference, or liked design appears** (see "Keeping this skill alive").

> Honest premise: my (the assistant's) *default* web taste is weak and drifts toward generic SaaS / AI-template output. This skill exists to override that with a system. When unsure, follow the system here, not instinct.

## The aesthetic (north star)

**Calm, trustworthy, premium fintech. Never loud, never generic, never "AI-template."** Same data and flow as a default UI — but with a point of view applied *everywhere*.

The five levers below are ~90% of "why a redesign looks completely different." In rough order of impact:

### 1. Depth, not flat fills — the single biggest lever
Flat color blocks read as generic. Give interactive/branded elements controlled dimension:
- **Primary action** = a subtle vertical **gradient** + a **1px inset white highlight** + a **soft *colored* glow shadow**. e.g. `background:linear-gradient(180deg,#1E7BEA,#0C63D4); box-shadow:0 1px 1px rgba(255,255,255,.25) inset, 0 6px 16px rgba(12,99,212,.32)`. That tiny dimension is the difference between "a colored rectangle" and "a button you want to press."
- **Cards** = soft, low-spread, **brand-tinted** shadows (navy-tinted, not black): `0 1px 2px rgba(10,31,69,.06), 0 10px 26px rgba(10,31,69,.07)`.
- **Rule of consistency:** *any* solid colored element gets the same treatment. A flat success-green next to a glossy gradient-blue reads cheap — give the green its own gradient + green glow too. (Learned the hard way: matching the *treatment* fixed the green more than changing its hue.)

### 2. Token discipline — what makes it "systematic"
- **One source of truth** for color / type / spacing / radius / shadow. No per-screen palettes, no inline hex, no inline font sizes scattered around.
- On the web: **CSS custom properties** (`--x`) defined once, with a `.dark` (or `[data-theme]`) override block. Namespace them (e.g. `--ma-*`, `.ma-*` utility classes) and scope where they load (a layout / global stylesheet) so they **don't leak** into unrelated UI.
- **One card, one input, one chip, one button family** — same radius / padding / shadow / height everywhere. Consistency is what the eye reads as "designed"; a component grab-bag reads as "thrown together" even when each piece is fine.
- Find drift by **grepping for stray hex** of old values, not by eyeballing.

### 3. Tight geometry + real type
- **Radii:** tight, not bubbly. ~`14 / 11 / 9` (card / control / chip), not 16px+ pills everywhere. Rounder = consumer-toy; tighter = premium/fintech. Full pills only for true chips/badges.
- **Type:** a real UI font with personality (e.g. **Manrope**), not the framework default. A deliberate weight/size scale (display → title → body → label → caption). For a money product, **tabular-nums** on all amounts so digits align (`36,000` looks engineered/trustworthy). A serif (e.g. Lora) only for document/contract "paper" contexts.

### 4. Calm canvas + color as an ACCENT, not a flood
- A quiet neutral **canvas** (e.g. `#EEF3FA`) with **white cards floating** on soft tinted shadows. Not gray-on-gray gradients.
- **Deep brand-tinted ink** for text (e.g. navy `#0A1F45`), never pure black/`#000`.
- Color appears **only where it means something**: brand color = action/selection; success-green = paid/verified/done; one reserved accent (e.g. gold) for a single special signal (an official seal). A grid of rainbow/gradient cards is the classic mistake. Restraint makes the few colored moments feel valuable.

### 5. Honest hierarchy & states (truthful affordances)
- **One focal point per screen** (a hero check, a summary card) — not equal-weight rows. All-equal-weight reads like "a first-year HTML file."
- **Disabled looks disabled** (muted fill + dimmed label) and *visibly* flips to the gradient when usable. Gate on the **real precondition** (the lookup *succeeded*), not a proxy (field has N chars).
- **Verified / looked-up data** drops into a distinct success panel that **echoes the matched key back** (the plate/reg from the *response*, never the typed input).
- **Sticky action bars** (translucent, blurred, hairline top border) keep the primary action reachable; prefer `position: sticky; bottom:0` on the last in-flow element over `fixed`+padding (no gap when content is short).
- **Cover every state**: loading (skeleton/spinner), empty (first-time vs cleared), error (what failed + retry), success. No silent completions.

## Light + dark is a token system, not an afterthought
Define a full dark token set alongside light (surfaces, ink, lines, the brand/semantic colors, *and* dark variants of gradients + glows). **Verify both on every change.** Watch framework footguns (e.g. a global `.dark button span{color:#fff}` rule, forced dark input backgrounds) — structure markup to dodge them (use `div role=button` for colored tiles so colored icons aren't force-whited).

## Working method (brownfield-first)
- **Match/extend the existing token set; never invent a parallel one.** Most web work is brownfield — read 3–5 existing components, extract the real tokens in use, and build from them.
- All user-facing text through the project's i18n (`translate('key')`), never hardcoded. New copy = new key (ask before adding if the project is strict about it).
- Reuse the framework's components (NuxtUI `UDivider`, shadcn, etc.) before hand-rolling — but restyle them to the tokens.
- **Verify by rendering, not by reading.** Build it, screenshot light + dark. A fast loop: headless Chrome (`google-chrome --headless --screenshot`) against a small static harness that includes the *real* extracted CSS, or a dev-only `?_preview=` seed to reach data-gated screens without the full flow.

## Anti-patterns (what makes web UI look generic / AI-default)
- Flat solid fills with no depth; default framework button styling.
- Mixed radii / mixed card styles / inline hex & font sizes (no system).
- Color everywhere (rainbow cards, full-bleed gradient backgrounds).
- Pure black text on white; framework-default font.
- Everything equal weight (no hero, no hierarchy).
- Happy-path only (no loading/empty/error); dead/dishonest disabled states.
- A glossy primary next to flat secondary accents (inconsistent treatment).

## Reference recipe (the insure mini-app — adapt per project)
The exact palette is project-specific; the *structure* is portable. Origin example:
```
--p:#0C63D4  --p-700:#0A4FB0  --p-50:#EFF5FF  --p-100:#D7E7FF      /* trust-blue */
--ink:#0A1F45  --ink-2:#5A6B86  --ink-3:#8A98AD                    /* cool navy ink */
--bg:#EEF3FA  --surface:#FFFFFF  --line:#E3E8F2                     /* calm canvas */
--ok:#0E9F58 (+ gradient #1FBF77→#0E9F58 + green glow)             /* premium emerald */
--gold:#B8902E                                                     /* official seal only */
radii 14/11/9 · navy-tinted shadows sm/md/lg · Manrope (UI) + Lora (contract doc) · tabular-nums
```
For the web-admin refactor: keep its own brand hue, but apply the same *levers* — depth, token discipline, tight geometry, calm canvas, hierarchy.

## Keeping this skill alive
This is a living document. When Sumiya states a new preference, approves/rejects a design, or a new pattern proves out — **add or revise a rule here** in the same session, concisely. Prefer durable principles over one-off specifics. Note the date on contested calls.
