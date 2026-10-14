# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives.**
- Eval bundle: the plan's scope and "Files:" list and its test plan sit in the "Plan context" block. Any deviation note is in that same block. The changed files are the `---/+++` headers in the Candidate PR's "Diff" section and the "Commits" list. The description's claims are in "Description".
- Live: `plan.md` (Scope, Changes, and `## Deviations`) in the top folder of the clone. The changed files come from `git diff main...HEAD --stat` and the hunks from `git diff main...HEAD`. The claims are in `pr_draft.md`.

**What good looks like.** Every changed file, or its folder, is named by the plan or by a deviation note, and nothing the plan promises is missing without a note. Every concrete claim in the description ("exactly as planned", "no X added", each Changes bullet) is shown by a line in the diff. A claim that something was not added is true if nothing in the diff adds it. Silent drift is a file or hunk the plan never named, a plan item missing from the diff with no note, or a description claiming more or less than the diff delivers. A deviation that plan.md and the description both state is honest and re-ties the mismatch.

## Test evidence (harness category: not-tested)

**Where it lives.**
- Eval bundle: the "Test evidence" section of the Candidate PR, read against the "Test plan" in Plan context and the repro commands in the repro evidence.
- Live: `test_evidence.md` and the Testing section of `pr_draft.md`, read against the Test plan in `plan.md`. The repo's own checks are the ones its PR template lists (for Path Review: make test-unit, make test-integration, make lint, make typecheck).

**What good looks like.** For each behavior the fix changes, the evidence shows the command or inputs and the specific observed result. The repro appears for both before (main) and after (branch). A vague sentence ("works now", "tests pass") is a claim, not proof; a specific result noted beside its command, or a stated result for a control or routine check, is evidence. A failing check that is reported honestly with its reason counts as evidence. A failure that is hidden or summarized does not. If the plan names two tests and the evidence runs one, the missing one is a gap unless the description discloses it.

## Diff quality (harness category: unreviewable)

**Where it lives.**
- Eval bundle: the unified diff and the commit list in the Candidate PR.
- Live: `git diff main...HEAD` (committed changes only) and `git log main..HEAD --oneline`. `git diff main...HEAD --stat` should list only the files the fix changes.

**What good looks like.** A reviewer can see the fix, and nothing unrelated rides along. Debris tells: debug prints, commented-out code, dead code, formatting churn on untouched lines, drive-by edits, and planning files (plan.md, pr_draft.md, test_evidence.md, comment.md) that were committed by mistake. A one-line fix with a matching one-line description is the reference case.

## Standards and comms (harness category: standards-wall)

**Where it lives.**
- Eval bundle: the "Repo facts" block (PR template sections, contribution rules, stated AI policy) and "Thread highlights" for any maintainer direction. These are read against the Candidate PR's title and description.
- Live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in your Path Review repo, the issue's thread on GitHub, and your branch name from `git branch --show-current`. These are read against the title and description in `pr_draft.md`.

**What good looks like.** Every section or ask the repo states is filled with real content, not boilerplate. The issue is referenced the way the repo asks (Closes #N, correct number). An AI-use disclosure is present wherever the template or policy asks, and always in live mode, written in the author's own words under Notes for Reviewers. Any explicit maintainer direction in the thread is engaged, not ignored. The branch name has a type prefix, the issue number, and a short description (live mode). If the repo states no asks, as with a repo that has no template and no AI policy, nothing can fail here. A visibly skipped ask is a failure even when the code is perfect.
