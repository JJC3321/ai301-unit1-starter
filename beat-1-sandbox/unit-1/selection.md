# Unit 1 — Issue selection

Live-mode grading with the installed `issue-select` skill against
`codepath/pathreview-ai301-fa26-s3`, using the same rubric that passed
the eval harness at 19/20 (`beat-1-sandbox/unit-1/eval-run.txt`).

## Run history

1. Wrote `skill/rubric.md` (five required checks + one preferred) and
   filled the fit profile in `skill/scope.md`.
2. Full eval harness run against the 20 scored bundles
   (`python run_eval.py --rubric ../skill/rubric.md --out results.json`),
   finishing at **19/20 agreement (PASS)**; confirming transcript saved
   as `eval-run.txt`. One disagreement: `issue-19` (gold accept, skill
   reject on `newcomer-scope` — umbrella / multi-item body). Left the
   rubric as-is because the category floor held and the bar was met.
3. Live mode on Path Review candidates `#72`, `#68`, `#53` via:

```text
claude "issue-select: grade these candidate first issues: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72 https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68 https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53"
```

All three graded **accept**. No GitHub claim posted yet (selection only).

## Issue analysis

Scoped source only: `codepath/pathreview-ai301-fa26-s3`. Three open
`good first issue` / `tier-1` bugs were graded:

| Issue | Area | Ask | Notes |
|---|---|---|---|
| [#53](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53) | `safety/` | PII scrubber misses parenthesized phone numbers | Collaborator-filed; repro snippet; four named failing tests in `tests/unit/test_pii_scrubber.py` |
| [#72](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72) | `api/` | `verify_password` raises `UnknownHashError` instead of returning `False` | Fail-closed change in `core/security.py`; covering xfail test; est. 1–2h; zero comments |
| [#68](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68) | `rag/` | Keyword search `ZeroDivisionError` on empty index | Covering xfail test; est. 2–4h; classmate claim comment (ignored per Path Review house rule) |

Repo liveness: not archived; latest default-branch commit 2026-09-16.
No AI ban in CONTRIBUTING (workflow conditions only).

## Check rationale

Required checks applied the same way as in eval mode, with the Path
Review house rule on claims:

- `repo-alive` — pass for all three (alive repo, recent commits).
- `issue-open` — pass for all three (`state: open`).
- `unclaimed` — pass for all three: no assignees, no open linked PRs.
  On `#68`, a student claim comment exists; house rule says classmate
  claims do not block.
- `newcomer-scope` — pass for all three: each is a collaborator-filed
  bounded bug with named files/tests and no unsettled design debate.
- `ai-policy-ok` — pass for all three: no outright AI ban.
- `has-good-first-label` (preferred) — pass for all three.

Nothing was rejected; ranking used the fit profile only.

## Trade-offs

- `#53` vs `#72`: `#72` is smaller (1–2h) and also fits (`api/`), but
  `#53` has the strongest ready-made acceptance criteria (four failing
  tests already named) and sits in `safety/`, which matches the fit
  profile preference for shipping a bounded fix with tests.
- `#53` vs `#68`: both are in preferred areas (`safety/` / `rag/`), but
  `#68` is larger (BM25 empty-index behavior) and already has a classmate
  claim comment. House rule allows claiming anyway; still a reasonable
  tiebreaker to prefer the quieter, more concrete `#53`.
- Leaving the rubric strict on umbrella issues (`issue-19` disagree in
  eval) keeps false accepts low at the cost of one gold miss — acceptable
  given 19/20 and a full category floor.

## Selection rationale

Selected `#53` because it is the best fit under `scope.md`: Python work
in `safety/`, a single bounded regex/behavior fix, and explicit failing
tests that define done. It passed every required check and the preferred
newcomer label. `#72` and `#68` remain strong backups if `#53` becomes
unavailable.

## Issue link

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53

## Verdict output

Skill live-mode output (accepted candidates ranked by fit; selected
issue first):

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "archived: False; latest default-branch commit 2026-09-16 (within 365 days of run date)"},
      {"name": "issue-open", "grade": "pass", "evidence": "state: open"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; 0 comments; no linked or cross-referenced PRs in the timeline"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "Collaborator-filed bug with repro code and 4 named failing tests: one bounded regex fix in safety/pii_scrubber.py"},
      {"name": "ai-policy-ok", "grade": "pass", "evidence": "No CONTRIBUTING/AI-policy ban found; docs/CONTRIBUTING.md sets workflow conditions only — silence/conditions pass"},
      {"name": "has-good-first-label", "grade": "pass", "evidence": "labels: ['bug', 'good first issue', 'safety', 'tier-1']"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "archived: False; latest default-branch commit 2026-09-16"},
      {"name": "issue-open", "grade": "pass", "evidence": "state: open"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; 0 comments; no open PR in this repo mentions #72"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "Collaborator-filed bug: make verify_password fail closed in core/security.py, covering xfail test named, est. 1-2 hours"},
      {"name": "ai-policy-ok", "grade": "pass", "evidence": "No CONTRIBUTING/AI-policy ban found; silence/conditions pass"},
      {"name": "has-good-first-label", "grade": "pass", "evidence": "labels: ['bug', 'good first issue', 'api', 'tier-1']"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "archived: False; latest default-branch commit 2026-09-16"},
      {"name": "issue-open", "grade": "pass", "evidence": "state: open"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; no linked PRs; one student claim comment — Path Review house rule: student claims do not block"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "Collaborator-filed bug: KeywordSearcher.index([]) should not raise, covering xfail test named, est. 2-4 hours"},
      {"name": "ai-policy-ok", "grade": "pass", "evidence": "No CONTRIBUTING/AI-policy ban found; silence/conditions pass"},
      {"name": "has-good-first-label", "grade": "pass", "evidence": "labels: ['bug', 'good first issue', 'rag', 'tier-1']"}
    ],
    "verdict": "accept"
  }
]
```
