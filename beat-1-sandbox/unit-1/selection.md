# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**https://github.com/codepath/pathreview-ai301-fa26-s3/issues/35**

**Verdict output**

Graded all three live against rubric.md. Scope check: all three are in codepath/pathreview-ai301-fa26-s3, the repo named in scope.md — in field.

Shared repo evidence (measured against today, 2026-09-22):
- Last default-branch commit 2026-09-16 by human Aburke225 (6 days ago); repo not archived; last push 2026-09-16; created 2026-09-10; no releases.
- docs/CONTRIBUTING.md + .github/PULL_REQUEST_TEMPLATE.md contain no AI ban — only workflow conditions (green CI, remove xfail, conventional commits).
- The repo has zero pull requests (0 open / 0 closed), so no candidate can have a linked PR, open or abandoned.
- No issue in the repo has any comments, so there is no maintainer first-response sample at all.

Accepted, in fit order

1. #3 — Add a content hash to detect unchanged documents and skip re-embedding (best fit)
Backend data-handling in Python (ingestion/pipeline.py, core/models/ingested_source.py), tier-2, 4–6h — the closest match to your stated preference for data handling and server-side application logic, and the only candidate below tier-3.

2. #35 — Implement a webhook system that notifies users when their review is ready
Squarely API/server-side (api/routes/webhooks.py, core/services/webhook_service.py), which fits your API interest, but tier-3 at 8–12h and it builds a new subsystem rather than changing one.

3. #51 — Add a database migration validation step to CI
Passes every required check, but it is CI/devops infrastructure (.github/workflows/ci.yml, scripts/validate_migrations.sh) rather than application logic — the least aligned with your "prefer backend application work, avoid unfamiliar infrastructure" profile.

Per-check read-out (identical grades on the required checks for all three): maintainer_alive pass, repo_in_use pass, bounded_newcomer_scope pass, contribution_available pass, ai_policy_allows_contribution pass. Preferred: good_first_issue_signal passes for #3 and #51 (no beginner label, but a concrete bounded deliverable with named files) and fails for #35 (the body describes a feature, leaving retry, auth, and persistence semantics unspecified); maintainer_responsiveness is unclear for all three.

One tension worth flagging, since the rubric decides and I don't: #35 and #51 are labeled tier-3 with 8–12h and 5–7h estimates, which is not first-issue sizing — but your rubric's bounded_newcomer_scope passes by default and lists no effort or difficulty threshold among its fail conditions, so they pass. If you want tier and effort to bite, that belongs in the rubric row, not in this run. Also note the repo does have a good first issue label in use (e.g. #53, tier-1) — none of your three candidates carries it.
```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/3",
    "checks": [
      {"name": "maintainer_alive", "grade": "pass", "evidence": "Human-authored default-branch commit 2026-09-16 by Aburke225, 6 days before today (< 180 days)."},
      {"name": "repo_in_use", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16 (< 365 days)."},
      {"name": "bounded_newcomer_scope", "grade": "pass", "evidence": "Single bounded change naming ingestion/pipeline.py and core/models/ingested_source.py; not a support question, umbrella, or design debate; repo has zero PRs so no closed unmerged linked PR."},
      {"name": "contribution_available", "grade": "pass", "evidence": "'Assignees: No one assigned'; 'Development Section: No linked pull requests'; no comments, so no maintainer-confirmed claim."},
      {"name": "ai_policy_allows_contribution", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no AI restriction; silence passes."},
      {"name": "good_first_issue_signal", "grade": "pass", "evidence": "No beginner label (labels: devops, docs, enhancement, ingestion, tier-2), but the body describes one bounded change with two named files."},
      {"name": "maintainer_responsiveness", "grade": "unclear", "evidence": "No issue in the repo has any comments, so no maintainer first-response sample exists."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/35",
    "checks": [
      {"name": "maintainer_alive", "grade": "pass", "evidence": "Human-authored default-branch commit 2026-09-16 by Aburke225, 6 days before today (< 180 days)."},
      {"name": "repo_in_use", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16 (< 365 days)."},
      {"name": "bounded_newcomer_scope", "grade": "pass", "evidence": "Names two new files with a stated deliverable; no umbrella list, no unresolved design thread (no comments), no maintainer core-internals claim; repo has zero PRs."},
      {"name": "contribution_available", "grade": "pass", "evidence": "'Assignees: No one assigned'; 'Development Section: No linked pull requests'; no comments, so no maintainer-confirmed claim."},
      {"name": "ai_policy_allows_contribution", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no AI restriction; silence passes."},
      {"name": "good_first_issue_signal", "grade": "fail", "evidence": "No beginner label (api, enhancement, tier-3) and the body describes a new subsystem at 8-12h with retry/auth/persistence semantics unspecified, not a clearly bounded change."},
      {"name": "maintainer_responsiveness", "grade": "unclear", "evidence": "No issue in the repo has any comments, so no maintainer first-response sample exists."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/51",
    "checks": [
      {"name": "maintainer_alive", "grade": "pass", "evidence": "Human-authored default-branch commit 2026-09-16 by Aburke225, 6 days before today (< 180 days)."},
      {"name": "repo_in_use", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16 (< 365 days)."},
      {"name": "bounded_newcomer_scope", "grade": "pass", "evidence": "One concrete deliverable (fresh DB, run migrations, diff schema against models) in .github/workflows/ci.yml and scripts/validate_migrations.sh; no umbrella, design debate, or core-internals claim; repo has zero PRs."},
      {"name": "contribution_available", "grade": "pass", "evidence": "'Assignees: No one assigned'; 'Development Section: No linked pull requests'; no comments, so no maintainer-confirmed claim."},
      {"name": "ai_policy_allows_contribution", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no AI restriction; silence passes."},
      {"name": "good_first_issue_signal", "grade": "pass", "evidence": "No beginner label (devops, enhancement, tests, tier-3), but the body describes a bounded change with two named files and a concrete pass criterion."},
      {"name": "maintainer_responsiveness", "grade": "unclear", "evidence": "No issue in the repo has any comments, so no maintainer first-response sample exists."}
    ],
    "verdict": "accept"
  }
]
```
## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

I completed these scored evaluation runs in order:

1. `agreement: 17/20 scored items (bar: 18/20: below the bar)`
2. `agreement: 1/3 scored items`
3. `agreement: 2/3 scored items`
4. `agreement: 3/3 scored items`
5. `agreement: 18/20 scored items (bar: 18/20: PASS)`

The last line matches the agreement line in the submitted `eval-run.txt`. The earlier setup attempts that produced errors are not included because they produced no scored agreement result.

**Issue analysis**

I analyzed issue-15. My rubric decided `reject`, and the gold label was also `reject`.

The issue facts showed: “linked PRs: zulip/zulip#20840 (closed); zulip/zulip#23123 (closed).” The thread also included the comment, “I have worked on this issue and I have made a PR. Review is pending.” These closed pull requests and repeated prior attempts indicated that the issue had a history of abandoned work. My rubric therefore rejected it under the bounded newcomer scope check, even though the repository itself was active and the issue had no current assignee.


**Check rationale**
My `bounded_newcomer_scope` check currently says:

“Pass by default. Fail if the issue is explicitly a pure usage/support question, umbrella/tracking issue, unresolved design discussion, or maintainer-confirmed core-internals work. Also fail if the issue has at least one closed unmerged linked pull request, or the thread documents repeated abandoned claims or prior contributors saying they made an unfinished pull request. Missing details alone do not fail: short wording, vague wording, missing reproduction steps, missing acceptance criteria, and missing beginner labels still pass. Do not return `unclear` merely because information is absent.”

I wrote this check to avoid rejecting issues simply because their descriptions are short or incomplete. At the same time, it looks for concrete evidence that an issue is unusually difficult, such as closed unmerged pull requests or repeated abandoned attempts.

**Trade-offs**
This check gives up rejecting issues solely because they have a larger estimated scope or lack a beginner label. For example, Issue #35 is estimated at 8–12 hours and does not have a beginner label, but the issue still has a concrete deliverable and named files, so the rubric accepts it.

I tested this trade-off by rerunning issues 01, 15, and 19 with `--only`. The targeted command printed the exact result: “agreement: 3/3 scored items.”

---

## Selection rationale

### 1. Fit to my interests and available time

Issue #35 fits my interest in backend application development because it involves creating a webhook system, API routes, and a backend service. My previous experience with ScanCode and Dagster gives me experience with larger software projects, so I am comfortable taking on a more substantial issue. Although the estimate is 8–12 hours, Beat 1 covers Units 1–4, giving me multiple weeks to work on it alongside school and work.

### 2. What the verdict identified and what I weighed separately

The verdict correctly identified that the repository is active, the issue is available, the contribution policy allows AI-assisted work, and the issue has a concrete backend deliverable. It also correctly noted that the issue does not have a beginner label and that retry, authentication, and persistence behavior are not fully specified.

The rubric ranked Issue #3 above Issue #35 because it is smaller and more clearly bounded. However, I weighed my previous development experience and my desire to grow through a more challenging backend task. Those personal learning goals were not part of the binary verdict, so I chose Issue #35.

### 3. Anticipated difficulty in claiming and completing it

I do not expect difficulty claiming the issue because it has no assignee or linked pull request. The main difficulty will be understanding the existing API and service structure and deciding how webhook authentication, retries, and persistence should work. I plan to use the early units to set up the repository and reproduce or understand the existing workflow before implementing the feature.


Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
