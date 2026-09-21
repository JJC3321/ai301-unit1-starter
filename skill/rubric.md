# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `repo-alive` | Repo-facts block: the `archived:` flag on the repo line, and the dates in "last 5 default-branch commits". Measure every recency threshold against the bundle's capture date in eval mode, or against today in live mode. | `archived` is `no`, AND at least one of the last 5 default-branch commits is dated within 365 days of the capture date (eval) or today (live). Bot-authored commits count if they merge human work (a merge commit or Dependabot bump still shows the repo is moving). | required |
| `issue-open` | Issue header in the bundle (or the live issue page): the `state` field. | State is `open`. | required |
| `unclaimed` | Repo-facts "this issue" line for assignees and linked PRs; plus the Comments section for claim language ("I'll take this", "working on this", "can I work on this", "I've started"). | All of: (1) assignees is `none`; (2) no linked PR is in state `open`; (3) no claim comment in the last 90 days (relative to capture date / today) that a maintainer (Owner/Member/Collaborator) has not clearly released. A closed or merged linked PR alone is not a claim. A bot nudge or stale claim older than 90 days does not block. | required |
| `newcomer-scope` | Issue title and body, plus the comment thread. | The ask is one bounded change a newcomer could finish without inventing product policy. Fail when any of these hold: the issue is an umbrella / megaissue / tracking list of sub-items; the thread shows unsettled design debate with no maintainer decision; the body is a one-line feature wish with no concrete behavior or acceptance criteria; a maintainer says the fix needs core internals redesign; or it is a usage/support question, not a contribution. Age alone does not fail the check. Short maintainer-filed bugs with a clear expected behavior pass. | required |
| `ai-policy-ok` | Repo-facts "contribution policy" line (and any linked CONTRIBUTING / AI policy text quoted there). | The policy does not ban AI-generated code or documentation outright. Silence passes. Disclosure, "understand your changes", testing, or human-review requirements pass (those are conditions to follow, not bans). Only an explicit refusal such as "we do not accept AI-generated code/documentation" fails. | required |
| `has-good-first-label` | Issue labels in the Issue header. | Labels include `good first issue`, `good-first-issue`, `easy`, or an equivalent newcomer label. | preferred |

## Verdict rule

Accept only if every `required` check grades `pass`. Any required `fail` or `unclear` makes the verdict `reject`. Preferred checks never change the verdict; on accepted issues they only help rank candidates.
