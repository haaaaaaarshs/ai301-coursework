# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54

**Verdict output**

1. #54 Resume section detection fails on text with leading whitespace: ACCEPT

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
paste the output here, including the closing JSON block
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

18/20
0/2
0/2
1/1
0/1
1/3
3/4
0/1
5/6
20/20

The final full run reported: `agreement: 20/20 scored items (bar: 18/20: PASS)`

**Issue analysis**

`issue-19`

My rubric's final decision: `accept`

Gold label: `accept`

Reasoning from the final rubric: the repository was active and unarchived, the issue was unclaimed, and there was no policy preventing AI-assisted contribution. The important scope distinction was that the issue's two named causes and additional implementation suggestions were maintainer-provided diagnosis of one performance problem rather than separate unrelated tasks. After revising `newcomer-scope` to account for that distinction, the rubric accepted the issue.

**Check rationale**

`newcomer-scope`

| newcomer-scope | Read the issue body and comment thread for the requested behavior, examples, maintainer clarification, tracking/umbrella language, unresolved product or design decisions, and abandoned implementation attempts. Also note whether a maintainer/collaborator authored the issue and explicitly marked it `good first issue`; this is supporting scope evidence, not sufficient by itself. | Pass when the issue identifies one concrete bug, behavior, or outcome that can be investigated as a coherent contribution. A short maintainer-authored bug report may pass when it is explicitly marked `good first issue` and gives concrete examples of the affected behavior, even if it does not enumerate every affected case. Multiple suspected causes or implementation suggestions may also pass when they address one concrete outcome. Fail when the issue is a codebase-wide or ongoing umbrella inviting arbitrary partial work, when the desired behavior requires an unresolved product/design decision, or when prolonged design debate or multiple abandoned attempts show that the scope is not settled. A `good first issue` label does not override any of these fail conditions. | required |

I revised this check because my original version treated multiple named causes or implementation suggestions as evidence that an issue was automatically too broad. `issue-19` showed why that was too strict: a maintainer can provide several possible causes or approaches while still describing one bounded bug. I tightened the check so that it rejects genuinely open-ended or underspecified work without rejecting a focused issue simply because the maintainer supplied useful diagnostic detail.

**Trade-offs**

Changing `newcomer-scope` risked making the rubric too permissive, so I re-ran scope canaries with `--only`. In particular, I tested `issue-04`, `issue-05`, `issue-10`, `issue-15`, `issue-19`, and `issue-20`. The revised check ultimately accepted `issue-04` and `issue-19` while still rejecting the four scope-category issues. The final full evaluation confirmed the trade-off worked as intended with `scope 4/4` and `20/20` overall agreement.

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

Answer all three:

1. Issue #54 fits my interests because it involves debugging actual Python parsing logic and working with failing tests rather than only making a documentation or test-fixture change. It also seems small enough to investigate and reproduce within the time available for the assignment.

2. My skill correctly identified that the repository is active, the issue is unclaimed under the Path Review rules, the problem is bounded, and there is no policy preventing AI-assisted work. What the rubric cannot really account for is my own preference for a debugging task where I can trace the problem and work toward making existing failing tests pass, which made #54 more appealing to me than the other accepted candidates.

3. I expect claiming the issue itself to be straightforward because the expected behavior and failing cases are already described. Several classmates have also shown interest in the issue, so I will still need to clearly document my own investigation and reproduction rather than relying on work already posted by others.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
