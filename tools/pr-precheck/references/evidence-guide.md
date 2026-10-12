# Evidence guide: where evidence lives in a PR package

Each family below lists where its evidence lives in an eval bundle
and in live mode, then what good looks like. In eval mode, the bundle
is the only source. In live mode, paths are relative to the working
copy root, and `<n>` is the issue number named in `plan.md`.

## Plan fidelity (harness category: silent-drift)

Checks: `diff_within_plan`, `planned_work_delivered`,
`description_matches_diff`.

**Where it lives**

| Evidence | Eval bundle | Live mode |
|---|---|---|
| Plan steps, boundary (named files plus not-in-scope list) | `## Plan context`, the "Plan:" paragraph: its steps, "Scope", "Not in scope", "Files:" | `plan.md`: Diagnosis/approach, `## Scope` (In scope / Out of scope), `## Files` |
| Deviation notes | `## Plan context`, if it carries any; otherwise the `### Description` under `## Candidate PR` | `plan.md`'s deviation notes section, and `pr_draft.md` |
| What actually changed | `### Diff` under `## Candidate PR`: the `--- a/` / `+++ b/` file headers and the `@@` hunks | `git diff main...HEAD` (file list: `git diff --stat main...HEAD`) |
| Fidelity claims | `### Title` and `### Description` | the title and description in `pr_draft.md` |

**What good looks like**

- Every file in the diff is either named in the plan's Files or Scope,
  or is a test, fixture, changelog, or doc file that the test plan or
  the repo asks for. Every hunk carries out a plan step.
- Every plan step has a hunk that delivers it.
- Every mismatch in either direction is named in a deviation note with
  a reason. A shortfall that the PR discloses honestly is acceptable
  for plan fidelity. Fidelity is the only thing a note settles: a
  noted unrelated change is still unreviewable (diff quality), and a
  noted gap in testing is still not proof (test evidence).
- Every claim in the description, such as "implements the plan
  exactly", "no changes beyond it", or "rendering unchanged", can be
  pointed at a hunk or an evidence line that bears it out. Code, test,
  and template claims point at the actual diff. A run claim points at
  evidence that meets the test-evidence family's proof standard for
  the mode, never at a ticked box alone. A named generator command
  reported to produce no changes bears out a "no changes" claim
  without a hunk.

**What silent drift looks like**

- *More than the plan*: an extra file or hunk with no note. For
  example, a refactor of a second module riding along with a one-file
  fix, or an edit to something the plan put out of scope.
- *Less than the plan*: a planned step, such as a marker removal, a
  second call site, or a changelog entry, that is missing from the diff
  with nothing in the package saying so.
- *Description drift*: the description claims fidelity that the diff
  contradicts (for example, "no changes beyond the plan" next to an
  unplanned file), or it omits a behavior change the diff makes. This
  is a plan-fidelity failure, not a communication problem.

## Test evidence (harness category: not-tested)

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


Checks: `repro_rerun_observable`, `planned_tests_accounted`.

**Where it lives**

| Evidence | Eval bundle | Live mode |
|---|---|---|
| Original failure and repro steps | `## Plan context`, "Repro evidence (from the accepted report)" | `plan.md`'s Diagnosis and repro quotes, and the student's repro comment on the issue (`gh issue view <n> --comments`) |
| Planned tests and expected-after | `## Plan context`, "Test plan:" | `plan.md`'s test plan section |
| Repo-required checks | `## Repo facts`, the "pull requests" and "contribution policy" lines (for example, a template checklist asking for tests) | `.github/PULL_REQUEST_TEMPLATE.md` `## Testing` checklist (`make test-unit`, `make test-integration`, `make lint`, `make typecheck`), and `docs/CONTRIBUTING.md` "Pull Request Process" (`make check && make test-unit`) and its rule about xfail markers on seeded bugs. The template's "CI is green" item is excluded by course instruction: CI runs after opening, and its box stays unticked in the draft. |
| Proof shown | `### Test evidence` under `## Candidate PR` | `test_evidence.md` (captured command output) |
| Tests added | test and fixture files in `### Diff` | test files in `git diff main...HEAD` (for example, under `tests/`) |

**What good looks like**

- **Automated commands, eval mode**: a named command or test with an
  explicit, specific result, in whatever form the bundle records it:
  printed output, a concrete before/after annotation (an inline
  comment counts), or a concise summary such as a named pytest run
  with its pass count, or a named integration test reported passing.
  Counts help but are not required. Judge specificity and relevance,
  not form. Example: the repro's command annotated `# before:
  DuplicateOptionError / after: parsed OK`.
- **Automated commands, live mode**: by the Unit 4 requirement, the
  command followed by its actual captured printed output in
  `test_evidence.md`, with before/after evidence for the repro.
  Example: `$ python3 -c "...configparser..."` followed by the printed
  line `parsed OK`, where the repro raised `DuplicateOptionError`.
- **Repro re-run (both modes)**: an identifiable re-run of the
  reported failing scenario, named or explicitly tied to the supplied
  repro, with a concrete observed outcome that matches the
  expected-after. A passing control offered alone does not show the
  failing scenario was fixed.
- **Genuinely manual tests** (a UI action, a visual check): the action
  taken, plus a concrete observed result tied to the expected
  behavior. For example, "opened the review page with an indented
  resume; the Skills and Education sections both render".
- **Not proof (both modes)**: a ticked checkbox alone, or a generic
  "works now", "colors work", "matches the issue's expected output",
  or "the full suite passes" with no named command or test and no
  concrete result, whether written as prose or as an inline comment.
  Each of these is a claim about a result, not a result.
- Every test-plan item and every repo-required check has either proof
  as above, or an honest disclosure that it was not run or that it
  failed, with a reason. A disclosed item is accounted for, but it is
  never presented as passing, and it does not prove the fix.
- Claims about tests, fixtures, and snapshots match the files actually
  in the diff.
- Live mode, by course instruction: CI runs only after the PR is
  opened, and its template box stays unticked in the draft. The
  pre-submit bar is the local commands the template names, each with
  captured output in `test_evidence.md`.

**What not-tested looks like**

- "Tests pass" or "works now" with no command and no result.
- An after result that is only a ticked box, or a generic "matches
  expected" with no concrete behavior, in any form.
- A passing control or unrelated test offered in place of a re-run of
  the reported failing scenario.
- Live mode only: an automated command shown as a summary or inline
  comment instead of its captured output.
- A missing or failing test described as passing.
- Output from a command that never exercises the reported behavior.
- A planned test or required suite that is missing from the evidence
  and never mentioned.
- Claimed fixtures or snapshots that are absent from the diff.

## Diff quality (harness category: unreviewable)

Checks: `change_focused`, `no_debris`, `commits_describe_change`.

**Where it lives**

| Evidence | Eval bundle | Live mode |
|---|---|---|
| The change | `### Diff` hunks, especially the `+` lines and new files | `git diff main...HEAD` |
| Commit list | `### Commits` | `git log --oneline main..HEAD` |

**What good looks like**

- A reviewer can find the fix immediately. Every hunk serves the fix,
  its tests, or a required changelog or doc entry. Lines unrelated to
  the change are untouched.

**Debris tells**

- Debug output: `print(`, `console.log(`, `dbg!`, `pdb.set_trace()`.
- Commented-out code blocks.
- Dead or unreachable code.
- New TODO, FIXME, or XXX markers.
- Scratch or generated files: `.orig`, `.DS_Store`, local config,
  build output.
- Conflict markers.
- Formatting or whitespace churn, and reordered imports.
- Behavior-neutral rewrites of code the fix does not need, for example
  a restyled function in a file the plan never names.
- Drive-by edits, typos fixed elsewhere, and renames.

## Standards and comms (harness category: standards-wall)

Checks: `repo_asks_honored`, `ai_policy_honored`,
`thread_direction_engaged`.

**Where it lives**

| Evidence | Eval bundle | Live mode |
|---|---|---|
| PR template asks | `## Repo facts`, "pull requests:" line (for example: "asks for a Description and a Checklist…", or "no PR template") | `.github/PULL_REQUEST_TEMPLATE.md`: Summary, Issue (`Closes #`), Changes, Testing checklist, Screenshots / Demo, Notes for Reviewers |
| Contributing asks | `## Repo facts`, "pull requests" and "contribution policy (CONTRIBUTING.md)" lines (for example: "PRs stay small and reference the issue") | `docs/CONTRIBUTING.md`: Branch Naming Convention, Commit Message Convention (Conventional Commits), Pull Request Process, xfail-marker rule, "Do not bulk-fix lint or type findings" |
| House rules | not applicable (scope is ignored) | `scope.md`, "Path Review house rules": `fix/<n>-<slug>` branch from your fork, one PR per issue, every template section filled |
| AI-use policy | `## Repo facts`, "contribution policy" line (for example, "no stated AI policy"). Only the repo's own policy applies. | **Unit 4 course instruction** (`SKILL.md`, scope seam): an honest AI-use disclosure is always required under `Notes for Reviewers` in `pr_draft.md`, whether or not the repo has an AI policy. Also search `docs/CONTRIBUTING.md` and the PR template for any AI policy the repo itself states. |
| CI box | not applicable | **Unit 4 course instruction**: the template's "CI is green on this PR" box stays unticked in `pr_draft.md`, because CI runs after opening |
| Maintainer direction | `## Issue` (note the author's role, such as MEMBER) and `## Thread highlights` | `gh issue view <n> --repo codepath/pathreview-ai301-fa26-s1 --comments` |
| Where the PR honors these | `### Title`, `### Description`, `### Commits` | `pr_draft.md`, `git log`, `git branch --show-current` |

**What good looks like**

- Every template section carries content specific to this PR. Every
  ticked checklist item is true per the diff and the evidence, judged
  under the same mode-specific proof standard as the test-evidence
  family; a ticked box is never its own evidence. An ask for formatted
  or generated code is met by the code itself or a named command with
  its result: do not require formatter logs the repo does not ask
  for, and a named generator command reported to produce no changes
  need not create a diff. An item
  that does not apply says why, for example "docs n/a, no API/CLI
  change".
- Every contributing ask the repo states is visibly met: an issue
  reference, a changelog entry, the commit and branch conventions.
- Eval: a stated AI policy is met in the form it asks for. Where no
  policy is stated, nothing is required.
- Live: `Notes for Reviewers` honestly says whether AI was used and
  for what, and the CI box is unticked.
- An explicit maintainer direction is followed, or the description
  engages it by name.

**What a standards wall looks like**

- Template sections left as placeholders, or filled with boilerplate
  that could be pasted unchanged into any PR.
- A ticked box the diff contradicts.
- A missing changelog or issue reference that the repo asks for.
- A missing disclosure that the repo's policy requires.
- A maintainer's stated constraint quietly ignored.

Requirements come only from what the repo states, plus, in live mode
only, the Unit 4 course instructions. Do not apply one repo's asks,
the course instructions, or general good practice to an eval package
whose repo does not state them.
