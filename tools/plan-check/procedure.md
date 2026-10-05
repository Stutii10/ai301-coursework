# Procedure

## Read order

1. In live mode, read `scope.md` first and confirm the issue is inside
   the scoped repo; note any house rules that affect how evidence is
   read. In eval mode, ignore `scope.md` entirely.
2. Read `rubric.md` and `references/evidence-guide.md`. List the
   rubric's checks (name, required or preferred) and its verdict rule
   before grading anything.
3. Read the whole package before grading anything: the issue (steps to
   reproduce, expected vs. actual), then the Repro evidence section
   (commands, timings, logs), then the candidate plan in this order —
   Diagnosis, Scope, Changes, Test plan — then the candidate plan
   comment last.
4. Do not grade from the plan comment alone. The comment is evidence
   only for the `plan-specific` check and is read after the plan it
   summarizes, never instead of it.

## Evidence gathering

1. For each rubric check, pull facts only from the sections
   `rubric.md`'s Evidence column names for that check. Do not borrow
   evidence from a section the check doesn't name, even if it would
   help the verdict.
2. When a check's Evidence column names more than one section in order
   (e.g. "Test plan first, then Changes only if the same test is named
   there"), read them in that order and stop once the pass condition
   is clearly met or clearly failed.
3. Quote or paraphrase the exact fact that decides each check — a
   command run, a file named, a stated expected result — not a general
   impression of the section.
4. If the named section is empty, missing, or doesn't address what the
   check asks, that's missing evidence for `?`, not an automatic fail.

## Check execution

1. Grade every check in the rubric table. Don't skip one because it
   seems to overlap with another.
2. Grade each check `P` (pass), `F` (fail), or `?` (unclear), with a
   one-line reason naming the evidence that decided it.
3. `?` means the evidence the check needs is genuinely absent or
   ambiguous from the sections named — not that you didn't look
   closely enough. Before grading `?`, re-check that you read every
   section the check's Evidence column names.
4. A check passes only if the plan meets its stated pass condition on
   its own terms. A check that "feels" right but doesn't meet the
   stated condition still fails or is `?`. Note any tension between the
   rubric's wording and your own judgment in the summary, but grade by
   the rubric's words, not the feeling.

## Verdict assembly

1. Apply `rubric.md`'s verdict rule exactly: ready only if every
   required check is `P`. Hold if any required check is `F` or `?`
   (treat `?` as hold).
2. Preferred checks are reported but never change the verdict.
3. There is no third verdict — only ready or hold.
4. In live mode, after reaching the verdict, hold the plan comment
   (`comment.md`) against `voice-guide.md` and note any broken rule in
   the summary. A broken voice rule does not change the verdict unless
   the rubric itself has a check that reads the voice guide.
5. Output a short readable summary first (one line per check, plus any
   voice-guide notes), then end with a fenced JSON block and nothing
   after it:

```json
   {
     "item": "<issue URL or bundle id>",
     "checks": [
       {"name": "<check name>", "grade": "P|F|?",
        "evidence": "<one line: the fact that decided it>"}
     ],
     "verdict": "accept|reject"
   }
```

   `accept` means ready; `reject` means hold. The eval harness parses
   the last fenced JSON block in the output, so it must be present,
   valid, and last.
