# 00 — Vision

## One-liner
An endless, looping dive through four hand-built low-poly worlds — each one a chapter of my life — where scrolling is flying *into* things, and the objects around you are my story, projects and goals.

## Why it exists
I'm a frontend developer. The site itself is the strongest piece of evidence of craft: motion design, 3D, shaders, performance, accessibility. It should feel like nothing on a typical portfolio list, while still letting a recruiter get what they need in under 10 seconds.

## Experience pillars
1. **Dive, don't scroll.** Scroll = camera flight and zoom. No sections, no stacked pages. Each world exit is a *match-cut*: we fly through an object that becomes the next world's entry.
2. **Infinite & looping.** After the last world we dive back into the first. There is no bottom. A HUD tells you where you are in life.
3. **Worlds are chapters.** Visual metaphor and career story are the same thing (see table).
4. **Alive, not static.** Every world has idle motion even when you stop scrolling, and reacts to pointer/gyro. You can grab and throw things.
5. **Moody painterly low-poly.** Dark, flat-shaded facets, fog, warm rim light, grain. One accent hue per world.
6. **Respectful.** A persistent fast path (Résumé · LinkedIn · Contact), a plain-HTML CV view, sound off by default, reduced-motion support.

## Worlds ↔ chapters
| # | World | Metaphor | Life chapter | Content hosted |
|---|-------|----------|--------------|----------------|
| 1 | Galaxy | planets in orbit | Origins & schools | Education |
| 2 | Chess | strategy, deliberate moves | Early career | Early jobs |
| 3 | Climbing | grind, risk, ascent | Growth & hard problems | Mid-career jobs, key projects |
| 4 | Gaming / Keyboard | play + craft | Now | Current job, LinkedIn card, projects |
| ∞ | Thread & Goals | one continuous line + stars | Whole life | Timeline, life goals |

## Signature moves
- **Match-cut dive** between worlds (see 03).
- **Timeline thread**: one glowing line that persists across worlds and changes form (orbit path → move trail → rope → code stream).
- **Live terminal** on the keyboard world with hidden commands.
- **Time-aware lighting**: light temperature follows the visitor's local time.

## Non-goals (v1)
- CMS, blog, multi-language, accounts, analytics dashboards.
- Imported 3D models/textures (upgrade path documented, not built).
- Pixel-identical mobile; mobile is a lighter but still beautiful variant.
- Scraping LinkedIn (blocked/forbidden). Content is authored in `docs/content.md`.

## Success criteria
- A first-time visitor says "what is this?" within 5 seconds and keeps scrolling past the second world.
- Recruiter can reach résumé/LinkedIn/contact in ≤2 clicks from any point.
- 60 fps on a mid-range desktop GPU, ≥30 fps on a mid-range phone (auto quality tier).
- Lighthouse (prerendered fallback): Performance ≥ 90, Accessibility ≥ 95, SEO ≥ 95.
- Initial interactive scene ≤ 3 s on broadband; total JS ≤ 600 KB gz (excluding lazy world chunks).
- Zero hard-coded copy in components.

## Reference
webdrift.be — zoom-in-on-scroll feel. We borrow the *feel* of scale-driven depth, not its structure.
