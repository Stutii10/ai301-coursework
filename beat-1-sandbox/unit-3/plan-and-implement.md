# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

---

## Posted upstream

**GitHub username**

Stutii10

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5986911778

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5986911778

I reproduced this myself (environment and steps above:
https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5878503760).
Root cause: `chunk.get("text", "")` returns `None` when the key is
present with value `None`, so `" ".join(...)` on line 38 of
`faithfulness_checker.py` raises before any scoring happens.

Plan: change that line to `chunk.get("text") or ""`, so a `None` text
is treated like a missing key. As `docs/CONTRIBUTING.md` asks for
seeded bugs, I'll also delete the `xfail(strict=True)` marker on
`test_none_context_chunk_text`; otherwise CI fails with `XPASS(strict)`
once the fix lands. So two files: the one-line fix in
`faithfulness_checker.py` and the marker removal in
`test_faithfulness_checker.py`. Not touching claim extraction, other
scorers, or the separate `RelevanceScorer` crash on the same input
(different exception, different file, not tracked by this issue).
Branch on my fork: `fix/60-none-chunk-text`.

Test, before and after:
- The issue's snippet: expect a float in `[0.0, 1.0]`, no exception.
- My posted repro command (`pytest ... -k none_context_chunk_text --runxfail`): expect pass.
- The same test without `--runxfail`, once the marker is gone: expect
  `1 passed`, the way CI runs it.
- The full `test_faithfulness_checker.py` file: nothing that passed before may fail.
- The `[{}]` → `0.0` and real-string → `1.0` controls from the earlier
  macOS repro above, which I'll run myself, should not change.

I see #74 and a couple of plan comments here already propose the same
one-line fix. I'm posting mine anyway since it's built from my own
reproduction, and I'm happy to step back if one of those merges first.
---

## Your branch

**Branch**

fix/60-none-chunk-text

**Evidence**

Before (issue's reproduction snippet):

    python3 -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; FaithfulnessChecker().check('Knows Python.', [{'text': None}])"

    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File ".../rag/evaluator/faithfulness_checker.py", line 38, in check
        context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
    TypeError: sequence item 0: expected str instance, NoneType found

After (same command):

    0.0

Before (targeted test, no --runxfail):

    pytest tests/unit/test_faithfulness_checker.py -v -k none_context_chunk_text

    FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text
    1 failed, 21 deselected in 0.07s

After (same command):

    tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text PASSED [100%]
    1 passed, 21 deselected in 0.11s

Before (full test file, from thread reproduction):

    18 passed, 4 xfailed

After (full test file):

    pytest tests/unit/test_faithfulness_checker.py -v

    19 passed, 3 xfailed in 0.09s

(The remaining 3 xfails belong to other, untouched issues — out of scope per the plan.)

Controls (unchanged before and after):

    python3 -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; print(FaithfulnessChecker().check('Knows Python.', [{}])); print(FaithfulnessChecker().check('Knows Python.', [{'text': 'Knows Python well'}]))"

    0.0
    1.0

## Eval iterations

**Run history**

1. Full run: 19/20 agreement. pkg-14 (clear-accept) disagreed — graded reject, failing all 5 checks. Cause: evidence-guide.md matched each check's evidence to the literal labels `Cause:`/`Change:`/`Test:` that only `calib-01.md` used; `pkg-14.md` used different labels (`Diagnosis:`/`Scope, one bounded change:`/`Files:`/`Test plan:`) for the same content, so every check found no evidence under the literal label and defaulted to hold.
2. Partial run (`--only pkg-14`), after rewriting evidence-guide.md to match each check by semantic role instead of literal label text: 1/1 agrees.
3. Partial canary run (`--only pkg-02,pkg-06,pkg-04,pkg-10,pkg-01` — one already-agreeing package from each category): 5/5 still agree, confirming the evidence-guide fix didn't flip anything that previously passed.
4. Full run (final, saved as `eval-run.txt`): 19/20 agreement. pkg-20 (thread-convention) disagreed — graded accept, gold is reject.

**Package analysis**

pkg-14 (zellij-org/zellij#5174, category: clear-accept). Gold verdict: accept. My rubric's first full run graded it reject, failing all five checks — not a judgment call on any one check, but a structural bug: evidence-guide.md told the skill to find evidence under the literal labels `calib-01.md` happened to use (`Cause:`/`Change:`/`Test:`), but `pkg-14.md`'s `## Candidate plan` section used different labels for the same four roles (`Diagnosis:`/`Scope, one bounded change:`/`Files:`/`Test plan:`, plus an optional `Risk:`). With no matching label text, every check found "no evidence" and the verdict rule treated that as hold. I rewrote evidence-guide.md to describe each check's evidence by semantic role ("whichever sentence states the root cause") rather than literal label text, with both packages' label styles given as examples. Re-graded alone, pkg-14 then agreed (accept): the diagnosis explains the fresh-attach/reattach split and accounts for the 0.44.1 control and the cache-clear observation; scope names the client-attach path and explicitly defers the Windows variant; the test plan gives concrete before/after conditions for the repro loop and both controls; the comment names the mechanism and explicitly acknowledges the thread's Windows reports without claiming to fix them.

**Check rationale**

"comment-matches-conventions | Thread highlights, Repo facts (contribution policy) | The plan comment doesn't contradict a stated repo convention or ignore a direct ask already in the thread — e.g. it respects a stated review-bandwidth or scope-minimizing note by keeping the described change small, or it addresses a maintainer's prior ask in the thread instead of only restating the original report. Passes when there are no thread comments and nothing in Repo facts the comment could contradict. Fails if the comment promises something a stated convention rules out (e.g. a large PR against a "keep it minimal" note), or ignores a specific request already posted in the thread. | required"

This check didn't exist in our first draft. The assignment's grading notes call out a "thread-and-convention" category explicitly, and our rubric had nothing that read `## Thread highlights` or the contribution policy at all — every package in that category would have passed by default regardless of content, for no principled reason. I added this check so the category has a real test behind it, and marked it required rather than preferred because a plan that contradicts a repo's stated review constraints, or ignores a maintainer's posted ask, isn't actually ready to post — it's not a minor style issue.

**Trade-offs**

It's deliberately permissive by default: with no thread comments and nothing in Repo facts to contradict, it passes automatically, so it only ever catches a plan that actively contradicts something stated — it does nothing to verify a plan engages with a thread that has no explicit policy or ask to point to. That gap showed up directly: pkg-20 (thread-convention) got a false accept on the final run, which I haven't root-caused. The check also can't verify compliance it can't see in a package's bundled evidence — on my own live plan for issue #60, it took the issue's real `docs/CONTRIBUTING.md` line about the strict xfail marker to catch a genuine scope gap, something no eval package's bundled "Repo facts" line had modeled, so I know the check depends on whatever convention happens to be visible to it rather than verifying completeness.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in `tools/plan-check/`.
