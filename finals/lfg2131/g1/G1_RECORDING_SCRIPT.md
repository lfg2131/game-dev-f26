# TOUCHLINE: Tactical Duel — G1 recording script

**Laszlo Godde / Sunday November 15, 2026 / 3–5 minutes required.**  
**Target rehearsal:** roughly **4 minutes 25 seconds**, depending on pacing. Read once with a timer; the duration is not guaranteed. **The video must show Laszlo presenting the short deck in his own voice.**

The script below follows the 11-slide revised deck. First-person wording is a proposed draft for Laszlo to check against what he genuinely decided and explored. It does not present future work or planned evaluations as completed evidence.

### Slide 1 — Title (0:00–0:18)

“Hi, I'm Laszlo Godde. My final project is TOUCHLINE: Tactical Duel. You manage a football club through six matches, study an opponent's adjustments, and try to outthink a coach who can respond to your recent approach. I want it to feel like a tactical chess match.”

### Slide 2 — Player and motivation (0:18–0:40)

“I'm a huge Football Manager fan, so the target player is familiar to me. The most interesting moment is when you lose, study the pattern, and change your tactics. If this game works, I should observe players doing that unprompted, not simply winning.”

### Slide 3 — Core loop (0:40–1:03)

“The player scouts public reports, selects the starting eleven and tactic, then sees chances, pressure, fatigue and score as the game responds. At halftime they interpret that evidence and make one adjustment. The next match uses what they've learned. Six matches make one complete episode.”

### Slide 4 — Concepts and pillars (1:03–1:30)

“I considered a modern, full-career management simulator, but the enormous roster and systems would consume the semester. My Classic midterm explored a compact 1982-style remake. Tactical Duel won because it keeps that manageable scope while testing a new experience. My pillars are clear tactical cues, fair counters and learning from losses.”

### Slide 5 — References and rights (1:30–1:57)

“Kevin Toms' 1982 Football Manager inspired the compact team choice and immediate match-result loop. Modern Football Manager inspired reading match patterns and reconsidering tactics. I transform those ideas around a bounded opponent. I'm using fictional clubs and original concept art, no copyrighted game screenshots, club databases, logos or imported code. My own Classic PR is the implementation starting point.”

### Slide 6 — System and information boundary (1:57–2:25)

“The engine owns the private world state, including my unrevealed selection and random seed. A filter builds only public observations: the last two completed reports, revealed half events and legal actions. The rival may propose one counter. The engine validates it before changing state and shows me public match feedback. The agent never sees hidden decisions or changes rules directly.”

### Slide 7 — Agent hypothesis and baseline (2:25–2:53)

“The scripted comparator gets the same three legal actions but follows fixed rules. The agent uses public history to choose a counter. Without that memory, I expect the player to lose the sense of being studied. I will test whether an uncoached player can identify the rival's approach and deliberately respond, alongside controlled action and latency logs.”

### Slide 8 — Scope and cuts (2:53–3:17)

“My existing Classic build has the six-match simulation, XI and fatigue, standings and automated replay tests. The adaptive rival is still proposed, and I don't have a genuine human playtest of it yet. The final scope keeps the short episode and clear halftime decisions. I'll cut animation or peripheral scouting first, and exclude licensed assets, multiplayer and 3D.”

### Slide 9 — Numbers and basis (3:17–3:46)

“My planning budget is at most two rival calls per match, so twelve per six-match session. At two thousand input and three hundred output tokens per call, Google's published Gemini Flash-Lite rates imply about four tenths of a cent of inference. I set a ten-cent hard ceiling, a three-second p95 target and five-second timeout with scripted fallback. These are projections; I must measure them.”

### Slide 10 — Risk profile (3:46–4:15)

“IP, audience and agent-team coordination are Medium risks. Technology, tactical legibility and timeline are High. They share one cause: I could spend weeks on an adaptive choice that players can't even perceive. I'll mitigate that with a strictly validated interface, one complete encounter before expanding, and an uncoached observation. My agents can recommend fixes, but I approve the creative scope.”

### Slide 11 — Decision (4:15–4:38)

“At G1 I'm asking for approval to prototype one complete tactical encounter with an adaptive rival, a scripted baseline, clear feedback and fallback. A Hold would require clearer information boundaries, a more readable tactical response, or a realistic budget and scope correction. The instructor decides the gate. My Braintrust question is: would you notice and counter this adaptation? Thank you.”

---

## Recording checklist

- [ ] Open the final slide PDF or PowerPoint. Use the updated 11-slide deck, not the superseded 9-slide draft.
- [ ] Do a timed rehearsal and stay **between 3:00 and 5:00**. If slow, cut secondary detail from slides 4, 5 and 10 while retaining required categories.
- [ ] Record your own voice and visible slides. On Mac press **Shift–Command–5**, select screen recording, and choose the microphone in Options; or use another permitted recorder.
- [ ] Listen to the **whole** recording. Check that text and audio are readable and there are no personal notifications or secrets.
- [ ] Upload video to YouTube **Unlisted** or Drive with a Viewer link accessible to the instructor/TAs. Do not commit the video to GitHub.
- [ ] Post a **single CourseWorks thread** titled `TOUCHLINE: Tactical Duel` with five verified links: recording, deck, Initial GDD, Initial Configuration Document, Initial Project Plan.
- [ ] Save the CourseWorks confirmation before **Sunday Nov 15, 11:59 p.m. New York time**.
- [ ] By **Thursday Nov 19, 11:59 p.m.**, reply to two classmates whose posts each have fewer than two replies, using actual video timestamps.

## One important unknown

The current G1 assignment does not contain the exact **G2/G3 combined gate date**. The plan marks that value as pending the current syllabus. Do not substitute an invented official date.
