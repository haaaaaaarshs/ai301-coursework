# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

haaaaaaarshs

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-5987739340

I'd like to investigate #54. To be clear up front, I'm claiming the investigation and reproduction, not a fix.

My plan, working against current `main` in my own fork:

1. Set up the project following `docs/SETUP.md`.
2. Run the snippet from the issue body (`ResumeParser().parse()` on the indented resume text) and check whether `metadata['detected_sections']` comes back empty. Then I'll run the same text with the indentation removed as a control, so leading whitespace is the only thing that changes.
3. Run `tests/unit/test_resume_parser.py`, including the three tests the issue names (`test_parse_single_column_resume_text`, `test_parse_resume_no_work_experience`, `test_detect_sections`), and note which tests carry the #54 `xfail` marker.

I'll post a repro report with my environment, the exact steps, and the output I get, and I'll still post it if I can't reproduce the bug. I've seen the reproductions classmates already posted here. Mine will come from my own run.

I'm working with AI assistance as part of AI 301 coursework.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-5998030683

Repro report for #54: reproduced. With leading whitespace, `detected_sections` comes back empty. The same text with the indentation removed detects both Education and Skills.

**Environment**
- macOS 26.6.2 (build 25G83), x86_64 (Intel)
- Python 3.13.0 (python.org build), in a fresh virtualenv
- pypdf 6.19.0, pytest 9.1.1
- Commit: `f89c06fc3ff292df2a04a39ac51319d32a76b779` (`main` of my fork, `haaaaaaarshs/pathreview-ai301-fa26-s1`, cloned fresh)

**Setup, and where it differs from `docs/SETUP.md`**

I forked the repo, cloned my fork and added `upstream`, as `docs/SETUP.md` step 1 says. After that I ran only the Python part of `make setup`. I don't have Docker installed, so I skipped `docker compose up`, the migrations, the seed data and the frontend install. The parser and its unit tests ran without them.

`pip install -e ".[dev]"` failed on this machine with `Failed building wheel for libcst`. `mutmut` needs `libcst>=1.9.0`, which has no prebuilt wheel for this Intel Mac and needs a Rust toolchain to build, which I don't have. So I installed the project and every other dev dependency, leaving out `mutmut`, which these tests don't use:

```bash
git clone https://github.com/haaaaaaarshs/pathreview-ai301-fa26-s1.git pathreview-fork
cd pathreview-fork
git remote add upstream https://github.com/codepath/pathreview-ai301-fa26-s1.git
python3.13 -m venv .venv
.venv/bin/python -m pip install --upgrade pip setuptools wheel
.venv/bin/pip install -e ".[dev]"   # fails: libcst wheel build
.venv/bin/pip install -e . "pytest>=7.4.0" "pytest-cov>=4.1.0" "pytest-asyncio>=0.23.0" \
  "pytest-benchmark>=4.0.0" "pytest-httpserver>=1.0.8" "hypothesis>=6.92.0" "ruff>=0.2.0" \
  "black>=24.1.0" "mypy>=1.8.0" "pre-commit>=3.6.0" "types-redis>=4.6.0"
```

**Steps and observed output**

1. I ran the snippet from the issue body, unchanged, from the repo root:

```bash
.venv/bin/python - <<'PY'
from ingestion.parsers.resume_parser import ResumeParser
r = ResumeParser()
res = r.parse('\n    John Smith\n    john@example.com\n\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n')
print(res.metadata['detected_sections'])
PY
```

Output:

```text
[]
```

Expected, per the issue: Education and Skills.

2. As a control, I parsed the same text again after removing the indentation with `textwrap.dedent`. The indentation is the only change.

```bash
.venv/bin/python - <<'PY'
from textwrap import dedent
from ingestion.parsers.resume_parser import ResumeParser
text = '\n    John Smith\n    john@example.com\n\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n'
r = ResumeParser()
print("indented:  ", r.parse(text).metadata['detected_sections'])
print("dedented:  ", r.parse(dedent(text)).metadata['detected_sections'])
PY
```

Output:

```text
indented:   []
dedented:   ['Skills', 'Education']
```

3. I ran the parser's test file:

```bash
.venv/bin/python -m pytest tests/unit/test_resume_parser.py -rx -q
```

Result: `5 passed, 5 xfailed in 1.62s`. All five xfailed tests have the reason "issue #54: resume section detection fails on leading whitespace":
- `test_parse_single_column_resume_text`
- `test_parse_resume_no_work_experience`
- `test_parse_markdown_resume`
- `test_detect_sections`
- `test_strip_markdown_syntax`

The issue names the first, second and fourth. The repo also marks `test_parse_markdown_resume` and `test_strip_markdown_syntax` as xfail for #54.

4. To see the real assertion failures instead of just the xfail marks, I ran the three tests the issue names with `--runxfail`:

```bash
.venv/bin/python -m pytest \
  "tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text" \
  "tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience" \
  "tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections" \
  --runxfail -q
```

Result: `3 failed in 0.36s`. Each one fails on a section-detection assertion:

```text
test_parse_single_column_resume_text:
>       assert any("experience" in s for s in detected_lower) or any(
E       assert (False or False)

test_parse_resume_no_work_experience:
>       assert any("education" in s for s in detected_lower)
E       assert False

test_detect_sections:
>       assert len(sections) > 0
E       assert 0 > 0
E        +  where 0 = len([])
```

**Outcome**

Reproduced. The issue's example returns `[]` for indented text, and the same text without indentation returns `['Skills', 'Education']`. The three tests the issue names fail on their section-detection assertions when run with `--runxfail`. I didn't run `test_parse_markdown_resume` or `test_strip_markdown_syntax` with `--runxfail`, so I haven't checked that they fail for the same reason. I haven't confirmed the cause or changed any code.

I'm working with AI assistance as part of AI 301 coursework.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke test: 3/3
2. Full run: 19/20 (bar: 18/20: PASS). Only miss: pkg-09, failed behavior-match.
3. Full run after I'd edited my evidence guide: 16/20 (below the bar). clear-accept dropped to 4/8; pkg-05, pkg-09, pkg-10 and pkg-12 were all rejected.
4. After revising the Steps and Behavior shown sections of the evidence guide: canary `--only pkg-05,pkg-09,pkg-10,pkg-12` 4/4, then canary `--only pkg-02,pkg-08,pkg-16,pkg-17,pkg-06,pkg-18,pkg-19,pkg-20` 8/8 (these runs don't count toward the bar).
5. Final full saved run (`eval-run.txt`): 20/20 scored items (bar: 18/20: PASS)

**Package analysis**

**pkg-09** (sharkdp/fd#2033, category clear-accept). Gold label: `accept`. My rubric's verdict was `reject` in run 2 (failed `behavior-match`) and in run 3 (failed `followable-steps`, `behavior-match`, `outcome-supported`). In the final run it's `accept`, which matches gold.

The package is a cannot-reproduce report. The reporter tried to trigger scenario 2 (a later `--exec-batch` command running before an earlier one when the argument-size limit is hit), and their `/tmp/order.log` shows every ONE batch before any TWO batch over 5 runs plus a padded re-run. The gold note calls this an "honest cannot-reproduce" that "names what differed (uniform name lengths, 2 MiB ARG_MAX)."

The run file only records which checks failed, not the grader's reasoning, so this is my reading. My evidence guide said "A cannot-reproduce report counts only when its artifact shows the expected behavior where the issue says it fails." The report itself admits the attempt may never have hit the trigger: "fd appears to flush both command buffers at the same file-count boundary on this input." So a strict grader decided the log doesn't show the expected behavior *where the issue says it fails*, and failed behavior-match. I changed that sentence to "A cannot-reproduce report counts when its artifact shows the actual result of a real attempt at the issue's trigger (for example, the output or log where the issue says the failure appears) and the report names what differed from the issue's conditions." pkg-09 does both, so it passes now.

**Check rationale**

| behavior-match | Use the Behavior shown section of `references/evidence-guide.md`: compare the report's output/log/screenshot evidence with the behavior described by the issue. | Pass when the supplied artifact actually demonstrates the behavior the issue describes, or clearly demonstrates its absence in a cannot-reproduce report; evidence of only an adjacent or different failure does not pass. | required |

The first part, "actually demonstrates the behavior the issue describes," and the last clause, "evidence of only an adjacent or different failure does not pass," are there to catch packages that show a real failure that isn't the one the issue is about. All 4 wrong-target packages were rejected in every run (wrong-target 4/4). The middle clause, "or clearly demonstrates its absence in a cannot-reproduce report," means an honest cannot-reproduce isn't failed just for not reproducing the bug. That matches outcome-supported, where an evidenced cannot-reproduce passes.

I kept the rubric wording the same and fixed how "clearly demonstrates its absence" gets read in the evidence guide instead. The old guide line made "clearly" mean "shows the expected behavior where the issue says it fails," and that rejected pkg-09 and pkg-10, two honest cannot-reproduce reports whose setups didn't quite match the issue's. The new line counts a real attempt at the trigger plus a stated difference. I kept "A different or adjacent failure does not count" so the wrong-target protection stays the same.

**Trade-offs**

Both evidence-guide changes loosen a check. The Steps change accepts a setup file that's described rather than printed in full, as long as the material content is pinned down. The Behavior shown change accepts a cannot-reproduce that shows a real attempt and names what differed, even if that attempt may not have hit the trigger. That fixed pkg-05, pkg-09, pkg-10 and pkg-12 (clear-accept 4/8 → 8/8).

The risk is that a looser check lets a should-reject package through. So before the full run, I re-ran the should-reject packages the changes could affect with `--only`: wrong-target pkg-02, pkg-08, pkg-16, pkg-17; unfollowable-comms pkg-06, pkg-18, pkg-19; and disclosure pkg-20, the one-package category. All 8 were still rejected. The final full run confirmed nothing else changed: disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4, 20/20 overall. The case I accept it could miss is a cannot-reproduce that makes a real attempt and names a difference, but whose attempt is too far from the trigger to tell anyone anything. The new wording would pass that report on behavior-match.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
