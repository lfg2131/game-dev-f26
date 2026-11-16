# TOUCHLINE: Tactical Duel — Initial Game Design Document

**G1 Concept / November 15, 2026**  
**Executive Producer and Creative Director:** Laszlo Godde (`lfg2131`)  
**Status:** Initial proposal. Instructor G1 approval is pending. Proposed final-game features are not claimed as implemented or playtested.  
**Platform:** Desktop web, keyboard and mouse; no licensed football-data dependency.

## 1. The X, player and observable hypothesis

**The X:** *TOUCHLINE: Tactical Duel is a six-match football-management game in which the player reads match evidence, adapts an XI and tactics against a bounded rival coach, and feels the tension of a fair tactical chess match.*

**Target player:** Football Manager and strategy players seeking mastery in short sessions. This includes players who enjoy diagnosing a defeat but do not have time for a long career simulator.

**Motivation hypothesis:** If the concept works, an uncoached player will refer to earlier match evidence, name the rival's apparent approach, make a deliberate adjustment, and explain what they expect the change to do. This is observable behavior, not an assumed emotion or a score proxy. The strongest intended player utterance is, “They read my press. I need another plan.”

**Agent hypothesis:** A rival coach that sees recent *public* match evidence and proposes one legal tactical counter will feel more responsive and strategically legible than an otherwise identical scripted coach that uses fixed priorities. **What is lost without the agent:** the sense that a rival remembers your patterns and may anticipate the next decision. That loss is a hypothesis to test with real people; it has not been demonstrated.

## 2. Concepts explored, decision, design pillars

The following three directions draw on Laszlo's actual earlier TOUCHLINE work:

| Direction | Attraction | Why it did / did not win |
|---|---|---|
| **Full modern football-management simulator** | A broad career, transfers, training, scouting, 20 clubs and hundreds of players | The simulation/UI/content surface would obscure the core agent hypothesis and is not finishable to a good standard within this course |
| **Faithful compact 1982-style manager** | A complete six-match episode with selection, fitness, finances, results and standings | This became the **Classic midterm foundation**. A mostly faithful reconstruction offers limited room to investigate the new experience of being tactically read |
| **TOUCHLINE: Tactical Duel** | Keep the short season while focusing decisions on anticipating and countering a rival | **Selected:** a fair, readable agent/baseline experiment with manageable scope and a direct human-playtest question |

Laszlo's creative decisions are the six fixtures, three rival tactical counters, a strict information boundary, visible feedback and the rejection of a full modern career scope. Agents may suggest engineering approaches and test designs; Laszlo chooses what to approve.

**Design pillars**

1. **Readable cause and effect:** players can tell when the rival changed approach and what the match showed.
2. **Fair rivalry:** the agent never reads hidden selections, future simulation outcomes, or privileged internal state.
3. **Learning from losses:** match reports support a thoughtful next move, not just a numerical result.

## 3. Sources, mechanics and rights

| Reference | Specific aspect considered | Borrow, change and terms |
|---|---|---|
| Kevin Toms, *Football Manager* (1982) | Compact team-selection and match/result cycle; the sensation that a managerial decision has a consequence | Historical design reference only. The modern game code, characters, art, audio and data are **not** copied. [Creator's own account](https://kevintoms.itch.io/kevin-toms-football-management) |
| Sports Interactive's *Football Manager* series | Reading match patterns and reconsidering tactical choices | Inspiration for the *type* of decision, not a clone of its screens or database. The series and brand are the intellectual property of their owners; the prototype uses original artwork and fictional clubs. [Sports Interactive/SEGA legal and licensed-content information](https://community.sports-interactive.com/sigames-manual/football-manager-2023-touch-and-console-edition/legal-r4761/) |
| Laszlo's TOUCHLINE Classic midterm | The six-match engine, XI/fatigue, simulated match/results, table and replay/baseline test infrastructure | Student-authored course work. Reuse only with branch reconciliation and tests. [Midterm PR #86](https://github.com/columbia-university-ai-games/game-dev-f26/pull/86), open at G1 preparation |

The G1 deck's two coach/stadium images are **AI-generated original concept illustrations** and must be labeled concept art. They are not authentic gameplay footage. No real football club name/logo, player likeness, match broadcast, proprietary database, screenshots or licensed music is proposed for the final game. Before adding any third-party library or asset, record its version, URL, license and attribution in the repo; absence of a license means **do not import it**.

## 4. Core loop

1. **Act / scout:** inspect legal, public next-opponent reports and squad fitness.
2. **Act / plan:** select XI, formation and a tactical style. The engine checks roster and action legality.
3. **Respond:** the authoritative simulator resolves the first half and reveals score, chances, pressure and fatigue. At a documented decision point, the rival chooses a validated tactical response.
4. **Interpret:** the player reads the revealed events and may make **one** halftime tactical adjustment.
5. **Next action:** simulate the second half, read the report, update standings, then prepare for the next fixture. End after **six** fixtures with a results summary.

**Proposed styles:** `HIGH_PRESS`, `LOW_BLOCK`, `COUNTERATTACK`. These are behavior labels with clear advantages and vulnerabilities; the exact numeric modifiers are a G2 balance question, not a claimed existing rule. Both agent and scripted baseline use exactly this action set.

### Valid session and state boundaries

- **Authoritative state:** fixture number, active lineup, squad stamina, public events/results, hidden player inputs, seeded random stream, tactical modifiers, league table and terminal flag.
- **Player input:** XI from eligible squad, declared tactic, and one legal halftime change. Invalid inputs are refused with a readable reason and no state mutation.
- **Agent observation:** only the last two completed **public** match reports and current revealed first-half event summary; at season start, fewer reports are available and the agent uses only those present.
- **Agent proposal:** exactly one enumerated tactic and a short reason code grounded in allowed evidence. It cannot directly change the state.
- **Validation:** authoritative engine checks enum, eligibility, phase and game version. Illegal or late output receives a deterministic fallback.
- **Feedback:** show the rival's *revealed* tactical approach and the effects the simulation makes visible, without exposing hidden seeds or internal analysis.
- **End condition:** six completed fixtures and a final table/report. Tie-break rules and resulting UI must be verified against the retained Classic engine.

## 5. Mechanics, controls and constraints

| Topic | G1 proposal | Verification needed |
|---|---|---|
| Difficulty | A short but meaningful tactical problem, not dozens of player attributes | Small uncoached observation for information overload |
| Starting XI | Eleven eligible players; fatigue creates a visible tradeoff | Re-run Classic legality tests after integration |
| Match | Deterministic seed for paired offline comparisons; scores/chances reflect validated choices | Verify balance and perceived causality |
| Opponent | Three legal responses; bounded history of two public reports | Prove hidden-state isolation and invalid action rejection |
| Halftime | One deliberate player tactical change | Verify impact is observable and understandable |
| Scoring | Six-match points/table and end report, using actual engine rules | Confirm tie-breaks and target session duration |
| Controls | Mouse/keyboard, readable labels, no color-only status, large click targets | Keyboard audit and external playtest |
| Failure | Lose a match or finish below first place without soft-locking | Test forced failures and complete termination |
| Reproducibility | Immutable simulation seed and recorded accepted actions | Deterministic replay test |

### A single encounter as the first prototype

Before expanding the interface, implement one end-to-end encounter: player chooses tactics, rival proposes a legal counter, engine validates, visible halftime report appears, player responds, match concludes. Only after this vertical slice passes do we integrate the full six-fixture loop.

## 6. Agent versus scripted baseline evaluation

**Control condition:** script maps the same public report signals to one of the same three tactics using fixed priorities. It has no adaptive history beyond what is necessary for the immediate scripted rule. Both conditions share one game engine and action timing.

**Agent condition:** the model receives a compact public report history and chooses from the validated enum. It may choose a different action because of the remembered public patterns.

**Engineering measures:** output legality, action distribution, timeout/fallback counts, identical-seed replay for scripted trials, model latency (p50/p95), input/output tokens and calculated USD cost. Do not treat model decisions as proof of player experience.

**Human-playtest question, written before play:** *Can a player identify the rival's apparent approach from revealed match evidence and deliberately adapt without coaching?* Observe pauses, statements, menu exploration and stated reasons. Afterward ask what the rival seemed to do and what the player would try next. An action log can capture clicks and tactical changes; only a human observer can notice hesitation, confusion or surprise.

**G2 exploratory decision rule (proposal, not a statistical result):** In an initial four-person study, aim for at least three participants who can name an opponent approach and explain a specific tactical response. Report all four cases, not an aggregate claim of significance. A failure prompts clearer causal feedback before adding agent complexity.

**No actual G1 human-playtest result is claimed.**

## 7. Included scope, exclusions and cut order

**Included for intended course release:** one six-match episode, fictional clubs, XI/fatigue, a minimal formation/style choice, visible pressure/chance/score feedback, one halftime adjustment, bounded adaptive rival, fixed scripted baseline, deterministic fallback, final report and accessible browser play.

**Excluded:** 20-league universe, exported FM datasets, real players/club marks, licensed kits, deep transfers/contracts, training pyramids, multiplayer, photorealistic 3D, open-ended coach chat and unbounded model memory.

**Cut order if schedule slips:** optional UI animation first; extra squad analysis second; any scouting/transfer embellishments third. Preserve the six-match episode and the agent-versus-baseline test unless Laszlo explicitly seeks a revised gate decision.

## 8. Risks and decision

The six-dimensional Initial Risk Profile, evidence and mitigation live in [Initial Project Plan](INITIAL_PROJECT_PLAN.md). High ratings in **Technology**, **Genre/Mechanics** and **Timeline** share a causal root: implementing an adaptive action that players cannot recognize would consume engineering time without producing the promised experience. Build and observe the one-match vertical slice first.

**Approve at G1:** authorize a bounded, one-encounter prototype, a validated agent interface, fixed baseline and observation protocol, progressing to six fixtures only when the core feels legible.

**Hold at G1:** request a smaller mechanic or clearer evidence if the legal agent boundary, human-test question, scope/owner decisions, latency/cost controls or source rights cannot be stated concretely. Instructor decides, Laszlo responds.

## 9. Open questions (G1)

- [ ] Which exact formation and halftime settings exist in the reused engine versus need new code?
- [ ] What minimal match cues let people distinguish high press, low block and counterattack?
- [ ] What are the actual tie-break and terminal-game rules in the current Classic branch?
- [ ] Is the intended 3-second response target feasible with the configured provider?
- [ ] Which external playtesters and dates can be confirmed for G2?
- [ ] What is the official combined G2/G3 review date in the current CourseWorks syllabus?

## 10. Sources and implementation status

**Implemented and evidenced in Classic midterm:** bounded match simulation, six-fixture episode, squad decisions, standings and automated tests, subject to revalidation on the final-project branch. **Not yet evidenced for Tactical Duel:** new adaptive opponent, complete held-out comparison, uncoached human observation, or an approved G1 gate. See [midterm PR #86](https://github.com/columbia-university-ai-games/game-dev-f26/pull/86) and its explicit limitations.

**Related G1 deliverables:** [Configuration](INITIAL_CONFIGURATION.md), [Plan and Risk Profile](INITIAL_PROJECT_PLAN.md), [G1 deck](TOUCHLINE_G1_Concept_Deck.pdf).
