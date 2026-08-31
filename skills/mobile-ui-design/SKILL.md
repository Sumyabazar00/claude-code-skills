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

## Rule 1 — hierarchy, before anything else

**The first question on any screen is "where does the eye go, and in what order?"** Almost every
interface built by an engineer has one disease: everything shouts at the same volume.

Three rules (from *Refactoring UI*):

1. **Not everything can be important.** To make something stand out, make the other things
   *quieter* — don't make it louder. The engineer reflex is bold + bigger + a colour. The designer
   reflex is to make the neighbouring label grey.
2. **Never use font size alone.** Size, weight, and colour work together. A de-emphasized element
   is usually the *same size* in a lighter grey.
3. **Two to three font weights and two to three text colours for the whole screen.** Seriously.
   Good interfaces use fewer than you would guess. Count them before you ship.

Corollary — **a screen with no dominant element has no hierarchy, only ranking.** If you cannot
name the one thing the eye lands on first, the layout is not finished.

### Proximity assigns membership — spacing is not decoration

Whitespace does not merely separate; it **states what belongs to what**. An element sits in whichever
group it is closest to, whatever the code structure says, and the reader has no way to override that
reading.

The practical form of the rule: **the grouping distance must be clearly smaller than the separating
distance**, or the pairing does not read at all. A gap of 16 above and 24 below is not a group — it
is a slightly lopsided list.

The failure is easy to miss because each gap looks defensible on its own. Ask instead: *which
neighbour is this element nearer to, and is that the one it actually belongs to?* On a keypad
screen, the readout is the keypad's **display** — it is the one thing that changes in response to
the keys — so it belongs to the keys, not to the heading it happens to sit under. Sixteen points
below the title and a long flexible gap above the keys had it reading as the tail of the heading.

**Where the flexible gap goes decides the grouping.** In a `VStack`, moving a `Spacer()` from below
an element to above it re-parents that element from one group to the other without changing a single
fixed value. That is the cheapest structural edit in layout — and the one most often skipped in
favour of nudging numbers.

### Reading order vs. emphasis order — the eyebrow

These are two different axes and they get confused constantly. When the language's sentence
structure puts the subordinate clause first (Mongolian does: *"Апп-аа нээхийн тулд ПИН код оруулна
уу"* — "to open your app, enter the PIN"), reading order demands that clause on the top line. It
does **not** follow that it gets the top line's *weight*.

Swap the **order**, keep the **emphasis** — the eyebrow (or kicker) pattern:

```
Апп-аа нээхийн тулд      <- line 1, 14 / 400 / grey 60%    (reading order)
ПИН КОД ОРУУЛНА УУ       <- line 2, 22 / 700 / white       (emphasis order)
```

Literally trading the two strings between a title slot and a subtitle slot instead makes the
loudest text on the screen the one sentence nobody needs to act on. **A "swap the title and the
subtitle" request almost always means "fix the reading order", not "invert the hierarchy" — ask
which one is meant before building it.**

## Rule 2 — spacing is a scale, and the scale is not linear

**Every gap in the UI is a value from the scale. Never 13. Never 27.** This is the single biggest
lever on how finished a screen looks, and it is a rule, not taste — which is exactly why engineers
skip it.

The steps are not "every multiple of 4". They grow: **4 · 8 · 12 · 16 · 24 · 32 · 48 · 64 · 96 ·
128**. The reason they grow is that adjacent values have to be *distinguishable*: 24 against 25 is
invisible on screen, so a linear scale hands you a hundred choices that all look the same and you
spend forever making meaningless ones. A scale with gaps means every choice you make is a choice
someone can see.

**Start with too much air and take it away.** Everyone under-spaces — the engineer reflex is to fit
more on the screen. Excess space reads as intentional; cramped reads as accidental. If you are
unsure between two steps, take the larger one first and only come down if it actually looks loose.

**The relationship rule is the whole game.** Space *inside* a group must be clearly smaller than
space *between* groups. Label 8 from its field, 32 to the next field. This is the numeric form of
"Proximity assigns membership" above: a layout where the inside gap and the outside gap are both 16
does not have groups, it has a list. One step of difference is not enough either — 16 inside and 24
between still reads as a lopsided list. Aim for a clear jump: 8/24, 8/32, 12/32.

**Don't stretch content to the full width just because the width is there.** On a phone this is the
gutter: body content needs a real margin (16–24), never edge-to-edge text. It becomes a genuine bug
on tablets, landscape, and foldables — a form that is 100% wide at 900pt is unreadable. Cap the
content column and centre it. The exception is deliberate and native: full-width primary buttons,
full-bleed hero imagery, and list-row separators are *supposed* to run the width.

## Rule 3 — typography is a scale too, plus four rules nobody sets

**A constrained, hand-picked scale — pick from the list, never in between.** What matters is that
the list is short and its steps are distinguishable, not which numbers are on it. insure's is
32 · 24 · 20 · 17 · 15 · 12 (`references/design-system.md`). If the size you want is not on the
scale, the answer is the nearest step, not a new step.

**Line height tracks size inversely, and you have to set it.** Small text wants tall lines
(1.5–1.6); large headings want tight ones (1.1–1.2). A 32pt heading at 1.5 looks broken in a way
that is very hard to diagnose by eye — you will blame the font. The trap on both platforms is that
the *default* is a single body-ish value applied to every size, so headings ship too airy and body
text ships too tight, and nobody ever names it.

- **Compose** — `lineHeight` in `TextStyle` is the *total* line box. `lineHeight = 36.sp` on a
  32.sp heading is ~1.12.
- **SwiftUI** — `.lineSpacing()` is **additional** leading, and it is added on top of the font's
  **own line box**, which is already taller than the point size. So the value to pass is
  `lineHeight − UIFont.lineHeight`, *not* `lineHeight − fontSize`. That second formula is the
  intuitive one and it is wrong by exactly the face's built-in leading, on every step. Measured on
  Roboto Condensed the natural box is **1.172 × size**, so `lineHeight − size` overshot a 13/18
  step by 2.2pt — it added 5.00 where 2.77 was wanted and rendered 20.2 against a target of 18.
  Per line that looks like a taste choice; across a paragraph it is plainly loose. Read the metric
  at runtime (`UIFontMetrics.scaledFont(for:).lineHeight`) rather than hardcoding the subtrahend,
  so the arithmetic survives a change of face. Passing a ratio (1.5) does almost nothing visible;
  passing the full line height triple-spaces the paragraph.
- **Scale both terms, not one.** A fixed leading stays fixed while the face grows, so at 200%
  Dynamic Type the lines crowd back together and the accessibility setting makes long text *harder*
  to read. Put the target through `UIFontMetrics.scaledValue(for:)` too.
- **Pair the face and its leading in one type.** `Font` carries size and weight; leading is a
  separate `View` modifier. Anything that hands out a bare `Font` hands out half a type style, and
  the other half is forgotten at every call site. insure's brief had a `lineHeight` column from the
  start and the app applied **not one value of it** for months, because the helper that would have
  applied it took `lineSpacing: CGFloat = 0` and nobody ever passed the argument. **An optional
  parameter defaulting to zero is not a design system** — make the step a value (`InsureTextStyle`)
  that carries both, so dropping the leading is not expressible.

Set it per scale step, in the type system, not at call sites. A helper that defaults to zero line
spacing means the app has no line heights at all, however many steps the scale has.

**Line length 45–75 characters.** At body size on a phone this mostly takes care of itself (390pt
at 15–17pt lands near 40–50 characters), so the rule bites in three specific places instead:
tablets and landscape, long-form bodies like article and legal text, and large Dynamic Type / font
scale settings where the layout reflows into a wall. Cap the paragraph column; don't let prose run
the full width of an iPad.

**One family, several weights.** Five-plus weights of one good face is more cohesive than three
families. The native corollary is the one that actually bites: **a weight or style exists only if
the face is really there.** A variable font with a `wght` axis needs the axis instanced explicitly
or the OS synthesises a smeared fake bold; a family bundled upright-only makes `.italic()` a silent
no-op that renders emphasis identically to its surroundings. Verify the face before designing
against it, and treat a synthetic oblique or a fake bold as a stopgap you have written down, not a
solution.

**Grey text on a coloured background looks dirty.** Neutral grey over an accent or tinted surface
reads as muddy, because the grey has no relationship to the hue underneath it. Use the background's
own hue at reduced opacity, or a tint of the background colour. The systemic consequence: a single
global `textMuted` cannot be correct everywhere — the muted token that reads right on `surface`
reads dirty on an accent card. Fixed-context surfaces get their own muted pair, alongside the other
fixed-context exceptions in `references/design-system.md`.

**Sizes go through the scaling unit, never a raw one.** Compose `sp` for text (never `dp`); SwiftUI
custom fonts through `UIFontMetrics`/Dynamic Type. The web form of this rule is "px or rem, never
em, because em compounds through nesting"; the native equivalent trap is hardcoded `dp`/fixed-point
text, which does not compound but ignores the user's accessibility setting — a bad experience and
an App Store review risk.

## Rule 4 — colour is a system, and the system is a set of ramps

Colour intimidates engineers more than any other part of design, and it shouldn't, because most of
it is mechanical. What follows is the mechanical part; taste only enters at the base hue.

**Think in HSL, not hex.** `hsl(220, 90%, 56%)` tells you hue, saturation and lightness, and you can
move one without wrecking the others. `#3B82F6` tells you nothing, so every adjustment is a guess
followed by a squint. Author and reason in HSL even where the token file stores something else.

- **Compose** has `Color.hsl(h, s, l)` and `Color.hsv(...)` built in.
- **SwiftUI has no HSL initialiser.** `Color(hue:saturation:brightness:)` is **HSB/HSV** — the third
  parameter is *brightness*, not lightness, and they are not the same axis. Feeding HSL numbers into
  it silently produces the wrong colour: HSL 50% lightness at full saturation is the pure hue, while
  HSB 50% brightness at full saturation is that hue darkened by half. Convert HSL to RGB yourself,
  or keep the HSL triple in a comment beside the literal so the intent survives.
- A "keep both platforms diffable by eye" rule is a good reason to store one identical format on
  both sides. It is **not** a reason for that format to be hex — move both together.

**You need more colours than you think.** Not one blue — nine. A working palette is roughly:

- **8–10 greys.** This is where most of the UI actually lives, and it is the set engineers
  under-build most.
- **5–10 shades of the primary.**
- **5–10 of one or two accents.**
- **Semantic sets** — red, yellow, green — each a small ramp, not a single value.

**Build a ramp by picking three points and filling in, not by turning a lightness dial.** Choose the
base (the one you would call "the blue"), then the lightest and the darkest ends, then fill the gaps
evenly. Mechanically stepping lightness off one hex produces dead, washed-out midtones, because it
holds saturation constant while lightness moves — which brings us to the rule that fixes it.

**The saturation rule: the further a shade sits from 50% lightness, the more saturation it needs to
hold the same perceived intensity.** Very light and very dark steps go grey without a bump. This is
the single most useful number-level fact about colour, and it is why "same hue, same saturation,
different lightness" ramps look lifeless.

**Never pure grey.** `#808080` is lifeless. Give greys a slight hue: cool (blue-tinted) reads
technical and precise; warm (yellow/red-tinted) reads friendly and human. Pick one direction and
hold it across the whole grey ramp — a mix of cool and warm greys in one UI reads as a mistake.

**Hold the hue constant on a grey ramp — but let warm chromatic ramps rotate as they darken.** This
is the exception that looks like sloppiness and is not. A yellow or amber at low lightness is
*perceived* as olive, not as dark yellow, so an amber ramp that keeps one hue turns khaki at its
dark end and stops reading as caution at all. Real ramps compensate: Tailwind's amber rotates
**43° → 22°** across its range, a deliberate 21° swing toward orange. Reds barely need it, greens
and blues need almost none. Verified the hard way — an amber built on a fixed hue 38 produced a
khaki "action required" card that was visibly worse than the ad-hoc value it replaced.

**Also: lightness is not luminance.** Two hues at the same HSL lightness have very different
contrast, so a shared lightness across a semantic ramp's steps is the wrong constraint. Green
needed L28 to clear 4.5:1 on white where amber cleared it at L39 — forcing amber down to green's
lightness is what made it dark enough to go khaki in the first place. **Solve each hue's step for
its own contrast**, then let the lightness land where it lands.

**Colour must never be the only carrier of meaning.** Colour-blind users, greyscale printing, a
phone in direct sunlight. Pair it with an icon, a label, a weight change, or a shape change. See
*Honest affordances* below — "change two channels, not one" is this rule applied to state.

### What this looks like when it has gone wrong

Measured on insure's iOS token set (2026-08-31), which is a fair example of a palette built by
accretion rather than construction. Every one of these was invisible in hex and obvious in HSL:

- **The primary ramp already existed but was never named.** Six distinct purples were in the file —
  L 87 / 76 / 56 / 52 / 44 / 39 — scattered across two `accent` tokens and the stops of two
  gradients. Nobody had built a ramp; a ramp had accumulated. **Look for this before designing a new
  one:** the shades a UI needs tend to get invented at the call sites that need them.
- **Its gaps were uneven and its saturation non-monotonic.** Lightness gaps ran 11, 20, 4, 8, 5 — a
  4-point gap is invisible, so two of those "different" shades were the same colour twice — while
  saturation went 100, 92, 78, 63, **83**, 60, jumping back up in the middle of the ramp.
- **`accentSoft` was darker and more saturated than `accent` in light mode** (L44/S83 against
  L52/S63) while being lighter in dark mode (L87 against L76). A token whose name is a lie in one
  of the two themes.
- **The greys were correctly tinted** (hue 232–240 throughout, cool) — this part was right — **but
  under-saturated at the dark end**: L81 carried S17% while L12 carried only S8%, so the darkest
  step was the most neutral, exactly inverting the saturation rule.
- **Two semantic tokens were byte-identical in one theme and different in the other** (`danger` and
  `notif`), which means the palette had not decided whether they are one concept or two.

### The alpha shortcut, and when it stops working

A palette with no ramp gets faked with opacity: one accent at 0.12 for a shadow, 0.18 for a border,
0.25 for a chip. It is seductive because it is one token, and it is **correct only over a known
ground** — an alpha-composited colour takes its hue from whatever is behind it. That is why it looks
fine on the app background and turns muddy the moment the same token lands on an accent card or a
fixed-white surface. Alpha is a legitimate technique for *one* surface; it is not a substitute for
named steps, and a token used at three different alphas is three shades wearing one name.

The same mechanism is why dark-mode neutrals built as white-at-alpha work: they inherit the hue of
the dark ground beneath them. Put one on a fixed-context surface that does not share that ground and
the inheritance stops, and the neutral goes truly neutral in a UI where nothing else is.

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

### SwiftUI-specific craft

- **Your flexible spacers *are* your vertical layout.** In a `VStack`, every bare `Spacer()` takes
  an equal share of the leftover height. So the count and the position of them decides where the
  empty space pools. One flexible spacer means every spare point collects in a single gap and the
  screen reads as two islands with a hole between them; two spacers split it. Deciding "should the
  slack sit above the keypad, below it, or both?" is a design decision — make it on purpose, not by
  leaving whichever `Spacer()` you happened to type.
- **A fixed top inset pins a header; a flexible one floats it.** If a screen has a brand mark, it
  wants a small *fixed* inset above it so it sits at the top of the screen. If it has no header, a
  flexible top spacer that centres the content block is usually right.
- **`Spacer().frame(maxHeight:)`** caps a gap while still letting it shrink on a small device — the
  tool for "breathe here, but not infinitely."
- Use `.continuous` corners (`RoundedRectangle(cornerRadius:style:.continuous)`), never the circular
  default. At radius 20 the difference is plainly visible, and the circular default is one of the
  small tells that an app was ported rather than designed for iOS.

### Ring, not fill — the low-weight key

On a dark ground, the default instinct for a tappable circle is a faint fill plus a hairline
border. It is the wrong way round: a 5–10% white fill under a 10% white hairline reads as a grey
blob whose edge you have to hunt for, and a grid of them lays a slab of lifted surface under the
brightest thing on the screen.

**Draw the edge instead of the body.** A ~1.5pt accent-tinted ring over a near-transparent fill
(≈5% white) makes the bounds unambiguous, costs almost no visual weight, and lets the background
gradient run through the grid unbroken.

- **Hold the accent tint near a third** (0.3–0.4 alpha). Twelve rings at full strength is a colour
  flood — the exact failure the accent-not-flood rule exists to prevent. At this weight the brand
  colour registers as identity, and the white glyphs stay the brightest thing in the group.
- **Drop the drop shadow.** A shadow behind a near-transparent disc is visible *through* it. The
  substitute for depth is the ring itself, not a lift.
- **Solid fill becomes meaningful again** once nothing else is filled — reserve it for the selected
  or pressed state, where it now reads unmistakably.

### Feedback elements must not be the quietest thing on the screen

The classic hierarchy inversion: the element whose entire job is telling the user where they are —
PIN dots, a step counter, a progress bar — is drawn in the faintest grey available, while the
decoration around it is bright. Ask of every screen: *is the thing that reports state at least as
legible as the thing that just sits there?*

When a state element toggles, **change two channels, not one.** Hollow-to-solid (shape + colour)
survives a bright street, a small screen and a colour-blind viewer; accent-purple-vs-grey (colour
alone) survives none of them. Empty = ring, filled = solid disc is the cheapest version of this and
it pairs for free with the ring-not-fill key above, so the readout and the pad read as one system.

### A readout must state the expected length before the first keystroke

An input readout has two jobs, and only one of them is obvious. The obvious one is *how far along
am I*. The quiet one is **how long is this thing supposed to be**, and it is needed *before* the
user types anything — because a person carries PINs for several apps and a bank card, and the
length is the hint that selects which secret to recall. A readout that starts blank asks them to
remember with no prompt at all.

Fixed placeholder slots do this for free: six dashes, six ghosted glyphs, six segments. A row that
*grows* as you type — which is what most reference pads show, because a mock is always drawn
mid-entry — silently drops the cue. **Judge an input component in its empty state, not the state
the mock happens to depict.**

Best current form: one short rule per slot, with the segment under a filled slot brightening. It
carries the length, the progress, and a second read of progress from the rules alone, with no extra
label and no width change as it fills.

### Reference mocks show one state; you ship all of them

Related and more general: a mock is a single frame. It shows the pad mid-entry, the list populated,
the form valid. Every affordance that only exists in the states the mock *didn't* draw is invisible
during a copy, and gets deleted without anyone deciding to delete it. Before adopting a reference
layout, enumerate the states it does not show — empty, first-run, one item, error, too long — and
check what the original design was doing for each that the reference frame cannot tell you.

### Caption a key whose glyph is a guess

An icon-only key in a group of labelled ones (a backspace among digits, a clear/enter among
numbers) makes the user decode an icon mid-task. Put a small caption under it — the reference pads
that do this well all do. **The caption carries the muted grey, never the accent:** naming a key is
help text, and help text that shouts competes with the content it exists to support. Use the same
string for the caption and the VoiceOver label so the two cannot drift.

### Circle grids need a tighter row gap than column gap

For a keypad or any grid of circular keys, equal row and column gaps **look** unequal: the gap
between two rows is perceived rim-to-rim through the widest part of each circle, while the gap
between two columns is read across the same rim at eye level. Equal numbers therefore read as
taller than they are and the pad falls apart into loose rows instead of one block. Set the row gap
noticeably smaller than the column gap (insure's keypad: 10 vertical against 16 horizontal).

### Don't chip what the ground already contrasts with

A solid pill or badge is the reflex for "make this stand out", and on a busy or low-contrast ground
it is right. On a **saturated, known ground** — a brand-coloured hero card, a dark gradient — it is
usually paying for prominence that size and weight give away free. Large white type on saturated
purple is already the highest-contrast thing that can sit there; wrapping it in a white box adds a
second bright rectangle to a card that probably already has one, and the two then compete.

Test before reaching for a chip: *is the plain type on this ground already legible?* If yes, promote
it with **size, weight and position** and leave the ground showing. Save the solid fill for the
element that genuinely needs to survive an unpredictable backdrop.

Corollary — **pick state tints for the ground they sit on, not from the palette's usual meanings.**
A danger red chosen against white (#EF4444) put on saturated purple is two strong colours of similar
value fighting, and reads as *less* urgent than the calm white it was meant to escalate past. On a
coloured ground, escalate through light tints (a pale amber, a light salmon) that stay high-contrast
against it.

### A logo IS the name — show one, fall back to the other

Printing a partner's logo *and* their name side by side spends the best corner of a card saying one
thing twice. Show the logo; let the name be the fallback for when the logo is missing or fails to
load, not its caption. That frees the space for the fact that actually changes.

Two details that decide whether the fallback reads as designed or as broken:

- **Fall back on failure, not on "not loaded yet."** Rendering the name during the loading state
  flashes text for one round trip and then swaps it for the logo — that reads as a glitch. Reserve a
  clear box of the right size while loading, and substitute the name only once the load has actually
  failed.
- **Bound third-party logos on both axes, fix neither.** Partner marks arrive as wordmarks, squares
  and everything between; a fixed `width × height` letterboxes the wide ones into illegibility. A
  height budget with a width ceiling gives every brand the same *optical weight* instead of the same
  box.

### Third-party logos span the full luminance range — audit before choosing a ground

Partner, carrier and payment-provider marks are not a palette you control. Measured across one real
set of eight, the ink ran from **luminance 26 (near-black) to 255 (pure white)**. No single
background survives that: a white plate erases the white-ink marks, and a coloured or dark ground
erases the dark ones. Whichever you pick by eye will look correct against the two or three logos you
happened to test with and silently blank one of the others in production.

So make it a measurement, not a judgement. Before choosing the ground:

1. Render **every** mark you might receive against **each** candidate ground, side by side, as one
   contact sheet. It takes minutes and it is the only way to see the failures.
2. Pick the ground that works for the **most** of them — usually white, because that is what brand
   guidelines assume their logo will sit on.
3. Treat the remainder as **wrong assets, not layout problems.** A white-only mark needs a
   colour-on-white variant sourced from the partner; no amount of frame tuning reaches it.

Also check the **alpha bounding box** while you are there. Baked-in transparent margin is common and
invisible in code review — one real asset was 47% empty vertically, which made a correctly padded
chip look hollow and sent the reviewer hunting through padding values that were never the cause.
When a container looks too big for its contents, measure the contents before you touch the container.

### The changing number is the point of the card

On any card summarising a thing the user owns — a policy, a subscription, a delivery — most of the
content is a **look-up they already know**: which car, which product, which address. Exactly one
field is usually live and actionable: the countdown, the balance, the status. If the look-ups are
set large and the live number is 10pt at the bottom, the hierarchy is inverted no matter how good
the card looks.

Ask of each field: *does the user already know this, or are they here to find it out?* The answer
orders the card.

## Icons & assets
- Use one consistent icon set (Lucide is the working stand-in). The gold standard is a **bespoke brand icon set** (custom SVGs matching the brand) — a designer deliverable; wire as vector drawables when it arrives. Don't mix icon sources.
- Bespoke 3D/illustration assets *are* the look — port them, don't substitute generic icons.

## Per-screen pre-flight checklist
1. What's the **one anchor**? 2. Is color an **accent**, not a flood? 3. Cards from the **calm-card** pattern? 4. Type from the **scale**, color from **tokens** (no inline)? 5. Is **every gap a scale value**, and is inside-a-group clearly tighter than between-groups? 6. Is **line height set per step** (tight headings, tall body) rather than defaulted? 7. **Honest states** (disabled looks disabled; gated on the real precondition)? 8. **Light + dark** both right? 9. Any **web-only effect** that needs a native substitute (flagged)? 10. Is every shade a **named step on a ramp** rather than one token at three alphas, and does any meaning ride on **colour alone**? 11. Does it look **intentional**, not AI-default?

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
