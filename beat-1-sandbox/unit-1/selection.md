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

### Scored eval issue: `issue-19` (zxcalc/zxlive#517)

`issue-19` is the one scored item my rubric got wrong (gold **accept**,
skill **reject**; `eval-run.txt` note: `failed: newcomer-scope,
has-good-first-label (preferred)`). Only `newcomer-scope` caused the
reject; the preferred label check cannot change a verdict.

- **The issue.** "Selecting large subgraphs in proof mode freezes the
  UI", filed by a COLLABORATOR, open, unassigned, no linked PRs, 0
  comments, labels `Type: bug, Category: Proof mode, Priority: High`.
  The body names "two potential causes which should be fixed" (slow
  quadratic matchers; UI waiting on the matching thread) and then lists
  three "Additional suggestions" (multiprocessing, matching only
  expanded categories, applying rewrites in a separate thread).
- **What the skill said** (`results.json`): `"Body lists 5 distinct
  sub-items (2 root causes + 3 architectural additions including
  multiprocessing and threading), forming an umbrella issue requiring
  core internals changes"`.
- **What gold says** (`gold-labels.json`): `"maintainer-diagnosed
  performance bug with named causes, unclaimed"`.
- **Why they disagree.** The grader treated a numbered list as a
  tracking list of sub-items. It is one bug (the UI freeze) with a
  maintainer's diagnosis attached; the "additional suggestions" are
  optional, not acceptance criteria. The evidence string also merges
  two different fail clauses ("umbrella" and "core internals
  changes"), and no maintainer actually says a redesign is needed. So
  the grader misread the clause, and the check made that easy because
  it bundles five fail clauses into one pass/fail.
- **Contrast.** `issue-04` (zxcalc/zxlive#555, same repo, same
  maintainer, same capture date) is a short maintainer-filed bug and
  passed `newcomer-scope`. The difference between the two is the shape
  of the body, not the scope of the fix. That is the signal my rubric
  overweights.

### Live candidates (Path Review)

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

`skill/rubric.md` is the single source of truth for check wording.
Everything quoted below is copied verbatim from it; if the rubric
changes, this section must be re-quoted, not paraphrased.

### `newcomer-scope` (the check that cost `issue-19`)

The rubric's pass condition, quoted:

> The ask is one bounded change a newcomer could finish without
> inventing product policy. Fail when any of these hold: the issue is
> an umbrella / megaissue / tracking list of sub-items; the thread
> shows unsettled design debate with no maintainer decision; the body
> is a one-line feature wish with no concrete behavior or acceptance
> criteria; a maintainer says the fix needs core internals redesign; or
> it is a usage/support question, not a contribution.

**What went wrong.** This one row covers five separate failure modes
(umbrella, unsettled debate, one-line wish, core redesign, support
question), and the grade is a single `pass`/`fail`. When `issue-19`
came back `failed: newcomer-scope` I had to open the bundle and the
`results.json` evidence to work out which clause fired. It turned out
to be two clauses at once ("umbrella" plus "core internals changes"),
and neither really fits: the list is a diagnosis of one bug, and no
maintainer says a redesign is needed.

**Revision reasoning.** The fix is to make the failing clause name
itself:

1. Split the row into narrower required checks: `not-umbrella`,
   `no-open-design-debate`, `concrete-ask`, `no-core-redesign`, and
   `is-contribution`. A failure then names the exact clause.
2. Or keep one row and require the evidence to start with the clause
   that fired (for example `umbrella: ...`) and quote the line that
   triggered it.
3. Tighten the umbrella clause: "a single reported bug whose body lists
   candidate causes or optional suggestions is not an umbrella; an
   umbrella is a list of independent deliverables".

I did **not** change the committed rubric this round. The run already
met the bar at 19/20 with every category floor held, and any edit
changes the `rubric.md` fingerprint in `eval-run.txt`. That would need
an `--only issue-19` revise loop and then a new full confirming run.
Option 1 plus the clarification in option 3 is the next revision. The
trade-off is under Trade-offs below.

### `ai-policy-ok` (the check that decided all three live candidates)

> The policy does not ban AI-generated code or documentation outright.
> Silence passes. Disclosure, "understand your changes", testing, or
> human-review requirements pass (those are conditions to follow, not
> bans). Only an explicit refusal such as "we do not accept
> AI-generated code/documentation" fails.

Path Review's `docs/CONTRIBUTING.md` only sets workflow conditions and
has no explicit refusal, so the "conditions to follow, not bans" clause
gives `pass` for #53, #72, and #68.

### `unclaimed` (with the Path Review house rule)

> All of: (1) assignees is `none`; (2) no linked PR is in state `open`;
> (3) no claim comment in the last 90 days (relative to capture date /
> today) that a maintainer (Owner/Member/Collaborator) has not clearly
> released.

`skill/scope.md` adds the house rule: "other students' claim comments
... do not block an issue". So the classmate claim on #68 does not
block it, and (1) and (2) still apply as written.

### Remaining checks

`repo-alive`, `issue-open`, and `has-good-first-label` passed for all
three candidates. Their evidence strings are in the Verdict output
below, so they are not restated here. Nothing was rejected, so ranking
used the fit profile only.

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

**Why it fits.** I selected
[#53](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53)
because it is the best match for my `scope.md` fit profile. It is
Python work in `safety/`, one of the areas I listed. It is one bounded
regex fix in `safety/pii_scrubber.py`, and failing tests define when it
is done. That is the "bounded OSS fixes with tests and clear acceptance
criteria" I said I want practice with. It has no UI styling and no
product policy for me to invent.

**What the verdict got right, and what it missed.** The `accept` holds
up. Every required check has evidence I can check against the issue:
the repo is open and not archived, the repo is active, nobody is
assigned, the issue is a collaborator-filed bug with a repro snippet,
and CONTRIBUTING has no AI ban. Ranking it above #72 and #68 is the
right use of the fit profile. Two pieces of evidence were thinner than
they look:

- The `unclaimed` evidence ("0 comments; no linked or cross-referenced
  PRs") was only true at the moment of the run. On re-checking
  (2026-11-13), #53 has 29 comments, all from non-maintainer
  classmates, and about 10 open PRs titled "fix(safety): redact
  parenthesized ... US phone numbers". Under the house rule this still
  passes, but the verdict did not show how crowded the issue is.
- The issue names four failing tests. A classmate's repro on the
  thread shows a fifth `xfail(strict=True)` test tied to #53
  (`test_mixed_pii_and_text`), so "done" means five tests, not four.

**Expected difficulty in claiming it.** Making the claim is easy:
nobody is assigned, no maintainer has commented, and the house rule
says classmate claims do not block, with course credit going to the PR
I open. The hard part is everything after the claim:

- *Overlap.* About 10 classmates have open PRs for the same fix. Before
  I write code I will read those PRs so my PR is not a duplicate. My
  plan is to cover all five `xfail` tests and handle the
  space-separated `555 123 4567` form explicitly, with tests that
  include `+1` and extension edge cases.
- *Reviewer attention.* With that many PRs, a maintainer will review
  slowly or choose one. My claim comment will include a reproduction
  report (exact commands and output). The PR will link #53, explain the
  regex change, and remove the `xfail` markers so CI shows the tests
  passing.
- *Fallback.* If a fix for #53 merges before mine, I will move to
  [#72](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72)
  (`api/`, est. 1–2h, zero comments). It passes every required check
  and is the least contested of the three.

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
