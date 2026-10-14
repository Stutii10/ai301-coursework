# Procedure

## Read order

1. In live mode, read `scope.md` first and confirm the PR's repo matches the Repo: line. If the line still holds the placeholder, stop without grading. In eval mode, ignore `scope.md` and `voice-guide.md` entirely.
2. Read `rubric.md` and `references/evidence-guide.md`. List the checks (name, required or preferred) and the verdict rule before grading anything.
3. Read the plan first: Scope, Changes, Test plan, then `## Deviations`. The plan is the yardstick, so read it before the things measured against it.
4. Read the diff next: the changed-file list first, then the hunks. Live mode: `git diff main...HEAD`, committed changes only. Eval mode: the diff section of the package.
5. Read the PR title and description (every section, including Notes for Reviewers), then the test evidence.
6. Read the repo's stated asks last: the PR template and CONTRIBUTING (live: the files in the repo; eval: the repo-facts block).
7. Do not grade from the description alone. It is a set of claims to check against the diff and the evidence. It is not evidence itself.

## Evidence gathering

1. For each rubric check, pull facts only from the parts its Evidence column names. Do not borrow from another part.
2. Plan fidelity: list every file in the diff, then mark each as named by the plan (file or its folder), named in Deviations, or named by neither. Also list what the plan promises that the diff lacks.
3. Description claims: list each concrete claim (each Changes bullet, "exactly as planned", "no X added", each ticked box). For each, find the diff line or pasted output that proves it. A claim that something was NOT added is proven if nothing in the diff adds it.
4. Test evidence: list the commands the plan's test plan and the PR template require. For each behavior the fix changes, find the command or inputs AND the specific observed result (see test-evidence-real in rubric.md). A bare "passed" or "works now" is not proof; for controls and routine checks, a stated result naming the command or a count is enough. Check that the repro appears for both before (main) and after (branch). A failing check reported with its reason counts as evidence.
5. Diff quality: scan the hunks for debug lines, commented-out code, unrelated edits, and files like plan.md, pr_draft.md, test_evidence.md, comment.md that should never be committed.
6. Standards: list each section and ask in the PR template and CONTRIBUTING, then find it in the draft: every section filled, Closes #N with the right number, the AI-use disclosure in Notes for Reviewers, the branch name format (live mode).
7. Quote or paraphrase the exact fact that decides each check, such as a file, a hunk, or a command with its output. A general impression of a section is not a fact.
8. If the named part is empty, missing, or doesn't address what the check asks, that is missing evidence, so grade unclear.

## Check execution

1. Grade every check in the rubric table. Don't skip one because it seems to overlap with another.
2. Grade each check pass, fail, or unclear, with a one-line reason naming the deciding evidence.
3. Unclear means the evidence the check needs is absent or ambiguous in the parts named. Before grading unclear, re-check that you read every part the Evidence column names.
4. A check passes only if the PR meets its stated pass condition on its own terms. A check that feels right but misses the condition fails or is unclear. Note any tension in the summary, but grade by the rubric's words.
5. Honest disclosure: a shortfall or deviation that both the description and `## Deviations` state is graded as disclosed. The same shortfall found only in the diff is drift.

## Verdict assembly

1. Apply `rubric.md`'s verdict rule exactly: accept only if every required check is pass. Reject if any required check is fail or unclear.
2. Preferred checks are reported but never change the verdict.
3. There is no third verdict: only accept or reject. Reservations go in the evidence lines.
4. In live mode, after the verdict, hold the PR title and description against `voice-guide.md` and note any broken rule in the summary. A broken voice rule does not change the verdict.
5. If the procedure was silent on something you had to decide, list it under "Procedure gaps" in the summary.
6. Output a short readable summary first (one line per check), then end with the fenced JSON block from the contract and nothing after it. The checks names must match the rubric's check names, and the grades must be pass, fail, or unclear.
