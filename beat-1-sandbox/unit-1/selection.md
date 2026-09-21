# Unit 1 — Issue Selection

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60

**Verdict output**

```
Mode: live. Candidate is in the scoped repo (codepath/pathreview-ai301-fa26-s1). Note: the fit profile section of scope.md is still the placeholder (Write a few sentences here.) — with a single candidate there is nothing to rank, so it didn't affect this grade, but fill it in before grading multiple issues.

Issue #60 — "Faithfulness checker crashes when a context chunk has text: None"

- maintainer-alive — pass: newest commit on main is 2026-09-16 (chore: track five more manifest entries against the tracker, Aburke225), 5 days ago.
- repo-in-use — pass: repo pushed_at is 2026-09-16, within 7 days; not archived, single branch main.
- newcomer-scope — pass: a single concrete bug with a named root cause (chunk.get("text", "") returns None when the key exists), a runnable repro, and a named failing test test_none_context_chunk_text. No sub-item checklist, no comments at all so no design debate, no core-internals statement, 0 linked PRs, no TBD markers.
- unclaimed — pass: assignees empty, repo has 0 pull requests total (so no linked PR), timeline holds only four labeled events and zero comments.
- ai-policy — pass: no CONTRIBUTING.md at root or .github/, no AI policy file; docs/CONTRIBUTING.md and the PR template state CI/test/xfail requirements but say nothing about AI-assisted work. Conditions, not a ban.
- good-first-issue-label (preferred) — pass: labels are bug, good first issue, rag, tier-1.
- maintainer-engagement (preferred) — fail: zero comments on the issue, so no Owner/Collaborator reply. The issue was opened by a Collaborator (Aburke225), but the check asks for a comment on the thread.

All five required checks pass → accept. One useful thing docs/CONTRIBUTING.md flags for when you take it: this is a seeded bug, so removing its @pytest.mark.xfail(strict=True) marker is part of the fix, or CI goes red on an unexpected pass.

{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
  "checks": [
    {"name": "maintainer-alive", "grade": "pass",
     "evidence": "Newest commit on default branch main is 2026-09-16T21:42:18Z by Aburke225, 5 days before today (2026-09-21)."},
    {"name": "repo-in-use", "grade": "pass",
     "evidence": "Repo pushed_at = 2026-09-16T21:48:27Z, within the last 7 days; archived: False."},
    {"name": "newcomer-scope", "grade": "pass",
     "evidence": "Body gives one concrete defect plus repro and a named failing test: \"Related failing test: test_none_context_chunk_text\"; no sub-item checklist, 0 comments, 0 linked PRs, no TBD markers."},
    {"name": "unclaimed", "grade": "pass",
     "evidence": "assignees: [], comments: 0, and the repo has 0 pull requests total, so no open or linked PR exists."},
    {"name": "ai-policy", "grade": "pass",
     "evidence": "No CONTRIBUTING.md at root or .github/ (404), no AI_POLICY.md/AI_USAGE_POLICY.md; docs/CONTRIBUTING.md and PULL_REQUEST_TEMPLATE.md require CI/tests but never mention AI-generated contributions."},
    {"name": "good-first-issue-label", "grade": "pass",
     "evidence": "Labels: ['bug', 'good first issue', 'rag', 'tier-1']."},
    {"name": "maintainer-engagement", "grade": "fail",
     "evidence": "comments: 0 — the timeline holds only four 'labeled' events, so no Owner/Collaborator comment on this issue."}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

**Run history**

1. First full run (initial rubric — four required checks with a label-based bypass on `newcomer-scope`): agreement: 16/20 scored items (bar: 18/20: below the bar; category floor unmet: no match in policy)
2. Partial re-check on the 4 misses (`--only issue-10,issue-12,issue-15,issue-20`), after adding a required `ai-policy` check and removing the label bypass: agreement: 3/4 scored items
3. Full run after further tightening `newcomer-scope` with a strict "must have engagement" clause: agreement: 14/20 scored items (bar: 18/20: below the bar)
4. Final full run, after removing the engagement clause and keeping only the "unresolved product-decision markers" signal: agreement: 18/20 scored items (bar: 18/20: PASS) — this is the run saved to eval-run.txt.

**Issue analysis**

Issue: issue-12

- Gold label: reject, with the note: "passes every liveness, scope, and claim check; the repo's contributing docs ban AI-generated code and documentation outright."
- My rubric's verdict: reject (agrees with gold).
- Reasoning: my first rubric had no check at all for contribution policy — it only covered the four families named in lecture (maintainer-alive, repo-in-use, scope, claimed). On my first full eval run this showed up directly as a category-floor failure: policy 0/1. Since this issue passes every liveness, scope, and claim signal cleanly, a rubric without an explicit policy check will always accept it regardless of luck — it's a structural blind spot, not a near-miss. I fixed it by adding a required ai-policy check that fails only on an explicit ban on AI-assisted contributions (not merely disclosure or review requirements), which correctly flips this issue to reject.

**Check rationale**

Check: ai-policy, quoted as currently written in my uploaded rubric.md:

"PASS unless a policy file is found AND it explicitly bans AI-generated or AI-assisted contributions outright. No policy file found = PASS. Conditions (disclosure required, human review required, testing required) are acceptable and still PASS."

Reasoning: my first instinct was to treat a missing policy file as unclear and therefore fail it, matching my strict rule elsewhere that unclear counts as fail. But most repos never state an AI policy either way, so that would have auto-rejected many of my clear-accept issues too. I decided only an explicit ban should be disqualifying — absence of a stated policy is not evidence of a ban.

**Trade-offs**

The ai-policy check's "no policy file found = PASS" rule gives up catching repos that might have an unstated or informal anti-AI norm not written into CONTRIBUTING.md — it will only ever catch bans that are explicitly documented. I accept this miss deliberately: I re-ran an early stricter version (treating missing-file as unclear/fail) and it cost me issues in the clear-accept category that had nothing to do with AI policy at all, which was a worse trade than occasionally missing an unwritten norm.

---

## Selection rationale

**Selection rationale**

1. Yes — a bounded bug like this is the right size for me to take on right now, and it's within a few hours of work rather than a multi-day investigation.

2. The rubric correctly identified the bug as bounded, unclaimed, with a named root cause and a failing test in an actively maintained repo. What I weighed myself, that the rubric has no way to check, was my own comfort with Python and whether the fix (handling a None value from chunk.get("text", "")) looked like something I could actually implement — a simple defensive check or default value — rather than something requiring deep RAG-pipeline knowledge.

3. Unfamiliarity with the codebase — I haven't worked in this repo before, so I'll need to spend time understanding how the faithfulness checker and its context-chunk handling fit into the broader RAG pipeline before I can be confident the fix doesn't break something else. I also expect to need to understand the @pytest.mark.xfail(strict=True) marker removal mentioned in CONTRIBUTING.md, since that's specific to how this repo seeds and verifies bugs.
