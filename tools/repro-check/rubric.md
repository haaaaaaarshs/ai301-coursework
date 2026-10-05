# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-match | Use the Environment section of `references/evidence-guide.md`: compare the repro report's environment record with the issue context and repo facts/docs. | Pass when the report names the relevant runtime, platform, dependency, or project versions needed to rerun the issue, and they match the issue's stated target; any material difference is explicitly identified. | required |
| followable-steps | Use the Steps section of `references/evidence-guide.md`: inspect the repro report's steps against the issue's stated setup and trigger. | Pass when the report gives enough ordered actions, including the necessary starting state and trigger, for a stranger using the recorded environment to attempt the same reproduction without guessing a missing material step. | required |
| behavior-match | Use the Behavior shown section of `references/evidence-guide.md`: compare the report's output/log/screenshot evidence with the behavior described by the issue. | Pass when the supplied artifact actually demonstrates the behavior the issue describes, or clearly demonstrates its absence in a cannot-reproduce report; evidence of only an adjacent or different failure does not pass. | required |
| outcome-supported | Use the Honesty section of `references/evidence-guide.md`: compare the report's stated result with its steps and artifacts. | Pass when the conclusion says only what the recorded run and evidence support. A supported reproduction and an evidenced cannot-reproduce both pass; unsupported certainty or claims beyond the evidence fail. | required |
| repo-conventions | Use the Comms section of `references/evidence-guide.md`: compare the claim and repro comments with the issue context, repo docs/templates, repo facts, and stated contribution policies. | Pass when the comments follow every applicable stated repository requirement, including required templates or AI-use disclosure, and the claim identifies the specific issue and promises only investigation/reproduction rather than a fix or deadline. | required |


## Verdict rule

Return `accept` only when every required check is `P`. A required check graded `F` or `?` makes the verdict `reject`. Preferred checks, if added later, may provide quality feedback but cannot change the verdict.

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
