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
| maintainer-active | Use the maintainer-life signals in `references/evidence-guide.md`: inspect the last 5 default-branch commits and the maintainer first-response sample under Repo facts, plus Owner/Member/Collaborator activity in the issue thread. | Pass when there is evidence of human maintainer activity within 90 days of the capture date: at least one non-bot maintainer commit or a bot merge of a human PR, or at least one Owner/Member/Collaborator response in the recent response sample or current issue within 90 days. Bot-only dependency/update activity does not pass by itself. | required |
| repo-active | Use the repo-use signals in `references/evidence-guide.md`: archived status, last push to any branch, latest release, and recent commit history under Repo facts. | Pass when the repository is not archived and has either a push to any branch or a release within 12 months of the capture date. | required |

| newcomer-scope | Read the issue body and comment thread for the requested behavior, examples, maintainer clarification, tracking/umbrella language, unresolved product or design decisions, and abandoned implementation attempts. Also note whether a maintainer/collaborator authored the issue and explicitly marked it `good first issue`; this is supporting scope evidence, not sufficient by itself. | Pass when the issue identifies one concrete bug, behavior, or outcome that can be investigated as a coherent contribution. A short maintainer-authored bug report may pass when it is explicitly marked `good first issue` and gives concrete examples of the affected behavior, even if it does not enumerate every affected case. Multiple suspected causes or implementation suggestions may also pass when they address one concrete outcome. Fail when the issue is a codebase-wide or ongoing umbrella inviting arbitrary partial work, when the desired behavior requires an unresolved product/design decision, or when prolonged design debate or multiple abandoned attempts show that the scope is not settled. A `good first issue` label does not override any of these fail conditions. | required |

| issue-unclaimed | Use the availability signals in `references/evidence-guide.md`: this issue's assignees and linked PRs under Repo facts, plus claim comments and PR references in the issue thread. | Pass when there is no current assignee, no open PR implementing the issue, and no unresolved comment indicating that another contributor is currently working on it. Closed/unmerged PRs or clearly abandoned claims do not fail the check by themselves. When formal linkage and the thread disagree, use the thread as evidence. | required |
| ai-policy-compatible | Use the contribution-policy signals in `references/evidence-guide.md`: CONTRIBUTING.md, dedicated AI-policy files, templates, and the contribution policy line under Repo facts. | Pass when the repository either says nothing about AI use or allows assistive AI use under conditions that can be followed. Fail when the repository explicitly prohibits AI-generated or AI-assisted contributions in a way incompatible with this course's AI-assisted workflow. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Return `accept` only when every required check is `P`. A required check graded `F` or `?` makes the verdict `reject`. Preferred checks, if added later, may provide additional guidance but do not change the verdict.
