# Toolkit Portfolio

Student: Stuti Goyal

## Featured Tools

I chose these three because each one caught a real problem on my own contribution, issue #37 in Path Review, and each one changed after it did. The issue-scout found the issue, the plan-check graded my plan before I built anything, and the ship-gate checked my work before the pull request opened.

### issue-scout

What it is, and the judgment hole I wrote: It searches a repo's issues and grades each one against a rubric. The judgment is mine: the three search queries, the tier-1 ranking rules, and the rubric checks, such as whether an issue is small enough for a newcomer, unclaimed, and free of AI-policy limits.

How it evolved, dated: On 2026-11-02 it rejected all five bug issues it graded. A `-linked:pr` filter removed every tier-1 bug, so I dropped it and counted PRs per issue instead. I widened the search to tier-1 docs issues, which found #37 and #11. On 2026-11-03 I patched the scope check to `scope-feasible`.

Evidence: The scout's run on #37, 2026-11-02:

    {"name": "maintainer-alive", "grade": "fail", "evidence": "Last default-branch commit 2026-09-16 (47 days before 2026-11-02), outside the 30-day window"}
    {"name": "repo-in-use", "grade": "fail", "evidence": "pushedAt 2026-09-16; only branch is main; no push within the last 7 days"}
    {"name": "maintainer-engagement", "grade": "fail", "evidence": "Zero comments on the issue"}

Verdict: reject. I chose #37 anyway, because all three failed checks measured repo activity, not the issue, and I wrote down why. The 2026-11-03 re-grade recorded `scope-feasible`: pass.

### plan-check

What it is, and the judgment hole I wrote: It grades my fix plan and a draft comment against the issue thread before I write any code. The judgment is my rubric of five checks, such as whether the diagnosis matches the reproduction and whether the plan names runnable tests.

How it evolved, dated: On 2026-11-03 its first run held my plan, because the plan missed what the issue asked for, a description and an example value for each field, and its test plan named no commands. I rewrote the plan with source comparisons, expected results and exact commands, and wrote the plan comment it had found missing. The second run accepted.

Evidence: The first run's grade on my test plan: "test-verifiable: F. Step 3 names no commands and no expected result." The revised test plan:

    - Each of the four fields has a description and an example value in the doc.
    - git diff --stat shows only docs/API.md changed.
    - The markdown renders correctly in a preview.
    - make lint, black --check and make test-unit still pass
      (this shows nothing broke, not that the doc is right).

The second run graded all five checks pass, and I committed the change as 0bb97b3.

### ship-gate

What it is, and the judgment hole I wrote: A hook that runs before a pull request opens. It runs the project's own checks, then my pr-precheck on my plan, evidence and description. The judgment is my manifest: which checks this project requires, and which I exclude with a dated reason. It lists lint, black and unit tests, and excludes `make typecheck` on 2026-11-03 because mypy fails on numpy's type stubs in my Python 3.13 environment, while the project's CI runs 3.11.

How it evolved, dated: On 2026-11-04 the first run rejected my branch. The code passed everything, but my records did not match my work. I fixed the records, not the code: I added a Deviations section to my plan, ran the missing comparison, previewed the markdown, and updated the PR description. The second run passed.

Evidence: First run:

    plan-scope-fidelity [fail]: Diff adds a general Authentication paragraph ... that plan.md never names; plan.md has no ## Deviations section.
    tests-cover-test-plan [fail]: Test plan item 'markdown renders correctly in a preview' is never mentioned in evidence or description; the resume-types/422 comparison with profiles.py is not shown
    test-evidence-real [fail]: ... the planned preview and the accepted-types/422 comparison were never run or shown.

Second run: `ship-gate: PASS`, with `plan-scope-fidelity [pass]: ... the curl examples and Authentication line are recorded under ## Deviations.` I then opened PR #252, and all five CI jobs passed.

## Inventory

- voice guard: A hook that checks my outgoing words against my voice guide. On 2026-11-04 it stopped my first PR attempt because a plan deviation sat under Changes, and I moved the line to Notes for Reviewers.
- repro-check: Grades my reproduction write-up. It rejected my first claim draft because it copied my own guide's example wording, and rejected a reproduction with unfilled placeholders. It accepted both after I rewrote them from what I read in the code.

## The Arc

Beat 1
Date: 2026-11-02
Friction: The scout rejected all five bug issues it graded, and every tier-1 bug already had PRs from classmates. When I widened the search to docs issues, it graded #37 a reject too, because its repo-health checks fail every issue in a quiet classroom repo.
Change: I dropped the `-linked:pr` filter, searched docs issues, chose #37 against the scout's verdict and wrote down why, and patched the rubric's scope check to `scope-feasible`.
Result: I claimed #37 with a comment on the issue thread, and the 2026-11-03 re-grade passed `scope-feasible`.

Beat 2
Date: 2026-11-03
Friction: The plan-check held my plan. It missed the issue's request for a description and an example value per field, and my test plan named no commands or expected results.
Change: I rewrote the plan and the test plan, and wrote the plan comment it found missing.
Result: The plan-check accepted it. I built the change, 41 lines in docs/API.md and no application code, and committed it as 0bb97b3.

Beat 3
Date: 2026-11-04
Friction: The ship-gate passed my lint, black and unit tests, and still rejected the branch, because I had added an Authentication paragraph and curl examples my plan never mentioned and had two test-plan items with no evidence. My voice guard then stopped my first attempt to open the PR, because a plan deviation sat under Changes.
Change: I added a Deviations section to my plan, ran the missing comparison, previewed the markdown, updated the PR description, and moved the deviation line to Notes for Reviewers.
Result: The second ship-gate run passed, I opened PR #252, and all five CI jobs passed. It is still waiting for its first review.

## Limitations and What's Next

The toolkit checks my paperwork well and my code only as far as the project's own checks go. It has never seen a maintainer's reaction, so I can't say whether it makes a PR easier to merge. PR #252 is still open, and I have had no response from a maintainer yet, which is a normal state for a PR.

I did not run the API locally or test a real file upload. I wrote the documentation by reading the code and checking each field against it, so the PDF upload behavior is unverified, and my PR says so. I also recorded my deviations only after the ship-gate flagged them. Next time I would record a plan change when I make it.

The scout's verdict was not the final answer and the ship-gate's rejection was not a failure. Both made me question my assumptions, and I submitted work I could back with evidence.

Tool limits.
issue-scout: Its repo-health checks only see commits, pushes and comments, so it rejects every issue in a quiet classroom repo and cannot see that CONTRIBUTING.md sets a 48-hour response expectation.
plan-check: Its own skill files disagreed with each other, with different grade labels, a scope note that contradicts its rules, and a rubric written around an older issue.
ship-gate: It is built for code fixes, so it felt heavy on a one-file docs change, and it only runs the checks my manifest names.
