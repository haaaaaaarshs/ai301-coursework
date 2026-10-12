# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/162

**Branch**

`fix/54-leading-whitespace-sections`

## Eval iterations

**Run history**

I ran four evaluations, in this order:

1. Full run: **13/20**. Every package was rejected, including all seven clear-accept packages.
2. Targeted run: **8/9**, using `--only` on pkg-02, pkg-08, pkg-11, pkg-13, pkg-04, pkg-07, pkg-01, pkg-03, and pkg-12. Only pkg-13 still disagreed with gold.
3. Targeted run: **3/3**, using `--only` on pkg-13, pkg-07, and pkg-01.
4. Final full run: **20/20**, with every category fully matched.

The targeted runs helped me check the revisions before paying for another full run. The committed `eval-run.txt` is from the final full run, not one of the targeted runs. Its agreement line is:

```text
agreement: 20/20 scored items  (bar: 18/20: PASS)
```

**Package analysis**

I focused on **pkg-13**, the Neovim Windows URL-opening fix. My rubric initially returned **reject**, while the gold label was **accept**. In the final full run, my rubric returned **accept**, matching gold.

The remaining problem after my first revision was `planned_tests_accounted`. The package said:

> No-ampersand control unchanged.

My rubric treated that as too vague, even though the earlier reproduction had already identified the control:

> a control without `&`
> (`?q=hello`) opens fully

The failing case also had its own concrete result:

```text
-- after:  { code = 0, stderr = "" },
--         browser shows https://duckduckgo.com/?q=hello&t=brave
```

Reading those together accounts for both the repaired failing case and the unchanged control. I revised the instructions to resolve a named control against its definition elsewhere in the same bundle. The package also openly defers percent-sign expansion in both the plan and description, so that limitation is disclosed rather than hidden drift.

**Check rationale**

This is the exact `planned_tests_accounted` check from the submitted `rubric.md`:

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| planned_tests_accounted | Every item in the plan's test plan, plus every check the repo requires before submission (from the template checklist, the contributing guide, or a suite the plan names), read against the test-evidence section and the test files in the diff. | Every planned test and every required check has one of two things. The first is proof under the mode's standard (see "Proof" above). Eval: a named command or test with its explicit recorded result, which may be a concise summary such as a named pytest run with pass counts or a named integration test reported passing; counts help but are not required. Live: the command plus its captured printed output. Genuinely manual (both modes): a concrete observed result. A test or fixture added in the diff counts only with such proof that it ran. The second is an honest disclosure that the item was not run, or that it failed, with a reason. A disclosed item is accounted for, but it is never counted or described as passing. Fail on: any item with neither proof nor disclosure; an item claimed as passing with no named command or test and no explicit result (a ticked box alone, or a bare "suite passes"); live only, an automated item shown without captured output; a missing or failed item described as passing; claimed fixtures or snapshots that the diff does not contain. (Live: CI on the PR is not a pre-submit item; see `repo_asks_honored`.) | required |

My first version demanded raw output too broadly. That made it reject valid frozen packages that recorded specific results as named test summaries or before/after annotations. I changed the eval standard to judge whether the result identifies what ran and what happened. For live use, I kept the course requirement for captured command output.

I also clarified that an honest disclosure accounts for an unrun or failed check without turning it into a pass. The separate `repro_rerun_observable` check still requires evidence that the actual failing scenario was rerun successfully. For pkg-13, the final revision allowed the control definition and result to be read together instead of demanding that the package repeat itself.

**Trade-offs**

Accepting concise recorded results in eval mode means relying on the frozen bundle's account of what happened. It gives less detail than a full terminal transcript, but requiring a transcript everywhere caused false rejections even when the supplied evidence was specific.

The boundary I kept is stated directly in the rubric:

> A passing control never substitutes for rerunning the failing
> scenario. Live-mode captured-output requirements remain unchanged.

After the control-reference revision, I reran pkg-13 together with pkg-07 and pkg-01. That targeted run scored **3/3**: pkg-13 became an accept, while pkg-07 and pkg-01 stayed rejects. In particular, a passing control in pkg-07 did not replace the missing rerun of its cross-file failure. The final full run also kept all four not-tested packages rejected.
