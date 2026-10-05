# Rubric

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-matches-repro | Repro evidence, Diagnosis (and Summary if it restates cause) | The plan's stated root cause is consistent with the repro — e.g. the slow end-seek tracks syntax highlighting (`--color=always` vs `--color=never`, and/or ~same wall time with `--paging=never` as with the pager), not only pager behavior. Fails if the plan blames pager/bindings alone while repro shows long runtime with `--paging=never` and color on, or rules out the pipeline repro implicates without explaining that away. | required |
| scope-bounded | Scope and Changes | The plan names each file it will change and states at least one concrete out-of-scope area. Fails if no files are named, or scope contradicts the diagnosis (e.g. "not touching highlighting" while claiming to fix a highlighting-driven delay with no other evidence). | required |
| test-verifiable | Test plan first, then Changes only if the same test is named there | The plan names a specific, repeatable test — either an automated test (unit/integration/smoke run in CI) with what runs and what assertion passes, or a concrete re-run of the repro steps (commands/actions) with an explicit expected result stated for before vs. after the fix. Fails if the test plan only lists manual steps with no stated expected outcome (e.g. "open file, press keys" with no "should now show X"), or says "add a smoke test" with no command/assertion. | required |
| plan-specific | Candidate plan comment | The comment names the concrete mechanism or file from the plan (not a generic "I'll fix the bug"). Fails if it could apply to any open issue on the repo. | preferred |
| comment-matches-conventions | Thread highlights, Repo facts (contribution policy) | The plan comment doesn't contradict a stated repo convention or ignore a direct ask already in the thread — e.g. it respects a stated review-bandwidth or scope-minimizing note by keeping the described change small, or it addresses a maintainer's prior ask in the thread instead of only restating the original report. Passes when there are no thread comments and nothing in Repo facts the comment could contradict. Fails if the comment promises something a stated convention rules out (e.g. a large PR against a "keep it minimal" note), or ignores a specific request already posted in the thread. | required |

## Verdict rule

Ready only if every required check passes. Hold if any required check fails or is `?`.
`?` means the evidence needed for that check is missing or ambiguous — treat `?` as hold (a fail).
Preferred checks never change ready/hold.
