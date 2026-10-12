# Rubric: is this pull request ready to submit?

Checks run in the order listed. Each belongs to one evidence family
in `references/evidence-guide.md`, and the families map one to one
onto the harness categories:

- Plan fidelity (silent drift): `diff_within_plan`,
  `planned_work_delivered`, `description_matches_diff`.
- Test evidence (not tested): `repro_rerun_observable`,
  `planned_tests_accounted`.
- Diff quality (unreviewable): `change_focused`, `no_debris`,
  `commits_describe_change`.
- Standards and comms (standards wall): `repo_asks_honored`,
  `ai_policy_honored`, `thread_direction_engaged`.

"Deviation note" means a recorded change to the plan with a reason.
It can live in the plan's deviation notes (live: `plan.md`; eval: the
plan-context block) or in the PR description. A deviation note re-ties
a mismatch between plan and diff. A mismatch with no note is drift.
An honest deviation can satisfy the plan-fidelity checks while other
required checks still fail. A note does not excuse an unrelated change
under `change_focused` or debris under `no_debris`, and it does not
establish that the bug is fixed. A disclosed missing or failed test is
honest, but it is not proof: it never makes `repro_rerun_observable`
pass. A missing test described as passing fails.

"Proof" means an identifiable command, test, or action together with
a concrete, relevant result. What form that result may take depends on
the mode:

- **Eval mode: a recorded result.** The frozen bundle may record a
  result as printed output, a concrete before/after annotation
  (including an inline comment), or a concise named-command or
  named-test summary, such as a named pytest run with its pass count or
  a named integration test reported passing. Judge the specificity and
  relevance of the result, not whether it is prose, an inline comment,
  or a terminal transcript. Counts help but are not always required.
- **Live mode: captured output.** By the Unit 4 requirement, an
  automated command is proven only by the command plus its actual
  printed output in `test_evidence.md`, and the repro re-run shows
  before/after evidence. A summary or inline comment in place of the
  output is a claim.

In both modes, a ticked checkbox alone, or a generic "works now",
"matches expected", or "suite passes" that names no command or test
and gives no concrete result, is a claim, not proof. A genuinely
manual test, such as a UI action or a visual check, has no printed
output. For one of those, a concrete observed result tied to the
action and the expected behavior counts in both modes.

## Checks

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


| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff_within_plan | Every changed file and hunk in the diff, read against the plan's stated files, scope, and not-in-scope list, plus any deviation notes. | Every changed file is one the plan names, or a test, fixture, changelog, or doc file the plan's test plan or the repo's asks call for. Every hunk carries out a planned step. Any change outside that boundary is covered by a deviation note that names it and gives a reason. Fail on any file or hunk that falls outside the plan with no note, including a change to something the plan lists as not in scope. | required |
| planned_work_delivered | Each step in the plan's plan or scope section, read against the diff, plus any deviation notes and the description. | Every planned step appears in the diff, or its absence is disclosed with a reason in a deviation note or the description. An honestly disclosed shortfall passes. Fail when a planned step is missing from the diff and nothing in the package says so. | required |
| description_matches_diff | Each substantive claim in the title and description, including what changed, which files, fidelity claims such as "implements the plan exactly" or "no other changes", "tests added", "changelog added", and "behavior unchanged", read against the diff and the test evidence. | Every substantive claim is borne out by the diff or the test evidence, and the description does not omit a behavior change that the diff makes. Claims about code, tests, and template content are checked against the actual diff. Claims about runs are checked against the test evidence under the mode's proof standard (see "Proof" above): in eval mode, a named command or test with an explicit recorded result supports a claim whatever its form, and a named generator command reported to produce no changes supports a "regenerated, no changes" claim without any diff. A ticked checkbox alone does not support a test claim. Fail on any claim the diff or the evidence contradicts. If a claim cannot be checked against anything in the package, grade the check `unclear`. | required |
| repro_rerun_observable | The test-evidence section, read against the reproduction's steps in the plan context and the plan's test plan's expected-after. | The evidence contains an identifiable re-run of the reported failing scenario, and a concrete observed outcome that matches the plan's expected-after and is distinguishable from the original failure. The command or action must be identifiable from the re-run itself and its explicit reference to the supplied repro. Eval: the outcome may be printed output, a concrete before/after annotation, or a specific prose statement of the observed behavior. Live: an automated re-run needs the command plus its captured printed output, with before/after evidence. Genuinely manual (both modes): the action plus a concrete observed result. Fail on: a generic "works now", "tests pass", "colors work", or "matches expected" with no concrete behavioral result; a ticked box alone; a result that does not exercise the reported failing scenario, including a passing control or unrelated test offered alone; a result that contradicts the expected-after. Also fail when the re-run is disclosed as not run or failing: disclosure is honest, but it is not proof. | required |
| planned_tests_accounted | Every item in the plan's test plan, plus every check the repo requires before submission (from the template checklist, the contributing guide, or a suite the plan names), read against the test-evidence section and the test files in the diff. | Every planned test and every required check has one of two things. The first is proof under the mode's standard (see "Proof" above). Eval: a named command or test with its explicit recorded result, which may be a concise summary such as a named pytest run with pass counts or a named integration test reported passing; counts help but are not required. Live: the command plus its captured printed output. Genuinely manual (both modes): a concrete observed result. A test or fixture added in the diff counts only with such proof that it ran. The second is an honest disclosure that the item was not run, or that it failed, with a reason. A disclosed item is accounted for, but it is never counted or described as passing. Fail on: any item with neither proof nor disclosure; an item claimed as passing with no named command or test and no explicit result (a ticked box alone, or a bare "suite passes"); live only, an automated item shown without captured output; a missing or failed item described as passing; claimed fixtures or snapshots that the diff does not contain. (Live: CI on the PR is not a pre-submit item; see `repo_asks_honored`.) | required |
| change_focused | Every hunk in the diff, read against the fix's purpose as the plan states it. | Every hunk is part of the fix, its tests, or a doc or changelog entry the change requires. Fail on any hunk that is an unrelated refactor, a rename, a drive-by edit, reformatting or whitespace churn, or reordering of code the fix does not need, even when it is behavior-neutral. | required |
| no_debris | The added lines (`+`) in the diff, and the files the diff adds. | The added lines and files contain none of the following: debug output (print, console.log, dbg!, or similar not used for real logging), commented-out code blocks, dead or unreachable code, new TODO/FIXME/XXX markers that the plan does not account for, stray files (scratch scripts, local config, generated artifacts, editor or OS files), or merge-conflict markers. | required |
| repo_asks_honored | The repo's stated asks: the PR template's sections and checklist, the contributing guide's PR rules (eval: the repo-facts block; live: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and the `scope.md` house rules), read against the title, description, commits, branch, and diff. | Every stated ask is met. Each template section has content specific to this PR, not the template's placeholder or boilerplate. Each ticked checklist item is true per the diff and the evidence, judged under the mode's proof standard (see "Proof" above); a ticked box is never its own evidence. An ask that the code be formatted or generated is met by the code itself, or by a named command with a recorded result (eval) or captured output (live); never require formatter logs the repo does not ask for, and a named generator command reported to produce no changes need not create a diff. Each item asked for elsewhere, such as an issue reference, changelog entry, commit convention, or branch naming, is present. An ask that does not apply passes when the description says why, for example "docs n/a, no API change". Live only, by course instruction: the template's "CI is green on this PR" box must stay unticked in the draft, because CI runs only after the PR is opened, so a ticked CI box fails. If the repo states no asks, pass, and record that it states none. Never apply an ask the repo does not state. | required |
| ai_policy_honored | Eval: the repo-facts block's contribution policy. Live: the Unit 4 course instruction (`SKILL.md`, scope seam), plus any AI policy in `docs/CONTRIBUTING.md` or the PR template. Each is read against the description, especially its `Notes for Reviewers` section, and the commits. | Eval: if the repo states an AI policy, the PR complies with it. Any required disclosure must be present, in the form and place the policy asks for, and the PR must not use AI in a way the policy bans. If the repo states no AI policy, pass, and record "no stated AI policy". Live: the draft's `Notes for Reviewers` section contains an honest AI-use disclosure that says whether AI was used, and for what. This applies even when the repo states no policy, and the PR must also meet any policy the repo does state. Fail on a missing disclosure, a disclosure placed outside `Notes for Reviewers`, or boilerplate that does not say what AI did. Never apply the course disclosure requirement to an eval package. | required |
| thread_direction_engaged | Comments from maintainers or members in the issue and thread (eval: the issue and thread highlights; live: `gh issue view --comments`) that prescribe or rule out an approach, a scope, or a constraint, read against the diff and description. | Every explicit maintainer direction is followed by the diff, or the description engages it and gives a reason for departing from it. Options a maintainer only floated, and opinions from other commenters, are not direction. If the thread has no explicit maintainer direction, pass, and record that there is none. Fail when the diff contradicts a maintainer direction and the description says nothing about it. | required |
| commits_describe_change | The commit list. | Each commit message says what that commit changes, rather than "wip", "fix", or "updates". (A repo commit convention, if one is stated, is graded by `repo_asks_honored`.) | preferred |

## Verdict rule

Accept only if every `required` check grades `pass`. Reject if any
`required` check grades `fail` or `unclear`: for a required check,
`unclear` counts as `fail`, because a PR that cannot be verified from
its package is not ready to submit. `preferred` checks never change
the verdict, whether they pass, fail, or are unclear. Report them in
the summary and the JSON only. A conditional check whose trigger is
absent (no stated template ask, no stated AI policy, no maintainer
direction) grades `pass` with that absence as its evidence, not
`unclear`. On a reject, the deciding check is the first required check
in table order that graded `fail` or `unclear`.
