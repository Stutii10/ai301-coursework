# Voice guide: how I talk upstream

## Who I am in threads

I'm a first-time contributor to this project, still learning the codebase. I'm here to investigate and report what I find carefully, not to present myself as more experienced than I am. Readers should expect precise, hedged claims from me — not confident assertions I can't back up.

## Rules I write by

### Rule: Promise investigation, not outcome

I promise to investigate and report back, never a fix or a delivery date.

- Wrong: "I'll have this fixed by tomorrow."
- Right: "I'm going to dig into this and report back what I find."

### Rule: Flag uncertainty instead of guessing confidently

Anything I haven't confirmed gets stated as a guess, not a fact.

- Wrong: "This is caused by the cache not invalidating properly."
- Right: "This looks like it might be a cache-invalidation issue, but I haven't confirmed the root cause yet."

### Rule: State exactly what happened, not what I assume happened

I report the literal observed result, not my interpretation dressed up as the result.

- Wrong: "Reproduced the crash — classic null pointer issue."
- Right: "Ran the steps above and got [exact error]. Haven't traced the root cause yet."

### Rule: No unearned camaraderie or over-familiarity

I keep a level, professional tone rather than acting more familiar with the maintainers or the codebase than I actually am.

- Wrong: "No worries, I've totally got this one covered!"
- Right: "I'll post an update once I've worked through the repro."

## Things I never post

- Never promise a specific fix or a delivery date.
- Never claim a root cause I haven't actually traced or confirmed.
- Never write "should work" or "probably fine" as a substitute for actually testing something.
- Never copy a template response without editing it to reflect what I actually found.
