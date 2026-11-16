# TOUCHLINE: Tactical Duel — Initial Project Plan and Risk Profile

**Initial G1 version, November 15, 2026**  
**Accountable owner:** Laszlo Godde, Executive Producer and Creative Director  
**Approval state:** G1 decision pending from instructor; plan does not authorize production or paid model calls.

## 1. Known course dates and gate schedule

| Event / gate | Date in New York | Status and dependency |
|---|---|---|
| Revised First Pitch Round | **Nov 10, 2026, 11:59 p.m.** | Laszlo reports submitting `TOUCHLINE: Tactical Duel` |
| **G1 Concept post / recording** | **Sun Nov 15, 11:59 p.m.** | Current submission, pending Laszlo's recorded presentation and CourseWorks post |
| **G1 live review** | **Mon Nov 16** | 6–8 projects live, others via recorded pitch |
| **G1 Approve/Hold decision** | **Tue Nov 17** | Instructor gate decision; a Hold requires corrective work |
| **Two Braintrust replies** | **Thu Nov 19, 11:59 p.m.** | Two classmates' recordings with fewer than two existing replies |
| **G1 graded feedback** | **By Nov 23** | Separate from Approve/Hold |
| **G2 Prototype gate** | **Exact course deadline not available in provided G1 brief** | Must confirm current syllabus/CourseWorks. G2/G3 share one review but remain distinct decisions (GDF v2.1) |
| **G3 Production-plan gate** | **Combined G2/G3 review date to confirm** | Plan becomes locked scope after separate G3 decision |
| **G4 Release gate and G5 reflection** | **To confirm from syllabus** | No invented official dates; revise this table once verified |

**Gate-date verification action:** Laszlo or the course assignment must supply the actual G2/G3/G4/G5 dates. This is an explicitly unresolved scheduling input, not an invented deadline. The internal milestones below are **planning targets**, not course due dates.

## 2. Initial task plan through G2

| Planned target | Task / deliverable | Owner (human accountable + proposed agent) | Dependency and acceptance criterion |
|---|---|---|---|
| Nov 15 | Submit G1 video/deck/GDD/config/plan | Laszlo + Release support | 3–5 min, 5 accessible links; no G1 self-approval |
| Nov 16–17 | Capture Braintrust/G1 decision and freeze prototype question | Laszlo + Producer/Design | Written decision, changes if Hold, no new unapproved feature scope |
| Nov 17–20 | Reconcile Classic code onto an isolated final-project branch | Tech + QA, Laszlo accepts | One playable fixture, reproducible offline tests; no regression of Classic |
| Nov 18–21 | Write exact tool schema and enemy tactic rules | Tech + Design | JSON schema, context allowlist, prohibited-state tests and fallback |
| Nov 20–24 | Implement **one vertical-slice match** with halftime feedback | Tech + Art/Design | Player can act, see a legal rival response, adjust and finish one match |
| Nov 22–25 | Add scripted baseline and per-turn trace labeling | Tech + BI/QA | Same legal action list/timing, deterministic runs with comparison logs |
| Nov 23–27 | Add the bounded agent behind feature flag | Tech + QA | Validated action only, forced timeout/invalid fallback, controlled cost |
| Nov 25–29 | External uncoached playtest, ideally four people | Design + BI; Laszlo observes | Consent, anonymized handwritten notes, action traces; compare stated understanding |
| Nov 27–30 | Extend/check six fixtures and final report | Tech + QA | Terminal state, seeded replay, keyboard/readability checks |
| **Before actual G2 review** | Assemble prototype video/build, evaluation, risk updates and new GDD/config | Producer/Release; Laszlo approves | Demo and linked test evidence sufficient for instructor's G2 decision |

**Owner discipline:** Asana becomes the task/acceptance/dependency record **from G2 onward**, as the reader specifies. This G1 plan is deliberately a simple repository document. Individual agent roles are planned in [Configuration](INITIAL_CONFIGURATION.md).

**Path to an executable extension:** the in-game rival is packaged as a versioned callable interface with a manifest and automated input/output contract tests. A prose role roster alone cannot satisfy the syllabus's extension expectation.

## 3. Six-dimensional Initial Risk Profile

| Risk dimension | Rating | Evidence / uncertainty at G1 | Mitigation and next evidence |
|---|---|---|---|
| **IP / Concept** | **Medium** | Historical and modern Football Manager references; risk of importing unlicensed art/data | Original code, fictional clubs, asset license ledger; document specific inspiration and exact terms |
| **Business / Audience** | **Medium** | Laszlo knows the genre, but no external player has yet demonstrated the proposed tactical-duel experience | Recruit target players; observe without coaching; compare stated reasoning |
| **Technology** | **High** | New rival may leak hidden state, return invalid action, time out or exceed budget; final implementation not evidenced | Input allowlist, strict schema, deterministic fallback, forced-failure tests, latency/cost traces |
| **Team** | **Medium** | Individual project with several proposed production agents and integration handoffs; risk of recommendations treated as approvals | Laszlo remains owner; isolated branches, reviewable handoffs, independent QA, no agent PR merge |
| **Genre / Mechanics** | **High** | A statistically different tactic can be imperceptible or feel arbitrary to a human | One-match vertical slice, explicit match cues, uncoached observation, simplify mechanics if unreadable |
| **Timeline** | **High** | Six-fixture ambition, model integration and testing compete for a short course window; G2 date requires confirmation | One playable fixture first, narrow agent contract, strict cuts, reserve QA/review time, confirm syllabus date |

**Shared cause among High risks:** If the adaptive choice is difficult to implement *and* invisible to the player, developers may spend too long building an agent that adds no perceivable game experience. This simultaneously raises **Technology**, **Mechanics** and **Timeline** risks. The same one-match vertical slice and no-coaching test directly address all three.

### High-risk triggers, contingency and evidence owner

| Trigger | Immediate action | Who checks |
|---|---|---|
| Hidden state or invalid action reaches model or engine | Stop model path; route through deterministic script, fix allowlist/schema | QA/Tech; Laszlo accepts fix |
| p95 model response exceeds 3s target or single call exceeds 5s | Show non-blocking feedback and use fallback; simplify prompt/model only with authorization | Tech/BI |
| Session inference estimate approaches $0.10 cap | No further paid calls that session; log cap reason, use script | BI/Tech; Laszlo authorizes spend |
| 2 or more of first 4 players cannot identify a rival approach | Clarify public cues and reduce tactical complexity; repeat observation | Design/Laszlo |
| One-fixture prototype incomplete near internal Nov24 milestone | Cut visual extras and secondary systems; preserve agent/baseline experiment | Producer/Laszlo |

## 4. Budgets and feasibility basis

Planned upper bound **12 model decisions/session**, each budgeted **2,000 input + 300 output tokens**. With [Google's published Gemini 2.5 Flash-Lite paid-tier prices](https://ai.google.dev/gemini-api/docs/pricing) of **$0.10 per million input** and **$0.40 per million output** (pricing snapshot, availability and billing tier to reverify), estimated inference spending is **$0.00384/session**. **Hard planning ceiling $0.10/session**, with a 5s timeout per call, no gameplay retry, p95 target <3s. These are not measured results; actual latency and rates are a G2 test. Laszlo must authorize paid usage.

**Human-test resource budget:** initial four-person exploratory observation, each completing one encounter. No claim of recruitment, consent or completed testing yet.

**Scope budget:** one polished match vertical slice before six-match integration. Cut optional visual polish, extra scouting, transfer depth and animations first.

## 5. G1 request and Hold conditions

**Approve requested:** authorize a narrow prototype measuring whether a legally bounded, history-aware rival coach creates a recognizable and respondable tactical counter relative to a fixed script. Deliver the one-match vertical slice, core feedback, script comparison, failure fallback and uncoached observation, then decide if the full six-match episode remains feasible.

**Hold would require:**

- a smaller tactical vocabulary or clearer description if players cannot distinguish counters;
- an auditable schema if the rival could see unrevealed decisions or mutate world state;
- a revised provider/budget plan if latency/cost cannot be capped;
- clarified ownership/rights for any non-original content; or
- a realistic shorter plan if the course schedule no longer supports six fixtures.

Only the instructor grants Approve/Hold; Laszlo personally accepts creative and scope changes recommended by agents or reviewers.

## 6. Open questions and update rhythm

- [ ] Exact G2/G3 combined review and G4/G5 dates from the current syllabus.
- [ ] Code/branch integration path from Classic PR #86.
- [ ] Allowed paid provider and actual live-model pricing/availability.
- [ ] First external playtester dates and actual session duration.
- [ ] Which three rival choices produce the clearest publicly visible effects?

This is Initial at G1. Reissue as Updated at G2 with tested behavior, real timing/cost traces and player observations, then Final baseline at G3 with authorized scope lock. Weekly PPP reports and detailed risk registration move to Asana once the G2 workflow begins.

**Related:** [Initial GDD](INITIAL_GDD.md), [Initial Configuration](INITIAL_CONFIGURATION.md), [G1 deck](TOUCHLINE_G1_Concept_Deck.pdf); [Course GDF](https://github.com/columbia-university-ai-games/game-dev-f26/blob/main/docs/gdf.md).
