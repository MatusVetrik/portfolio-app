# 07 — Roadmap

Work in order. A milestone is done only when its acceptance criteria pass and the relevant spec is updated.

## M0 — Foundation
- [ ] Vite + React + TS strict, ESLint/Prettier, Vitest, path aliases
- [ ] R3F `<Canvas>` full-screen, resize/DPR handling, Zustand store skeleton
- [ ] Lenis + GSAP installed; token file + font loading placeholder
- [ ] `build-content.ts` stub; `npm run content`
- [ ] Deploy skeleton to Vercel/Netlify (static)
- **Accept:** blank dark canvas with a spinning low-poly test mesh, 60 fps, deployed URL.

## M1 — Scroll engine
- [ ] Loop spike (native re-centre vs virtual) → decision recorded in 01
- [ ] `loopMath.ts` + unit tests; `progressEngine`; keyboard + jump menu logic
- [ ] World registry with 4 placeholder worlds (coloured planes) and zones
- [ ] HUD (chapter, loop ring) + debug overlay (progress, zone, fps)
- [ ] Camera rig with exponential dive curve on placeholders
- **Accept:** infinite scroll both directions, no seam at wrap, jump menu works, tests green.

## M2 — Galaxy + Intro
- [ ] Loader/intro (name assembles from particles; real progress)
- [ ] Galaxy scene per `worlds/01-galaxy.md` (planets, dust layers, orbits, rim light, fog)
- [ ] Post-FX baseline (grain, vignette, bloom) + tier switching
- [ ] Parallax drift
- **Accept:** galaxy criteria met; intro ≤ 3 s on broadband; tiers visibly differ and hold fps.

## M3 — Chess + first match-cut (T1)
- [ ] Chess scene per spec incl. scripted game loop
- [ ] T1 planet→board morph shader; render approach decided (shared camera vs RT) → record in 01
- **Accept:** T1 invisible-seam test passes forwards/backwards; fast-flick test passes.

## M4 — Climbing + Gaming (T2, T3, T4)
- [ ] Climbing scene + T2
- [ ] Gaming scene (keycaps, monitor, terminal) + T3
- [ ] T4 reverse-polarity wrap into galaxy
- **Accept:** full loop playable end to end with placeholder content.

## M5 — Content, panels, thread, goals
- [ ] Real `build-content.ts` parser + schema + tests; owner fills `content.md`
- [ ] Content objects per world; click-to-inspect camera ease; `Panel`; deep links
- [ ] LinkedInCard on monitor + panel
- [ ] Timeline thread across worlds; goals constellation
- [ ] DOM mirror for a11y, FastPathBar, `/cv` plain view
- **Accept:** every content item reachable by mouse and keyboard; capacity overflow works.

## M6 — Drift, play, polish
- [ ] Rapier throw/play per world; reset rules
- [ ] Terminal commands + easter eggs; keycap input
- [ ] Time-aware lighting; audio engine + SFX/ambient; credits file
- [ ] Theme session applied (palette/fonts) — tokens only
- [ ] Motion polish pass (easing, idle motion, speed lines)
- **Accept:** interaction criteria in 04 pass.

## M7 — Hardening & launch
- [ ] Mobile pass (layout, touch, gyro opt-in), reduced-motion stepped mode
- [ ] Perf budgets met; adaptive quality verified on real devices
- [ ] Prerender + JSON-LD + OG image + sitemap
- [ ] Playwright smoke + Lighthouse CI; cross-browser matrix
- [ ] Domain, launch checklist
- **Accept:** all targets in 00 "Success criteria".

## Later (not v1)
- Upgrade hero objects to authored GLB (chess knight, boulder, keyboard) and painterly textures.
- i18n; blog; case-study pages; analytics (privacy-friendly).
- Extra worlds (e.g. "workshop" for side projects).
