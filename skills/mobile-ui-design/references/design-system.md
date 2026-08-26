# Design system — tokens, scale, components

Each app sets its **own brand accent + font**; the *structure* below is constant. Values shown are insure-mobile's reference implementation (Jetpack Compose).

## Color token set (the template)

A dark-aware token object provided via `CompositionLocal` (`InsureColorTokens` / `InsureTheme.colors`). Fields every app should have:

`appBg · screenBg(gradient) · surface · surfaceStrong · border · borderStrong · text · textMuted · textSubtle · textFaint · accent · accentSoft · navBg · menuBg · sheetBg · inputBg · inputBorder · chipText · chipBorder · success · danger · notif` + gradients `heroCard · ctaBg · promoBg · headlineGrad · numberGrad`.

**Insure brand accent = purple `#5136D1` (light) / `#A78BFA` (dark).** CTA gradient `#7438E7 → #5136D1 → #3B289E` (used by every primary button, FAB, hero — all unified). Surfaces/text from a neutral dark/light pair. Rule: screens read `colors.*`; **no inline hex, no per-screen palette.**

Fixed-context exceptions (legitimate literals, but centralize repeats):
- always-white cards (cards that stay light in both themes) → fixed dark text + fixed brand purple on them.
- always-dark surfaces (e.g. auth background) → fixed light text + the dark accent variant.
- per-category status colors (e.g. distinct hues per activity kind) — intentional multi-color, not drift.

## Type scale

One named scale → `MaterialTheme.typography`. Font is per-app (insure: **Roboto Condensed**, MN Cyrillic + Latin). Steps (semantic → slot → size/weight):
Display `displayLarge` 32/700 · H1 `headlineLarge` 24/700 · H2 `headlineMedium` 20/600 · Title `titleLarge` 17/600 · Body `bodyLarge` 17/400 · Body-sm `bodyMedium` 15/400 · Label `labelLarge` 15/600 · Caption `bodySmall` 12/400. **No inline `.sp`.** Pick from the scale, never between two steps.

**Line height per step — set it, don't inherit a default.** insure's are fixed by `MOBILE_DESIGN_BRIEF` §3.2 and that table is authoritative over anything here:
Display 32→40 · H1 24→32 · H2 20→28 · H3/Title 17→24 · Body-lg 17→24 · Body 15→22 · Body-sm 13→18 · Caption 12→16 · CTA 17→22 · Mono 14→20.
Compose takes the **total** (`lineHeight = 40.sp`); SwiftUI `.lineSpacing()` takes the **difference** (`40 − 32 = 8`). Wire it into the scale itself so call sites can't forget.

Note the tension with Rule 3: the brief's headings sit at 1.25–1.4, not the 1.1–1.2 the rule asks for. For a *new* app, follow the rule. For insure, the brief wins until it is deliberately revised — a skill update is not the place to change a locked design doc.

## Spacing & radius
- Spacing scale: 4 · 8 · 12 · 16 · 24 · 32 · 48 · 64 (insure's `space.1`…`space.8`, `MOBILE_DESIGN_BRIEF` §3.3), extended with **96 · 128** for section-level air on tall or sparse screens in other apps. Default card padding 16; tight groupings 8.
- **The scale is the whole set of legal values.** Never 13, never 27. The steps grow rather than stepping evenly because adjacent values must be distinguishable on screen (see SKILL.md Rule 2).
- **Inside a group < between groups, with a clear jump.** 8/24, 8/32, 12/32 — not 16/24.
- Radius: sm 6 · md 12 · lg 16 · xl 24. Cards/sheets ~20; bottom-sheet top corners ~28.

## Component catalog (recipes)

- **Calm card** — `surface` fill + 1dp `border` + `softShadow(accent @ ~0.12, blur 16–18, offsetY 8–10, spread -6/-8)` + 20dp radius. The default container.
- **Primary button** (`InsureButton`) — `ctaBg` gradient when enabled; **disabled = muted solid fill + dimmed label** (never the gradient). Center content; loading = spinner.
- **Icon chip** — 40–48dp rounded square, `accent @ 0.12` fill, the icon tinted `accent`. The "color lives here" element on calm cards.
- **Identity/plate badge** — a small gradient pill (insure: teal `#1FC6AA→#129F84` + emblem) carrying the key value; the signature anchor, reused across screens for continuity.
- **Option toggle card** — calm card + icon chip + label + checkbox; border picks up the accent when selected.
- **Bottom-sheet picker** (`ModalBottomSheet`, `containerColor = sheetBg`) — title + list rows (icon chip + primary/secondary text + trailing action like delete). Doubles as lightweight management UI.
- **Branded numeric keypad** — for PIN/secure entry; circular soft-shadow keys, accent-fill dots, shake-on-error. Never the OS keyboard for these.
- **softShadow** — `Modifier.softShadow(color, blur, cornerRadius, offsetX, offsetY, spread)` drawing a blurred round-rect behind; the colored-glow substitute (Compose elevation can't).
- **Profile header** — centered: avatar (~88dp circle + hairline border) → name (bold) → muted meta line (e.g. masked id) → verified pill. The anchor for a profile/account screen (beats a left-aligned avatar+name row). Add an avatar edit-pencil ONLY if avatar-change is wired.
- **Recessed contact row** — icon chip + `Column(small muted label ABOVE the value)`; left-aligned, no right-aligned value. Add a trailing chevron ONLY if the row is actually navigable (else it's a false affordance).

## Verify
Light + dark every change. On Android, screenshot via `adb shell screencap -p /sdcard/s.png` + `adb pull` (not `exec-out > file` — PowerShell corrupts the binary).
