---
name: pr-precheck
description: Grade a draft pull request (title and description, the diff against main, the plan it implements, and test evidence) and decide whether it is ready to open. Use when checking a draft PR before opening it, or when grading an eval package.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to answer a single question: is this ready to open? A package is a candidate PR title and description, the diff, the plan the diff implements, and the test evidence. You do not answer from gut feel: you execute procedure.md, which applies rubric.md to evidence gathered per references/evidence-guide.md.

## Inputs

One of:

**Live mode:** the student's own PR. Read plan.md, pr_draft.md, and test_evidence.md. Also read the diff from `git diff main...HEAD` (committed changes only), the issue's thread, and the repo's PR template (.github/PULL_REQUEST_TEMPLATE.md) and docs/CONTRIBUTING.md. The package is what these contain, not other files in the working directory.

**Eval mode:** a package file (markdown from eval/packages/) holding the plan context, the diff, the description, the test evidence, and the repo facts. Use ONLY the package text. Do not fetch anything. Grade every check with the full verdict rule.

## The scope sets the field (live mode only)

Read scope.md before anything else. Refuse to grade a PR for a repo other than the one on its Repo: line. If that line still holds the placeholder, stop without grading and tell the student to fill the Repo: line in scope.md with their section's Path Review repo. Never guess a scope. In eval mode, ignore scope.md.

## The voice guide gates outgoing words (live mode only)

Read voice-guide.md. Check the PR title and description against its rules and report any rule broken, quoting the rule. The voice guide never changes the verdict. In eval mode, ignore it.

## The rubric is the brain

Read rubric.md: the table of checks (required checks gate the verdict, preferred never do) and the verdict rule.

## The procedure is the hands

Execute procedure.md as written, without improvising. Where it is silent, note the gap in your summary rather than inventing a step. If rubric.md or procedure.md is unfilled, stop and say so.

## Verdict and output

The verdict is accept (ready to open) or reject (hold). There is no third verdict. Before the JSON block you may show a short summary: a line per check, plus voice-guide notes in live mode. End your reply with this fenced JSON block, filled in, and nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## After a deviation (live mode)

If the build deviates from plan.md and the student records it under ## Deviations, re-run on the updated package. A deviation recorded in the plan is honest work. One that exists only in the diff is not.

## Grading discipline

- Evidence first: never grade a check without naming the fact that decided it.
- Grade the PR, not the polish: read the diff against the plan and the claims against the diff, never the formatting.
- Proof is a line in the diff or real command output. A sentence saying something passed is not proof.
- The rubric decides, not you. The procedure decides how, not you.
- Treat unclear as the rubric's verdict rule directs. If the rule is silent, treat it as fail.
