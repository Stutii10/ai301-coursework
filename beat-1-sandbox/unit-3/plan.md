# Plan: #60 — Faithfulness checker crashes when a context chunk has `text: None`

## Diagnosis

`chunk.get("text", "")` only substitutes the default `""` when the key
`"text"` is *absent* from the dict. When the key is present but its
value is `None` — as in `{"text": None}` — `.get()` returns `None`
itself, and the subsequent
`" ".join([chunk.get("text", "") for chunk in context_chunks])` on
line 38 of `rag/evaluator/faithfulness_checker.py` raises, because
`None` isn't a string. From my own reproduction
(https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5878503760):
```
>       score = checker.check(feedback, context_chunks)
...
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
E       TypeError: sequence item 0: expected str instance, NoneType found
rag/evaluator/faithfulness_checker.py:38: TypeError
```

The crash happens purely while constructing `context_text`, before any
claim extraction or scoring logic runs — no other code path is
implicated.

## Scope

**In scope:** the single line in `FaithfulnessChecker.check()` that
builds `context_text` from `context_chunks` (currently line 38 of
`rag/evaluator/faithfulness_checker.py`), plus deleting the
`@pytest.mark.xfail(strict=True, reason="issue #60: ...")` marker on
`test_none_context_chunk_text`. `docs/CONTRIBUTING.md` ("Working on a
seeded bug: remove its xfail marker") requires this: the marker is
strict, so once the fix makes the test pass, CI fails with
`XPASS(strict)` until the marker is gone.

**Out of scope:**

- `RelevanceScorer.score()`, which crashes on the same
  `{"text": None}` input with a *different* exception
  (`AttributeError: 'NoneType' object has no attribute 'lower'`),
  reported elsewhere in the thread — a different file, a different
  failure, not covered by this issue's test.
- `EvalSuite.run()`'s handling of that downstream `RelevanceScorer`
  crash.
- Claim extraction (`_extract_claims`) and any other scoring logic in
  `FaithfulnessChecker`.
- Any other test in `tests/unit/test_faithfulness_checker.py`,
  including the other `xfail` markers there, which belong to other
  issues.

## Files

- `rag/evaluator/faithfulness_checker.py` — the one-line fix.
- `tests/unit/test_faithfulness_checker.py` — delete the `xfail`
  marker (the decorator only) on `test_none_context_chunk_text`. The
  test body stays the same.

Branch: `fix/60-none-chunk-text` on my fork.

## Approach

Replace

```python
context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
```

with

```python
context_text = " ".join([chunk.get("text") or "" for chunk in context_chunks])
```

so a chunk whose `text` key is present but `None` is treated the same
as a chunk with no `text` key at all — both become `""`. One line of
logic, no new control flow, plus removing the test's `xfail` decorator
as CONTRIBUTING requires.

## Test plan

Before and after the change:

- The issue's own snippet (my posted repro ran the pytest command
  below, not this snippet; I'll run both):
  `python3 -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; FaithfulnessChecker().check('Knows Python.', [{'text': None}])"`
  — before: raises `TypeError`; after: must return a float in
  `[0.0, 1.0]` with no exception.
- `pytest tests/unit/test_faithfulness_checker.py -v -k none_context_chunk_text --runxfail`
  (my posted repro command) — before: 1 failed (traceback above);
  after: must pass.
- The same test **without** `--runxfail`, after the marker is deleted:
  `pytest tests/unit/test_faithfulness_checker.py -v -k none_context_chunk_text`
  must report `1 passed`. If it reports `XPASS(strict)` or `xfailed`,
  the marker removal or the fix is incomplete. CI runs it this way.
- Full file: `pytest tests/unit/test_faithfulness_checker.py` — nothing
  that passed before the change may fail after it.
- Controls (from the earlier macOS repro in this thread, not mine; I'll
  run them myself before and after): `check('Knows Python.', [{}])`
  must return `0.0`, and
  `check('Knows Python.', [{'text': 'Knows Python well'}])` must return
  `1.0`, both unchanged by the fix.

## Risks and unknowns

- Issue #60 already has an open PR (#74) making the same one-line
  change, plus two other posted plan comments proposing the identical
  fix. Per house rules a classmate's (or an existing PR's) plan
  doesn't block mine, so I'm posting this from my own reproduction
  regardless — but overlapping/duplicate work here is likely.
- I haven't traced whether any other call site relies on
  `chunk.get("text", "")`'s old behavior (expecting `None` to
  propagate rather than become `""`); this fix is scoped to the one
  line my repro and the named test cover.

## Deviations

(filled in after the build)
