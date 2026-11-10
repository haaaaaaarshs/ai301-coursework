# Contribution: Copter: vehicle may backup if mission with takeoff as first command is initiated

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Harsh Sabu
**GitHub Username:** haaaaaaarshs
**Issue:** https://github.com/ArduPilot/ardupilot/issues/6322
**Pull Request:** Not opened yet.

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Building and refining my issue scout — October 18, 2026**

I upgraded my Unit 1 issue-select skill to issue-scout and wrote searches for Python API bugs, dataset issues, and C++ simulation bugs based on my backend, ML, and robotics interests. My first scan accepted one issue and rejected four, showing that unassigned issues can already have open PRs and that my rubric did not explicitly handle work already fixed by a merged PR. I updated the availability check, added a filter for the fixed-on-dev label, and clarified that a project using AI does not establish its policy on AI-assisted contributions. I also graded three picks from the course site, but the two accepted results still had concerns about an existing fix or work allocated to a bot, so I have not chosen a final issue.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: Installed the ship-gate, wrote an ArduPilot gate manifest, and ran it on my branch (2026-10-30)**

On October 30 I installed the staff ship-gate around my existing pr-precheck, changed its
scope to real repositories, and wrote a sourced ArduPilot manifest with three checks:
configure SITL, build Copter, and run the TakeoffAlt and TakeoffCheck autotests. The
`gh pr create --help` canary was stopped by both voice-guard and ship-gate; Claude only
showed ship-gate's stop at first, but matching 18:38 entries in both logs confirmed that both
fired, so no bypass or settings repair was needed. The real manual run on
`fix/6322-inflight-takeoff` against `master` passed all three checks and then stopped
because the branch has no changes yet ("There is nothing to ship yet."), which is an
empty-diff stop, not a passing verdict and not a rejection from my pr-precheck rubric. One
improvement I would make is to check for an empty branch before spending over 20 minutes on
builds and tests; this is a proposal, not something I have implemented.

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

[1-2 paragraphs in your own words: how you searched, what your scan
surfaced, what your scout's verdict said, why this issue fits your
profile, and what you hope to learn. If your scout's verdict on it
was reject and you chose it anyway, say so here and say why; choosing
against your own tool silently is what loses points. Close with an
honest read of the tool itself: where the scout fell short on a real
wild search and what you changed or would change, or what convinced
you it held up. **Record the link to your claim comment in this section too.**
Not claimed yet? Write `claim pending` and today's date where the link goes,
and add the link the moment your claim lands.]

- Claim comment: https://github.com/ArduPilot/ardupilot/issues/6322#issuecomment-6423029848
- Progress update (posted October 30): https://github.com/ArduPilot/ardupilot/issues/6322#issuecomment-6534217165

**Toolkit**

- Tool: issue-scout (your scout: your search strategy, your rubric),
  and how you used it
- What it produced: [the scan and its verdict on this issue, described]
- Difficulties: [where it fell short, or "none met" if none did]
- What changed: [what you patched or would update, or "nothing"]

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

I cloned ArduPilot and its submodules on my Intel Mac and used the
`ardupilot-sitl` Conda environment with Python 3.10.18. I configured
and built Copter SITL at commit `72b238201f`. My disk filled during
setup, so I removed old build artifacts and chose to remove the Windows
virtual machine to free space. I then completed the build.

**Steps to Reproduce**

1. I started Copter SITL and MAVProxy, set `MIS_RESTART` to `1`, and
   loaded a mission with a 50 m takeoff followed by a waypoint.
2. I took off in GUIDED mode to 10 m, switched to LOITER, moved forward,
   and then switched to AUTO while moving at approximately 4.5 m/s.
3. I inspected the recorded position and velocity around the AUTO switch.

The issue's original steps start the takeoff in Stabilize or Loiter.
My initial GUIDED takeoff is a difference in this attempt.

**Reproduction Evidence**

- **Commit showing reproduction:** No reproduction commit yet. The
  simulator reported baseline commit `72b238201f`.
- **Screenshots/logs:** I saved the telemetry as
  `~/ai301-backups/issue6322-flight-20261110-113517.tlog`.
- **My findings:** In this one run, the copter slowed after switching
  to AUTO and settled roughly 4.7 m north of the first recorded AUTO
  position. It briefly moved south by approximately 0.25 m from its
  furthest sampled position. I did not observe the substantial return
  described in the issue. The AUTO time is approximate because it
  comes from the first recorded heartbeat, and one run is not enough
  to establish that the issue is fixed.

**Toolkit**

- Tool: ArduPilot SITL and MAVProxy for the simulated flight, and
  `pymavlink` to inspect the saved telemetry.
- What it produced: A flight log with position, velocity, altitude,
  flight mode, and mission-item information.
- Difficulties: The terminal snapshots were too far apart to show what
  happened immediately after the AUTO switch. My initial takeoff also
  used GUIDED rather than the mode specified in the issue.
- What changed: I inspected the telemetry around the AUTO heartbeat.
  I would repeat the attempt with the original takeoff steps and compare
  the results before proposing a fix.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** Not opened yet.

**PR Description:** Not written yet. No PR exists, and there is no fix to describe yet.

**Maintainer Feedback:**

- None yet. No PR has been opened, so there is no PR feedback to record or address.

**Status:** Not opened. The branch `fix/6322-inflight-takeoff` has no changes against
`master` yet.

**Toolkit**

- Tool: ship-gate, the staff gate that wraps my existing pr-precheck. I changed its scope to
  real repositories and wrote a sourced ArduPilot manifest with three checks: configure SITL,
  build Copter, and run the TakeoffAlt and TakeoffCheck autotests. Each command uses the
  absolute Conda runner (`/opt/anaconda3/bin/conda run -n ardupilot-sitl ...`), so it selects
  the required environment without manual activation. I ran the gate by hand on
  `fix/6322-inflight-takeoff` against `master` on 2026-10-30.
- What it produced: all three checks passed (configure 7s, Copter build 1073s, takeoff
  autotests 212s), and then the gate stopped with: "the branch fix/6322-inflight-takeoff has
  no changes against master ... There is nothing to ship yet." This was an empty-diff stop,
  not a passing readiness verdict and not a rejection from my pr-precheck rubric. The existing
  takeoff tests passing does not show that #6322 is fixed; I have not made a fix. My first reproduction attempt did not show a substantial backup.
- Difficulties: the `gh pr create --help` canary was blocked by both voice-guard and
  ship-gate, but Claude at first showed only ship-gate's stop; the matching voice-guard and
  ship-gate log entries (both 2026-10-30 18:38) confirmed both had fired. The manifest leaves
  out ArduPilot's full Linux CI matrix, which I documented; that means reduced coverage, and
  full CI has not been verified. The gate also spent over 20 minutes building and testing
  before reporting that the branch was empty.
- What changed: no bypass or settings repair was needed. My next work is refining the reproduction, analyzing the cause, and writing a meaningful fix with a regression test. One
  possible gate improvement is to detect an empty branch before running builds and tests;
  this is a proposal, not an implemented change.

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
