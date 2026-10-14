# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

PR: https://github.com/codepath/pathreview-ai301-fa26-s1/pull/189

**Branch**

Branch: fix/60-none-chunk-text

## Eval iterations

**Run history**

Run 1 (full): 13/20. All 7 clear-accept packages were rejected, mostly on test-evidence-real, and the clear-accept category had no match.
Run 2 (partial, 7 packages): 2/7, after I loosened test-evidence-real. pkg-02 and pkg-05 flipped to accept.
Run 3 (partial, 11 packages): 7/11. This graded the same rubric as run 2, because the proposed edit had not been applied yet.
Run 4 (partial, 11 packages): 10/11, after applying the new rubric, procedure and evidence guide together. pkg-11, 13 and 19 flipped to accept and the six gold-rejects stayed reject.
Final run (full): FINAL, with every category matched. This is the run in eval-run.txt.

**Package analysis**

Package: pkg-05 (gold: accept, category clear-accept). My rubric rejected it in the final run, failing only test-evidence-real, so it is a miss.

The package prints real before and after tables for its central repro: before, one atuin row (modifier none, keycode up); after, two rows (control char_r and none up). Its routine checks are sentences: "cargo test -p nu-protocol passes (312 tests)" and "fmt and clippy clean". My rubric accepts a stated result naming a command or count for routine checks, so those parts pass.

My reading of why the grader rejected it: the package also changes a second behavior, a one-time warning when the same key is redefined, and for that it only says "Same-key redefine prints the one-time warning". No message text or output is shown. My row requires a specific observed result for every behavior the fix changes, so the grader treated that sentence as an asserted behavior. I think that reading is defensible under my own wording, but the gold label treats the package as ready, so the miss is on my rubric. pkg-05 also accepted in run 2 under an earlier, looser version of the row, which shows how close it sits to the line.

**Check rationale**

Check as it reads in tools/pr-precheck/rubric.md:

> ROW

I revised this row more than any other. My first version required real printed output for every command and said a sentence saying "passed" or a # note was not proof. That came straight from the activity worksheet, and it rejected all 7 gold-accept packages in my first full run (13/20). I split the check by weight: every behavior the fix changes must be shown run with its inputs and a specific observed result, while controls and routine checks (suite, lint, format, typecheck) only need a stated result naming the command or a count. I added that a specific result noted beside its command counts, and that a step-by-step transcript naming inputs and the exact result counts for interactive behavior. I rejected dropping the check or accepting any stated pass, because "works now" with nothing named has to keep failing: that is what keeps pkg-04 rejected.

**Trade-offs**

Loosening test-evidence-real means a PR can state "tests pass" with a named command or count and no printed output, and my rubric will accept that part. I accepted that risk because the first version wrongly held every honest clear-accept package. To check the cost, I re-ran canaries with --only: after the edit, pkg-04, 07, 10 and 14 (not-tested) and pkg-01 and 20 (standards-wall) all stayed reject in run 4, and the full run kept not-tested 4/4 and standards-wall 2/2. What it still misses is pkg-05 and pkg-16, which I chose not to tune further, because each additional loosening risked flipping a gold-reject.

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
