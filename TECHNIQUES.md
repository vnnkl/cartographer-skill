# Discovery Techniques

The catalog for resolving a located unknown — organized by quadrant, cheapest technique first. Ticket types in parentheses. Adapted from Thariq's "A Field Guide to Fable: Finding Your Unknowns".

## Unknown unknowns — not yet aware what to ask

- **Blindspot pass** (research) — unfamiliar territory: a new part of the codebase, a new domain. Scan it and surface the unknown unknowns — what questions to ask, what good looks like, what historical work exists, what potholes to avoid — each with the prompt or plan fix that closes it. Use the literal words "blindspot pass" and "unknown unknowns"; they anchor the behavior.
- **Teach me my unknowns** (research) — the human doesn't yet know what good looks like in this domain (color grading, auth flows). Teach the vocabulary and the quality bar first, so they can judge and specify at all. Prefer this over generating variations to pick from — variations are useless to someone who can't yet evaluate them.

## Unknown knowns — recognized on sight, never written down

Verbalizing these while prototyping is cheap; discovering them during implementation is expensive — small spec changes cause drastically different implementations, and reverting is harder than reacting.

- **Mock before you wire** (prototype) — placement, hierarchy, or interaction is the question. A throwaway mock with fake data to react to — no backend routes, no state, never production code. The `/prototype` skill fits here.
- **Four design directions** (prototype) — taste is the question. Present wildly different directions to react to; structured pick-and-comment feedback beats "looks fine".
- **Brainstorm the intervention** (prototype) — the solution space is unmapped. Search the territory and spread candidate interventions from cheapest to most ambitious; the human marks what resonates. Also the default opening move when charting: brainstorming first prevents scoping too narrow or too wide.

## Known unknowns — the question is nameable

- **The interview** (grilling) — the human holds the answer. One question at a time, prioritized by whether the answer would change the architecture; synthesize into a decisions table. `/grilling` is the sharpest form.
- **Point at a reference** (research) — the answer exists as an artifact the human can't verbalize. The best reference is source code: point at the folder that implements the wanted behavior — even in another language — and extract the semantics before reimplementing. Diagrams, docs, and screenshots are weaker fallbacks.
- **The tweakable plan** (task) — the plan itself is the artifact to de-risk. Lead with the decisions most likely to be tweaked — data models, type interfaces, anything user-facing — and bury the mechanical work at the bottom, so review attention lands where input matters.

## During and after

The deviation log and the handoff (pitch + quiz) are modes of the skill itself — see [SKILL.md](SKILL.md).
