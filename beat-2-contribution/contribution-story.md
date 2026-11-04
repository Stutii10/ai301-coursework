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
**Pull Request:** [not opened yet]

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

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

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

- Tool: issue-scout (your scout: your search strategy, your rubric),
  and how you used it
- What it produced: [the scan and its verdict on this issue, described]
- Difficulties: [where it fell short, or "none met" if none did]
- What changed: [what you patched or would update, or "nothing"]

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

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
