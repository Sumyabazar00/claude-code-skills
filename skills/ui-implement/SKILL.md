---
name: ui-implement
description: Implement UI inside an existing product so it matches the product's established aesthetic and covers every real-world state. Use when the user asks to build a feature screen, modal, form, dashboard, list, or component inside a codebase that already has its own look and feel — especially when they've provided design mocks, or when the work lives in a repo with existing admin/user pages to match. NOT for greenfield creative work (use `frontend-design` for that).
---

# ui-implement

Use this skill when writing UI that lives inside an existing product. The job is not to invent an aesthetic — the product already has one. The job is to match it, wire real state, and cover every edge case users will actually hit.

This is the skill for brownfield UI work. For greenfield creative projects, use `frontend-design` instead.

## Why this skill exists

The common failure mode in brownfield UI work:

1. Read the task, jump to a template, write generic SaaS-looking output
2. Cover happy-path only — no skeletons, no offline, no empty-vs-empty distinction
3. Invent visual primitives (new card style, new button variant) instead of using what the codebase already has
4. Hardcode display strings that should be translated
5. Ship functional-correct but design-wrong UI that then gets entirely redone

Each gate below exists because skipping it causes one of those failures.

## Gate 0 — Design source check

Before writing a single line of template, answer:

**Is there a pixel-level mock for this screen?**

- **Yes** → Describe the mock back in words before coding. Force yourself to actually read it: layout regions, spacing rhythm, color roles, copy, icon choices, state variants shown. Treat anything not in the mock as a decision you need to make deliberately (not accidentally).
- **No** → Stop. Either ask the user for a mock, or explicitly enumerate the aesthetic choices you're about to invent: "No mock provided — I'll be deciding card radius, empty-state tone, error-state treatment, and icon set. OK to proceed, or do you want to provide mocks first?"

A prose design doc ("card with usage breakdown") is not a mock. It pins behavior, not taste.

## Gate 1 — Aesthetic audit

Before writing any new component, open **3–5 existing components in the codebase that solve a similar problem** and extract:

- Color tokens actually used (not the full palette — what's in practice)
- Card/container treatment (radius, padding, border vs shadow, dark-mode variants)
- Spacing rhythm (gap-2 vs gap-3 vs gap-4 — pick the one the codebase uses)
- Typography scale (what size is a section title? a label? a caption?)
- Icon set (heroicons-outline? solid? mixed? what sizes?)
- Empty-state pattern (illustration? icon+copy? link-to-action?)
- Loading pattern (spinner? skeleton? inline vs overlay?)
- Error pattern (inline text? alert card? toast?)
- Modal size conventions (sm:max-w-md? sm:max-w-3xl? what's standard here?)

Write these down — even informally in your scratch. You're building a compressed style guide from code, not from a style guide.

If the codebase has no established pattern for something you need, that's a signal to ask before inventing.

## Gate 2 — Translation / copy inventory

List every user-facing string you'll display, before writing the template. Group them:

- Titles / headings
- Labels
- Body copy / descriptions
- Button labels
- Empty-state copy
- Error messages
- Success messages
- Placeholders / helper text

Seed them in one batch. If the project has a translation system (e.g. `translate('key')` pattern), don't sprinkle keys as you go — that leaks untranslated English into the UI. Inventory first, seed once, reference in template.

## Gate 3 — State coverage matrix

Every screen or significant component must handle:

| State | When | What the user sees |
|---|---|---|
| Loading | First fetch in-flight | Skeleton (not spinner unless <400ms) matching final layout shape |
| Empty — first-time | User has no data yet, never will have without action | Guidance ("You haven't created any X") + primary CTA |
| Empty — cleared | User had data, now has none (filter, resolved all, etc.) | Different copy ("All caught up" / "No results for filter X") |
| Error | Fetch failed | Inline card with: what failed, why (if known), retry button, alternative path if one exists |
| Offline | Network unavailable | Distinct from generic error — icon + copy acknowledging connectivity, retry |
| Success | Data present | The actual UI |

Before declaring a screen done, go through this list. If a state doesn't apply, say why. If a state applies but isn't designed, flag it.

"First-time empty" vs "cleared empty" is the one most often collapsed into a single state. Don't collapse it — a brand new user seeing "All caught up" is jarring.

## Gate 4 — Use established primitives

If the codebase uses `UButton`, `UCard`, `UModal`, `UInput`, etc. — use them. Don't write raw `<button class="...">` when a variant of `UButton` exists. Don't invent a new card style when `UCard` is the convention.

Before introducing a new reusable primitive, ask: does the pattern recur ≥3 times, or am I solving a one-off?

## Gate 5 — Data wiring conventions

Before implementing, grep for how the codebase handles:

- Fetching: `useCustomFetch` / direct axios / composables / react-query — use the project convention
- Auth context: `auth.GetTokenInfo(c)` / `useSession` / whatever — don't reinvent
- Real-time: socket / SSE / polling — if the codebase uses sockets, don't default to polling
- Encryption / envelope: some platforms wrap every response (AES, etc.) — use the existing helper, don't bypass it

One grep upfront saves a rewrite later.

## Gate 6 — Multi-screen consistency pass

When the feature includes >1 screen/modal (usually the case), after you've built them all, do a single pass across the whole set:

- Same concept → same icon? (not `clock` in one place and `calendar` in another for "due date")
- Same action → same button color/placement?
- Error card styling identical across all screens?
- Empty-state tone consistent?
- Copy tone consistent? (formal vs casual, "You" vs imperative)

This pass usually finds 3–5 inconsistencies that would otherwise ship.

## Gate 7 — Verify, then declare done

- Typecheck: run it and look only for errors in files you touched
- Dev server: load the feature, click through the happy path AND at least one error/empty state
- If you can't test the UI in a browser, say so out loud — don't claim success from typecheck alone

## Anti-patterns to watch for

- **"Generic SaaS default"**: defaulting to Stripe/Linear aesthetic when the product has its own voice
- **"Pretty screenshot, hollow edges"**: polished happy path, no loading/error/empty
- **"Hardcoded strings"**: English text in template when project has a translation system
- **"New primitive for a one-off"**: introducing a new component variant for a single use
- **"Polling when sockets exist"**: ignoring existing real-time infrastructure
- **"Mocking with lorem ipsum"**: building with fake data shapes that don't match the real API

## When in doubt

- No mock? Ask, or enumerate what you're about to invent
- Can't find the pattern? Ask, don't invent
- Weird edge case you're not sure is real? Ask — the user knows their product

The cost of asking is low. The cost of a whole redesign pass is high.
