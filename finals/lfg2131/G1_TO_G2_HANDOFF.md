# TOUCHLINE: Tactical Duel — G1 → G2 handoff and review worksheet

**Prepared:** November 15, 2026 (New York)  
**Student Owner / Executive Producer / Creative Director:** Laszlo Godde (`lfg2131`)  
**Source of authority:** [Course GDF v2.1](https://github.com/columbia-university-ai-games/game-dev-f26/blob/main/docs/gdf.md) and current CourseWorks syllabus.  
**Status:** Preparation only. This document does **not** imply that CourseWorks accepted the G1 post, the instructor approved G1, agents ran paid calls, or a playtest occurred.

## 1. Immediate submission and Gate 1 follow-up

- [ ] Confirm that the CourseWorks thread **TOUCHLINE: Tactical Duel** is actually published, and save a screenshot or thread URL. GitHub files by themselves are **not** the graded submission.
- [ ] Confirm [YouTube recording](https://youtu.be/81eM5hj9t2Y) plays for a viewer without the uploader's login. It should be 3–5 minutes and show the deck with Laszlo presenting.
- [ ] If needed, give viewers a [Google Drive backup](https://drive.google.com/file/d/1q0hUHQ7zE27SezXEs0xY-jpvRBB9g9wb/view?usp=sharing) (Columbia members with link); check the actual access level in Drive rather than assuming “Copy link” grants permission.
- [ ] **Monday Nov 16:** take notes on the assigned classmate's recording *before class* as the professor's announcement requests. Attend the G1 review; six to eight students present live.
- [ ] **Tuesday Nov 17:** read the instructor's actual CourseWorks Approve/Hold comment. Preserve the wording and conditions. **Do not self-approve G1 or alter the gate outcome.**
- [ ] **Thursday Nov 19, 11:59 p.m. New York:** post **two** Braintrust replies on classmates' G1 pitches, prioritizing threads with fewer than two replies. These are separate from the initial Nov 16 preparation.
- [ ] **By Nov 23:** compare the grading feedback against the G1 decision and document any required changes.
- [ ] Retrieve the current syllabus's **exact G2/G3 combined-review and G4/G5 dates**. They are not confirmed in the supplied G1 instructions.

Current GitHub review branch: `g1/touchline-tactical-duel-20261115`; [draft PR #1](https://github.com/lfg2131/game-dev-f26/pull/1) is deliberately **unmerged**. [G1 files](g1/README.md).

## 2. Braintrust observation worksheet (fill only after viewing the real recording)

Use **one worksheet per classmate**. Do not create quotes, timestamps, experience claims or gameplay findings before watching.

**Classmate / working title:** [first name or project nickname]  
**CourseWorks post URL:** [actual URL]  
**Recording URL:** [actual URL]  
**Replies already present when choosing:** [count]  
**Watched on:** [date/time]  

1. **Time-coded moment:** At `[mm:ss]` the speaker `[describes exact slide or says/paraphrases a specific point]`.
2. **What I understood / experienced:** `[concrete interpretation; distinguish yours from the speaker's stated claim]`.
3. **What remained unclear:** `[missing definition, scope boundary, evidence, or tradeoff]`.
4. **Possible cause, stated as a hypothesis:** `[why the gap might exist; avoid assigning motive]`.
5. **Useful question / candidate experiment:** `[small action the classmate can take at G2 to test their agent hypothesis or scope]`.
6. **One strength to preserve:** `[specific successful design decision]`.

**Reply structure (adapt, not fill with invented data):**

> At **[timestamp]**, when you presented **[specific moment]**, I understood **[what registered for me]**. I was unsure **[what remained unclear]**, because **[observed gap; reason is an interpretation]**. I wonder if **[cause hypothesis]** is part of it. For G2, could you test **[narrow, observable agent-versus-baseline or scope question]**? I would keep **[specific strength]**, because **[why it supports the intended player experience]**.

Before posting, confirm (1) timestamp is real, (2) comment is about the classmate's actual pitch, (3) no fabricated player tests, and (4) they have fewer than two replies as instructed.

## 3. What G2 and G3 actually require (from GDF v2.1)

**G2 Prototype decision:** Does real evidence support the core experience and the agent's contribution?

Bring:

- A **first playable with the bounded agent** and a **recording**, including a visible complete core loop.
- An **Initial Playtest Log** documenting **at least one external player** using that loop in the combined G2/G3 review, with observations and limits.
- **Update** GDD and **Update** Configuration Document, including which agent roles actually ran and produced evidence.
- Updated plan and Risk Register, with **Asana** for tasks, dependencies, acceptance criteria, PPP and gate decisions **from G2 onward**.
- Tests, traces, failures, model cost/latency evidence, and a reproducible build/launch path. A successful model API call is not proof of good player experience.

**G3 Production decision (same review, separate decision):** Is the experience feasible within the remaining capacity? Provide the production scope-lock plan, exclusions, contingencies, Alpha/Beta/Gold acceptance checks and linked G2 evidence. Instructor approval alone locks the commitment.

**Later G4 playtesting:** across G2→G4, observe **at least two distinct external players**, including a revised encounter after the combined G2/G3 review and before G4. Do not present the same person or synthetic run as a new external participant.

## 4. Proposed design boundaries to test, pending G1 approval

These are **proposals pending G1 approval**, not claims of implemented code:

- Episode: **six fixtures**; first build a **one-fixture vertical slice**.
- Player acts: choose an eligible XI and tactic, read first-half evidence, make one halftime tactical adjustment, finish the encounter.
- Rival action vocabulary: `HIGH_PRESS`, `LOW_BLOCK`, `COUNTERATTACK`, with one bounded response at the agreed decision point.
- **Authoritative simulator owns world/hidden state**. Model observation is built from only **two most recent *public* completed match reports** and revealed current-half cues; no hidden player selections, internal RNG seed, future simulation result or unauthorized model write.
- Agent output: strict, versioned JSON object `{"action":"LOW_BLOCK","reason_code":"PUBLIC_OPPONENT_PRESSURE"}`, using enums defined in code. Reject illegal, stale, malformed, oversized or out-of-phase proposals.
- Scripted baseline: the **same legal tactics and timing**, fixed deterministic priority rules; run identical scenarios/seeds where the engine permits.
- Budget **projection** from G1, not measurement: up to 12 model calls per six-match session; 2,000 input + 300 output tokens/call; estimated $0.00384 using the provider's previously cited rates; **$0.10/session ceiling**, `p95 < 3s` target, **5s timeout with no gameplay retry and deterministic scripted fallback**. Reverify current provider availability and prices before any live call; no unapproved spend.
- Design evidence: whether a player notices a rival's approach and deliberately selects a response *without coaching*. A technical model action change that nobody can perceive is insufficient.

**Cut first:** animation, visual extras, scouting embellishments, transfer depth. **Do not add:** real club logos/players/databases, multiplayer, 3D stadiums or a full modern career simulator for this course prototype.

## 5. G2 acceptance check matrix (planned checks; none claimed passed)

| ID | Minimum evidence to record | Pass condition |
|---|---|---|
| ENG-01 | Fresh-start screen recording and README instructions | A second person can start and finish one full match without developer intervention |
| ENG-02 | Hidden-state adversarial unit test | Agent observation never contains unrevealed lineup/tactic, RNG seed or future result |
| ENG-03 | Schema/phase negative tests | Malformed, unauthorized and out-of-phase rival proposals produce no direct world-state mutation |
| ENG-04 | Offline deterministic forced-failure test | Provider timeout/failure finishes the match using the scripted fallback |
| ENG-05 | Paired scripted/agent traces | Both paths have identical action vocabulary, intervention phase and scoring engine |
| ENG-06 | Six-fixture deterministic replay | Fixtures terminate, results/standings are internally consistent; no soft lock |
| ENG-07 | Timing, tokens and session-cost traces | Actual tested metrics labeled by provider/version; any miss against G1 targets is disclosed |
| UX-01 | Uncoached external playtest with paper notes | Observer logs exact hesitation, confusion, player statements and decisions, not only a score |
| UX-02 | Post-play interpretation | Participant can explain what rival did and why they selected or would select a counter |
| PUB-01 | Accessible build URL or reproducible local launch | Reviewer can reach/play the experience; no account secrets are exposed |
| GATE-01 | Asana-linked evidence packet | Instructor can see actual gate request, versions, unresolved risk and evidence links |

## 6. Claude Code handoff — **only after the instructor authorizes G1 prototype work**

Paste this to the **main coordinator terminal**, not to several agents independently. It is a preparation prompt and does not itself authorize implementation or purchases:

```text
You are the main coordinator for TOUCHLINE: Tactical Duel, a Columbia University individual final-game project owned by Laszlo Godde, Executive Producer and Creative Director.

Do not start until Laszlo confirms the instructor's G1 Approve decision and its conditions. Read the exact G1 decision, the project AGENTS.md, finals/lfg2131/g1/INITIAL_GDD.md, INITIAL_CONFIGURATION.md, INITIAL_PROJECT_PLAN.md, finals/lfg2131/G1_TO_G2_HANDOFF.md, and the GDF reader. Read the original TOUCHLINE Classic midterm PR #86 and identify which code is actually present; do not assume it was merged.

First deliver a READ-ONLY inventory: relevant branches/commit SHAs, dirty worktrees, framework, actual game build, tests, existing agent capabilities, missing credentials/dependencies, and the minimum safe integration path. Then present a one-fixture prototype plan with acceptance criteria, dependencies, cut order and risk controls for Laszlo's approval.

After approval, coordinate separate role lanes (design, engineering, independent QA/evaluation, release) on isolated branches/worktrees. Preserve all existing user changes. One concern per commit, no force pushes, no unrelated moves, no merging without Laszlo's explicit decision. Do not spend provider credits or access secrets beyond configured task scope without approval. Do not claim a synthetic run is a human playtest.

Core experiment: validated rival proposes one of HIGH_PRESS, LOW_BLOCK, COUNTERATTACK after seeing only two recent PUBLIC reports and REVEALED halftime cues. Compare with a deterministic script choosing from the same three legal actions at the same phase. Engine owns state; schema validation and timeout fallback are mandatory. One playable encounter with readable halftime cues comes before the six-fixture expansion.

Demand inspectable evidence: hidden-state isolation, illegal action rejection, provider timeout fallback, seeded baseline replay, complete end state, tests/CI, measured latency and token/cost. Clearly distinguish existing midterm evidence, newly implemented behavior and untested hypotheses. Prepare an Initial Playtest Log structure; Laszlo recruits and observes an actual external player.

Do not self-approve gates, lock scope, invent CourseWorks feedback, or publish a submission. After the initial plan and risk review, wait for Laszlo's authorization to implement.
```

## 7. Analog board-game playtest — separate weekly assignment, due Nov 29 at 11:59 p.m.

This is **not** part of the TOUCHLINE G1/G2 human playtest. It is an in-person analog board/card-game observation, approximately 300 words as a **CourseWorks text entry**.

**Low-friction suggested game:** UNO, with at least **two actual players** (observer may watch rather than play), or any other physical board/card game accessible to you.

**Before play:**

- Re-read the game's printed rules, even if familiar.
- Write **Lens #103** on paper: (1) what I want to know, (2) who is playing (first names/nicknames), (3) where, (4) what to watch for, (5) how I will record it.
- Example research question to decide *before* the session: *When a player encounters a disputed card effect, what do players actually do before agreement, and does the social negotiation improve or interrupt the experience?*
- Tell the players you are observing for a game-design class and will use **paper/pen only**, no audio/video. Anyone may stop. Do not write private information.

**During play:** note the actual game/players/place, exact moments of hesitation, disputes or expectation changes, what was said/done, how rules were checked, and any difference between printed rule and practiced rule. Record facts separately from guesses about feelings.

**After play:** transcribe a ~300-word response: the five Lens lines, observation not recoverable from score/board state, one player-model-loggable **behavior** vs observer-only **experience** distinction (Yannakakis and Togelius ch. 10), one rule you would change and one you would preserve with reasons. Do **not** invent an observation.

## 8. Laszlo's information still required

1. Confirm the **G1 CourseWorks post** is published; optionally provide its link/screenshot.
2. Provide the **assigned Nov 16 peer** and **two real G1 peer recordings/posts** (prefer fewer than two replies). We can then draft truthful, specific Braintrust responses with timestamps.
3. Provide the instructor's **Approve/Hold comment** on Nov 17 and exact dates from the syllabus.
4. After approval, approve or adjust the proposed Claude Code one-fixture plan and explicitly authorize any spending.
5. Recruit **one genuine external player for G2**, and a **second distinct player before G4**; obtain consent and capture honest observations.
6. Before Nov 29, physically observe an analog game with handwritten notes and send the transcribed facts so the 300-word CourseWorks entry can be finalized.

**No student identity, CourseWorks post, actual playtest, live model run, video playback, or gate decision has been fabricated or assumed.**
