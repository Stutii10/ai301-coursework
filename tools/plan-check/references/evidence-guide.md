# Evidence guide

A package (`.md` file, or the equivalent bundle fields in live mode)
has these sections, in this order: a header block (source, captured
date, calibration flag), `## Repo facts`, `## Issue`, `## Thread
highlights`, `## Repro evidence`, `## Candidate plan`, `## Candidate
plan comment`.

There is usually no separate markdown heading for "Diagnosis,"
"Scope," "Files," or "Test plan" — they live inside the single
`## Candidate plan` section as inline labels. **The exact label
wording varies by author and is not itself meaningful** — e.g.
`calib-01.md` labels its parts `Cause:` / `Change:` (which folds
scope, files, and the out-of-scope note together) / `Test:`, while
`pkg-14.md` labels the same four roles `Diagnosis:` / `Scope, one
bounded change:` / `Files:` / `Test plan:` (plus an optional `Risk:`
note some plans add). Your actual `plan.md` will likely use the
`pkg-14` style (Diagnosis/Scope/Files/Test plan/Risks), since that's
the heading set the assignment itself asks for.

**Match on semantic role, never on literal label text.** For every
check below, read the whole `## Candidate plan` section and find
whichever sentence(s) play that role, regardless of what word
introduces them. Treat a check's evidence as missing (`?`) only if no
part of the section addresses that role at all — never because a
specific label string wasn't present.

## diagnosis-matches-repro

- **Where it lives:** whichever sentence(s) in `## Candidate plan`
  state the root cause — labeled `Cause:` in `calib-01.md`,
  `Diagnosis:` in `pkg-14.md`, or any similar wording — checked against
  `## Repro evidence` (and the `## Issue` body only for the reporter's
  original expected-vs-actual, since the repro is the verified version
  of that).
- **What good looks like:** the stated cause names a specific
  mechanism (a model, a callback, a flag, a handshake ordering) that is
  consistent with the repro's numbered steps — specifically, it
  explains the exact step where expected and actual diverge, not just
  the symptom in general, and it accounts for any control/regression
  data in the repro (a version that doesn't reproduce, a condition that
  clears it). Example from `calib-01.md`: the repro's step 3 vs. step 4
  (color stays stale until Esc/re-enter) is explained by "that view's
  model is not refreshed" after push. Example from `pkg-14.md`: the
  diagnosis explains why fresh attach is clean but reattach leaks, and
  explicitly accounts for the 0.44.1-clean control and the cache-clear
  observation from the thread — don't fail a diagnosis for being
  longer or more hedged than `calib-01`'s; length and hedging aren't
  the check.
- **What bad looks like:** a cause that only restates the symptom
  ("the color doesn't update"), or that contradicts a step in the
  repro (e.g. blaming an input binding when the repro shows the
  *push itself* succeeds and only the *display* is stale), or that
  ignores a control/regression result the repro reports instead of
  explaining it away.

## scope-bounded

- **Where it lives:** whichever sentence(s) in `## Candidate plan`
  state what's in scope and out of scope, and which file(s) will
  change — this may be one combined label (`Change:` in `calib-01.md`)
  or split across two (`Scope, one bounded change:` plus a separate
  `Files:` label in `pkg-14.md`). Read the whole section; don't require
  both halves under one label.
- **What good looks like:** at least one concrete file, path, or
  specific named component for what will change (e.g.
  `` `pkg/gui/controllers/sync_controller.go` `` in `calib-01.md`, or
  "the client attach/reattach path in `zellij-server`... and
  `zellij-client`'s terminal query issuance" in `pkg-14.md` — a named
  component is enough even before the exact function is pinned, as
  long as the plan says why it isn't pinned yet), and at least one
  concrete named area the plan will *not* touch (e.g. "any change to
  how push status is computed," or `pkg-14`'s explicit deferral of the
  Windows variant and the theme-detection cache).
- **What bad looks like:** no file, path, or component named at all,
  only a vague area ("the gui code"), or an out-of-scope claim that
  contradicts the stated cause (claiming not to touch the thing the
  cause says is broken).

## test-verifiable

- **Where it lives:** whichever sentence(s) in `## Candidate plan`
  state how the fix will be verified — `Test:` in `calib-01.md`,
  `Test plan:` in `pkg-14.md`, or similar. Only check the scope/change
  sentences as a fallback if the exact same test is also named there.
- **What good looks like:** either (a) a named automated test — a
  specific unit/integration/smoke test with a command and an
  assertion, or (b) a concrete re-run of the numbered repro steps with
  an explicit expected result stated for after the fix. Example from
  `calib-01.md`: "at step 3 the color must flip without leaving the
  view" is a concrete, checkable expected result tied to a specific
  repro step. Example from `pkg-14.md`: "5 consecutive SSH reattach
  cycles with no rgb strings in any pane," plus the control cases
  ("fresh-create still clean," "cache control... must become 'always
  clean'") restated as pass/fail conditions — not just "test that it
  works."
- **What bad looks like:** "add a smoke test" or "verify manually"
  with no named command/assertion and no explicit expected result tied
  to a specific step.

## plan-specific

- **Where it lives:** `## Candidate plan comment` only.
- **What good looks like:** the comment names the same concrete
  mechanism or file the plan names (e.g. "sync controller's push
  callback"), so it's clearly about this specific plan.
- **What bad looks like:** generic language ("I'll fix this issue")
  that could be pasted onto any open issue in the repo without
  editing.

## comment-matches-conventions

- **Where it lives:** `## Thread highlights` (prior comments on the
  issue) and `## Repo facts`' contribution-policy line, checked
  against `## Candidate plan comment`.
- **What good looks like:** when `## Thread highlights` is empty (as
  in most calibration packages) and nothing in the contribution policy
  bears on the comment, this passes by default. When there is a stated
  policy (e.g. "review time is scarce, prefer minimal PRs"), the
  comment should honor it explicitly or implicitly — e.g. `calib-01`'s
  comment says "keeping it minimal given the review-bandwidth note in
  CONTRIBUTING," directly acknowledging the policy. When there are
  thread comments, the plan comment should address any direct ask in
  them rather than ignore it. Example from `pkg-14.md`: the thread
  reports a Windows-only variant; the comment doesn't need to fix it,
  but it does need to acknowledge it rather than stay silent — which
  it does ("I cannot test the Windows session-switch variant reported
  here, so I am explicitly leaving it out and will flag the shared fix
  site in the PR").
- **What bad looks like:** a comment that promises something a stated
  policy rules out (a large multi-file PR against a "keep it minimal"
  note), or that never engages with a specific request already posted
  in the thread.
