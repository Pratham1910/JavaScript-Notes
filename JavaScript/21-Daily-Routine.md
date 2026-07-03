# Chapter 21 — The Daily Routine

Chapter 19 gives you the 12-week roadmap (which chapters, which weeks) and a one-line daily habit suggestion. This chapter is the missing middle layer: **what an actual day and an actual week look like**, hour by hour, so "study DSA daily" turns into a routine you can just run instead of re-deciding every morning.

Two tracks are given below — pick the one matching your actual situation, not the one that sounds more impressive. A consistent 90-minute track beaten daily for 12 weeks outperforms a 6-hour track that collapses after 9 days.

## 21.1 Track A — Working / studying alongside DSA prep (~1.5–2 hrs/day)

| Block | Duration | What happens |
|---|---|---|
| **Warm-up** | 10 min | Re-solve (from memory, no notes) one problem you already checked off in the Question Bank (Ch. 20) at least 3 days ago — this is your daily spaced-repetition dose, not new material. |
| **New problem, attempt** | 35–40 min | One new problem from today's chapter's current tier (Basic/Medium/Hard, per Ch. 19's phase). Timer on. Full, unaided attempt per the Ch. 19.3 process — no looking anything up yet. |
| **Resolve** | 20–30 min | If solved: implement cleanly, state complexity out loud, check the box in Ch. 20. If stuck: look at the pattern name only (Ch. 18), re-attempt for 10 more minutes, then read the approach (not code) and re-derive it yourself. |
| **Log** | 5 min | One line in your mistake log (21.4): what pattern this was, what you got wrong or confirmed you already knew. |

**Total: ~75–90 minutes.** On days with more time (weekends), extend the "new problem" block to two problems instead of one, rather than lengthening any single block — variety across two problems in one sitting reinforces pattern recognition better than one long problem.

## 21.2 Track B — Full-time prep (~5–6 hrs/day)

| Block | Duration | What happens |
|---|---|---|
| **Warm-up** | 15 min | Two spaced-repetition re-solves from the Question Bank (see 21.1's warm-up, doubled). |
| **Theory** | 45 min | Read/re-read the current chapter's theory section (Ch. 0–17) — the "why it was invented" and pattern sections, not just skimming code. |
| **Focused practice block 1** | 90 min | 2–3 new problems from the current chapter, Basic → Medium, full unaided attempts each. |
| **Break** | 15 min | Actually step away — screen off. This is not optional; the next block's quality depends on it. |
| **Focused practice block 2** | 90 min | 2–3 new problems, Medium → Hard, same process. |
| **Review & log** | 30 min | Revisit anything left unsolved from today, write clean final implementations, update the mistake log. |
| **Weekly-only: mock interview** | 60 min | One full timed mock (see 21.3), on the same day each week, starting in Phase 4 of the roadmap (Ch. 19). |

**Total: ~5 hours on a normal day, ~6 on mock-interview days.** Full-time prep is a marathon, not a sprint — a hard cap of two focused practice blocks per day with a real break between them prevents the late-afternoon quality collapse that makes hour 5 of unbroken grinding nearly worthless.

## 21.3 The weekly cadence

Both tracks follow the same weekly shape — only the daily time budget differs:

| Day | Focus |
|---|---|
| Mon–Thu | New material: proceed through the current roadmap phase's chapter(s), Basic → Medium → Hard, per 21.1/21.2. |
| Fri | **No new topics.** Pure review: re-solve 3–5 problems from earlier in the week without looking at your prior code, plus one spaced-repetition problem from 2–3 weeks back. |
| Sat | Mock interview day (once you've reached Phase 4 in Ch. 19 — earlier than that, treat Saturday as a second full practice day). Timed, explain your approach out loud as if to an interviewer, no IDE autocomplete, no looking anything up mid-problem. |
| Sun | **Rest, or light review only.** Read one chapter's theory section for pleasure/refresher with zero problem-solving pressure, or take the day fully off. Burnout from a 7-day-a-week schedule with no release valve is a more common failure mode than under-practicing. |

> **Why Friday is review, not new material.** Introducing new patterns five days a week with no consolidation day means early-week material is half-forgotten by the following Monday. A dedicated review day is what actually moves problems from "I solved this once" into the kind of durable recall Ch. 19.3's spaced-repetition schedule (3 days, 1 week, 3 weeks) depends on.

## 21.4 The mistake log — the single highest-leverage five minutes of the day

Chapter 19.3 mentions keeping a mistake log; here's the concrete format. One line per problem, every day, no exceptions:

```
Date       | Problem                        | Pattern            | Root cause
-----------|--------------------------------|--------------------|--------------------------------------
2026-07-03 | Longest Substring w/o Repeat   | Sliding window     | Forgot to shrink window from left
2026-07-03 | Course Schedule II              | Topo sort (Kahn's) | Confused in-degree with out-degree
2026-07-04 | Kth Largest Element             | Heap / Quickselect | Solved cleanly — no mistake, reinforce
```

Review this log in full every Friday (21.3). After 3–4 weeks, a pattern usually emerges — most people's mistakes cluster into 2–3 repeat root causes (off-by-one errors, misreading constraints, defaulting to a familiar-but-wrong pattern) — and *that* cluster, not the individual problems, is what your review sessions should actually target.

## 21.5 How the daily routine shifts across the 12-week roadmap

The daily *shape* above stays constant; what changes is the ratio of theory to practice to mocks as you move through Chapter 19's phases:

| Roadmap phase (Ch. 19) | Theory : Practice : Mock ratio |
|---|---|
| Foundations (Weeks 1–2) | 40 : 60 : 0 — more time re-reading and internalizing complexity analysis and base patterns |
| Core Linear DS (Weeks 3–4) | 25 : 75 : 0 |
| Non-Linear DS (Weeks 5–7) | 25 : 75 : 0 |
| Algorithmic Thinking (Weeks 8–10) | 15 : 75 : 10 — mocks begin here |
| Advanced / Polish (Week 11) | 10 : 70 : 20 |
| Mock Interviews (Week 12+) | 5 : 45 : 50 — practice shifts almost entirely to interview simulation |

> **Why mocks don't start on day one.** A mock interview measures performance under time pressure and explanation pressure simultaneously — running one before you have enough patterns internalized just measures "I don't know this yet," which the untimed practice blocks already tell you far more efficiently. Mocks earn their cost once you're mostly solving Medium problems correctly untimed and the remaining gap is speed/communication, which is what Phase 4 onward is calibrated for.

## 21.6 A minimal daily checklist (print this, or pin it)

- [ ] One spaced-repetition re-solve from the Question Bank (Ch. 20), unaided
- [ ] New problem(s) for today, full timed attempt before looking anything up
- [ ] Complexity stated out loud for every problem solved
- [ ] Mistake log updated (one line, even on a clean solve)
- [ ] Box checked in Ch. 20 only after solving from scratch, not after reading a solution

---
**Prev**: [Question Bank](20-Question-Bank.md) | **Back to**: [README](README.md)
