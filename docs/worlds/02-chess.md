# World 02 — Chess (Strategy / Early career)

**Accent:** amber/gold (`accent.chess`). **Chapter:** first jobs, learning how the game is played.

## Scene composition
- **Board:** 8×8 plane of slightly bevelled square tiles (dark/light wood-like colours from tokens, procedural grain shader), floating in a dark void/room with a hint of a table edge. Subtle emissive coordinate letters/numbers (mono) along edges.
- **Pieces:** lathe geometry (pawns, rooks, knights via extruded/sculpted low-poly profile, bishops, queens, kings), flat-shaded, two materials (ivory-ish / charcoal). Content pieces (≤ 8) carry a small accent ring on their base.
- **Environment:** a spotlight cone from above (volumetric look via additive cone mesh), floating dust motes, large blurred silhouettes of oversized pieces in the dark background (depth cue).
- **Lights:** warm key spot above-front, cool rim from behind, soft ambient.

## Scripted game
- Loop a short, famous, readable game (e.g. an "Immortal Game"-style sequence ≈ 20–30 plies) encoded as SAN list in `chessGame.ts`. Pieces animate between squares with arcs (knights hop), captures tip the captured piece off-board to the side rail where it joins the "captured" row (this row holds Projects).
- Moves are **time-driven when idle** (1 move/1.2 s) and **scroll-driven during dwell** (scroll advances the game; reversing scroll un-plays moves).
- At the final move (localT ≈ 0.78) the last-moved piece lands on the target square for T2.

## Entry (from T1)
Camera starts top-down over a board that is still partially planet-textured; planet surface fades to wood grain as the board "cools"; pieces rise from the squares (staggered, 0.12 window).

## Scroll beats
| localT | Beat |
|---|---|
| 0.12–0.30 | Camera tilts to 3/4 view; opening moves; first job piece highlighted |
| 0.30–0.60 | Middle game: each move reveals/highlights a job piece in chronological order; thread draws the move trail (arc lines) |
| 0.60–0.78 | Checkmate sequence; king tips over (or resigns) → camera glides over the final square |
| 0.78–1.00 | **Exit dive T2** into the final square (grain → rock) |

## Interactions
- Hover/click job pieces → Panel (role, company, dates, bullets).
- Drag pieces onto squares (snap, with legal-looking highlight only, no engine). Flick = knock over; auto-reset after 3 s or on scroll.
- Pointer nudges nearby loose dust; piece shadows follow cursor light slightly (spot target lerps toward pointer).
- Hobby caption: chess `facts` (e.g. rating) shown in mono along the board edge.

## Content mapping
Experience[world=chess] → pieces by seniority (see 05). Projects[world=chess] → captured pieces rail. Hobby chess → caption.

## Quality tiers
- low: no dust, no cone volumetric, pieces simple profiles, no physics.
- mid: dust, cone, 15-body physics.
- high: soft shadows, DOF-lite blur on background silhouettes.

## Budget
≤ 100k tris, ≤ 100 draw calls (instanced squares, merged same-type pieces where possible).

## Acceptance
- Game loops cleanly and is reversible with scroll without teleporting pieces.
- Every job piece clearly identifiable and keyboard-accessible.
- T1 and T2 seams invisible.
- Physics reset never leaves a piece off its authored square after leaving the world.
