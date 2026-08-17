---
name: mobile-ui-design
description: Sumiya's native-mobile design system + taste. Use when designing, building, or restyling ANY native mobile UI (Android Jetpack Compose now; SwiftUI later) — for layout, color, type, components, spacing, motion, or "make this screen look good / less basic." Applies a calm fintech aesthetic (color as accent not flood, the "calm card", honest affordance states, strict token discipline) and the native substitutes for web-only effects. Consult BEFORE creating or redesigning a screen, and when judging whether a UI looks generic/AI-default. NOT for web UI (use frontend-design) or backend.
---

# Mobile UI Design — Sumiya's system

A living, portable design skill: my taste + rules for native mobile UI, reusable across every mobile app I build. Grown during insure-mobile development; **update it whenever a new rule, preference, or liked design appears** (see "Keeping this skill alive").

Android/Jetpack Compose is the primary target today; the *principles* carry to SwiftUI when iOS comes.

## The aesthetic (north star)

**Calm, trustworthy fintech. Never loud, never generic, never "AI-template."**

1. **Color is an ACCENT, not a flood.** Full-saturation gradient cards read as loud and hurt readability. Use a neutral surface and let the identity color live in a small icon chip / one accent element. (A grid of rainbow gradient cards is the classic mistake.)
2. **The "calm card"** — the default container: neutral surface + hairline border + a *soft colored shadow lift* + ~20dp radius. Keep a card family visually consistent (same radius / shadow / padding / tile size). Reads especially well in dark mode.
3. **Readability first** — strong, high-contrast titles; muted (not faint) secondary text; clear hierarchy (one focal point per screen, not equal-weight blocks). Never white text on a bright gradient.
4. **One visual anchor per screen** — a hero element (a styled badge, an illustration, a summary card) so the eye lands somewhere. A screen that's all equal-weight rows looks like "a first-year HTML file."
5. **Intentional, not decorative** — every color, size, and space is a deliberate choice from the system, not a one-off.

## Honest affordances (states must tell the truth)

- **Real disabled state** — a disabled primary button must look inactive (muted fill + dimmed label), not the full gradient. It must *visibly change* disabled→active when it becomes usable.
- **Gate on the real precondition**, not a proxy (e.g. enable Continue when the lookup actually *succeeded*, not when the field merely has N characters).
- **Hide the input once you have the result** (e.g. hide a keypad after a successful lookup) so the result reads clearly; give a tap-to-edit path back.
- **Echo a looked-up key back** in the result for at-a-glance verification (show the matched plate/id in the result card, sourced from the response — never the typed input).
- **Fixed-length / typed inputs**: cap + filter to the valid format; use the right keyboard; security inputs (PIN) get a branded on-screen keypad, never the OS keyboard.
- **Every action gives feedback** — success or the real error. No silent completions.

## Token discipline (this is what makes it "systematic")

- **One source of truth** for color, type, spacing, radius. No per-screen color palettes, no inline hex, no inline font sizes.
- **Color** via a theme token set (a `CompositionLocal` in Compose); dark-aware. One brand accent (+ a dark variant). Decorative gradient stops and fixed-context surfaces (always-white cards, always-dark auth) may use literals, but centralize the repeated ones.
- **Type** via a named scale (Display / H1 / H2 / Title / Body / Body-sm / Label / Caption) → `MaterialTheme.typography`. Never inline `.sp`.
- **Spacing** on a 4-pt grid; **radius** as named steps (sm/md/lg/xl).
- Find drift by grepping for stray hex of the *old* accent values, not by eyeballing.
- Light + dark both verified on every change.

## Native craft — match the *perceived* design, flag what can't be 1:1

Web/React mocks use compositor effects Compose doesn't have. Match the look; where it's impossible 1:1, say so and use the native substitute (don't ship a silent 80% copy):
- **Colored glow shadows** → a custom blurred round-rect behind (`Modifier.softShadow`); Compose elevation can't do colored/spread/multi-layer.
- **`backdrop-filter` blur (glassmorphism)** → a translucent solid surface (real backdrop blur needs an expensive snapshot).
- **`filter: drop-shadow()` on a PNG (silhouette shadow)** → a soft radial glow ellipse behind the image.
- **`inset` highlights** → a 1px top gradient/hairline.
- **Edge-to-edge** is on → apply `statusBarsPadding()` per screen root (after `.background()`) and `navigationBarsPadding()` on floating bars/keypads, or top/bottom controls sit under the system bars and won't register taps.
- **Compose trap:** `Modifier.alpha` stacked over a `drawBehind` glow can fail to paint until invalidated — express enabled/disabled via the brush, not an alpha layer.

## Icons & assets
- Use one consistent icon set (Lucide is the working stand-in). The gold standard is a **bespoke brand icon set** (custom SVGs matching the brand) — a designer deliverable; wire as vector drawables when it arrives. Don't mix icon sources.
- Bespoke 3D/illustration assets *are* the look — port them, don't substitute generic icons.

## Per-screen pre-flight checklist
1. What's the **one anchor**? 2. Is color an **accent**, not a flood? 3. Cards from the **calm-card** pattern? 4. Type from the **scale**, color from **tokens** (no inline)? 5. **Honest states** (disabled looks disabled; gated on the real precondition)? 6. **Light + dark** both right? 7. Any **web-only effect** that needs a native substitute (flagged)? 8. Does it look **intentional**, not AI-default?

## Keeping this skill alive
This skill is meant to grow. When a new rule, preference, component pattern, or liked design shows up:
- A rule/preference → add it to the relevant section here (or `references/design-system.md`).
- A reusable component → add to `references/design-system.md`.
- A design I like (from the internet or a designer) → add an entry to `references/inspiration.md` with **"what to take from it."**
- A Compose/SwiftUI craft lesson → the "Native craft" section.
Treat "update the design skill" as a normal step, like updating memory. Project-specific decisions stay in that project's memory; only the **durable, portable** ones graduate into this skill.

## References
- `references/design-system.md` — concrete tokens, the type scale, spacing/radius, the component catalog.
- `references/inspiration.md` — the curated library of designs I like + what to take from each.
