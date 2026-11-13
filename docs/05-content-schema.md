# 05 — Content Schema

`docs/content.md` (human-edited) → `scripts/build-content.ts` → `src/content/content.json` (typed, validated).

## Parsing rules for content.md
- Top-level `# Heading` = section. Lists of items use `## <Item title>` blocks containing `key: value` lines (one per line; multi-line values use `key: |` followed by indented lines; arrays use `- ` bullets under the key).
- Dates: `YYYY-MM` or `YYYY`; `present` allowed for `to`.
- Unknown keys → warning; missing required keys → error with line number.
- `TODO` as a value is allowed in dev builds (renders placeholder), fails `npm run build` unless `--allow-todo`.

## Types
```ts
type World = 'galaxy' | 'chess' | 'climbing' | 'gaming';

interface Profile {
  name: string; headline: string; location?: string; email: string;
  pitch: string;               // ≤ 140 chars, hero line
  bio: string;                 // 2–4 sentences
  links: { linkedin: string; github?: string; website?: string; other?: {label:string;url:string}[] };
  resumeUrl?: string;
  skills: { group: string; items: string[] }[];
}

interface Education { id: string; institution: string; field: string; from: string; to: string;
  story: string; highlight?: string; world: 'galaxy'; }

interface Experience { id: string; company: string; role: string; from: string; to: string | 'present';
  summary: string; bullets: string[]; tech: string[]; world: World; }

interface Project { id: string; title: string; year: string; oneLiner: string; role: string;
  stack: string[]; links: { live?: string; repo?: string }; media?: string; world: World; }

interface Goal { id: string; goal: string; horizon: 'now' | '1y' | '5y' | 'someday'; why: string; }

interface HobbyChapter { world: 'chess' | 'climbing' | 'gaming'; facts: string[]; story?: string; }

interface LinkedInCard { headline: string; about: string; topSkills: string[];
  recommendations: { from: string; role?: string; quote: string }[]; }

interface EasterEgg { command: string; response: string; action?: 'hire'|'resume'|'chess'|'goals' }

interface Content {
  profile: Profile; education: Education[]; experience: Experience[]; projects: Project[];
  goals: Goal[]; hobbies: HobbyChapter[]; linkedin: LinkedInCard; easterEggs: EasterEgg[];
  extras: { tone?: string[]; avoid?: string[] };
}
```
`id` is auto-slugged from title if omitted; must be unique across all item types (used for deep links `#/job/<id>`).

## Mapping content → worlds (default distribution)
| Content | Where it appears |
|---|---|
| Education[] (each) | One planet per entry in Galaxy; planet size ∝ duration |
| Experience[] with `world: chess` | A chess piece per job (role→piece: junior=pawn, mid=knight/bishop, lead=rook/queen) |
| Experience[] with `world: climbing` | A hold per job along the route, ordered by date |
| Experience[] with `world: gaming` | A keycap per job + monitor "current job" window |
| Projects[] | Objects in their `world`: moon on a planet, captured-piece row, side-hold, secondary keycap/window |
| Goals[] | Constellation stars in thread/goals layer (x = horizon, brightness = nearer-term) |
| Hobbies[] | Flavour caption in its own world (e.g. chess rating shown on board edge) |
| LinkedInCard | Monitor in Gaming world + panel |

Capacity limits (avoid clutter): Galaxy ≤ 6 planets, Chess ≤ 8 pieces with content, Climbing ≤ 8 holds, Gaming ≤ 8 keycaps. Overflow → grouped into "+N more" object that opens a list panel.

## Validation tests (Vitest)
- Fixture markdown parses to expected JSON.
- Missing required fields produce errors with correct line numbers.
- Duplicate ids rejected.
- Overflow grouping logic.
- Every `content.md` form field exists in the types (kept in sync by a test that diffs the template headings).
