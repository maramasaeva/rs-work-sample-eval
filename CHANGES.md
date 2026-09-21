# CHANGES

Running log. Required by the working agreement: what changed, why, what it does not establish.

## 2026-09-21

**`notes.md` (new)** — read-only audit of the repo as committed: dataset counts, what the
accuracy denominator holds, which fields reach the judge, what the cached run produced,
ground-truth provenance. Plus weakness candidates C0–C7 not already in `task_1.md` #1–7, and
a draft ranking marked as Claude's.
*Does not establish:* nothing in the eval was run or changed to produce it. Every number is
read off the committed files.

**`task_1.md`** — added "What I would improve, and why" before the existing "What I fixed,
and why". Ranked three changes; recorded the rejected `incomplete` verdict and the reason.
*Does not establish:* it is a proposal. Nothing was implemented at the time of writing.

**`score_v2.py` (new)** — the reworked judge. New file rather than an edit so `score.py` stays
byte-identical for side-by-side diffing and re-running. Three fixes, documented in its header
with the previous behaviour quoted: (1) blind the judge — `failure_trap`, `model` and `qid`
removed from the prompt; (2) claim-level grading — per-claim `asserted_correct` /
`asserted_wrong` / `omitted` with a required evidence quote, replacing one bit per
multi-fact reference; (3) `source_authority` computed deterministically from an allowlist
instead of judged. `aggregate()` excludes ERROR/UNSCOREABLE rows from every rate and reports
them as counts, separates controls from ground_truth, and states every denominator.
`--selftest` covers the pure functions with no API key; it passes.
*Does not establish:* it has not been run against the cached answers — it refuses to, because
the claim lists and the domain allowlist are unwritten (slots a–e in the header, Mara's to
write). No v2 number exists yet. It also does nothing about the unvalidated judge: v2 changes
what the instrument is pointed at, not whether the instrument is any good. v1 and v2 outputs
are different shapes and are not comparable as a before/after.

**`task_1.md` weakness 6 + `score_v2.py` (fix 4)** — a judge endpoint that rejects
`temperature=0` (reported of GPT Astra) fails both branches of v1's `judge_one`, because the
fallback varied only `response_format` and kept `temperature=0`. Result: all 30 items error,
`scores.json` says `accuracy: 0.0` for both models, no error count, exit 0. Weakness 6 now
carries this as a worked example. `score_v2.py` adds a three-rung retry ladder whose last rung
sends no `temperature` parameter at all, records the rung used as `judge_call_mode`, and exits
non-zero if any row failed to score.
*Does not establish:* the ladder makes such a judge usable, it does not make it comparable —
a run that reaches the last rung is not the "single run, temperature 0" the README claims, and
`judge_call_mode` is what makes that visible rather than silent. Still no v2 numbers; the
slots remain unfilled. Tested only against a stub endpoint in `--selftest`, not against GPT
Astra itself.
