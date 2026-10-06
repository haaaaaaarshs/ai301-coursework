# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

1. Read the Repro evidence first. Record:
   - the behavior that reproduces the bug,
   - the expected behavior,
   - the observed behavior,
   - any comparison cases or experiments that isolate the cause,
   - and any evidence that rules out a possible cause.

2. Read the Issue and Thread highlights next. Record:
   - the original problem being reported,
   - maintainer guidance,
   - proposed causes or fixes discussed in the thread,
   - and any constraints or unresolved disagreement.
   Treat thread claims as hypotheses unless the Repro evidence supports them.

3. Read the Repo facts. Record any contribution rules, repository conventions, or technical constraints relevant to the proposed change.

4. Read the Candidate plan. Record:
   - its stated diagnosis,
   - scope and out-of-scope items,
   - files or components it intends to change,
   - implementation approach,
   - test plan,
   - and stated risks or unknowns.

5. Read the Candidate plan comment last. Compare its claims with the full plan and with the issue thread.

Do not grade any check until all of the above evidence has been gathered.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->
1. For diagnosis evidence, compare the plan's stated cause directly with the Repro evidence. Identify which reproduction result supports the cause and whether any result contradicts it.

2. For fix-target evidence, connect each proposed implementation change to the supported diagnosis. Record whether the change addresses the demonstrated cause or only a symptom or unrelated theory.

3. For scope evidence, list the files, components, and behaviors the plan says it will change and anything it explicitly excludes. Compare that scope with what is necessary to address the reproduced problem.

4. For executability evidence, record whether the plan identifies a concrete implementation target and enough technical direction for another contributor to begin. Record unresolved assumptions separately.

5. For test evidence, map the proposed test back to the original reproduction. Record the observable result that would prove the bug is gone and whether the test would fail or differ before the fix.

6. For thread and repository evidence, compare the plan and plan comment with Thread highlights and Repo facts. Record:
   - maintainer direction,
   - repository conventions,
   - issue constraints,
   - and any affirmative requirements such as required disclosures, tests, or submission rules.
   Verify that mandatory requirements are actually satisfied in the plan or comment; absence of a required item is evidence of failure.


## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->
1. Grade `diagnosis_supported` first because the remaining implementation checks depend on whether the cause is supported.

2. Grade `diagnosis_supported` as pass when the reproduction supports or reasonably narrows toward the proposed cause and does not materially contradict or rule it out. Do not require the reproduction to prove every internal implementation detail. If the exact mechanism remains uncertain, verify that the plan identifies that uncertainty rather than stating it as established fact.

3. Grade `fix_targets_cause` next. If the diagnosis failed, independently determine whether the proposed fix is nevertheless supported by the reproduction; do not assume it passes because it matches the plan's own diagnosis.

4. Grade `scope_bounded`, `executable_plan`, `test_proves_fix`, and `thread_and_repo_fit` using only the evidence gathered for each check.

5. Use:
   - `pass` when the pass condition is supported,
   - `fail` when the evidence contradicts the pass condition,
   - `unclear` when necessary evidence is genuinely absent or insufficient.

6. Do not replace missing evidence with assumptions from general knowledge or with unsupported claims from the issue thread.

7. Once the necessary evidence for a check has been recorded, grade from those notes without reinterpreting unrelated parts of the package.


## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

1. Apply the verdict rule in rubric.md after all checks have been graded.

2. Accept only when every required check passes.

3. Treat `unclear` as a failure for required checks.

4. Reject when one or more required checks fail or are unclear.

5. In the explanation for each check, cite the specific evidence that determined the grade.

6. For a rejected package, quote or identify the evidence behind the first decisive required failure, especially any reproduction result that contradicts the plan.

7. Return the final verdict consistently from the completed check grades without overriding it based on whether the plan sounds plausible.
