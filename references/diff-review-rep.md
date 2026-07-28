# Diff Review Rep

Create a compact, realistic review exercise that trains defect detection and engineering judgment.

## Construct the Patch

- Match the active repository's language, framework, testing style, and conventions.
- Keep the patch reviewable in roughly 15–25 minutes: usually 1–3 files and 30–100 changed lines.
- Give the change a plausible product goal and enough surrounding context to judge behavior.
- Plant 2–5 meaningful findings appropriate to the user's demonstrated level.
- Include some correct or defensible code so every changed line is not suspicious.
- Make planted findings internally consistent with the scenario and answer key.

Vary findings across reps:

- incorrect state, stale closures, mutation, or derived-state drift;
- async races, missing cleanup, error handling, or loading-state failures;
- accessibility, semantics, keyboard behavior, or focus management;
- unsafe TypeScript modeling or invalid boundary assumptions;
- performance regressions with credible user impact;
- brittle tests, missing cases, or assertions that cannot catch the defect;
- API, security, privacy, persistence, or data-integrity risks;
- maintainability issues only when they create a concrete cost or failure mode.

Avoid trick trivia, purely subjective style complaints, invisible requirements, and unrealistic combinations of unrelated mistakes. Do not label a tradeoff as a defect when reasonable teams could choose it.

## Keep an Answer Key

Before showing the diff, record privately for each planted finding:

- file and line or hunk;
- classification: defect, risk, or anti-pattern;
- severity and likely impact;
- triggering condition;
- expected reasoning;
- one proportional fix;
- tests or checks that would expose it.

Do not put the answer key in a file visible to the user during the attempt. Preserve only a concise continuation note in `ACTIVE_WORKOUT.md`; archive the full answer after the review is complete.

## Calibrate Difficulty

- Foundational scope: local correctness, semantics, state flow, clear edge cases, and basic tests.
- Mid-level scope: connected component behavior, boundaries, async behavior, accessibility, and maintainability.
- Senior scope: ambiguous risk, failure modes, rollout concerns, observability, migration, and prioritization.
- Staff scope: cross-system consequences, platform contracts, adoption risk, governance, and organizational tradeoffs.

Increase difficulty through subtlety, ambiguity, and consequence—not by making the diff longer.

## Review the Review

Evaluate whether the user:

- found high-impact issues before polish;
- cited concrete code and triggering behavior;
- assigned proportionate severity;
- separated certainty from assumptions;
- proposed the smallest reliable fix;
- identified useful verification or regression tests.

Accept additional valid findings not in the answer key. Explain why a claimed issue is contextual or stylistic when it is not a defect.
