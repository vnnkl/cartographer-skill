# Cartographer

A [Claude Code skill](https://code.claude.com/docs/en/skills) for planning and steering work too big for one agent session — as a shared map of tickets on your issue tracker, kept honest against the territory.

> The map is not the territory. The map is the plan — tickets, notes, assumptions; the territory is the codebase and the real world. The gap between them is your **unknowns**, and every unknown the agent hits mid-work silently becomes a guess about what you want.

Cartographer exists to shrink that gap deliberately, instead of discovering it in a bad diff. It merges two lineages:

- **[Wayfinder](https://github.com/mattpocock/skills/tree/main/skills/in-progress/wayfinder)** by [Matt Pocock](https://github.com/mattpocock/skills) — the mapping machinery: a canonical map issue with child tickets, fog of war, a frontier of unblocked work, claim-before-work, one ticket per session, so many sessions (and many people) can drive one big effort concurrently.
- **[A Field Guide to Fable: Finding Your Unknowns](https://x.com/trq212/status/2073100352921215386)** by Thariq (Anthropic) — the surveying discipline: locate every unknown in a quadrant, resolve it with the cheapest discovery technique, and never leave anything important as loose prose in a conversation that will be forgotten.

## Install

```bash
git clone https://github.com/vnnkl/cartographer-skill ~/.claude/skills/cartographer
```

## The map

One issue, labelled `cartographer:map`, is the canonical artifact. It is an **index, not a store** — one-line gists linking to the tickets that hold the detail. Open tickets are never listed in it; they are child issues found by query, and the tracker's native blocking renders the frontier visually in your tracker's own UI.

```markdown
## Notes             ← domain, skills to consult, your starting point
## Decisions so far  ← one line per closed ticket, linked
## Deviations        ← where the territory contradicted the map
## Fog               ← what you can see coming but can't yet phrase
```

**Fog or ticket?** The test is whether you can state the question precisely *now* — not whether you can answer it. Sharp question → ticket, even if blocked. Dim view → fog, uncut, until the frontier reaches it.

## Locate the unknown

When you bring the agent a problem, break it down four ways. The quadrant an unknown sits in picks the technique — and the ticket type — that resolves it (full catalog in [`TECHNIQUES.md`](TECHNIQUES.md)):

| Quadrant | What it is | Resolve with |
|---|---|---|
| Known knowns | What you tell the agent you want | Nothing — already in the prompt |
| Known unknowns | You can name the question, not the answer | Interview (`grilling`) · reference (`research`) · tweakable plan (`task`) |
| Unknown knowns | Too obvious to write down — but recognized on sight | Mock before you wire · design directions · brainstorm (`prototype`) |
| Unknown unknowns | You don't yet know what to ask, or what good looks like | Blindspot pass · teach-me (`research`) |

## The four modes

1. **Chart the map** — bring a loose idea. The session gets your starting point on the table, brainstorms scope, runs a blindspot pass over the real territory, grills you on the open decisions — then charts tickets and fog and stops. Charting is one session's work.
2. **Work through the map** — each session claims exactly one frontier ticket and resolves it with the technique written on it. The answer lands as a resolution comment; the map gets a one-line gist; cleared fog graduates into fresh tickets.
3. **Walk the territory** — during implementation the territory answers back. The agent takes the conservative option, keeps going, logs the deviation — and folds every entry back into the map before the session ends, so the next session starts from a corrected map, not a stale one.
4. **Hand off** — shipping transfers your unknowns to whoever inherits the work. A pitch doc pre-answers the objections an expert would raise, with evidence from the territory. A quiz on the change must be passed perfectly before merging.

## How to use it well

1. **Start with your position, not the problem.** Where you are in your thought process, what you know about this codebase, what you've already rejected. Every technique downstream calibrates to it.

   > "I want offline sync for the app. I've never touched the storage layer, and I don't know what's possible with SQLite here. /cartographer — chart this."

2. **Say the magic words.** "Blindspot pass" and "unknown unknowns" are literal anchors — they trigger the hunting behaviour.
3. **One ticket, one session.** Resist finishing "just one more". The discipline is what makes the map shareable — run three terminals on three frontier tickets concurrently; claims and blocking keep them from colliding.
4. **Let deviations be data.** When implementation bends away from the plan, that's the territory teaching you. Don't fight it mid-session — fold it back into the map, and listen when the deviations say the problem should be solved differently altogether.
5. **Don't skip the quiz.** After a long session the agent has done more than you realize, and diffs alone won't show behavior that rides on existing code paths. Merge only after you pass. Every miss is an unknown you were about to ship.

## Files

- [`SKILL.md`](SKILL.md) — the full process
- [`TECHNIQUES.md`](TECHNIQUES.md) — the discovery-technique catalog, by quadrant

## Credits

- [Matt Pocock](https://github.com/mattpocock/skills) — [wayfinder](https://github.com/mattpocock/skills/tree/main/skills/in-progress/wayfinder), whose map/ticket/fog machinery this skill builds on, and [writing-great-skills](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-great-skills), which guided how this skill is written.
- [Thariq (@trq212)](https://x.com/trq212) — [A Field Guide to Fable: Finding Your Unknowns](https://x.com/trq212/status/2073100352921215386), the source of the quadrants and discovery techniques.
