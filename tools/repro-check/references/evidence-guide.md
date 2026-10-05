# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

**Where it lives:** In eval bundles, compare the issue context and repo-facts block with the environment portion of the repro report. In live mode, compare the issue thread and the repository's setup or contribution documentation with the environment recorded in the draft repro comment.

**What good looks like:** The report identifies the runtime, platform, dependency, or project versions that materially affect the issue. Those values match the issue's target environment, or any meaningful difference is explicitly called out so another contributor knows what was actually tested.

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->

## Steps

**Where it lives:** In eval bundles, read the reproduction steps in the repro report together with any prerequisite or starting-state information in the issue context. In live mode, compare the draft report's steps with the issue description and relevant setup instructions in the repository documentation.

**What good looks like:** The steps establish the necessary starting state and proceed in an order that reaches the action that triggers the reported behavior. A stranger using the recorded environment can attempt the reproduction without inventing a material missing action. A setup file or script does not need to be printed in full when the report pins down its material content (for example, the sections an input file contains, or "the issue's script, unchanged" with the parameters that matter). Only a missing action, input, or value that a stranger would have to guess counts as a gap.

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->

## Behavior shown

**Where it lives:** In eval bundles, compare the output excerpts, logs, test results, or screenshots in the repro report with the observed and expected behavior described in the issue context. In live mode, compare the artifacts quoted in the draft repro comment with the behavior described in the issue body and thread.

**What good looks like:** The artifact itself shows the specific behavior the issue reports (the same wrong output, error, or failing assertion, under the same trigger), not just a statement that it happened. A different or adjacent failure does not count. A cannot-reproduce report counts when its artifact shows the actual result of a real attempt at the issue's trigger (for example, the output or log where the issue says the failure appears) and the report names what differed from the issue's conditions. A bare statement that nothing happened does not count.

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->

## Honesty

**Where it lives:** Compare the repro report's stated outcome with its recorded environment, steps, and artifacts. Also check the claim comment for statements about work that had not yet happened when the claim was written.

**What good looks like:** The conclusion does not go beyond what the evidence demonstrates. A successful reproduction states what was observed without claiming an unproven cause, while a cannot-reproduce report clearly says that the behavior was not observed and records what happened instead.

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->

## Comms

**Where it lives:** In eval bundles, compare the claim and repro comments with the issue context, repo-facts block, and any stated contribution template, communication rule, or AI-use disclosure policy. In live mode, check the issue thread and repository contribution documentation before evaluating the draft comments.

**What good looks like:** The claim refers to the specific issue and states the contributor's next investigation or reproduction step without promising a fix or deadline. The comments satisfy applicable repository-specific requirements, including required formatting or AI-use disclosure, and describe the contributor's own work rather than piggybacking on another report.

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->
