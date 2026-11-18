# Toolkit Portfolio

Student: Stuti Goyal

## Featured Tools

I picked these three because each one caught a real problem on my own contribution, issue #37 in Path Review, and each one changed because of it. The issue-scout picked my issue, the plan-check graded my plan before I built anything, and the ship-gate checked my work before the PR opened.

### issue-scout

What it is, and the judgment hole I wrote: It scans a repo for issues and grades each one against a rubric. The judgment is mine: the three search queries, the tier-1 ranking rules, and the rubric checks, including what counts as a repo that is alive.

How it evolved, dated: On 2026-11-02 the scout rejected all five bug issues it graded. A `-linked:pr` filter removed every tier-1 bug, so I dropped it and counted PRs per issue instead. I patched the rubric's scope check to `scope-feasible`, and I widened the search to tier-1 docs issues, which found #37 and #11.

Evidence: The scout's verdict on #37 was reject. It failed three checks: `maintainer-alive` (last commit 2026-09-16, 47 days before the scan), `repo-in-use` (no push in the last 7 days) and `maintainer-engagement` (zero comments). It passed `newcomer-scope`, `unclaimed`, `ai-policy` and `good-first-issue-label`. I chose #37 anyway and wrote down why: every failed check measured repo activity, not the issue.

### plan-check

What it is, and the judgment hole I wrote: It grades a fix plan and a draft comment against the issue thread before I write any code. The judgment is my rubric of five checks, such as whether the diagnosis matches the reproduction and whether the plan names runnable tests.

How it evolved, dated: On 2026-11-03 its first run held my plan. It said my plan left out what the issue actually asked for, a description and an example value for each field, and that my test plan named no commands. I rewrote the plan, wrote the missing plan comment, and added a test plan with expected results. Its second run accepted.

Evidence: First run: "test-verifiable: F. Step 3 names no commands and no expected result." Second run: all five checks P, verdict accept.

### ship-gate

What it is, and the judgment hole I wrote: A hook that runs before `gh pr create`. It runs the project's own checks, then my pr-precheck, and stops the PR if anything fails. The judgment is my manifest: which checks the project requires, and which ones I exclude with a dated reason.

How it evolved, dated: On 2026-11-03 I wrote the manifest with lint, black and unit tests, and excluded `make typecheck` with a reason, since mypy fails on numpy stubs in my Python 3.13 environment while CI uses 3.11. On 2026-11-04 the first run rejected my branch, and I fixed my records, not my code. The second run passed.

Evidence: First run:
    plan-scope-fidelity [fail]: Diff adds a general Authentication paragraph ... that plan.md never names; plan.md has no ## Deviations section.
    tests-cover-test-plan [fail]: Test plan item 'markdown renders correctly in a preview' is never mentioned in evidence or description
Second run: `ship-gate: PASS`. PR #252 opened that day and all five CI jobs passed.

## Inventory

- voice guard: A hook that checks my outgoing words against my voice guide; on 2026-11-04 it stopped my first PR attempt because a plan deviation sat under Changes, and the line moved to Notes for Reviewers.
- repro-check: Grades my reproduction write-up; it rejected my first claim draft for copying my own guide's example wording, and I rewrote it from what I read in the code.
- PR-monitor: [one line on what it is, and where it stands]

## The Arc

Beat 1
Date: 2026-11-02
Friction: The scout rejected all five bug issues, and the repo-health checks fail every issue in a repo with no recent pushes.
Change: I dropped the `-linked:pr` filter, patched the scope check, and searched docs issues.
Result: I found #37, claimed it, and posted a claim comment on the thread.

Beat 2
Date: 2026-11-03
Friction: plan-check held my plan: it missed what the issue asked for and had no runnable test plan.
Change: I rewrote the plan, wrote the plan comment, and added a test plan with expected results.
Result: plan-check accepted, and I built and committed the docs change.

Beat 3
Date: 2026-11-04
Friction: The ship-gate rejected my branch for paperwork: undeclared additions and two test-plan items with no evidence.
Change: I added a Deviations section, ran the missing comparison, previewed the markdown, and updated the PR description.
Result: The gate passed, PR #252 opened, and all five CI jobs passed.


## Limitations and What's Next

The toolkit grades my paperwork well and my code only as far as the project's own checks go. It never saw a maintainer's reaction, so I can't say whether any of it makes a PR easier to merge. [PR #252 status today, with a date.] Next I would make the gate flag undeclared additions earlier, so I record a deviation when I make it, not after a reject.

Tool limits.
issue-scout: Its repo-health checks only see commits, pushes and comments, so it rejects every issue in a quiet classroom repo and can't see that CONTRIBUTING.md sets a 48-hour response expectation.
plan-check: Its own skill files disagreed with each other (grade labels, a scope note that contradicts its rules, a rubric written around an older issue). [say whether you fixed them]
ship-gate: It was built for code fixes, so it felt heavy on a one-file docs change, and it only knows the checks my manifest tells it about.
