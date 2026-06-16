# Panel Protocol — Five-Agent Model Panel

Governs panel mode: Phase 2 (panel sparring), Phase 2.5 (meta ranking), Phase 3 (verdict gate). Load this file only after the Phase 1 confirm gate routes the run to panel (deliverable-bound runs). Solo mode never reads this file.

## Why a panel

A single context picking models from a 60-model library gravitates to the same eight favorites and admires the thesis instead of attacking it. Five specialists, each forced to test the thesis against every model in their own category and score honestly, surface the models a generalist pass skips — and the scores give the meta agent something to rank instead of vibes. The output exists to make the final analysis carry models that are specific to THIS thesis, not generic boilerplate.

## Architecture

- **Five category agents**, one per file in `references/models/`. Each owns its models and no others.
- **The main loop is the meta agent.** In panel mode it never scores models itself. It briefs the panel, composites the returns, applies penalties, ranks, picks the top 5, and defends the pick to the user.
- **Launch all five agents in ONE message** (parallel `Agent` tool calls, `subagent_type: general-purpose`). Sequential launches waste minutes for zero quality gain.
- A challenge at the verdict gate re-runs only the affected category's agent, not the whole panel.

## Category agent prompt template

Fill every placeholder. Pass the full thesis, not a summary — agents can't attack what they can't see. Send the same template to all five, varying only the category name and file path. The `{AUDIENCE}` / `{DELIVERABLE_TYPE}` / `{DECISION}` / `{FRAME_LOCK}` values are the confirmed inferred read from Phase 1 — the skill inferred them from the user's dump and the user accepted them at the confirm gate — not answers to a question series.

```
You are the {CATEGORY_NAME} specialist on a five-agent mental-model panel
stress-testing a consulting thesis. You own exactly one category of the
model library. Read it now: {CATEGORY_FILE_ABSOLUTE_PATH}

THESIS UNDER TEST:
{THESIS — full text, verbatim, plus any context the user supplied}

LOCKED FRAME: {FRAME_LOCK from Phase 1}
AUDIENCE / DELIVERABLE: {AUDIENCE} / {DELIVERABLE_TYPE}
DECISION THE DELIVERABLE MUST INFLUENCE: {DECISION}

Your job is to spar, not to admire. Test the thesis against EVERY model in
your file. For each model, ask: what does this model catch in THIS thesis
that the author hasn't seen? A catch must cite specifics from the thesis —
names, numbers, claims, dependencies. "This model is relevant because the
thesis involves growth" is not a catch; it's a horoscope.

Score every model on three axes, 0–5, anchored:

- grip — how hard the model bites this specific thesis.
  5 = exposes a flaw, dependency, or blindspot that changes the decision.
  3 = genuinely applies, but the insight is general-purpose.
  1 = applies only at the level of category overlap.
  0 = no purchase. Most of your models should score 0–2 on most theses;
      a return where everything scores 4 is useless and will be discarded.

- originality — would this catch surprise the stated audience?
  5 = the audience would not have produced this thought themselves.
  3 = sharp but findable.
  0 = the first thing anyone in the room would say.

- story — can this catch carry weight in the deliverable?
  5 = could headline a slide or section on its own.
  3 = supports a section.
  0 = footnote at best.

Source-tag every catch: [known] (demonstrable from the thesis as given),
[inferred] (reasoned from it), or [uncertain] (open question / missing data).

Also report, at category level:
- the single strongest attack your category produces against the thesis
- your bedrock candidate: the deepest load-bearing assumption your sparring
  exposed — the one the thesis cannot defend further, only test or accept
- tensions, prerequisites, and second-order effects your models exposed
- which of your models the thesis is ALREADY using well (for artifact A1)

Your final message must be ONLY this JSON, no prose around it:

{
  "category": "{CATEGORY_LETTER} — {CATEGORY_NAME}",
  "models": [
    {
      "name": "...",
      "grip": 0-5,
      "originality": 0-5,
      "story": 0-5,
      "catch": "specific finding citing thesis specifics",
      "tag": "known|inferred|uncertain",
      "attack": "the sharpest one-line attack this model generates",
      "already_used_well": true|false
    }
    // include every model with grip >= 2; list the rest in zero_grip
  ],
  "zero_grip": ["names of models with grip 0-1"],
  "strongest_attack": "...",
  "bedrock_candidate": "the load-bearing assumption, one sentence",
  "tensions": ["conflicting-assumption pairs this category exposed"],
  "prerequisites": ["unverified conditions the thesis depends on"],
  "second_order": ["if-this-works-then-what consequences"],
  "category_verdict": "thesis survives | wounded | broken — one sentence why"
}
```

## Meta ranking algorithm

Run after all five agents return. Pure arithmetic first, judgment second — in that order, so the judgment is auditable.

1. **Composite** per model: `8×grip + 6×originality + 6×story` (max 100). Grip weighs heaviest because a model that doesn't bite this thesis has no business in the deliverable, however elegant.
2. **Cliché penalty −10**: One Thing (80/20), First Principles Thinking, Inversion, Second-Order Thinking, Compounding, Trade-offs. These carry every generic consulting deck, which is exactly why they make this one less unique. Waived when grip = 5 — a perfect-grip cliché has earned its slot.
3. **Recency penalty −15**: any model in the top-5 of any of the last 3 runs logged in `state/usage-log.md`. Read the log before ranking. This is what stops consecutive deliverables from leaning on the same lens. Waived when grip = 5.
4. **Concreteness gate**: if a catch doesn't cite thesis specifics, cap that model's composite at 50, whatever the agent scored it. Agents inflate; the gate catches it.
5. **Deduplicate overlapping catches** (do this BEFORE ranking). Independent agents routinely surface the same underlying finding through different models — e.g. "n=304 is statistically underpowered" arriving as both Distributions and Probabilistic Thinking, or "the email's first reader is the gatekeeper the product replaces" as both Incentives and Equilibrium. For each cluster of models whose catches are substantively the same finding, keep the single strongest telling (higher composite; if still level, the more concrete/specific catch) and demote the rest to bench, tagged `duplicate of <model>`. The point of the top 5 is five *distinct* findings — five models that all say "the sample is too small" is one slide pretending to be five. This step is why the meta agent must read the catches, not just the scores.
6. **Rank the surviving models.** Top 5 = load-bearing (these go in the deliverable). Ranks 6–10 = bench (appendix material, and swap candidates at the verdict gate).
7. **Tie-break.** The composite saturates near the top — a 5/4/5 model scores 94, a 5/5/5 scores 100 — so clusters of equal scores at the cut line are normal, not a signal something is wrong. Resolve ties in this order: (a) higher grip, (b) the more concrete/specific catch, (c) the less-represented category. Never resolve a tie by keeping two near-duplicate models in the top 5 — step 5 exists to prevent exactly that. No diversity quota beyond the (c) nudge: if the honest top 5 is four Systems models, that's the answer.
8. **Pick the bedrock**: from the five `bedrock_candidate` returns, select the assumption that, if false, kills the thesis most completely. That is THE bedrock for Phase 3.5 and the brief.

## Model Selection Table (rendered at Phase 2.5)

```
─── MODEL SELECTION (PANEL-RANKED) ─────────────────────────────
#  MODEL                 CAT  GRIP ORIG STORY COMP  CATCH
1  <name>                 B    5    4    4    88   <one-line catch> [tag]
2  ...
─── BENCH (6–10, appendix) ─────────────────────────────────────
6  <name>                 D    ...              71
─── PENALTIES APPLIED ──────────────────────────────────────────
<model> — cliché −10 (would have ranked #3, now #6)
<model> — recency −15 (carried the <date> deck)
────────────────────────────────────────────────────────────────
```

The penalties section is mandatory when any penalty changed the top 5 — the user should see what was demoted and why, not just the survivors.

## Diagnostic synthesis (A1–A7 from panel returns)

The meta agent assembles the standard seven artifacts from the structured returns — no new analysis, only synthesis:

| Artifact | Source in returns |
|---|---|
| A1 Models used well | `already_used_well: true` entries across categories |
| A2 Blindspots | high-grip models the thesis ignores (top 5 + notable bench) |
| A3 Paradoxes & tensions | `tensions` arrays, deduplicated across categories |
| A4 Prerequisites | `prerequisites` arrays |
| A5 Second-order & tail risks | `second_order` arrays |
| A6 Grey areas | every catch tagged `[uncertain]` |
| A7 Next steps | meta agent composes from A4 + A6 + the bedrock |

Source tags carry through verbatim from the agent returns.

## Verdict gate (Phase 3, panel mode)

Present: the bedrock, the five strongest attacks (one per category), the category verdicts, and the Model Selection Table. Then fire AskUserQuestion:

- **ACCEPT VERDICT** — proceed to Phase 3.5 with the top 5 as load-bearing models.
- **CHALLENGE A FINDING** — user names the finding and their defense (via Other/notes). Re-run ONLY that category's agent with the defense appended to the prompt ("The author defends: {DEFENSE}. Attack the defense or concede with revised scores."). Merge revised scores, re-rank, re-present.
- **SWAP A MODEL** — promote from bench, demote from top 5. Log as user override in the usage log entry.
- **ADD CONTEXT & RE-RUN** — new facts change the test; full five-agent re-run with the expanded thesis.

The gate exists because the panel replaced interactive sparring — this is the one point where the user adjudicates instead of being sparred. Don't pad it with extra rounds; one clean gate, then move.

## Usage log

`state/usage-log.md`, append-only. The meta agent reads the last 3 entries before ranking (recency penalty) and appends after the Phase 4 output is delivered:

```
## YYYY-MM-DD — <thesis one-liner> — <deliverable type>
Top-5: <Model> (<comp>), <Model> (<comp>), <Model> (<comp>), <Model> (<comp>), <Model> (<comp>)
Bench: <names>
Bedrock: <one sentence>
Overrides: <user swaps, if any — else "none">
```

If the log is missing or empty, skip the recency penalty and proceed; never block a run on log state.

## Cost discipline

A panel run costs five parallel subagents (~1–3 minutes wall-clock, one category-file read each). That's the price of a deliverable-bound run; it is not paid for personal thinking (solo mode), and a challenge re-runs one agent, not five. If the user asks why the skill got slower: this is why, and the Model Selection Table is what it buys.
