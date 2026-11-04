# Contribution:  API reference doc is missing the POST /profiles request body schema #37

Path: (https://github.com/codepath/pathreview-ai301-fa26-s1/issues/37)

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Stuti Goyal
**GitHub Username:** Stutii10
**Issue:** https://github.com/codepath/pathreview-ai301-fa26-s1/issues/37
**Pull Request:** https://github.com/codepath/pathreview-ai301-fa26-s1/pull/252

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built an issue scout and found its repo-health checks reject a busy classroom repo**

I wrote three `gh search issues` queries and ranking rules for tier-1 bugs, then ran the scout. It rejected all five issues it graded, mostly because `maintainer-alive` and `repo-in-use` fail for every issue in a repo last pushed on 2026-09-16. I tried a `-linked:pr` filter, but it removed every tier-1 bug (all had linked PRs), so I dropped it and counted PRs per issue instead. Widening to tier-1 docs issues found #37 and #11, which have no PRs and no comments.

**Unit 6: Installed a voice guard and wrote a repo memory file; both caught mistakes**

I added three upstream rules (claims, questions, pings) to my voice guide and wired in the hook. The wiring test was blocked as intended. `repro-check` rejected my first claim draft because it copied my own guide's example wording, so I rewrote it from what I read in `profiles.py` and `review.py`. I also patched the rubric's scope check to `scope-feasible` with a difficulty note. `CLAUDE.local.md` holds the repo's branch, commit and CI rules; it is excluded from git. [Add what you'd change next time.]

**Unit 7: Installed the ship-gate and a manifest; it rejected my first run for paperwork, not code**

My manifest runs make lint, black --check and make test-unit; make typecheck is excluded with a dated reason, since mypy fails on numpy stubs in my Python 3.13 environment (CI passes it on 3.11). On my real branch, the first run passed those checks but my pr-precheck rejected the plan, evidence and description, because I had added an Authentication paragraph and curl examples without recording them as deviations and had two test-plan items with no evidence. I fixed the paperwork instead of the code, and the second run passed. My voice guard then stopped the first PR attempt over a line that belonged under Notes for Reviewers.

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I started with bug queries for tier-1 issues. Every tier-1 bug already had open PRs from classmates, and the one without a crowd (#26) needed Docker, which I wanted to avoid. So I widened the search to tier-1 docs issues. That surfaced #37, #11, #47 and #36, and #37 and #11 had no PRs and no comments. I picked #37 (the missing request-body schemas for POST /profiles and POST /reviews in docs/API.md) because it is Python-adjacent, short, needs no Docker, and nobody else is on it. I hope to learn how this API's inputs actually work (multipart form fields versus a JSON body) and how to write docs a maintainer will merge.

My scout's verdict on #37 was reject. It failed three checks: `maintainer-alive` (last commit 2026-09-16, 47 days before the scan), `repo-in-use` (no push in the last 7 days), and `maintainer-engagement` (zero comments on the issue). It passed `newcomer-scope`, `unclaimed`, `ai-policy` and `good-first-issue-label`. I chose it anyway. Every failed check measures repo activity, not the issue itself, and the issue passed every check that is about the work: it is small, unclaimed, and has no AI-policy restriction.

Those three checks only look at commits, pushes and comments, so a classroom repo with no recent pushes fails them for every issue. They can't see that CONTRIBUTING.md sets a 48-hour response expectation or that about 170 PRs are open. That is where the scout fell short on a real search. In response, I patched the rubric scope check to `scope-feasible` and recorded the repo-health tension in my scan notes. Next time I would add a docs query to the search strategy.

Claim comment: (https://github.com/codepath/pathreview-ai301-fa26-s1/issues/37#issuecomment-6595482005)

**Toolkit**

- Tool: issue-scout (my search strategy and rubric), run as a Claude Code skill, plus manual `gh` queries for PR counts.
- What it produced: a scan that rejected all five bug issues it graded, then re-grades of #37 and #11 that rejected on repo-activity checks only.
- Difficulties: the repo-health checks fail every issue in a frozen classroom repo; `-linked:pr` removed every tier-1 bug; the first scan ranked a tier-2 issue first.
- What changed: patched the rubric scope check to `scope-feasible` with a difficulty note; dropped `-linked:pr`; chose #37 against the scout's verdict and wrote down why.
---

## Understanding the Issue

**Problem Description**

docs/API.md lists POST /profiles and POST /reviews in one line each and says nothing about what a request must contain. Someone has to read the source to learn the fields, the content types, or that auth is required.

**Expected Behavior**

Each endpoint’s entry says how to call it: the content type, each field with its type and whether it is required, the auth requirement, and what comes back.

**Current Behavior**

[Both entries are one-line summaries. portfolio_url on POST /profiles isn’t mentioned anywhere in the doc, and neither is the profile_id body for POST /reviews.

**Affected Components**

docs/API.md is the only file to change. The facts come from api/routes/profiles.py, api/routes/reviews.py and api/schemas/review.py, which are read-only here.

**Toolkit**

- Tool: reading the source by hand; Claude helped me draft this section
- What it produced: the four answers above
- Difficulties: none met
- What changed: nothing

---

## Reproduction Process

**Environment Setup**

macOS 15.7.3, zsh. No Docker and no running API; this reproduction reads the docs and source only. I forked codepath/pathreview-ai301-fa26-s1, cloned it, added `upstream`, and branched `docs/37-request-body-schemas` from main at commit f89c06f. Challenges: `gh repo fork --clone` failed because my Unit 4 clone already existed, so I reused it and added the upstream remote.

**Steps to Reproduce**

1. `grep -n -e 'POST /profiles' -e 'POST /reviews' docs/API.md` shows each endpoint is one line.
2. `grep -n -e 'Form(' -e 'File(' api/routes/profiles.py` lists the form fields.
3. `sed -n '22,28p' api/routes/reviews.py` shows the POST /reviews handler.
4. `sed -n '14,15p' api/schemas/review.py` shows the request schema.
5. Compare: the code defines request fields that docs/API.md does not describe. Observed: no request-body section exists for either endpoint.

**Reproduction Evidence**

- **Commit showing reproduction:** none; docs-only, base commit f89c06f
- **Screenshots/logs:**
  - API.md:18 and :24 are one-line entries for the two endpoints.
  - profiles.py:25-27: `github_username: str = Form(default=None)`, `portfolio_url: str = Form(default=None)`, `resume_file: UploadFile = File(default=None)`
  - reviews.py:22-24: `@router.post("", response_model=ReviewResponse)`, `async def create_review_endpoint(`, `data: ReviewCreate,`
  - review.py:14-15: `class ReviewCreate(BaseModel):` / `profile_id: UUID`
- **My findings:** The gap is real. POST /profiles takes three optional multipart fields, and `portfolio_url` is not mentioned anywhere in API.md. POST /reviews takes a JSON body with one field, `profile_id`. I did not run the API, and PDF upload behavior is unverified. Parts of this reproduction were drafted with Claude's help; I ran and checked the commands myself.

**Toolkit**

- Tool: repro-check (my skill), run on my reproduction draft.
- What it produced: a fail on unfilled placeholders first, then an accept once I replaced them with real commands and output.
- Difficulties: it refused to grade until the scout's #37 read-out was saved to scan-output.md, and my draft had a stray "4. 4." numbering typo.
- What changed: appended the #37 grade to scan-output.md and fixed the typo.

---

## Solution Approach

**Analysis**

The doc is thin because endpoints were summarized in one line each. The request shapes are defined in code: create_profile_endpoint in api/routes/profiles.py takes github_username and portfolio_url as optional form fields and resume_file as an optional upload (PDF, Markdown or plain text, otherwise 422), behind get_current_user. create_review_endpoint in api/routes/reviews.py takes a ReviewCreate JSON body (api/schemas/review.py) with one field, profile_id (UUID), and returns a ReviewResponse with status="pending" because the work runs in the background.

**Proposed Solution**

Add a short Request section under each of the two endpoints in docs/API.md, in the doc’s existing style, listing content type, fields (type, required or optional), auth, and the success response. Not changing: any code, the other endpoints, the Swagger and ReDoc sections. I’m also not documenting or fixing the PDF parsing behavior, because I haven’t verified it and it is outside this issue.

**Implementation Plan**

1. Re-read the three source files and write each field exactly as the code defines it.
2. Edit docs/API.md to add the Request section for POST /profiles, then for POST /reviews.
3. Re-run my reproduction commands to confirm the doc now matches the code, and preview the markdown.
4. Run make lint, .venv/bin/black --check . and make test-unit (the manifest checks).

**Deviations**

- Added one general paragraph under Authentication (bearer token from POST /auth/login, 401 otherwise) instead of repeating it in both Request sections. Not in the first plan.
- Added a curl example under each Request section. Not in the first plan.
- The doc had no existing request format to follow, so the table layout is new.

  
**Toolkit**

- Tool: plan-check (my skill), run on plan.md and plan-comment.md.
- What it produced: a hold on the first run, then an accept after I revised the plan.
- Difficulties: it first found no plan-comment.md for #37 and read the old #60 comment.md; the plan was missing the issue's request for a description and example value for each field; the test plan named no commands or expected results.
- What changed: wrote plan-comment.md, added descriptions and examples to the plan, and rewrote the test plan with commands and expected results. Its notes on my own skill files (grade labels, a scope note that disagrees with its rules) are still to fix.

---

## Testing Strategy

Before the change, a grep of docs/API.md at base commit f89c06f for the four request fields matched only the {profile_id} path parameters of GET and DELETE. After the change, the same grep finds each field under the right endpoint. I compared every field name, type and required or optional status with the Form( and File( lines in api/routes/profiles.py and ReviewCreate in api/schemas/review.py, compared the accepted resume types and both 422 cases with profiles.py, and previewed the rendered markdown. git diff --stat shows only docs/API.md changed. make lint, black --check and make test-unit pass (375 passed, 53 xfailed). make typecheck fails locally before checking any code (numpy stubs in my Python 3.13 environment) but passes in CI on Python 3.11, where all five jobs passed.

**Toolkit**

- Tool: grep and the repo's make targets, collected into test_evidence.md by shell commands.
- What it produced: before and after output, the code comparisons, and lint, black and unit test results.
- Difficulties: the "before" grep showed two profile_id matches that are path parameters, not request fields, so I added a note; mypy fails in my local environment.
- What changed: excluded make typecheck in my manifest with a dated reason; CI later confirmed it passes.


---

## Implementation Notes

**2026-11-03**

Wrote and ran plan-check, which held my first plan, revised it, and got an accept. Added a Request section for POST /profiles and POST /reviews to docs/API.md, plus a curl example under each and one Authentication line. Committed as 0bb97b3 on docs/37-request-body-schemas. The curl examples and the Authentication line were not in my plan.

**2026-11-04**

The first ship-gate run rejected the branch: lint, black and unit tests passed, but my pr-precheck failed three checks because the Authentication paragraph and curl examples were not recorded as deviations and the markdown preview and resume-type comparison in my test plan had no evidence. I added a Deviations section to plan.md, ran the comparison, previewed the markdown, and updated pr_draft.md. The second run passed. My voice guard then blocked the first PR attempt because a plan deviation sat under Changes, so it was moved to Notes for Reviewers. Opened PR #252; all five CI jobs passed.

**Toolkit**

- Tool: the ship-gate and my ship-manifest.md.
- What it produced: a reject on the first run, a pass on the second.
- Difficulties: the first reject was about paperwork, not the code; my terminal paste got cut off once while writing test_evidence.md.
- What changed: fixed the plan, evidence and description rather than the branch.

---

## Pull Request

**PR Link:** https://github.com/codepath/pathreview-ai301-fa26-s1/pull/252

**PR Description:** Adds a Request section under POST /profiles and POST /reviews in docs/API.md, with content type, authentication, and each field's type, description and example value, plus a curl example and the response fields. POST /profiles takes multipart form data, not JSON. Fixes #37. Docs only, no code changes. All five CI jobs pass.

**Maintainer Feedback:**

- 2026-11-04: none yet.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate, run by hand and then through the Claude Code hook when opening the PR.
- What it produced: PASS on the second run; the hook stopped the first PR attempt.
- Difficulties: see the Implementation Notes entry for 2026-11-04.
- What changed: moved the plan-deviation line to Notes for Reviewers.

---

## Learnings and Reflections

**Technical Skills Gained**

How FastAPI separates multipart form fields (Form, File) from a JSON body (a Pydantic model such as ReviewCreate), and how get_current_user reads a bearer token from the Authorization header. I also learned to check a doc against the code with grep and sed instead of trusting my reading of it.

**Challenges Overcome**

Every tier-1 bug already had PRs, so I moved to a docs issue. My ship-gate rejected my first run for paperwork: I had added an Authentication paragraph and curl examples without recording them as deviations, and two test-plan items had no evidence. I fixed the plan, evidence and description instead of the code. mypy failed locally on numpy stubs under Python 3.13, so I excluded it in my manifest with a dated reason; CI later passed it on 3.11.

**What I'd Do Differently Next Time**

Write the test plan with exact commands and expected results before building, and record every addition as a deviation at the moment I make it, not after the gate catches it.
---

## Resources Used

- docs/CONTRIBUTING.md and .github/workflows/ci.yml in the repo, which told me what checks to put in my manifest.
- Issue #37 and my claim comment, which fixed the scope (both endpoints, a description and example value for each field).
- api/routes/profiles.py, api/schemas/review.py and api/middleware/auth.py, the source I checked the doc against.
