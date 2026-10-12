---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: **is this
pull request ready to submit?**

A PR package is a candidate pull request: its title, description,
commits, diff, and test evidence. Read it against two things: the
plan it claims to implement, including the plan's scope, test plan,
and any deviation notes, and the issue that plan belongs to, including
the thread and the repo's stated standards. Grade nothing else. Do not
review the code's general quality, re-litigate the plan, or judge
whether the issue was worth fixing. Grade one package per run. Never
answer from gut feel. Answer by executing `procedure.md`, which
applies the checks in `rubric.md` to evidence located with
`references/evidence-guide.md`.

## Inputs and modes

Run in exactly one of two modes. Decide which one before reading
anything else. If you were given a package bundle file, use eval mode.
Otherwise, use live mode.

**Live mode** is the student's own submission, checked before it is
opened. The inputs are these, and only these:

| Input | Source |
|---|---|
| Plan, with deviation notes | `plan.md` in the working copy root. A house-chain student reads the house plan the instructor routed them to and the house repro pack instead. |
| Diff | `git diff main...HEAD` (three dots), run from the working copy. This is everything the branch changes relative to the default branch. Grade the committed branch. Uncommitted changes are not part of the PR. |
| Commits | `git log --oneline main..HEAD`, run from the working copy. |
| Draft PR title and description | `pr_draft.md` in the working copy root. |
| Test evidence | `test_evidence.md` in the working copy root: the captured command output the PR will cite. |
| Issue and thread | The issue named in `plan.md`, read with `gh issue view <n> --repo <scoped repo> --comments`. |
| PR template | `.github/PULL_REQUEST_TEMPLATE.md` in the working copy. |
| Contributing rules and stated policy | `docs/CONTRIBUTING.md` in the working copy. |

If a live input is missing, do not hunt for a substitute file. Record
it as missing. The checks that need it grade it as absent evidence,
as `procedure.md` directs. One exception: if `pr_draft.md` is missing,
stop without grading and tell the student to write their draft title
and description there. There is no PR package without that draft.

**Eval mode** treats the package bundle as the whole world. A bundle
is a markdown file with a repo-facts block, the issue, thread
highlights, a plan-context block (repro evidence and the accepted
plan), and the candidate PR (title, description, commits, diff, test
evidence). Use only the bundle text as factual evidence. Fetch
nothing. Do not read the working copy, git, GitHub, `scope.md`, or
`voice-guide.md`. Always grade the complete package: every check in
the rubric, with the full verdict rule. Use the bundle id from its
heading (for example `calib-01`) as `item`.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before any other
input.

1. If its `Repo:` line still contains a bracketed placeholder (any
   `<...>` value), stop without grading. Tell the student to fill the
   `Repo:` line in `scope.md` with their section's Path Review repo.
   Never guess a scope.
2. Find the repo the PR will target: the `upstream` remote's
   `owner/name` (from `git remote get-url upstream`), and the repo of
   the issue named in `plan.md`. If either differs from the scoped
   repo, refuse to grade. Say which repo you found and that it is out
   of scope. Do not grade it anyway.
3. Carry the scope's house rules into grading as repo requirements.
   These are the branch naming rule (`fix/<issue-number>-<slug>`, read
   with `git branch --show-current`), one bounded change per PR, and
   the rule that every template section gets real content. The
   `repo_asks_honored` check reads them alongside the PR template and
   `docs/CONTRIBUTING.md`.
4. Apply the Unit 4 course instructions below. They are stated by
   the assignment, not inferred by this tool, and they hold even when
   the repo itself states nothing on the subject:
   - **AI-use disclosure.** The draft description must contain an
     honest AI-use disclosure under its `Notes for Reviewers` section,
     even though the repo states no AI policy. `ai_policy_honored`
     grades it.
   - **CI runs after opening.** CI on the PR only runs once it is
     opened, so the template's "CI is green on this PR" box must stay
     unticked in the draft. Do not grade CI as a test outcome. A
     ticked CI box in the draft is an untrue checklist claim, and
     `repo_asks_honored` grades it as a fail.

In eval mode, ignore `scope.md` and these course instructions
entirely. An eval package is held only to the asks its own repo-facts
block states.

## The voice seam (live mode only)

In live mode, after grading the rubric checks, read `voice-guide.md`.
Hold the outgoing PR text, the title and description in
`pr_draft.md`, against each of its rules and against its "Things I
never post" list. For every rule the draft breaks, report the rule
name in the readable summary, quote the offending line, and quote the
guide's "Right" pattern. The voice guide is personal style, not a
submission standard, so it never changes a check grade or the verdict.
No rubric check reads it. In eval mode, ignore `voice-guide.md`
entirely.

## Component reads

- `rubric.md` is the brain. It defines every check (name, evidence,
  pass condition, `required` or `preferred` weight) and the verdict
  rule. Grade only the checks it defines, using their exact names.
- `references/evidence-guide.md` is the map. For each evidence
  family, it gives where the evidence lives in an eval bundle and in
  live mode, and what good looks like there.
- `procedure.md` is the hands. Execute it as written: its read order,
  its gathering moves, its check order, and its verdict assembly.

Where the procedure is silent on a step you need, do not invent one.
Take the most literal reading of the rubric's evidence column, and
report the gap under a "Procedure gaps" line in the summary. A gap is
feedback for the student.

Before grading, confirm that `rubric.md` has at least one filled check
row and a verdict rule, and that `procedure.md` has written steps
under its headings. HTML comments do not count as content. If either
file is empty, stop and say which one is empty. Emit no checks and no
JSON. This tool cannot grade without a rubric and a procedure, by
design.

## Verdict and output

The verdict is binary. `accept` means the PR is ready to submit.
`reject` means hold. There is no third verdict and no score.
Reservations go in check evidence lines, not in the verdict.

Before the JSON block, write a short readable summary: one line per
check with its grade, the deciding check if the verdict is `reject`,
and, in live mode, any missing inputs, procedure gaps, and voice-guide
notes. Then end the reply with this fenced JSON block, filled in.
Include one `checks` entry per rubric check, in rubric order. Each
`evidence` value is a single line. The block must be valid and last,
with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode, before a PR exists, use the branch as `item`, written as
`<scoped repo>@<branch>`.

## Grading discipline

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


- **Evidence first.** Never grade a check without naming the specific
  fact or quote that decided it, such as a file path, hunk, command
  output line, recorded result, or description sentence. "Looks fine"
  and a bare "tests pass" that names no command or test are not
  evidence.
- **Grade the thing, not the polish.** A terse, complete PR can be
  ready. A long, confident PR can be hiding drift. Read the diff
  against the plan, the evidence against the test plan, and the
  description against both. Never grade formatting, length, or tone.
- **The rubric decides, not the run.** If a check passes by its stated
  condition but feels wrong, it still passes. You may note the tension
  in the summary. The fix belongs in `rubric.md`.
- **The procedure decides how, not the run.** Follow `procedure.md`
  as written, and report its gaps instead of papering over them.
- **Requirements come from the record.** A repo requirement exists
  only if the repo states it: in its template, contributing guide, or
  stated policy, in the bundle's repo-facts block, or (live only) in
  `scope.md` and the Unit 4 course instructions listed in the scope
  seam. Never invent a requirement a repo does not state.
- **Proof is specific, and its bar depends on the mode.** In both
  modes, proof identifies what was run (a command, a test, or an
  action) and gives a concrete, relevant result. A ticked box alone,
  or a generic "works now", "matches expected", or "suite passes" that
  names no command and gives no concrete result, is a claim, not
  proof. For a genuinely manual test, a concrete observed result tied
  to the action and the expected behavior is proof in both modes.
  - **Eval mode:** the frozen bundle may record results as printed
    output, concrete before/after annotations (including inline
    comments), or concise named-command or named-test summaries, such
    as a named pytest run with its pass count or a named integration
    test reported passing. Judge how specific and relevant the
    recorded result is, not whether it is prose, an inline comment,
    or a terminal transcript.
  - **Live mode** keeps the Unit 4 bar: an automated command
    is proven only by the command plus its actual captured output in
    `test_evidence.md`, and the repro re-run shows before/after
    evidence. A summary or inline comment in place of captured output
    is a claim.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's
  verdict rule directs. Where the rule is silent, an unverifiable
  claim is a failing one. A PR you cannot verify from the package is
  not ready to submit.
- **Honest shortfalls are not failures, and not proof.** A PR that
  does less than the plan, and says so with a reason in a deviation
  note or its description, can satisfy plan fidelity. Silence about the
  shortfall is what fails. Disclosure does not excuse unrelated
  changes, show that the bug is fixed, or turn a missing or failed
  check into a passing one. Every other required check still grades on
  its own evidence. A missing test must never be described as passing.
