# TOUCHLINE: Tactical Duel — Initial Configuration Document

**G1 initial / November 15, 2026**  
**Human owner:** Laszlo Godde, Executive Producer and Creative Director  
**Status:** Proposed role and runtime configuration. No claim that the final-project agent extension has been deployed or evaluated. No credentials are included.

## 1. Authority and team structure

Laszlo owns concept, player experience, scope, budget approval, model/provider selection, human playtest ethics, release and response to gate decisions. The instructor alone issues the G1–G5 gate decision. An agent may propose or inspect work but may **not** approve a gate, merge a PR, spend funds without authorization, impersonate a player or fabricate human feedback.

The production agent team follows Laszlo's existing main coordinator and independent lanes pattern. Roles are **planned coverage**, not proof of currently running role-specific agents.

| GDF role | Planned executable artifact and purpose | Allowed context and tools | Output, handoff and evaluation | Human boundary |
|---|---|---|---|---|
| **Producer** | `agents/producer/AGENT.md`; sequence milestones, maintain risk/PPP | GDD, plan, issue/PR status; repo read/search | Dated plan and dependency updates to Laszlo; compare to gate scope | Laszlo sets scope and approves schedule changes |
| **Design Lead** | `agents/design/AGENT.md`; rules and player experience | GDD, anonymized playtest notes; repo docs | Revised mechanics and acceptance questions; peer QA review | Laszlo accepts intended experience |
| **Tech Lead** | `agents/engineering/AGENT.md`; engine, schema, tests | Local source, fixtures and test runner | Small patches and reproducible tests to integrator | No unreviewed merges, credentials or paid calls |
| **Art Lead** | `agents/art/AGENT.md`; readable tactical feedback | GDD, approved palette/original art | Accessible mockups plus visual checks to Design Lead | No licensed assets without explicit rights check |
| **Narrative / Audio** | `agents/content/AGENT.md`; concise original copy | GDD and original content inventory | Event microcopy and attribution ledger to Art Lead | No commercial soundtrack/voice/likeness cloning |
| **QA** | `agents/qa/AGENT.md`; independent validation | Source, seed fixtures, diff, CI logs | Bug report, replay and failure evidence to Laszlo | Can block acceptance, cannot self-approve |
| **Business Intelligence** | `agents/evaluation/AGENT.md`; baseline comparison and budget ledger | Anonymized traces and aggregate metrics | Outcome and cost/latency table to Producer | Must label live vs replay and measurement vs estimate |
| **Publishing / Release** | `agents/release/AGENT.md`; reproducible public package | Approved build/docs only | Release checklist and playable link to Laszlo | Laszlo signs off on external publication |

**Handoff protocol (planned):** each lane uses its own feature branch or isolated worktree. Handoff includes a one-paragraph change summary, changed-file list, test commands/results, risks and demo evidence. The coordinator verifies integration. QA tests independently. Laszlo approves final direction and repo PR merge. No agent merges on his behalf.

## 2. Proposed in-game rival contract

**Executable extension target:** `agents/rival-coach/manifest.json`, `agents/rival-coach/choose_rival_tactic.py` (or equivalent versioned callable adapter), `agents/rival-coach/schema.json`. These are **proposed locations** to implement by G2, not links to files already committed.

The final executable artifact must include manifest fields for its name/version, purpose, input/output schema, permissions, prohibited actions, dependencies, entry point, evaluation fixtures and owner. Documentation alone does not satisfy the executable extension requirement.

**Runtime provider candidate:** Gemini 2.5 Flash-Lite via a course-authorized Google API/Vertex route **only if available to Laszlo's existing account and permitted in the project**. The model identifier and exact endpoint must be confirmed before live use; a deterministic baseline always remains available.

### Context and state

```json
{
  "interface_version": "0.1-proposal",
  "game_phase": "declared_half_or_fixture_decision",
  "public_last_two_match_reports": [
    {"fixture_id": "id", "result": "public score", "pressure": "public summary"}
  ],
  "revealed_current_half": {
    "score": "public score",
    "events": ["public event summaries"],
    "minute": 45
  },
  "legal_actions": ["HIGH_PRESS", "LOW_BLOCK", "COUNTERATTACK"]
}
```

The above is a **schematic** contract, not test data and not literal production input; a versioned, strict schema with bounded string lengths and canonical enum values must be implemented.

- **Model may read:** at most two *completed public* reports, revealed current events, phase and the enum list.
- **Model may return:** one enum value plus a short predefined reason code grounded in public fields.
- **Model may not read:** unrevealed human tactic, hidden random seed, protected player attributes, future fixture result, private notes, credentials, system prompts or network content.
- **Model may not do:** execute arbitrary code, call external tools, edit match state or alter any rule.
- **Engine authority:** checks input phase and schema, validates enum, applies legal tactic, updates game state and emits observable feedback.
- **Memory:** at most two completed reports carried from the authorized game engine; no free-form persistent chat history or cross-user memory.
- **No personal data:** external playtest logs use anonymous labels and separate consent handling; no Health or user-account data enters prompts.

### Error behavior and human override

Invalid JSON, enum mismatch, timeout, refused response, provider outage, rate limit or budget cap triggers a documented deterministic rival tactic. No automated retry in the gameplay path; record the reason and continue without losing player progress. The player never sees secret agent reasoning, but the match can disclose the rival's resulting tactical style. Developer logging must not include secrets.

Laszlo can disable the model globally, select baseline mode and compare replay traces. Every prompt/tool change gets a version ID. No undocumented fallback to a different model.

## 3. Budgets with numerical basis

All numbers below are **G1 planning assumptions**, not measured results.

| Budget | G1 target | Reason / enforcement |
|---|---|---|
| Decision frequency | At most 2 model decisions per fixture, 6 fixtures, **12 calls/session** | Prevent runaway latency and cost; fixed event hooks |
| Input tokens | **2,000 tokens/call** maximum target | Two short reports, revealed first-half events and tool schema; truncate oldest public detail first |
| Output tokens | **300 tokens/call** cap | A single enum and short reason need much less; cap guards unexpected verbosity |
| Latency | Target p95 **under 3.0 seconds**, hard timeout **5.0 seconds** | Player wait tolerance is a design target; must be measured on deployment, not assumed |
| Session cost | Estimated **$0.00384**, hard planning ceiling **$0.10/session** | Token estimate and published Gemini API standard text prices below; cost meter fails closed |
| Fallback | 100% of invalid/timeout cases use script, **0 illegal actions applied** | Authoritative schema validation and forced-failure tests |

**Cost calculation at a reference rate:** 12 × 2,000 = 24,000 input tokens; 12 × 300 = 3,600 output tokens. Using the [Google Gemini Developer API published Gemini 2.5 Flash-Lite paid text prices](https://ai.google.dev/gemini-api/docs/pricing) (USD **$0.10 per 1M input** and **$0.40 per 1M output**), estimated inference cost = 0.024 × $0.10 + 0.0036 × $0.40 = **$0.00384 per session**. These are snapshot rates, not proof that the model is accessible, that production traffic uses this tier, or that actual token use/latency meets target. Other platforms, thinking token use and taxes may change costs. Recheck before the first paid run and enforce the hard ceiling; Laszlo authorizes any spend.

## 4. Proposed agent/system prompt skeleton

> You are the rival coach for a single fictional football-management fixture. Use **only** the public fields supplied. Never infer hidden lineup, unrevealed tactics or simulation seeds. Choose exactly one value from `legal_actions`. Return only the validated JSON object with `action` and an enumerated `reason_code`. If evidence is thin, choose conservatively. The game engine may reject your proposal; you cannot alter state.

Prompt version `rival.v0.1-proposal`; no unbounded dialogue. Candidate output format:

```json
{"action":"LOW_BLOCK","reason_code":"PUBLIC_OPPONENT_PRESSURE"}
```

The example is illustrative, not evidence of a live call.

## 5. Evaluation, CI and privacy

**Tests before G2:**

1. Reject hidden-state fields at the observation-builder boundary.
2. Reject invalid JSON, enum, wrong phase, oversize context and suspicious instructions in report text.
3. Force timeout/provider failure; fallback must finish fixture without mutation from rejected output.
4. Verify seeded baseline replay on both supported environments where practical.
5. Measure p50/p95 latency, token counts and per-session USD estimate from the actual configured provider.
6. Validate all six fixtures terminate and the final report is internally consistent.
7. Keep live calls and replay traces labeled separately; do not describe synthetic fixtures as human playtests.

**Existing course starter test commands:** `python -m unittest discover -s tests -v` and `python -m course_game.setup_check --offline --headless`, on the branch and environment that actually contain the relevant code. Existing midterm CI evidence is linked in [PR #86](https://github.com/columbia-university-ai-games/game-dev-f26/pull/86); those checks are not a new final-game evaluation.

**Acceptance / versioning:** QA provides reproducible command/output evidence. Laszlo accepts behavior and scope; the instructor decides gates. Store no secrets, personal participant details or unrelated data in source control.

## 6. Open questions

- [ ] Which provider and model are truly available in the approved project now, and at which exact pricing tier?
- [ ] Does the existing Classic simulator have stable visible pressure metrics for each rival style?
- [ ] Which source code path will become the versioned callable extension?
- [ ] What are measured p95 latency and actual prompt/output distributions on the target environment?
- [ ] Which production lanes and handoffs are actually running versus planned?
- [ ] What is the course's specific G2/G3 gate date and required CI evidence?

**Related:** [Initial GDD](INITIAL_GDD.md), [Initial Plan](INITIAL_PROJECT_PLAN.md), [G1 deck](TOUCHLINE_G1_Concept_Deck.pdf).
