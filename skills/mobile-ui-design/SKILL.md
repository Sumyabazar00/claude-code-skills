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

## The laws — the shared vocabulary

Heuristics, not physics. But they are the words designers actually argue in, and each one names a
failure that keeps recurring. Every rule further down this file is one of these laws made specific.

**Jakob's Law — people spend almost all their time in *other* apps, and expect yours to work the
same way.** The most important one here. Novelty in navigation, form layout, icon meaning, or what a
tap does is nearly always a mistake. Spend originality on what actually differentiates the product;
make everything else boring and familiar.
- *Tapping a photo opens the photo.* It does not retake it, and it does not do nothing. The
  assessment step shipped thumbnails that opened nothing and the evidence wizard shipped a shot card
  whose tap **replaced** the photo — a destructive answer to the gesture people make to inspect
  something.
- *A capture control offers camera or library.* Routing straight to the camera and demoting the
  library to a small link was "faster" and was wrong: the link went unnoticed entirely.
- *Affirmative right, escape left.* A camera overlay with only a Cancel on the left and an empty
  trailing slot is missing the Done that every camera on the phone has.
- Before inventing an interaction, name three apps the user already uses that do it the other way.
  If you cannot, you are not being original, you are being unfamiliar.

**Fitts's Law — time to hit a target grows with distance and shrinks with size.**
- 44×44pt minimum for anything tappable, and the *hit area* is not the same as the ink: a 6pt
  progress segment needs `contentShape(Rectangle().size(width: 44, height: 44))`.
- Primary actions live in the thumb's reach — the pinned bottom bar, not the top of a scroll.
- Destructive controls go **away** from primary ones. A delete that shares an edge with a submit
  will be hit by someone eventually.
- Screen edges and corners are effectively infinite targets; a bar pinned to the bottom edge is
  easier to hit than the same button floating 20pt above it.

**Hick's Law — decision time grows with the number of options.** Ten menu items is not more helpful
than five; it is slower and more tiring.
- Show the common path; hide the rest behind progressive disclosure.
- A dialog that always offers the same two answers and always gets the same one is a tax, **but**
  see Jakob: removing the second option entirely is worse than the tax. Reduce options, do not
  amputate them.
- One question per screen beats a form of twelve fields. If a field can be answered *for* the
  customer from data already held, that is one fewer decision — the insurer picker is answered from
  their own policy rather than offered as a list of every company in the market.

**Miller's Law — working memory holds ~4–7 chunks.** Do not make people carry things between
screens.
- **Show the summary at the confirmation step.** This is the whole reason a review screen exists:
  the customer entered a description, a date, an insurer, three answers and several photos across
  two screens, and cannot be expected to hold them while deciding whether to pay.
- Chunk long content into labelled groups of a few rows rather than one list of fifteen.
- A number the user has to reassemble across a line break has already failed this.

**Aesthetic–Usability Effect — attractive interfaces are *perceived* as more usable, and their flaws
are forgiven.** This is the business answer to "why does polish matter". It is not licence to
decorate: it means the cost of a screen that looks unfinished is paid in trust, not just in taste.

**Peak–End Rule — an experience is judged by its most intense moment and by its ending.**
- **Error states and success confirmations therefore deserve disproportionate care**, more than the
  happy middle of a flow. They are the two things people remember.
- A success screen that is a data dump with a grey "Done" button is an ending that says "form
  submitted". The ending should say what happened, what it means, and what happens next.
- The most intense moment is usually the moment of payment or of failure. Those two screens carry
  the memory of the whole flow.

**Von Restorff (Isolation) Effect — the thing that differs is what gets remembered.**
- **One primary button per screen. Two primaries are zero primaries.** If a screen needs a second
  action, it is secondary (outlined) or tertiary (plain), never a second filled slab.
- The corollary is the one that keeps being missed: **a control that differs from nothing reads as
  nothing.** Accent-coloured text among body text is emphasis, not a button — it has no edge, no
  fill, and no target. Shipped twice on one screen and unnoticed both times.
- Also the reason the single accent works: on a calm neutral screen the *one* saturated element is
  unmissable. Flood the screen with colour and the isolation effect is spent.
- Before adding a control, ask whether the primary action can simply **say the other thing**
  instead — a button whose label is always "what the next tap does" beats a button plus an override
  beside it.

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

### A number never wraps, and something else must give

Translation and a wider face both cost horizontal space, and a row that runs out of it does not
report an error — the layout engine resolves it by breaking the *text*. When the text it picks is a
number, the result is not a tight layout, it is a **wrong value**: `3,595.21` set as `3,59` / `5.21`
is read at a glance as 3,59. Same for a measurement and its unit, which are one token: `3м/с`
stacked as `3м` over `/с` reads as neither.

So decide the order of sacrifice explicitly, before the device does it for you:

1. **Make the readings incompressible** — one line, intrinsic width. Numbers, units, currency.
2. **Then name what gives way.** There is almost always a label whose job is already done by
   something else in the row — a word beside a chevron that means "expand", a caption under an
   unambiguous icon. That is the element to drop.
3. **Drop it, don't truncate it.** "Дэлгэрэн…" loses the meaning *and* announces the failure;
   removing the word loses only a hint the chevron still gives.

The native tool for step 3 is a **fit-first container** (SwiftUI's `ViewThatFits`, and the same idea
by hand elsewhere): offer the full arrangement, then the reduced one, and let the runtime pick. No
breakpoint, no size class, and nothing to re-sync when the copy or the font changes — which is what
makes it survive the *next* translation rather than only fixing this one.

**Truncate a name only when nothing identifies it.** A tile whose subtitle is cut is still findable;
a tile whose *title* is cut cannot be identified at all. Give the name the second line — and if it
sits in a grid, reserve that line on every cell (`reservesSpace`) so one wrapping title does not
make its row ragged.

### Review on the narrowest device you ship, not the newest simulator

The default simulator is whatever is newest, which is the widest phone in the range. Every
width defect is invisible there, so a screen can pass review and fail on the device in the user's
hand. Measured on one real app: five truncation and wrapping bugs at 375pt, **zero** of them visible
at 402pt.

Anything that reads well narrow reads well wide; the reverse is not true. So make the narrow device
the default for review and use the large one to check that a layout does not simply stretch. And
when a string is close to its container, **measure rather than squint** — the failure margin is
routinely 1–2%, which no amount of looking will settle. Read the advance widths out of the actual
font file for the actual string.

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

## Rule 5 — depth: shadows, borders, and the layer stack

**Shadows encode elevation.** Keep a set of about five, from barely-there to dramatic, and use them
consistently: small for cards and buttons, medium for dropdowns and popovers, large for modals. The
size of the shadow tells the user how far off the page the element is. One shadow used everywhere
tells them nothing — it is decoration, not information.

**Combine two shadows.** Real depth needs an *ambient* shadow (large, soft, low opacity, spread
wide) plus a *direct* shadow (smaller, tighter, offset downward). One shadow looks fake; two looks
physical.

**Borders are often the wrong tool.** If you are adding a border to separate two things, ask first
whether spacing, a background colour shift, or a shadow would do it. **Too many borders is the #1
visual signature of engineer-built UI** — it is the reflex reach, because a border is the one
separator that is trivially expressible in code.

**Accent borders add colour cheaply.** A 3–4px coloured border on the top of a card or the left of
an alert gives you personality without redesigning anything.

### A tinted fill under a matching ring says one thing twice

The specific shape a notice grows by hand: a semantic tone at ~10% behind the content, and *the same
tone* at 30–40% as a full 1px ring around it. It looks careful and it is redundant — the colour is
carrying the message on two channels, and the block still reads flat, because a symmetrical ring
adds no structure a fill has not already given.

**Replace the ring with an edge, not with nothing.** A 3pt bar down the leading side puts the tone
at full strength exactly where the eye enters the block, leaves the other three sides to the fill,
and gives the notice a *direction* — which a ring, by construction, cannot. It is also the one
place this rule endorses a border outright: an accent edge is personality, not a fill's understudy.

Two boundaries worth holding:

- **The failure is tint-on-tint, not colour-on-a-card.** A warning hairline around a *neutral* card
  fill is the endorsed accent border — it marks a card that needs attention. A warning hairline
  around a warning fill is the repetition. The fill underneath decides which one you have.
- **A notice is not a button.** A bar on a centred label is a notice pretending to be one; tonal
  controls take the fill alone. And red is not one role: a routine action and a destructive one can
  share the ink while differing in weight. Give the fill only to the one that cannot be undone —
  otherwise the calmest screen in the app acquires its loudest element for its least important
  action.

### Which way is "down" belongs to the call site

Every step of an elevation scale offsets its shadow downward, because the mental model is an object
lying on a page. A bar docked to the **bottom edge of the screen** breaks that: the content it
covers is *above* it, so its shadow falls **upward**. Give it the scale's downward offset and the
shadow is thrown off the screen entirely — the bar reads as painted on rather than laid over, and
nothing in the code looks wrong.

So let the step own the magnitudes and let the call site own the sign. One `edge` parameter, not a
parallel set of steps — a second scale for docked things is how a scale stops meaning anything.

### A border grep finds a shape, not a defect

When sweeping borders out of a codebase, resist counting hits. Grepping the border token returns one
syntactic shape and at least four unrelated jobs, and only one of them is the defect:

- the border **is the state** — a selection or focus ring that changes colour and width
- the border **is a divider** — a fill on a hairline rectangle, not a panel edge
- the border **is a button's outline** — the same exemption a card's own hairline gets
- the border **is standing in for a fill** — the only one to fix

Expect roughly a fifth. Counting the grep produces a confident number that is wrong in the expensive
direction, because it becomes the plan for the next session.

**Reduce the shadow on press.** Button pressed = element moves toward the page = smaller shadow.
Two lines, and it makes the whole thing feel real.

### A translucent fill cannot carry a shadow

The one that is invisible in code and obvious the moment you sample a pixel. A shadow is painted
*behind* the view (SwiftUI `.shadow`, and the same is true of any compositor-drawn shadow), so a
card whose fill is less than fully opaque shows its own ambient layer **through itself** and comes
out darker than an unlifted card beside it. The elevation step that was supposed to raise it makes
it look dirty instead — a perfect inversion, from a token that reads as harmless.

Measured on one real screen: page (244,246,249), the *lifted* card (247,248,250), the flat card
below it (253,254,254). The raised card was six units closer to the page than the flat one.

Two things follow:

- **Give card fills an opaque value**, at whatever the translucent one already composited to over
  the page. On a flat ground that is a no-op visually and it makes the whole elevation scale mean
  something. Translucency stays legitimate where it is doing real work — a dark theme's neutrals
  built as white-at-alpha inherit the ground's hue, which is the point of them.
- **Check it by sampling, not by looking.** A three-unit error reads as "hmm, that card looks a bit
  grey" and gets argued about; the numbers settle it in one command.

Corollary worth its own line: **an elevation helper built on `.shadow` needs no corner radius.**
`.shadow` derives its shape from the view's rendered alpha, so applied *after* the clip it already
follows the rounded corner — and applied before, it gets clipped away. A radius parameter on such a
helper is a value every call site passes and nothing reads, which is a worse trap than no argument
at all. (A helper that paints an explicit round-rect to emulate CSS *spread* does need one — that
is a different mechanism, and the two should not share a signature.)

### The nesting trap: a lighter panel on a light card is not a layer

The specific way this goes wrong in a token set. A card sits on the page as `surface`; someone needs
a panel *inside* it and reaches for the next token along, which is usually named something like
`surfaceStrong` — and on a light theme that token is **lighter** than the card, often pure white
against the card's 85%-white. On a near-white page that is a contrast of almost nothing, so the
nested panel does not read as nested; it does not read at all. Measured on one real set: light
`surface` = white @ 85%, `surfaceStrong` = pure white; dark was 4% white against 6% white.

**Layers go one way: content nests *downward*, not upward.** An inset panel is *recessed* — a step
**darker** than its parent in light mode (and darker in dark mode too, since the parent is already a
lightening of the ground). An element that floats *above* the card earns a shadow, not a lighter
fill. Reaching for the lighter token is the reflex because "more important = brighter", and it
inverts the physical model the whole system is built on.

The tell that you are in this trap: you find yourself adding a **border** to make a nested panel
visible. That border is a workaround for a fill that is not doing its job, and it lands you straight
in the "too many borders" signature above.

### A card is a card everywhere, or it is not a system

Full-bleed artwork tiles are the usual place a card family quietly breaks. The image goes to the
corners, so the surface, the hairline and the lift all look redundant while you are building it —
and the tile ships as a bare rounded image. Then it sits next to real cards and reads as a stray
graphic that someone forgot to finish, which is exactly what a user reports as *"those look like
just images."*

An image tile still gets the card's chrome, and for reasons that survive the artwork changing:

- **Transparent PNGs.** An illustration with no background is a floating cut-out on a bare tile; on
  a surface with an edge it is a card with a picture on it.
- **Artwork lighter than the page.** A pale image with no lift has no boundary at all.
- **Any admin-uploaded asset**, which is every size, format and background you did not choose.

If the artwork genuinely must run to the corners, it still needs the *lift* even where it does not
need the fill — the shadow is what states "this is an object on the page."

## Honest affordances (states must tell the truth)

- **Real disabled state** — a disabled primary button must look inactive (muted fill + dimmed label), not the full gradient. It must *visibly change* disabled→active when it becomes usable.
- **Gate on the real precondition**, not a proxy (e.g. enable Continue when the lookup actually *succeeded*, not when the field merely has N characters).
- **Hide the input once you have the result** (e.g. hide a keypad after a successful lookup) so the result reads clearly; give a tap-to-edit path back.
- **Echo a looked-up key back** in the result for at-a-glance verification (show the matched plate/id in the result card, sourced from the response — never the typed input).
- **Fixed-length / typed inputs**: cap + filter to the valid format; use the right keyboard; security inputs (PIN) get a branded on-screen keypad, never the OS keyboard.
- **Every action gives feedback** — success or the real error. No silent completions.
- **A switch's hint says why you would turn it on, not what it does.** "Turning this on shows only
  covid products" narrates the mechanism, and the list changing under the customer's thumb says
  that already. What they cannot see is the reason to want it: "if your destination requires covid
  cover, or you want the risk of falling ill with it on the trip covered." Name the situation that
  makes the answer yes. (Sumiya, 2026-09-14.)
- **A confirmation names people by the record, not by a pronoun.** "Өөрөө" (self) on a review
  step verifies nothing: it is true whatever the account's stored name and ID are, including when
  they are wrong. Print what the server holds for the insured — the name as it will appear on the
  contract and the ID number — sourced from the response, so a wrong record is caught before the
  customer pays rather than on the policy.
- **An estimate is shown at the precision it has, with its spread.** A model's output carries every
  digit ("34,512,346₮"), and printing them claims an accuracy an estimate does not have: the false
  digits make the number read *less* trustworthy. Round to the step the estimate actually resolves
  (₮100,000 for a used-car price), show the range beside the point value, and state the source. One
  number alone reads as a quote. Round for display only; the unrounded values stay the API contract.

### A filter's selected state is not an answer's selected state

Two controls look alike and do different jobs, and the selected state has to say which one it is:

- **A filter or tab chooses what the screen shows** ("Миний / Гэр бүл", "Бүгд / Хариу шаардлагатай").
  Its selected state can be a solid ink slab: it is a mode switch, there are two or three of them,
  and the slab states "this is the view you are in".
- **An answer chooses a value that becomes data** — a trip's purpose, a day count, a cover, a
  region. It takes the system's **option-selected state**: accent wash, accent ring, and a check or
  a heavier ring as the channel that is not colour. The same state an option card has.

Borrowing the filter state for answers is the mistake that looks like reuse. A 3x3 grid of day
counts with one solid near-black tile in it puts the heaviest object on the screen in the middle of
a question, where it competes with the one primary button (Von Restorff: two slabs are zero). And a
flow whose region card, cover row and day tile each use a different selected state has three
dialects for one idea.

**Before reusing a chip, ask whether the tap changes the view or answers the question.** If the
component's name says "filter" and the tap answers, write the answer variant instead of passing a
flag.

### Layout can rank what the data cannot compare

Putting a number on every row of a list turns the list into a ranking, whether or not anything
else says "cheapest". That is right when the rows are like for like, and wrong when they are not —
insurers in one travel region sell different covers, variants and limits, so their lowest premiums
are not comparable, and a price on each collapsed row invites exactly the comparison the product
cannot stand behind.

**Show on the collapsed row only what is fair to compare** (who sells here, which covers, how many
options), and put the number inside, where it sits beside the thing it is the price of. Order by
something neutral (the name), and do not pre-open the first row, which is its own recommendation.

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

- **`.dynamicTypeSize(...)` does not bound a font you built through `UIFontMetrics`.** The two
  scaling mechanisms look interchangeable and are not: `@ScaledMetric` reads the SwiftUI
  environment, so the modifier clamps it; `UIFontMetrics.scaledFont(for:)` resolves against the
  *global* content size category at the moment it is called and bakes the result into the `Font`,
  so the modifier is never consulted. Put the modifier on a view containing both and you get a
  layout that half-obeys: icons hold still while labels sail past them, which reads as a mystery
  layout bug rather than as two mechanisms. To bound text, build the font against an explicit
  `UITraitCollection(preferredContentSizeCategory:)` — and take `min(current, cap)`, because a
  bound is a ceiling and a reader at the small end must keep their setting. Cap the leading too, or
  a bounded face pairs with an unbounded gap.
- **`fixedSize` has no ceiling, and an incompressible child can push a whole page off the screen.**
  Making a number incompressible is the right answer to "a rate rendered as `3,59` over `5.21`" —
  a broken number is a wrong number, not a tight layout. But `fixedSize(horizontal: true)` states
  *this must never shrink* with no upper bound, so at a large text size the element's minimum width
  becomes larger than the display, its parent stack inherits that minimum, and the scroll content
  goes wider than the screen. The symptom is nothing like the cause: the logo and the bell get
  clipped at opposite edges and every card bleeds past both margins, on a screen whose own layout is
  fine. **Incompressible is right; unbounded is not.** Pair every `fixedSize` with an escape at
  large sizes — reflow the row to a stack (`ViewThatFits`, or a branch on
  `dynamicTypeSize.isAccessibilitySize`), which keeps the number whole *and* full size, rather than
  capping the text, which overrules the reader's setting.
- **Fixed-height chrome must bound what it scales, and only chrome may.** A bar with a hard-coded
  height holding `@ScaledMetric` icons and scaling labels is a container that lies about its size:
  at accessibility sizes the contents burst it and collide with whatever else it holds. Bound the
  chrome so the app's *frame* holds still while its *content* scales the whole way. A cap on a
  paragraph is a different thing entirely — that is an accessibility setting quietly overruled.
- **At accessibility sizes, drop a chrome label rather than shrinking or truncating it.**
  `minimumScaleFactor` shrinks text *because* the reader asked for bigger text, which inverts the
  setting. `lineLimit(1)` leaves a word cut mid-syllable, which names nothing. The icon is a whole
  symbol at any size; pair dropping the label with
  `.accessibilityShowsLargeContentViewer { Label(name, systemImage: ...) }` so a long-press still
  gives the name at full size. This is what UIKit's own tab bar does, so it is also the Jakob
  answer.


### An explicit `withAnimation` overrides every implicit animation inside it

The one that costs three attempts, because the code is textbook-correct and the symptom is "nothing
moves". A looping indicator built the standard way — a state flag flipped in `onAppear`, with
`.animation(.repeatForever(...).delay(i * stagger), value: flag)` per element — renders **completely
static** when it is inserted into a container that animates its own insertion. An explicit
transaction wins over the implicit modifiers of everything it encloses, so the repeating animation
is replaced by whatever the enclosing `withAnimation` specified: a single 0.25s ease to the final
value, then nothing.

Nothing about the indicator looks wrong, so the search goes to the indicator. In one real case the
enclosing transaction was a chat transcript's own `withAnimation { proxy.scrollTo(...) }`, fired by
the same state change that inserted the indicator — two features, both correct, in different files.

**Drive a continuous animation from the clock instead of from state.** `TimelineView(.animation)`
recomputes its body every frame, so the value is derived rather than animated and there is no
animation for an enclosing transaction to take over:

```swift
TimelineView(.animation) { timeline in
    let t = timeline.date.timeIntervalSinceReferenceDate
    ForEach(0..<3, id: \.self) { i in
        let wave = (sin(t / cycle * 2 * .pi - Double(i) * stagger) + 1) / 2
        Circle().offset(y: -rise * wave).opacity(0.4 + 0.6 * wave)
    }
}
```

Reach for it for anything that loops **while being inserted into an animated container** — typing
indicators, pulsing badges, shimmer placeholders.

Two things that travel with it:

- **Animate position, not size, when the element is small.** Scaling an 8pt dot from 0.55 to 1.0
  moves 3.6pt: legible in two screenshots compared side by side, invisible at arm's length. The same
  dot *rising* 5pt is unmistakable, which is why every messenger typing indicator bounces. When a
  reviewer says "it isn't animating" and the frames say it is, the amplitude is the bug.
- **Don't silence a loading indicator under Reduce Motion.** That setting exists for parallax, zoom
  and large-scale movement; the system's own `ProgressView` keeps spinning under it. A progress
  signal that stops signalling is worse for exactly the people it protects — and a static fallback
  is indistinguishable from a broken animation, so it also hides the defect from you. Halve the
  travel; do not stop it.

**And when someone reports "no animation", measure before you rewrite.** Capture several frames a
few hundred milliseconds apart and diff the region. Three attempts went into the indicator above and
the third rewrite was unnecessary — the frames showed it moving all along, and the real faults were
amplitude and a Reduce Motion branch.

- **A picture with a free axis sizes its parent, and the damage lands on the text.** An image set
  `.resizable().scaledToFill().frame(height: H)` has no width constraint, so it reports
  `H x aspectRatio` as its *ideal width* — a 1500x841 upload wants 200pt inside a 168pt card. The
  enclosing stack sizes to the picture, the card's own `.frame(width:)` then centres that oversized
  content, and the clip shaves a slice off both edges. What you see is not a stretched image (a
  photo cropped by 16pt looks fine); it is **the first glyph missing from every text line** and a
  corner badge cut in half — a padding bug that is not a padding bug, on a card whose padding is
  correct. Put the artwork in as an `.overlay` on a `Color.clear` sized in *both* axes: an overlay
  never contributes to its parent's size, so the card's width stays the card's decision whatever an
  admin uploads. Reach for this the moment artwork stops being a bundled asset you chose.
- **A row of cards sized to their own content is not a set.** In a horizontal `ScrollView` each card
  measures independently, so one title that wraps to two lines makes that card taller than its
  neighbours and the row reads as broken. `lineLimit(2, reservesSpace: true)` on the title (iOS 16+)
  holds the height whether or not it wraps. Give the reserved line to the **name** and cap the
  subtitle at one: a cut subtitle is still identifiable, a cut name is not.
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
- **A `Spacer` in an `HStack` costs the stack's spacing twice.** It is a child like any other, so
  `HStack(spacing: 12)` puts 12pt on *both* sides of it, on top of its own `minLength`. In a row of
  chip · text · Spacer · chevron that is an extra gap the text column pays for — measured on one
  picker card at 375pt: 185pt of column against a 201.5pt title, which wrapped for want of 16pt.
  Give the text column `.frame(maxWidth: .infinity, alignment: .leading)` instead and drop the
  Spacer; the column takes the slack and the row has one gap per boundary. Measure the string before
  shrinking the chip or the type — the fix is usually structural.
- Use `.continuous` corners (`RoundedRectangle(cornerRadius:style:.continuous)`), never the circular
  default. At radius 20 the difference is plainly visible, and the circular default is one of the
  small tells that an app was ported rather than designed for iOS.

### Two flows sharing steps: share values and views, not a view model

When a second flow reuses most of an existing one's screens (domestic travel after international:
the same dates, cover list and boarding pass; a different first step, no details step), the reflex is
one view model with a mode flag. It then branches on that flag in every method, and each flow's
bugs land in the other.

Make the shared screens take **values and closures** (`trip`, `groups`, `onSelect`, `onContinue`),
and keep one view model per flow. Each flow's step is then a ten-line wrapper that passes its own
state in. The rules both flows must agree on live in a value type they both hold (`TripDates`: the
earliest start, the longest trip, the inclusive day count), not in a base class. A protocol-generic
step (`Step<Model: SomeProtocol>`) is the other way to do it, and costs a protocol that grows a
member for every difference; values make the difference visible at the call site.

Two traps that came with the refactor:

- **A long array-concatenation expression inside a view initializer can crash the type checker**
  (`failed to produce diagnostic for expression`), pointing at the whole `body`. Hoist the expression
  into a function returning the concrete type.
- **An unlabelled tuple passed where a labelled optional tuple of a closure is expected** fails the
  same way. Use a small struct (`CovidSwitch { isOn; set }`).

### An ordered multi-pick shows its order where the eye lands

A question whose answer is a sequence (a route through aimags, stops tapped in order) cannot show
the order in its chips: a chip says in or out, not first or third. Put the sequence in the step's
anchor and let it grow as the customer taps, so a stop tapped in the wrong order is visible before
it is stored. A vertical line of stops is the form people already read as a route (every transit
app). Make the ends solid and the stops between them rings, so start and finish read without
colour, and never cut a stop's name: it is the entire content of the row.

### Centre an overlay on a measured line with a zero-height frame

Placing a badge on a line you measured (a stamp on a boarding pass's tear line) by
`.alignmentGuide(.top) { $0[VerticalAlignment.center] }` then `.offset(y: measured)` inside an
`.overlay(alignment: .topTrailing)` did not take: the stamp landed 10pt low, top edge on the line,
exactly as if the guide were absent. `.frame(height: 0)` then `.offset(y: measured)` does: a
zero-height frame centres its content on its own top edge, so the offset puts the content's middle
on the line whatever its height. A hard-coded top padding (the first version's `183`) is correct
only for the one route height it was measured against, and breaks the moment a second flow puts a
taller section above the line.

### A cover's configuration belongs to its item, not to state beside a Bool

`fullScreenCover(isPresented: $open)` plus a separate `@State` that configures what opens (a filter,
a preselected product, a plate) is wrong on exactly one presentation: the first. The cover's content
is an escaping closure, and SwiftUI tracks a state only once some body has read it. If nothing
outside that closure reads the configuring state, the tap that sets it and flips the Bool builds the
cover from the value before the tap. Every later tap works, because the closure has read the state
once by then. So it survives every review that opens the screen twice, and it reads to a user as "it
ignored me, then it worked".

Measured on one app: Home's family card set `lines = .person` and `buying = true`; the first tap
showed all seven product lines, the second showed the two person lines. A DEBUG host that
constructed the flow directly passed, because it skipped the cover.

Present with `fullScreenCover(item:)` (or `sheet(item:)`) and put the configuration **in** the item:
`struct PurchaseRequest: Identifiable { let id = UUID(); var lines: Lines? }`. The content receives
its values as a parameter, so there is no second state to be stale, and a fresh id per request makes
two opens two presentations. Reach for it whenever a presentation carries anything more than
"open". And test presentation bugs through the real presenter, not a host that bypasses it: prove the
trigger reproduces the bug on the old code before trusting it to pass the new.

### Scroll an opened section into view from one place, after the fold settles

An accordion row that opens below the fold opens invisibly: the header moves, the new content lands
under the bottom bar, and the tap looks like it did nothing. The usual fix is a
`proxy.scrollTo(id)` in the tap handler, and it fails twice. Called in the same transaction as the
expansion, it measures the layout *before* the rows exist and lands short. And it only covers the
tap: when the view is rebuilt with the section already open (Back from the next step, a restored
state), nothing scrolls, and the customer returns to the top of a list whose answer is below it.

Drive it from the state instead: `.task(id: openSectionKey)` on the scroll content, which sleeps
~250ms (the length of the expand animation) and then `withAnimation { proxy.scrollTo(id, anchor:
.top) }`. The key must change on both the open id and the arrival of the data. One mechanism then
covers the tap, the restore and a late-loading list, and the scroll is measured in the final layout.

### Five things SwiftUI gets wrong by default, from one flow

Each of these rendered wrong on the first try while the code looked right.

- **`.position` animates in a straight line, whatever path you drew.** Move a plane along a flight
  arc by animating `.position(point(at: t))` and it slides along the chord between the two ends,
  because SwiftUI interpolates the *output* (a CGPoint), not the parameter you computed it from. To
  follow a curve, animate the parameter: a `ViewModifier` that conforms to `Animatable`, with
  `animatableData` set to `t`, whose `body` calls `point(at: t)` itself. SwiftUI then interpolates
  `t` and asks for a fresh position every frame. Same fix for anything that moves along a curve,
  a circle or a spline.
- **A `PreferenceKey`'s `reduce` decides which child wins, and the default is the last one.**
  `value = nextValue()` keeps the last reporter in tree order. When several children report a
  measurement (the y of a ticket's tear line, say), that is rarely the one you meant, and the
  result looks like the effect sitting at the wrong place, not like a measurement bug. Write the
  reduction you actually mean: `max`, `min`, or a sum.
- **Cut a hole, don't paint one.** Ticket notches drawn as circles in the screen's background colour
  look right on one background and wrong everywhere else: in dark mode, over a gradient, or behind
  a shadow, they read as dark dots stuck on the card. Punch them out instead: mask the card with the
  notch shapes using `.blendMode(.destinationOut)` inside `.compositingGroup()`. Then the card
  really has a hole, and whatever is behind it shows through.
- **A compact `DatePicker` ignores your layout and your locale's length.** With `mn_MN` its long
  date is too wide for a row and breaks it. It also shows English month names unless
  `.environment(\.locale, ...)` is set on the picker itself. Show the date as your own text in the
  row and open a sheet with a `.graphical` picker; the row stays a row, and the calendar gets the
  room it needs.
- **Artwork at the top of a `ScrollView` slides under the status bar.** A scroll view's content
  starts in the safe area when the background ignores it. A zero-height view placed above the
  ScrollView, in the same `VStack`, gives the scroll view a top edge below the safe area without
  pushing the art down by a fixed amount.

### Truncating a string is not a width budget

The card that pins artwork bottom-trailing in a `ZStack` and lays a text column over it at
`maxWidth: .infinity` is the most common layout in a product grid, and it reserves **nothing**.
Every line is free to run the full width and land on the picture. The trap is that the code looks
deliberate — the artwork has an explicit frame and an explicit alignment — while the only thing
holding the text off it is the copy happening to be short.

The reflex fix is `lineLimit(1)` on whatever overflowed. That is a guard, not a budget, and it fails
in two ways at once: the capped line still sits **on top of** the artwork (a cap controls length, not
position), and the words it drops were the message. A character cap in an admin form is worse again
— nothing in a CMS can know what a string renders to in a given face at a given size.

**Reserve the area, not the string.** Make the artwork's footprint an element of the layout — a
`Color.clear` of the band's height as the last child of the text column, with a `Spacer(minLength:)`
above it — so "the text cannot enter this region" is a structural fact rather than a coincidence. A
magic `minHeight` on the card does not do this: it holds the card's size while saying nothing about
where the content inside it may go.

Two consequences worth taking together:

- **Fix the band, scale the text.** The band is chrome and holds its size; the reserved text lines
  are metric-scaled. So a larger Dynamic Type setting makes the card *taller* rather than walking
  the copy back onto the picture — the correct direction, and free once the band is a real element.
- **Reserving a line and reserving the band are different jobs — do not let the first inherit the
  second's justification.** The band is what stops the collision; `reservesSpace` on a text slot
  only decides whether cards sit ragged beside each other. So a reservation has to earn its height
  on *alignment alone*, and most do not: a slot whose copy fits one line today buys nothing and
  costs a blank line on every card, forever. Reserve the slot that genuinely wraps (it holds the
  slots below it on one baseline across the row) and cap the rest without reserving. The slot with
  only flexible space beneath it — usually the last one — never needs it at all, because a second
  line there costs its neighbours nothing. Getting this wrong made one card 20% taller than the
  broken version it replaced.

And when a chip is what overflowed, weigh the chrome against the words: a capsule spends its
horizontal padding plus its icon — a quarter of a narrow card's line — on a wash that is barely
visible on a neutral surface. Dropping the pill and keeping the icon buys the copy back, keeps two
channels (symbol and weight), and gives you something that *can* wrap. A capsule cannot.

### Frame the ink, not the file

A set of illustrations is not a set of margins. Transparent padding is baked into artwork by
whoever exported it, it is invisible in code review, and it is different in every file — measured
across four illustrations shipped together: one was 95% x 98% ink, another 77% x 86%. Frame the
*file* and identical code gives every card a different optical inset. The bug reads as "that one
illustration is too big", so it gets answered per-asset by eye — a scale factor on the offender, a
`bleed` flag on the one whose ink is half its own height — and the fudge table then rots silently
when an asset is replaced.

**Draw the file at whatever size puts its ink inside the container, then offset so the ink's corner
lands on the container's.** Padding is then a real margin for every asset, and a replacement only
needs its rect re-measured rather than the layout re-tuned. It costs one normalised rect per asset
and about ten lines of arithmetic, and it removes every per-asset special case at once.

Write the derivation next to the numbers (`Image.open(p).convert("RGBA").getchannel("A").getbbox()`,
normalised to the file's own size). A wrong value here renders a lopsided card and never an error,
which is exactly why it has to say where it came from.

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
- **Country flags are a bundled image set drawn as discs, not emoji and not a CDN.** Emoji flags are
  small rounded rectangles in each OS's own art, so they fork between iOS and Android and cannot
  overlap into a compact stack; a CDN (flagcdn and the like) spends a request per flag, leaks which
  countries the user browses, and is someone else's uptime — countryflagsapi.com went dark in 2023
  and took every app on it with it. Flags change a few times a decade, so they belong in the binary:
  ~0.7 MB for ~250 square PNGs, one shape everywhere. Clip to a circle with a hairline ring so a
  mostly-white flag (Japan) keeps an edge on a white card, and overlap a stack only when it is
  truncated with "+N" — a short list spaced out is read flag by flag. Compared side by side on
  device before choosing; the discs won on shape and consistency.

## Per-screen pre-flight checklist
1. What's the **one anchor**? 2. Is color an **accent**, not a flood? 3. Cards from the **calm-card** pattern? 4. Type from the **scale**, color from **tokens** (no inline)? 5. Is **every gap a scale value**, and is inside-a-group clearly tighter than between-groups? 6. Is **line height set per step** (tight headings, tall body) rather than defaulted? 7. **Honest states** (disabled looks disabled; gated on the real precondition)? 8. **Light + dark** both right? 9. Any **web-only effect** that needs a native substitute (flagged)? 10. Is every shade a **named step on a ramp** rather than one token at three alphas, and does any meaning ride on **colour alone**? 11. Is every shadow a **named elevation step**, is depth carried by fill and shadow rather than by another **border**, and does any nested panel go *darker* than its parent rather than lighter? 12. Does it look **intentional**, not AI-default?
13. **Jakob:** does every gesture do what it does in other apps (tap a photo → view it; tap a field → edit it), and is anything here novel *on purpose*?
14. **Fitts:** is every target ≥44pt including its hit area, is the primary action in thumb reach, and is nothing destructive adjacent to it?
15. **Hick:** can any decision be removed, deferred, or answered from data already held?
16. **Miller:** does a confirmation or payment step show the summary, so nothing has to be carried from an earlier screen?
17. **Von Restorff:** exactly one primary button — and is every interactive thing visibly interactive (no accent-coloured text pretending to be a control)?
18. **Peak–End:** if this is a success, failure, or payment screen, has it had *more* care than the ordinary screens, not less?
19. **Selected states:** does every answer (a value that becomes data) use the option-selected state — accent wash, ring, check — and is the solid-ink slab left to filters and tabs? Is it one selected dialect across the flow?

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

### A loading indicator is the most expensive view on the screen, and the worst one to get wrong

`TimelineView(.animation)` re-evaluates its body **every display frame** — 60Hz, 120Hz on
ProMotion — for as long as it is on screen. That is the right cost for a 300ms transition and the
wrong cost for a wait, and a loading indicator is by definition on screen for the wait. Measured on
one real app: three animated dots inside the `LazyVStack` holding a chat transcript cost **22.6% CPU
on an M1 simulator**. A Debug build on an A13 is several times slower than that, and it saturated
the main thread.

The symptom is not a slow animation. It is a **dead screen** — the back button stops responding,
the keyboard stops responding, and the indicator itself never draws. The one element whose job is
to say "still working" is what stopped the screen working, which is also why it is invisible in the
place you would look for it.

`.periodic(from: .now, by: 1.0/20.0)` costs a sixth of the redraws and a rise-and-fall still reads
as smooth at 20Hz. (This is not the earlier failure of stepping a 0.4s `Timer` between two discrete
sizes — 2.5 updates a second ticks, 20 does not.) Reach for a periodic schedule for anything that
loops for an unbounded time; keep `.animation` for short, bounded motion.

**And measure CPU *during* the wait, not after it.** Two reproductions failed before this one was
found, both because the repro was wrong rather than the theory: the first sampled the process after
the work had finished, and the second ran on a simulator with the hardware keyboard connected, so
the software keyboard the bug needed never appeared. `sample <pid>` plus `ps -o %cpu` during the
suspect window is the whole diagnosis; a screenshot cannot tell a settled screen from a frozen one.
