# Cartographer

A [Claude Code skill](https://code.claude.com/docs/en/skills) for planning and steering work too big for one agent session — as a shared map of tickets on your issue tracker, kept honest against the territory.

> The map is not the territory. The map is the plan — tickets, notes, assumptions; the territory is the codebase and the real world. The gap between them is your **unknowns**.

Cartographer merges two ideas:

- **[Wayfinder](https://github.com/anthropics/claude-code)-style mapping** — a canonical map issue with child tickets, fog of war, a frontier of unblocked work, and one-ticket-per-session discipline, so many sessions (and many people) can work one big effort concurrently.
- **[A Field Guide to Fable: Finding Your Unknowns](https://x.com/trq212/status/2073100352921215386)** (Thariq, Anthropic) — locating every unknown in the Rumsfeld quadrants and resolving it with the cheapest discovery technique: blindspot passes, interviews, mocks, design directions, references, tweakable plans, deviation logs, pitches, and quizzes.

## Install

```bash
git clone https://github.com/vnnkl/cartographer-skill ~/.claude/skills/cartographer
```

## Use

Four modes, all reachable via `/cartographer` or autonomously by the agent:

1. **Chart the map** — bring a loose idea; the agent surfaces your unknowns (blindspot pass, grilling), then charts them as tickets and fog.
2. **Work through the map** — each session claims and resolves exactly one ticket with the technique written on it.
3. **Walk the territory** — during implementation, deviations from the plan are logged and folded back into the map before the session ends.
4. **Hand off** — before merge: a pitch doc with pre-answered objections, and a quiz you must pass perfectly.

See [`SKILL.md`](SKILL.md) for the full process and [`TECHNIQUES.md`](TECHNIQUES.md) for the discovery-technique catalog.
