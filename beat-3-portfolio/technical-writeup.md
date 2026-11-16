# From Issue Discovery to Pull Request: My Open-Source Contribution to PathReview

## Overview

As part of CodePath's 10-week AI301 | AI Open Source Capstone, I contributed to PathReview, an open-source project whose API documentation lacked request details for two endpoints: creating profiles and requesting reviews. I added field descriptions, data types, authentication requirements, and examples to help developers use these features without examining the source code. My contribution is documented in [Pull Request #252](https://github.com/codepath/pathreview-ai301-fa26-s1/pull/252). I also developed my own issue-scout search strategy and evaluation rubric, a voice guide for contribution communication, and a ship-manifest defining validation checks. The PR remains open, with all five CI checks passing and no maintainer response yet.

## What happened

### 1. Finding an issue and questioning my own tool

I developed an issue-scout using Claude Code to evaluate potential contributions against a rubric I designed.

My initial search focused on beginner-friendly bugs, but all five evaluated issues were rejected. Adding `-linked:pr` eliminated every tier-1 bug, so I expanded my search to documentation issues and found Issue #37.

The scout recommended rejecting it.

**Evidence — Actual issue-scout output, November 2:**

```json
{"name": "maintainer-alive", "grade": "fail", "evidence": "Last default-branch commit 2026-09-16 (47 days before 2026-11-02), outside the 30-day window"}
{"name": "repo-in-use", "grade": "fail", "evidence": "pushedAt 2026-09-16; only branch is main; no push within the last 7 days"}
{"name": "maintainer-engagement", "grade": "fail", "evidence": "Zero comments on the issue"}
```

The scout's recorded verdict was `"reject"`.

I overrode this recommendation because the failures reflected repository inactivity, not the complexity of the task. The issue was unclaimed, clearly scoped, and aligned with my Python experience.

I documented my reasoning and revised the rubric's scope evaluation to `scope-feasible`. The November 3 re-grade recorded:

```json
{"name": "scope-feasible", "grade": "pass", "evidence": "One identifiable task: add request bodies for POST /profiles and POST /reviews to docs/API.md, relevant files named"}
```

### 2. Reproducing the documentation gap

After selecting Issue #37, I examined `docs/API.md` and compared it against the implementation using `grep` and `sed`.

The documentation originally summarized `POST /profiles` and `POST /reviews` in one line each without explaining their request bodies.

**Evidence — Fields defined in `api/routes/profiles.py`:**

```python
github_username: str = Form(default=None)
portfolio_url: str = Form(default=None)
resume_file: UploadFile = File(default=None)
```

These lines confirmed that the profile endpoint accepted optional form fields and a file upload. I also verified that the review endpoint accepted JSON containing a `profile_id`.

My contribution documented these fields, their types, whether they were optional or required, and example values. I also included authentication requirements, responses, and practical request examples.

My first repro-check rejected an incomplete draft because it contained placeholders and lacked evidence. After correcting the report and recording the source comparisons, the reproduction was accepted.

### 3. Revising the plan and implementing the contribution

Before implementation, I created a plan and evaluated it with plan-check.

The first review placed the plan on hold because it lacked complete field descriptions, examples, and a sufficiently specific test strategy.

I revised it to include source-code comparisons, expected results, and exact validation commands.

**Evidence — Final `plan.md`, Test Plan:**

```text
- Each of the four fields has a description and an example value in the doc.
- git diff --stat shows only docs/API.md changed.
- The markdown renders correctly in a preview.
- make lint, black --check and make test-unit still pass
  (this shows nothing broke, not that the doc is right).
```

The revised plan was accepted.

On November 3, I implemented the documentation changes in `docs/API.md` and committed them as **0bb97b3**.

The commit added 41 lines of documentation, including request-body tables and `curl` examples, without changing application code.

### 4. The ship-gate caught what I missed

I created a ship-manifest defining the checks required before submitting my contribution.

On November 4, my first manual ship-gate run passed linting, formatting, and unit tests, but rejected the submission because three documentation and evidence checks failed.

**Evidence — First ship-gate run:**

```text
- verdict: BLOCK (block)

- plan-scope-fidelity [fail]: Diff adds a general Authentication paragraph ('Endpoints marked "requires authentication" expect the token…') that plan.md never names; plan.md has no ## Deviations section.

- tests-cover-test-plan [fail]: Test plan item 'markdown renders correctly in a preview' is never mentioned in evidence or description; the resume-types/422 comparison with profiles.py is not shown (only Form( and File( lines).

- test-evidence-real [fail]: Before/after grep, code comparison, lint, black and 375 passed are real output, but the planned preview and the accepted-types/422 comparison were never run or shown.
```

These failures revealed that I had made useful additions without updating my plan and had not collected all the evidence I promised.

**What I changed:** I added a `## Deviations` section to `plan.md`, documenting the authentication paragraph and the `curl` examples.

The final plan explicitly records:

```text
- Added one general paragraph under Authentication saying protected endpoints take a bearer token from POST /auth/login and return 401 otherwise, so the two Request sections do not each repeat it. Not in the original plan; it documents behavior of get_current_user in api/middleware/auth.py.

- Added a curl example under each Request section to show an example value in context. Not in the original plan.
```

I also completed the Markdown preview and missing source-code comparisons, then updated my test evidence.

**Evidence — Second ship-gate run, November 4 at 00:05:**

```text
- verdict: PASS

- plan-scope-fidelity [pass]: Only docs/API.md changed, which plan.md names; the curl examples and Authentication line are recorded under ## Deviations.

- tests-cover-test-plan [pass]: test_evidence.md has before/after grep, Form(/File( and sed comparison, git diff --stat, markdown preview, and lint/black/test-unit output, covering each test-plan item.
```

**The result:** The second ship-gate run passed because I corrected the gaps identified in the first run. My custom validation process caught mistakes before submission and gave me a clearer record of what I had actually tested.

My voice guard also identified a misplaced explanation in my pull request description, which I moved into the reviewer notes.

On November 4, I opened [PR #252](https://github.com/codepath/pathreview-ai301-fa26-s1/pull/252). All five CI jobs passed, with unit tests reporting **375 passed and 53 expected failures**.

The PR has not been reviewed or merged.

## Limitations and what's next

My issue-scout relied heavily on repository activity, causing it to reject suitable issues in a quiet classroom repository. Next time, I would include documentation issues in my initial search and evaluate task feasibility separately from repository activity.

I also did not run the API locally or verify PDF upload behavior. My documentation was based on reading the implementation. Running the application and testing actual file uploads would help validate those details.

Finally, I documented deviations only after the ship-gate flagged them. In future contributions, I would record changes to the implementation plan as they happen.

**My biggest takeaway was learning to use AI-assisted tools without surrendering engineering judgment.** The scout's rejection was not the final answer, and the ship-gate's rejection was not a failure of the contribution. Both were opportunities to question assumptions, improve my process, and submit work supported by evidence.
