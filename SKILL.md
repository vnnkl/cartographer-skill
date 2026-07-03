---
name: cartographer
description: Chart a huge chunk of work — more than one agent session can hold — as a shared map of tickets on the issue tracker, and keep the map honest against the territory. Use when the user brings a big loose idea to chart, wants to work a map, hits an implementation deviation from the plan, is preparing a merge or handoff, or asks what they're missing (blindspot pass).
---

The map is not the territory. The map is the plan — tickets, notes, assumptions; the territory is the codebase and the real world. The gap between them is the **unknowns**, and every unknown hit mid-work becomes a guess about what the human wants. This skill charts a loose idea — too big for one session — as a **shared map** on the repo's issue tracker, then keeps map and territory in sync: unknowns hunted before charting, deviations folded back during implementation, remaining unknowns handed off deliberately at the end. Domain-agnostic — engineering work, course content, whatever fits the shape.

**Disclose the starting point first.** Every mode works better with the human's position on the table: where they are in their thought process, their experience with this problem and this territory. If it isn't stated, ask before anything else.

## Refer by name

Every map and ticket is an issue, so it has a **name** — its title. In everything the human reads, refer to it by that name, never by a bare id or number; the id and URL ride *inside* the name as its link, never stand in for it.

## The map

A single issue labelled `cartographer:map` — the canonical artifact. It is an **index**, not a store: it gists each decision in one line and links the ticket that holds the detail, never restates it. Where child issues, blocking, and label queries physically live is tracker-specific: consult `docs/agents/issue-tracker.md` for how this repo expresses them; if that doc is absent, default to the local-markdown tracker.

The map body — the whole effort at low resolution, loaded once per session. Open tickets are **not** listed; they are open child issues, found by query:

```markdown
## Notes

<domain; skills every session should consult; standing preferences; the human's starting point>

## Decisions so far

- [<closed ticket title>](link) — <one-line gist of the answer>

## Deviations

- [<ticket or session>](link) — <what the map said / what the territory forced / what was chosen>

## Fog

<!-- see "Fog of war" -->
```

### Tickets

Each ticket is a **child issue** of the map. Its body is one `## Question` — the decision or investigation it resolves — sized to one 100K-token agent session, plus the discovery technique chosen for it (see [Locate the unknown](#locate-the-unknown)). It carries a `cartographer:<type>` label — `research`, `prototype`, `grilling`, or `task` — set by that technique.

A session **claims** a ticket by assigning it to the dev driving the map, **first**, before any work; an open, unassigned ticket is unclaimed. Blocking uses the tracker's **native** dependency relationship, so the human sees what's takeable in the tracker's own UI. A ticket is **unblocked** when everything blocking it is closed; the **frontier** is the open, unblocked, unclaimed children — the edge of the known. Answers are recorded on resolution, never in the body; assets are linked from the issue, not pasted in.

## Locate the unknown

The quadrant an unknown sits in picks the technique — and the ticket type — that resolves it. The catalog lives in [TECHNIQUES.md](TECHNIQUES.md):

- **Known knowns** — already in the prompt or plan. Not unknowns; leave them.
- **Known unknowns** — the question can be named, the answer can't → interview (`grilling`), reference (`research`), tweakable plan (`task`).
- **Unknown knowns** — too obvious to write down, but recognized on sight → mock, design directions, brainstorm (`prototype`).
- **Unknown unknowns** — not yet aware what to ask, or what good looks like → blindspot pass, teach-me (`research`).

## Fog of war

The map is _deliberately_ incomplete: don't chart what you can't yet see. The **Fog** section holds the dim view — suspected questions, areas to revisit, deferred risks. Resolving a ticket clears the fog ahead of it, graduating whatever's now specifiable into fresh tickets. **Fog or ticket?** Whether the question can be stated precisely *now* — not whether it can be answered now. Ticket when sharp (even if blocked); fog when it can't be phrased that sharply — don't pre-slice fog into ticket-sized pieces.

## Invocation

Four modes. Expect other sessions to be editing the tracker concurrently, and **never resolve more than one ticket per session.**

### Chart the map

User invokes with a loose idea.

1. Get the starting point on the table, then brainstorm the intervention (see [TECHNIQUES.md](TECHNIQUES.md)) to set scope — neither too narrow nor too wide.
2. Run a **blindspot pass**: walk the territory the idea touches — the actual code, data, and systems, not anyone's summary of them — and hunt the assumptions the idea makes that the territory hasn't confirmed: data shapes, hidden couplings, external systems and their failure modes, intent vagueness, taste questions, prior art, potholes. Last sweep: what class of unknowns is this list itself missing?
3. Run `/grilling` and `/domain-modeling` to sharpen the open decisions.
4. **Create the map** (Notes filled in, Decisions and Deviations empty, Fog sketched), then the tickets you can specify now — each located in its quadrant, typed, and carrying its technique — and wire blocking edges in a second pass (issues need ids before they can reference each other).
5. Stop — charting is one session's work. Done when every unknown surfaced is a ticket, in the Fog, or answered and recorded in Notes; none left as loose prose.

### Work through the map

User invokes with a map; a ticket is optional — without one, the agent picks, not the user.

1. Load the map — the low-res view, not every ticket body.
2. Choose the ticket (the named one, else first on the frontier). **Claim it before any work.**
3. Resolve it with the technique on the ticket, zooming into related closed tickets on demand and invoking the skills the Notes name.
4. Record: post the answer as a resolution comment, close the issue, append the one-line gist to Decisions so far.
5. Add newly-surfaced tickets (create-then-wire); graduate any fog the answer made specifiable, clearing it from the Fog; update or delete tickets the decision invalidated.

### Walk the territory

Fires during **implementation** — in or out of a ticket — whenever the territory answers back: an edge case forces a choice the map never covered, or proves it wrong.

1. Take the conservative option and keep going; don't stall the session on a mid-work unknown.
2. Log it in the map's Deviations (via the claimed ticket if there is one): what the map said, what the territory forced, what was chosen.
3. Before the session ends, fold every entry back: update decisions, re-scope or delete invalidated tickets, add Fog. If the deviations point to solving the problem a different way altogether, say so rather than patching the map.

Done when Deviations holds no unfolded entries — the next session starts from a corrected map, not a stale one.

### Hand off

When the work ships, whoever inherits it — reviewer, teammate, next session — starts with the unknowns the author began with. Before merge or handoff:

1. **Pitch**: package the artifacts (prototype, spec, the Deviations log) into one doc that leads with the demo and pre-answers the objections an expert would raise — with evidence from the territory (code, tests, measurements), not from the map.
2. **Quiz**: pose the hardest questions about what changed and why — including behavior that depends on existing code paths the diff only touches. The human merges only after passing perfectly; every miss is an unknown about to ship — ticket it, fog it, or close it now.

Done when no objection stands unanswered and the quiz is passed.
