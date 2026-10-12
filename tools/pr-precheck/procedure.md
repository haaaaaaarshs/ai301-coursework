# Procedure: how this tool grades a PR package

## Read order

Read in this order, taking the notes listed for each step, before
grading any check. The plan is read before the diff on purpose. Every
plan-fidelity and test check is a side-by-side read, so the boundary
and the expected-after must already be written down when the diff and
the evidence are opened. Otherwise the diff starts to define what
"the plan" meant. The description is read last, so that its claims are
checked against what you have already seen rather than framing it.

0. **Preflight (live mode only).** Run the scope seam in `SKILL.md`:
   read `scope.md`, then stop or refuse as it directs. Then confirm
   that each live input from `SKILL.md`'s input table exists, and
   record any that are missing. In eval mode, skip this step, and do
   not open `scope.md` or `voice-guide.md`.
1. **Repo facts.** Eval: the repo-facts block. Live: the PR template,
   `docs/CONTRIBUTING.md`, and the `scope.md` house rules. Write down
   every stated ask as a numbered list: template sections, checklist
   items, required commands or checks, changelog, commit or branch
   convention, issue reference. Also write down the AI policy, quoted,
   or "no stated AI policy". In live mode only, add the two Unit 4
   course instructions from `SKILL.md`'s scope seam to the list: the
   AI-use disclosure under `Notes for Reviewers`, and the CI box left
   unticked. Never add them in eval mode.
2. **Issue and thread.** Note the reported behavior and the expected
   behavior. List every maintainer or member comment that prescribes or
   rules out an approach, scope, or constraint, quoted with its author.
   Write "none" if there is none.
3. **Plan context.** Eval: the plan-context block. Live: `plan.md`, or
   the house plan and house repro pack. Write down:
   - the repro's failing command or action and its observed output;
   - **Plan steps**: a numbered list of each change the plan says it
     will make;
   - **Boundary**: the files the plan names, and the not-in-scope list;
   - **Test plan**: a numbered list of each planned test, with its
     expected-after;
   - **Deviation notes**: each one, quoted, or "none".
4. **Diff.** Eval: the candidate PR's diff. Live: `git diff
   main...HEAD`. List every changed file. Under each file, list its
   hunks by header, with one line each saying what the hunk does.
5. **Commits.** Eval: the commit list. Live: `git log --oneline
   main..HEAD`. Copy each message. Live: also record the branch name
   from `git branch --show-current`.
6. **Test evidence.** Eval: the test-evidence section. Live:
   `test_evidence.md`. List each command, test, or action shown, with
   its result as given (printed output, a before/after annotation, or
   a stated summary), noting which form it takes, and note whether a
   before result is shown.
7. **Title and description.** Eval: the candidate PR's title and
   description. Live: `pr_draft.md`. Split the text into a numbered
   list of substantive claims, one claim per line. A substantive claim
   is any statement about what changed, which files, fidelity to the
   plan, behavior, tests, the checklist, or deviations. Pure framing
   sentences are not claims.

## Evidence gathering

**Eval control cross-references.** In eval mode, resolve a named
control against its definition elsewhere in the same bundle.
An explicit result such as "control unchanged" accounts for that
control when its input/action and baseline behavior are unambiguous
in the supplied repro or plan. Cite both the result statement and
the earlier definition. Do not require the same details to be
repeated. If the control is ambiguous, has no recorded outcome, or
contradicts other evidence, this rule does not establish a pass.
A passing control never substitutes for rerunning the failing
scenario. Live-mode captured-output requirements remain unchanged.


Build these pairings from the notes. Each one feeds the checks named
in brackets. Record every pairing result as a short line you can quote
later.

1. **Diff vs boundary** [`diff_within_plan`]. For each changed file
   and hunk from step 4, mark it **in plan**, **plan-required
   support** (a test, fixture, changelog, or doc that the test plan or
   a repo ask calls for), **covered by deviation note N**, or
   **outside**. Also flag any hunk that touches something on the
   not-in-scope list.
2. **Plan steps vs diff** [`planned_work_delivered`]. For each plan
   step from step 3, record the file and hunk that delivers it. If none
   does, record whether a deviation note or the description discloses
   that the step is absent, quoting it.
3. **Claims vs diff and evidence** [`description_matches_diff`]. For
   each claim from step 7, record **supported by** (a hunk or evidence
   line), **contradicted by** (a hunk or evidence line), or **not
   checkable** (nothing in the package bears on it). Check claims
   about code, tests, and template content against the actual hunks.
   For a claim about a run, an evidence line supports it only if
   pairing 5 would label it proof under the current mode; a ticked box
   alone never does. A named generator command recorded as producing
   no changes supports a "no changes" claim without a hunk. Also list
   any behavior change in the diff that the description never
   mentions.
4. **Repro vs re-run** [`repro_rerun_observable`]. Pair the repro's
   failing command and observed output (step 3) with the matching
   entry in the test evidence (step 6). First confirm the entry is an
   identifiable re-run of the reported failing scenario: its command
   or action is named, or it explicitly refers to the supplied repro.
   A passing control or an unrelated test, offered alone, does not
   pair with the repro. Classify the after result as exactly one of:
   - **captured**: an automated command with its actual printed
     result;
   - **recorded**: a named command or test, or an identifiable re-run,
     with a concrete, specific result given as a before/after
     annotation (including an inline comment) or a concise summary;
   - **observed**: a genuinely manual action with a concrete observed
     result;
   - **claim only**: a ticked box alone, or a generic "works now",
     "matches expected", or "suite passes" with no named command or
     test and no concrete result;
   - **disclosed not run / failed**;
   - **absent**.
   Quote the result. For captured, recorded, and observed results,
   record whether the result is a concrete behavioral outcome that
   matches the plan's expected-after.
5. **Test plan and repo checks vs evidence** [`planned_tests_accounted`].
   Make one list from each test-plan item (step 3) and each
   repo-required check (step 1). Classify each entry with the same
   six labels as pairing 4, and quote the deciding line. A named
   pytest run with its pass count, or a named integration test
   reported passing, is **recorded**; counts help but are not
   required. A test file added in the diff counts as proof only if the
   evidence shows it ran, under the mode's standard. An ask for
   formatted code is met by the code itself; do not require a
   formatter log the repo does not ask for. For every entry labeled disclosed
   not run / failed, check that the description and evidence never call
   it passing. Then check every claim in the evidence about added
   tests, fixtures, or snapshots against the files actually in the
   diff. Live mode, by course instruction: CI runs only after the PR is
   opened, so the "CI is green on this PR" item is recorded as
   "post-open by course instruction, not a test outcome". The local
   commands the template and `docs/CONTRIBUTING.md` name are the
   pre-submit items (in this repo: `make test-unit`,
   `make test-integration`, `make lint`, `make typecheck`, and
   `make check`).
6. **Hunks vs purpose** [`change_focused`]. For each hunk marked
   **outside** in pairing 1, and for any hunk inside a planned file
   that does not serve a plan step, label what it is: refactor,
   rename, reformat, reorder, or drive-by.
7. **Added lines scan** [`no_debris`]. Scan only the `+` lines and the
   new files for the debris list in the rubric. Record each hit as
   file plus quoted line.
8. **Asks vs PR** [`repo_asks_honored`]. For each ask from step 1,
   record where the PR meets it (a description section, a commit, the
   branch name, or a diff file) or that it does not. For each ticked
   checklist item, confirm it with pairing 3 or 5: a box ticked for a
   command counts only if pairing 5 labeled that command as proof
   under the current mode (step 5 of check execution). A ticked box is
   never its own evidence.
   Live mode: record whether the CI box is ticked. By course
   instruction it must be unticked.
9. **Policy vs PR** [`ai_policy_honored`]. Eval: if step 1 recorded an
   AI policy, quote the PR's disclosure line, or record its absence;
   otherwise record "no stated AI policy". Live: quote the AI-use
   disclosure from the draft's `Notes for Reviewers` section, or
   record that it is absent or placed elsewhere. Also apply any repo AI
   policy that step 1 recorded.
10. **Direction vs PR** [`thread_direction_engaged`]. For each
    maintainer direction from step 2, record the hunk or description
    line that follows it or engages it, or record that it is
    contradicted with no mention.
11. **Commit messages** [`commits_describe_change`]. For each commit
    from step 5, record whether its message names what it changes.

## Check execution

1. Grade the checks in the rubric's table order, one at a time. Grade
   each one from its own pairing lines alone. Re-open the package only
   to quote the deciding line, never to reinterpret a different
   check's evidence.
2. Apply the pass condition literally to the pairing lines:
   - `pass`: every pairing line satisfies the pass condition, or the
     check is conditional and its trigger is recorded as absent (no
     stated ask, no stated policy, no maintainer direction).
   - `fail`: at least one pairing line contradicts the pass condition.
     A required item that is absent where the package should contain it
     is a fail, not unclear: for example, a test-plan item with no
     outcome and no disclosure, or a template section left blank.
   - `unclear`: the evidence needed to decide is genuinely not in the
     package. Examples are a truncated diff, a missing live input, or a
     description claim the package cannot check. Name what is missing.
3. Grade every check independently. If one mismatch fails several
   checks, each one fails. An out-of-plan refactor, for example, can
   fail `diff_within_plan`, `change_focused`, and
   `description_matches_diff` (when the description claims "no other
   changes"). Do not suppress one because another already failed.
4. A deviation note or an honest disclosure counts only for the items
   it names, and only toward the checks that accept disclosure:
   `diff_within_plan`, `planned_work_delivered`, and accounting in
   `planned_tests_accounted`. It can make plan fidelity pass while
   other required checks still fail. A noted unrelated change still
   fails `change_focused`, and noted debris still fails `no_debris`. A
   disclosed missing or failed test still leaves `repro_rerun_observable`
   failing, because disclosure is not proof that the bug is fixed.
5. For test checks, what counts as proof depends on the mode:
   - Eval: **captured**, **recorded**, and **observed** results all
     count as proof. Judge the specificity and relevance of the
     result, not whether it is prose, an inline comment, or a terminal
     transcript.
   - Live: for an automated command, only **captured** counts, per the
     Unit 4 requirement; **recorded** is a claim. For a genuinely
     manual action, **observed** counts.
   Results labeled **claim only** are never proof in either mode. A
   command that could be run automatically does not become "manual"
   because its output was left out. For `repro_rerun_observable`, the
   proof must also be the re-run of the reported failing scenario with
   a concrete outcome matching the expected-after.
6. Never fill a gap with general knowledge of the project or with a
   claim the PR makes about itself. A description claim is evidence
   that the claim was made, not evidence that it is true.
7. Write each check's evidence as one line that quotes or cites the
   deciding fact, such as a file and hunk, an output line, or a claim
   and the hunk that contradicts it.
8. Live mode, after all checks are graded: run the voice seam from
   `SKILL.md` against the title and description, and record any broken
   rules for the summary. This step does not touch any grade.

## Verdict assembly

1. Apply the verdict rule in `rubric.md`. Accept only if every
   `required` check is `pass`. Reject if any `required` check is
   `fail` or `unclear`, since `unclear` counts as `fail`. `preferred`
   grades never change the verdict.
2. The deciding check is the first `required` check in rubric table
   order that graded `fail` or `unclear`. In the summary, quote its
   evidence line as the reason for the hold, then list any other
   failing required checks by name. On an accept, there is no deciding
   check. State that every required check passed, and list any
   reservations from preferred checks or evidence lines.
3. Write the summary in this order: one line per check (name, grade,
   evidence), the verdict with the deciding check, then (live only)
   missing inputs, procedure gaps, and voice-guide notes.
4. End with the JSON block from `SKILL.md`. Give one entry per check in
   rubric order, using the same grade and evidence line as the summary.
   Nothing may follow the block.
