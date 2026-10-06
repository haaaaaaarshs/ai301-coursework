# Plan for issue #54

## Diagnosis

The section detector does not recognize resume section headers when the line begins with indentation.

The reproduction shows that the issue input returns no detected sections:

> indented:   []

while the same text with only the indentation removed returns:

> dedented:   ['Skills', 'Education']

The three issue-named tests also fail on their section-detection assertions when run with `--runxfail`.

In `ResumeParser._detect_sections`, each section name is matched with patterns anchored either at the start of the text or immediately after a newline, for example:

- `^{section}...`
- `\n{section}...`

Those patterns do not allow spaces or tabs before the section header. As a result, lines such as `    Education:` and `    Skills: Python` are skipped even though the same unindented headers are detected.

## Scope

### In scope

- Update section-header matching in `ingestion/parsers/resume_parser.py` so known resume section headers can have leading whitespace.
- Update the Markdown header stripping logic in the same file where its start-of-line matching has the same leading-whitespace problem.
- Preserve existing behavior for unindented headers.
- Re-run all five tests currently marked `xfail` for issue #54 and remove the #54 `xfail` markers from tests that are fixed by this change, following the repository convention for issue-linked tests.

### Out of scope

- Changing the names in `SECTION_HEADERS`.
- Reworking the overall resume parser architecture.
- Changing PDF extraction behavior.
- Fixing parser behavior unrelated to leading whitespace or issue #54.

## Files

- `ingestion/parsers/resume_parser.py`
- `tests/unit/test_resume_parser.py`

## Approach

1. Update `_detect_sections` so its section-header regexes allow leading horizontal whitespace before a known section name while preserving the existing header and separator requirements.
2. Update `_strip_markdown`'s Markdown-header pattern so leading indentation before a Markdown heading does not prevent the heading syntax from being stripped.
3. Keep both changes targeted to line-start whitespace handling rather than dedenting or normalizing the entire resume.
4. Re-run all five tests marked `xfail` for issue #54.
5. Remove the #54 `xfail` markers from the tests covered by the fix once they pass normally.
6. If a #54-marked test still fails for a demonstrably different reason, document that evidence rather than broadening the implementation without justification.

## Test plan

First, re-run the Unit 2 reproduction:

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

Before the fix, the reproduced behavior is:

```text
indented:   []
dedented:   ['Skills', 'Education']
```

After the fix, both the indented and dedented inputs should detect the same relevant sections. The order may vary because `_detect_sections` removes duplicates through a set, but both results must contain `Education` and `Skills`.

Next, run the full resume parser test file:

```bash
.venv/bin/python -m pytest tests/unit/test_resume_parser.py -rx -q
```

Then run all five tests currently associated with issue #54 using `--runxfail` so their real results are visible:

```bash
.venv/bin/python -m pytest \
  "tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text" \
  "tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience" \
  "tests/unit/test_resume_parser.py::TestResumeParser::test_parse_markdown_resume" \
  "tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections" \
  "tests/unit/test_resume_parser.py::TestResumeParser::test_strip_markdown_syntax" \
  --runxfail -q
```

Expected after the fix: the assertions affected by issue #54 pass, and the corresponding #54 `xfail` markers can be removed. After removing those markers, run the full parser test file again normally and confirm the fixed tests pass without `xfail`.

The final verification should show that:

- indented section headers are detected,
- existing unindented section headers still work,
- the issue-linked tests pass normally,
- and the fix does not introduce new failures in `tests/unit/test_resume_parser.py`.

Before opening the PR, I will also follow the repository contribution checks:

```bash
make check
make test-unit
```

Both commands should complete successfully.

The implementation branch will follow the repository naming convention, using:

```text
fix/54-leading-whitespace-sections
```
Before opening the PR, I will confirm that CI is green.

## Risks and unknowns

Allowing leading whitespace should stay narrow enough that arbitrary body text is not mistaken for a section header. The change should permit indentation before known headers while preserving the existing section-name and separator requirements.

The Markdown stripping path has a similar start-of-line assumption, so that behavior will be checked alongside section detection. If any issue #54 test still fails for a clearly different reason, I will document that rather than broadening the fix without evidence.

## Deviations

The implementation followed the plan. The only minor deviation was formatting cleanup in `tests/unit/test_resume_parser.py`: Ruff removed whitespace from blank lines and Black reformatted the test file while running the repository checks. No change to the intended fix scope was required.