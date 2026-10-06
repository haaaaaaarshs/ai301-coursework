# Evidence guide

## Reproduction evidence

Location:
- In practice packages, use the `## Repro evidence` section.
- For a live issue, use the reproduction report or repro comment the plan is based on.

Look for:
- exact reproduction steps,
- expected behavior,
- actual behavior,
- comparison cases,
- timings or outputs,
- evidence that isolates a cause,
- and evidence that rules out a proposed cause.

Good evidence:
- directly observed behavior,
- repeatable commands or steps,
- before/after or comparison results,
- results that distinguish between competing explanations.

Do not treat a theory from the issue thread as reproduced evidence unless the reproduction results support it.

## Diagnosis evidence

Location:
- The diagnosis, cause, or summary portion of the Candidate plan.
- Compare it directly with `## Repro evidence`.

Look for:
- the cause the contributor claims is responsible for the bug,
- which reproduction result supports that cause,
- and whether any reproduction result contradicts it.

Good evidence:
- the claimed cause explains all important reproduced behavior,
- comparison cases point toward the same cause,
- and the plan does not ignore evidence that rules the cause out.

## Scope evidence

Location:
- The Candidate plan's scope, change, files, or out-of-scope statements.

Look for:
- which files or components will change,
- which behavior will change,
- what is explicitly excluded,
- and whether the work expands beyond what is necessary for the reproduced bug.

Good evidence:
- a bounded change tied to the reproduced problem,
- clear implementation targets,
- explicit limits when nearby work is intentionally excluded.

## Implementation evidence

Location:
- The Candidate plan's changes, approach, or implementation steps.
- Compare these with the diagnosis and reproduction evidence.

Look for:
- what the contributor intends to modify,
- how that modification addresses the diagnosed cause,
- and whether the change targets the demonstrated cause rather than a symptom.

Good evidence:
- the proposed edit has a clear causal connection to the reproduced failure,
- another contributor could identify where to begin,
- and important unknowns are acknowledged rather than presented as certainty.

## Test-plan evidence

Location:
- The Candidate plan's test section.
- Read it against the original `## Repro evidence`.

Look for:
- whether the original failing behavior is re-tested,
- the expected post-fix result,
- commands or actions that produce an observable result,
- and whether the test would distinguish the fixed behavior from the original failure.

Good evidence:
- reuses or adapts the original reproduction,
- defines a concrete observable success condition,
- and would expose the bug if the fix did not work.

## Thread evidence

Location:
- `## Thread highlights`.
- For a live issue, read relevant issue comments and maintainer replies.

Look for:
- maintainer direction,
- accepted or rejected approaches,
- scope constraints,
- warnings,
- unresolved questions,
- and competing hypotheses.

Good evidence:
- statements from maintainers or collaborators that constrain the plan,
- conclusions supported by reproduction evidence.

Treat unverified contributor theories as hypotheses, not facts.

## Repository evidence

Location:
- `## Repo facts`.
- For a live repository, use contribution documentation and stated repository conventions.

Look for:
- contribution rules,
- review expectations,
- required tests,
- repository-specific conventions,
- AI policies if stated,
- and technical constraints relevant to the change.

Good evidence:
- explicit repository documentation or conventions that materially affect the proposed plan.

## Plan-comment evidence

Location:
- `## Candidate plan comment`.
- For a live issue, use the draft comment intended for the issue thread.

Look for:
- whether the comment accurately represents the plan,
- whether it acknowledges relevant maintainer guidance,
- whether it makes unsupported certainty claims,
- and whether it introduces scope that is missing from the plan.

Good evidence:
- a concise summary consistent with the full plan,
- no contradiction with the reproduction, thread, or repo conventions,
- and no new unsupported claims.