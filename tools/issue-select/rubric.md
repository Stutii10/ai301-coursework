# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Last commit date to the default branch (repo front page commit history) | At least one commit to the default branch within the last 30 days | required |
| repo-in-use | Date of most recent push to any branch (repo front page "commits" across all branches, or branch list with last-updated dates) | At least one push to any branch within the last 7 days | required |
| newcomer-scope | Issue body and full comment thread: umbrella/tracking markers (numbered sub-item checklist, "megaissue" or similar self-description), unresolved design debate, maintainer statements about core-internals, count of closed-unmerged linked PRs, whether the issue states a concrete expected behavior/spec vs. a vague wish, and unresolved product-decision markers (phrases like "TBD," "not yet decided," an unchecked roadmap-fit checkbox) | PASS only if all of: not an umbrella/tracking issue (and not self-described as one), no unresolved design debate in the thread, no maintainer statement it requires core-internals changes, fewer than 2 closed-unmerged PRs linked to it, the issue states a concrete expected behavior/fix rather than an open-ended feature wish, AND the issue does not itself flag unresolved product decisions (e.g., "TBD," "not yet decided," an unchecked roadmap-fit checkbox) that would require the contributor to decide direction rather than implement a specified fix | required |
| unclaimed | Assignees box (issue sidebar); Development box linked PRs and their state (open/closed); comment thread for claim language and any maintainer/Owner/Collaborator response to it | PASS unless: an assignee is set, OR an open linked PR exists, OR a maintainer/Owner/Collaborator has explicitly acknowledged or assigned a claim (e.g. replied "go ahead," or added an assignee) within the last 6 months with no sign it went stale. A bare claim comment from a non-classmate with no maintainer acknowledgment does not block. Claims from classmates never block (Path Review house rule). | required |
| ai-policy | CONTRIBUTING.md (repo root or .github/); any AI_POLICY.md/AI_USAGE_POLICY.md; PR/issue templates for AI-use disclosure | PASS unless a policy file is found AND it contains language prohibiting AI-generated or AI-assisted contributions — phrases such as "do not use AI," "AI-generated code is not permitted," "prohibited," or "will not be accepted" applied specifically to AI/LLM-authored contributions. No policy file found = PASS. A policy that imposes conditions rather than a prohibition (e.g., "AI use must be disclosed," "AI-assisted code requires additional review/testing") still PASSes — conditions are not bans. | required |
| good-first-issue-label | Labels listed on the issue sidebar | Issue carries a good-first-issue (or equivalent beginner-friendly) label | preferred |
| maintainer-engagement | Issue thread: presence of a comment from someone with an Owner/Member/Collaborator badge on this specific issue | At least one maintainer/collaborator comment on this issue | preferred |

## Verdict rule

Accept if and only if every required check passes. A single required fail or unclear produces a reject.

preferred checks never change the verdict; they rank accepted issues against each other (more preferred checks passed = ranked higher).

unclear is treated as fail for every required check: if you can't verify it, don't accept it.
