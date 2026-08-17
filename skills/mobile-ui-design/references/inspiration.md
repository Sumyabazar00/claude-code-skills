# Inspiration library

Designs Sumiya likes, each saved with **"what to take"** (the transferable idea) and **"what to adapt"** (so we don't copy blindly — everything gets translated into our calm-card + token system, not pasted).

When a new liked design comes in: add an entry here. Keep the actual image in `assets/` if useful; describe it well enough to apply even without the image.

---

## #1 — Profile UX/UI Redesign (Seva Kogaev) — added 2026-06-07
A before/after profile screen. "Before" = one long flat list of label/value rows. "After" = clean, grouped, breathable.

**What to take:**
- **Group into sectioned cards**, not one long list. Distinct labeled sections ("My documents", "Banking Documents") with their own card grouping.
- **Centered avatar header** — larger avatar, name centered *below* it, with an edit affordance on the avatar — instead of a small left-aligned avatar+name row. Gives the screen its anchor.
- **Document items as horizontal TILES** (Passport / Social Security / TIN as a row of equal tiles with an icon + label + masked value) instead of stacked list rows. Compact, scannable.
- **Recessed-but-legible secondary text** — values are muted/light but still readable; labels small and quiet above the value.
- **Generous whitespace** + soft grouping = the "calm" feel.

**What to adapt:**
- It's bright iOS-white. Translate to our calm-card + dark-aware tokens (both themes), our accent, our type scale — don't import its raw colors.
- Our profile already has the grouped contact card + icon rows; the upgrades to steal are the **centered avatar header** and the **document/vehicle tiles** pattern.

**Applies to:** our Профайл/Миний tab; the tile pattern also fits any "set of small items" (saved vehicles, documents, policies summary).

**Applied 2026-06-07** — insure Миній tab redesigned with the centered avatar header + recessed contact rows (graduated into the component catalog: "Profile header", "Recessed contact row"). The document-tile pattern is still on the shelf for when we have a set of small items to show.
