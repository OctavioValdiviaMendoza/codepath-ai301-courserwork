# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->
## Checks
| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer_alive | The "last 5 default-branch commits" and "maintainer first-response sample" under Repo facts; maintainer badges in the Comments section | Pass if at least one of these is true: a human-authored default-branch commit occurred within the last 180 days, or a user with Owner, Member, or Collaborator association responded to an issue within 90 days. | required |
| repo_in_use | "archived:", "last push to any branch", and "latest release" under Repo facts | Pass if the repository is not archived and there was a push to any branch within the last 365 days. | required |
| bounded_newcomer_scope | The issue body and comment thread, plus "linked PRs:" under Repo facts; use the scope guidance in `references/evidence-guide.md` | Pass by default. Fail if the issue is explicitly a pure usage/support question, umbrella/tracking issue, unresolved design discussion, or maintainer-confirmed core-internals work. Also fail if the issue has at least one closed unmerged linked pull request, or the thread documents repeated abandoned claims or prior contributors saying they made an unfinished pull request. Missing details alone do not fail: short wording, vague wording, missing reproduction steps, missing acceptance criteria, and missing beginner labels still pass. Do not return `unclear` merely because information is absent. | required | contribution_available | "this issue: assignees:" and "linked PRs:" under Repo facts; the Comments section | Pass if there is no assignee, no open linked pull request, and no maintainer-confirmed active claim. A student's unconfirmed claim comment alone does not fail this check in the course's Path Review repository. | required |
| ai_policy_allows_contribution | The "contribution policy" line under Repo facts, including any quoted AI policy, `AI_POLICY.md`, `AI_USAGE_POLICY.md`, `AGENTS.md`, and templates | Pass unless the repository explicitly bans AI-generated or AI-assisted contributions. Disclosure, testing, understanding the changes, or human-review requirements count as conditions to follow, not as bans. Silence also passes. | required |
| good_first_issue_signal | The issue body, labels, and comment thread | Pass if the issue has a maintainer-applied `good first issue`, `help wanted`, or equivalent beginner label, or if the issue text clearly describes a bounded change. This check never changes the verdict and only helps rank accepted issues. | preferred |
| maintainer_responsiveness | The "maintainer first-response sample" under Repo facts and maintainer comments in the issue thread | Pass if at least one maintainer response in the sample or thread occurred within 30 days. | preferred |

## Verdict rule

Accept if every required check passes. Preferred checks never change the verdict and are used only to rank accepted issues. An `unclear` result fails a required check unless the check's pass condition explicitly says to pass when disqualifying evidence is absent. Reject if any required check fails.
