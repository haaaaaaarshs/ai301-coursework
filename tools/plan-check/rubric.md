# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis_supported | The plan's stated diagnosis read against the Repro evidence, especially the observed behavior, comparison cases, and results. | The proposed cause must reasonably explain the reproduced behavior and must not be contradicted or ruled out by the reproduction. If the reproduction narrows the failure to a component or path but does not prove the exact internal mechanism, the diagnosis may still pass when that uncertainty is acknowledged rather than presented as established fact. | required |
| fix_targets_cause | The plan's proposed changes read against the diagnosis and Repro evidence. | The proposed change addresses the cause or failure path supported by the reproduction. When the exact internal mechanism is still being confirmed, the plan may pass if the proposed target follows from the evidence and the remaining uncertainty is explicitly identified and will be resolved during implementation. | required |
| scope_bounded | The plan's scope statement, files to be changed, proposed changes, and any explicit out-of-scope items. | The work is limited to a specific, justified change needed to fix the reproduced problem and does not introduce unrelated work or unexplained expansion. | required |
| executable_plan | The plan's implementation steps, files or components named, risks or unknowns, and relevant Repo facts or thread guidance. | A contributor could begin the investigation or implementation from the targets and approach given without having to invent the main direction. Exact functions or final edit sites do not need to be known in advance when the plan identifies the relevant component or path and explains how the remaining location will be confirmed. | required |
| test_proves_fix | The plan's test plan read against the original Repro evidence and expected behavior. | The test re-checks the behavior that demonstrated the bug and defines an observable post-fix result that would distinguish success from the original failure. | required |
| thread_and_repo_fit | The Candidate plan comment and plan read against Thread highlights, Repo facts, and repository contribution conventions. | The plan and comment follow all relevant maintainer guidance, issue constraints, and stated repository requirements. Mandatory requirements, such as required disclosures or submission conventions, must be explicitly satisfied rather than merely not contradicted. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept only if every required check passes. A preferred check, if any are added later, does not change the verdict. An unclear grade counts as a failure for a required check because the plan is not ready to build from when the necessary evidence cannot be confirmed. Reject if any required check fails or is unclear.
