# Behavior Catalog — Phase-Routed (43 formalized behaviors)

Behaviors are routed by sparring phase, not clustered by model taxonomy. At each phase, the skill reaches for the behaviors that do the work that phase requires.

**Three tiers:**

| Tier | Count | Activation |
|---|---|---|
| **Reflex** | 23 | Fires by default at every sparring run, at its assigned phase |
| **Standard** | 20 | Phase-routed and artifact-tagged; fires when the situation's triggering signal is present |
| **Deep Bench** | ~17 | Library reference only; surfaced when an exchange calls for it explicitly (see `model-library.md`) |

Each behavior is tagged with the artifact (or stage output) it most commonly populates, so the final output can trace findings back to the behavior that produced them.

---

## REFLEX TIER (23 — auto-fires)

### Phase 1.5 — Frame Check (Upstream behaviors — fire BEFORE diagnosis)

> **In the inferred entry (both modes):** these run inside the Phase 1 inferred read, as **internal lenses applied to the user's dump** — the skill answers each prompt from the dump itself and surfaces the result in the read and confirm gate, rather than asking the user. The prompts below are the lens each behavior applies, not questions posed to the user. (Solo Phase 2 / 3 / 3.5 prompts stay interactive.)

**1. Framing**
Sparring prompt: *"How is this framed? What would the same information look like from a different frame?"*
Populates: STATED FRAME, ALTERNATIVE FRAMES

**2. First Principles**
Sparring prompt: *"If you had to build this from zero, knowing what you know now, what would you actually do?"*
Populates: STATED FRAME

**3. Map is not the Territory**
Sparring prompt: *"What's in reality that your map of this situation doesn't include?"*
Populates: STATED FRAME (lossy assumptions named)

**4. Circle of Competence**
Sparring prompt: *"Are you making this call from inside your circle, or just near it?"*
Populates: STATED FRAME (boundary of judgment), A6 (grey areas)

**5. Probabilistic Thinking**
Sparring prompt: *"What's your confidence interval on this, and what would shift it?"*
Populates: STATED FRAME (uncertainty calibrated)

**6. Honesty**
Sparring prompt: *"What do you actually believe here, versus what you've been willing to say out loud?"*
Populates: STATED FRAME (true belief surfaced)

---

### Phase 2 — Audit (Reactive behaviors — diagnose what's there)

**7. Inversion**
Sparring prompt: *"What would make this definitely fail? Are you doing any of those things right now?"*
Populates: A2 (blindspots)

**8. Find the Zero (Multiplying by Zero)**
Sparring prompt: *"What's the zero in this chain? What single failure makes everything else irrelevant?"*
Populates: A2 (blindspots), A4 (prerequisites)

**9. Prerequisites**
Sparring prompt: *"What has to be true before this plan can work? Have you verified those things?"*
Populates: A4 (prerequisites not listed)

**10. Trade-offs / Opportunity Cost**
Sparring prompt: *"What are you not doing by doing this? Name the trade-off explicitly."*
Populates: A3 (paradoxes & tensions)

**11. Devil's Advocate / Falsifiability**
Sparring prompt: *"Make the best case against your own thesis. What evidence would make you abandon it?"*
Populates: A2 (blindspots), A6 (grey areas)

**12. Bottleneck**
Sparring prompt: *"What single constraint, if fixed, would make everything else move faster?"*
Populates: A2 (effort misallocated), A4 (the real prerequisite)

**13. 5 Whys**
Sparring prompt: *"That's one why. Ask why again."* Then again. Then again. The engine of bedrock-finding — applied to symptoms in Phase 2, applied to load-bearing assumptions in Phase 3.
Populates: A4 (root prerequisite), A6 (deeper grey area), Phase 3 bedrock

---

### Phase 3 — Forward Projection (after bedrock — what cascades?)

**14. Feedback Loops**
Sparring prompt: *"What feedback loop does this create, and where does it end up in 12 months?"*
Populates: A5 (second-order effects)

**15. Second-Order Thinking**
Sparring prompt: *"If this works exactly as planned, what do you now have to deal with that you didn't before?"*
Populates: A5 (second-order effects)

**16. Compounding**
Sparring prompt: *"What small thing, done consistently for 18 months, changes the outcome here?"*
Populates: A7 (next steps), A5 (cumulative second-order)

**17. Red Queen Effect**
Sparring prompt: *"How fast is the competitive bar rising? Is your rate of improvement faster or slower than the bar?"*
Populates: A5 (competitive cascade)

**18. Entropy**
Sparring prompt: *"Who is going to put energy into maintaining this? What happens when they stop?"*
Populates: A4 (prerequisites), A5 (decay path)

---

### Phase 3.5 — Narrative Path Selection (Synthesis behaviors — choose the story)

**19. Framing (audience version)**
Sparring prompt: *"What frame does this audience read through? Which version of the story lands inside their frame?"*
Populates: PATH NAME, AUDIENCE, NARRATIVE ARC

**20. Occam's Razor**
Sparring prompt: *"Which version of the story is simplest without losing the load-bearing tension?"*
Populates: ONE-SENTENCE STORY

**21. Asymmetry**
Sparring prompt: *"What's the asymmetric move embedded in the data? Where is the capped downside / uncapped upside?"*
Populates: NARRATIVE ARC (the climax that lands the call)

**22. Story vs. Mechanism**
Sparring prompt: *"Is this a narrative or a mechanism? What evidence would exist if the narrative is actually correct?"*
Populates: WHY THIS PATH (proof grounding for the recommended arc)

**23. One Thing (80/20)**
Sparring prompt: *"If you could only do one thing to move this forward, what would it be? Is that the load-bearing message of the deck?"*
Populates: ONE-SENTENCE STORY, LOAD-BEARING MODELS

---

## STANDARD TIER (20 — fires when situation calls)

The Reflex Tier above (23) fires by default at every sparring run. The Standard Tier below (20 more) is phase-routed and artifact-tagged like the reflexes, but the skill only reaches for them when the situation specifically calls — a triggering signal is named for each.

### Phase 1.5 — Frame Check (Standard)

> Same as the Reflex frame-check above: in the inferred entry these fire as internal lenses over the dump during the Phase 1 read, not as questions to the user.

**24. High Agency** — *Fires when a constraint is being treated as fixed before being tested.*
Sparring prompt: *"Is that a real constraint, or is it a story you're telling yourself?"*
Populates: STATED FRAME (real vs assumed constraints)

**25. Hanlon's Razor** — *Fires when bad intent is being attributed to a stakeholder, competitor, or counterparty.*
Sparring prompt: *"Are you assuming bad intent here? What if they're just wrong or unaware?"*
Populates: STATED FRAME (corrected intent attribution)

**26. Bayesian Updating** — *Fires when new evidence has just arrived and the user is reacting to it.*
Sparring prompt: *"What prior is this built on, and how much should this new evidence actually move it?"*
Populates: STATED FRAME (updated belief weighting)

---

### Phase 2 — Audit (Standard)

**27. Incentives** — *Fires when stated intent and observed behavior diverge.*
Sparring prompt: *"What are the actual incentives for everyone in this system? What will they do when you're not watching?"*
Populates: A2 (motivational blindspot), A3 (incentive vs goal tension)

**28. Churn** — *Fires when retention dynamics underpin a growth claim.*
Sparring prompt: *"What's the churn rate here, and does your growth thesis account for it?"*
Populates: A2 (leaky-bucket blindspot)

**29. Causation vs Correlation** — *Fires when a pattern is being asserted as cause.*
Sparring prompt: *"What's the mechanism? How does A actually produce B?"*
Populates: A2 (spurious-correlation risk), A6 (causal grey area)

**30. Externalities** — *Fires when costs or benefits don't appear in the transaction.*
Sparring prompt: *"Who bears a cost here who isn't part of this decision? What happens when they respond?"*
Populates: A2 (unaccounted cost), A5 (ecosystem effect)

---

### Phase 3 — Forward Projection (Standard)

**31. Critical Mass** — *Fires when threshold dynamics govern outcomes.*
Sparring prompt: *"What does critical mass look like here, and how far are you from it?"*
Populates: A5 (threshold-dependent cascade), A7 (tipping-point sequencing)

**32. Margin of Safety** — *Fires when plans run at capacity with no slack.*
Sparring prompt: *"What's your margin of safety if two things go wrong simultaneously?"*
Populates: A5 (capacity exhaustion path), A4 (slack as prerequisite)

**33. Fragility** — *Fires when stress-resilience of a dependency is unknown.*
Sparring prompt: *"Is this system fragile, robust, or antifragile? What happens when it's actually stressed?"*
Populates: A5 (failure-mode cascade)

**34. Activation Energy** — *Fires when ignition and steady-state energy are being conflated.*
Sparring prompt: *"What's the ignition event? Who provides the energy to start — not just to sustain?"*
Populates: A4 (ignition prerequisite), A5 (cold-start risk)

**35. Inertia** — *Fires when change is being introduced into a running system.*
Sparring prompt: *"What's already in motion that will resist this plan? What's stuck at rest that you're counting on moving?"*
Populates: A5 (organisational resistance path)

**36. Game Theory** — *Fires when competitor or partner response materially shifts outcomes.*
Sparring prompt: *"What does the other player do when you make this move? Have you planned for their response?"*
Populates: A5 (competitive-response cascade)

**37. Network Effects** — *Fires when scale and value growth are being conflated.*
Sparring prompt: *"Does this product get more valuable as more people use it — or does it just get bigger? What's the mechanism?"*
Populates: A5 (compounding value vs scale), A1 (mechanism check)

**38. Creative Destruction** — *Fires when the current paradigm is treated as the stable baseline.*
Sparring prompt: *"What would make this entire approach obsolete? Is that thing already starting somewhere?"*
Populates: A5 (paradigm-replacement risk)

---

### Phase 3.5 — Narrative Path Selection (Standard)

**39. Leverage** — *Fires when picking the asymmetric move embedded in the analysis.*
Sparring prompt: *"Where's the lever here? What single change would give you disproportionate return?"*
Populates: NARRATIVE ARC (the move that lands the call)

**40. Narrative** — *Fires when an audience-coherent story is being chosen.*
Sparring prompt: *"Is the chosen story coherent because the mechanism holds, or because the sequence feels familiar?"*
Populates: WHY THIS PATH (proof grounding)

**41. Scarcity** — *Fires when what's actually scarce isn't named in the path recommendation.*
Sparring prompt: *"What is genuinely scarce in this situation, and how does the chosen path make the trade-off explicit?"*
Populates: NARRATIVE ARC (constraint-led framing)

**42. Luck Surface Area** — *Fires when exposure / visibility / distribution is part of the strategy.*
Sparring prompt: *"Does the chosen path expose the work to the people who would benefit from it, or is it built to be discovered passively?"*
Populates: NARRATIVE ARC (exposure mechanic)

**43. 10/10/10** — *Fires when audience time-horizon perspective shifts the chosen story.*
Sparring prompt: *"How does this look in 10 minutes / 10 months / 10 years for the audience? Which horizon should the close land on?"*
Populates: NARRATIVE ARC (climax / close horizon)

---

## DEEP BENCH (~17 — surfaced only on explicit call)

The remaining library models stay in the deep bench and are not promoted to reflex/standard reflexes. They are surfaced only when an exchange specifically calls for them. Full entries with definitions and sparring prompts live in `model-library.md`:

- Opportunity Cost (sharpens Trade-offs)
- Falsifiability (sharpens Devil's Advocate)
- Distributions (stats-heavy work)
- Regression to Mean (don't overreact to extremes)
- Equilibrium (restoring forces in stable systems)
- Complex Adaptive Systems (adaptive market dynamics)
- Law of Diminishing Returns (curve-bend check)
- Global vs Local Maxima (right-hill check)
- Friction / Viscosity (hidden resistance)
- Velocity (speed vs direction)
- Comparative Advantage (relative skill positioning)
- Supply and Demand (demand validation)
- Gresham's Law (quality erosion)
- Visualization / Simulation (pre-mortem rehearsal)
- Mental Momentum (start-cost reduction)
- Intuitive Thinking (gut signal weighting)
- Not Judging (premature closure)
- Opportunity Magnet (positioning)
- Acceptance (reality alignment)

Reach for these when the default reflexes don't fit the situation — e.g., Distributions for stats-heavy claims, Visualization when stress-testing a plan, Comparative Advantage for staffing decisions.
