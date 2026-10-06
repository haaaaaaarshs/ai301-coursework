# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**haaaaaaarshs**

[Your GitHub username, exactly as it appears on your profile - no @, no
profile URL. Your comment upstream is identified by this name, and it is
the only thing that ties it to you. Several students may plan the same
house issue, so this is what keeps their comments off your score and
yours off theirs.]

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-6008171172

I reproduced #54 and traced the failure to line-start matching in the resume parser.

The repro shows that indented input returns no detected sections, while the same text with only the indentation removed detects `Education` and `Skills`. `_detect_sections` currently expects the known section name immediately at the start of a line, so leading whitespace prevents a match. The Markdown header stripping path has the same kind of line-start assumption for indented headings.

My plan is a bounded whitespace-handling fix in `ingestion/parsers/resume_parser.py`: allow leading indentation where section headers and Markdown headings are matched, without dedenting or broadly normalizing the resume.

For verification, I’ll re-run the Unit 2 indented-vs-dedented reproduction and all five tests currently marked `xfail` for #54. Once they pass for the intended reason, I’ll remove the corresponding #54 `xfail` markers as required by the repo’s test convention and run the parser test file normally.

I’ll build this on a `fix/54-...` branch, run `make check` and `make test-unit`, and make sure CI is green before opening the PR.

I’m using AI assistance as part of CodePath AI 301 coursework and reviewing the plan and implementation myself.

---

## Your branch

**Branch**

fix/54-leading-whitespace-sections

**Evidence**

Before the fix:

```bash
.venv/bin/python - <<'PY'
from textwrap import dedent
from ingestion.parsers.resume_parser import ResumeParser

text = '''
    John Smith
    john@example.com

    Education:
    - B.S. Computer Science

    Skills: Python
'''

r = ResumeParser()

print("indented:  ", r.parse(text).metadata["detected_sections"])
print("dedented:  ", r.parse(dedent(text)).metadata["detected_sections"])
PY
```

Output:

```text
indented:   []
dedented:   ['Skills', 'Education']
```

After the fix, I re-ran the same reproduction:

```bash
.venv/bin/python - <<'PY'
from textwrap import dedent
from ingestion.parsers.resume_parser import ResumeParser

text = '''
    John Smith
    john@example.com

    Education:
    - B.S. Computer Science

    Skills: Python
'''

r = ResumeParser()

print("indented:  ", r.parse(text).metadata["detected_sections"])
print("dedented:  ", r.parse(dedent(text)).metadata["detected_sections"])
PY
```

Output:

```text
indented:   ['Skills', 'Education']
dedented:   ['Skills', 'Education']
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `agreement: 18/20 scored items (bar: 18/20: PASS)`
2. Targeted `--only` retry: `6/6` packages matched their gold labels.
3. `agreement: 20/20 scored items (bar: 18/20: PASS)`

**Package analysis**

Package: `pkg-14`

My rubric's final verdict: `accept`

Gold label: `accept`

The package described a Zellij reattach bug where raw OSC color responses leaked into the terminal after reattaching to a session. The reproduction showed that fresh attach was clean, reattach consistently leaked, version 0.44.1 was clean, and clearing the cache temporarily restored one clean attach. The candidate plan identified the reattach handshake as the failure path and proposed consuming pending OSC responses before pane input was wired.

My first rubric version rejected this package because `diagnosis_supported`, `fix_targets_cause`, and `executable_plan` were too strict about requiring the exact internal mechanism and final function locations to already be proven. I revised those checks so that a plan can pass when the reproduction narrows the failure to a supported component or path, the plan explicitly acknowledges remaining uncertainty, and another contributor can still begin the work from the stated target and approach.

With that revision, `pkg-14` passed because its diagnosis was supported by the reproduction, the proposed fix targeted the reattach path shown by the evidence, and the plan was specific enough to begin implementation while being transparent about what still needed tracing.

**Check rationale**

Quoted check from `rubric.md`:

> `diagnosis_supported` — Evidence: The plan's stated diagnosis read against the Repro evidence, especially the observed behavior, comparison cases, and results. Pass condition: The proposed cause must reasonably explain the reproduced behavior and must not be contradicted or ruled out by the reproduction. If the reproduction narrows the failure to a component or path but does not prove the exact internal mechanism, the diagnosis may still pass when that uncertainty is acknowledged rather than presented as fact. Weight: `required`.

I revised this check after my first eval run because it was too strict. It was originally expected that the reproduction would support the proposed cause more directly, which caused `pkg-14` to fail even though its reproduction clearly narrowed the bug to the reattach path, and the plan openly acknowledged that the exact internal function still needed tracing.

I changed the check so that a diagnosis can still pass when the reproduction supports the relevant component or failure path without proving every internal detail, as long as the plan does not present the remaining uncertainty as fact. I kept the rule that contradictory reproduction evidence is still a failure, so cases like a plan whose proposed cause is directly ruled out by the repro remain rejected.

**Trade-offs**

The revised `diagnosis_supported` check is intentionally more tolerant of plans that have strong reproduction evidence for a failure path but have not yet proven every internal implementation detail.

The trade-off is that this can accept a plan whose exact mechanism later turns out to be slightly different, as long as the plan is transparent about that uncertainty and the proposed work still follows from the reproduced behavior.

I checked that loosening this rule did not break obvious reject cases by re-running canaries with `--only`, including `pkg-01` for a wrong-cause reject and `pkg-10` for an unbuildable reject. Both remained rejected, while `pkg-14` correctly flipped from reject to accept.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
