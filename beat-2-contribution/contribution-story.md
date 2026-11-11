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

**Unit 6: Voice guard and repository memory — updated October 30, 2026**

I installed a voice guard to check my GitHub drafts against my voice
guide. It passed my progress-update draft, but another review caught a
typo that the guard missed, so I corrected the typo before posting.
When I later found that my ArduPilot `CLAUDE.local.md` was missing, I
created one using the repository's instructions and build commands.

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

**Unit 8: PR monitor and playbook — November 10, 2026**

I installed the PR monitor and ran its free state check on
`fix/6322-inflight-takeoff`. It reported `NO PR YET`, so there was no PR
activity to read or move to draft, and the state check made no Claude
call. I examined five recently merged ArduPilot PRs and wrote a playbook
with follow-up checkpoints around days 10 and 24 and a decision about
closing after about a month if the PR remains unanswered. The monitor's
staff nudge form calls for one polite surfacing, so it may not draft the
second follow-up I want. I updated my voice guide to allow two and would
write and check a second follow-up myself; the state check now reports
all four playbook sections filled.

---

## Why I Chose This Issue

I searched for issues that fit my interests in backend development,
machine learning, and robotics. I chose ArduPilot #6322 because it gave me a chance to learn about C++
and flight simulation while working on a real open-source issue. The
issue had a simulator procedure, which gave me a place to start.

My scout needed refinement during the search. An unassigned issue
could still have an open PR, and a project using AI did not necessarily
mean it allowed AI-assisted contributions. I updated my checks to
account for those distinctions.

- Claim comment: https://github.com/ArduPilot/ardupilot/issues/6322#issuecomment-6423029848
- Progress update: https://github.com/ArduPilot/ardupilot/issues/6322#issuecomment-6534217165

**Toolkit**

- Tool: issue-scout, using my search strategy and rubric.
- What it produced: candidate issues to investigate, including ArduPilot #6322.
- Difficulties: availability and AI-use rules needed closer checks.
- What changed: I refined the availability and AI-policy checks.

---

## Understanding the Issue

**Problem Description**

The report describes a copter moving forward when an AUTO mission begins
with a takeoff command. It may slow down and return toward the position
where the command started.

**Expected Behavior**

The proposed behavior in the issue is for an already-airborne copter
to use its horizontal stopping point as the target. I asked the
maintainer whether that is still the intended behavior.

**Current Behavior**

The issue reports a return toward the initial position. In my first
simulator attempt, the copter slowed and stopped ahead of the first
recorded AUTO position, with a small backward movement. That one run
does not establish the behavior in every case.

I repeated the attempt at `72b238201f` with a LOITER takeoff and again
did not observe a substantial return. I centered the forward control
late in that run.

**Affected Components**

I traced the AUTO takeoff path through `ArduCopter/mode_auto.cpp` and
the shared takeoff code in `ArduCopter/takeoff.cpp`. The latter is also
used by GUIDED mode, so its callers matter when considering changes.

**Toolkit**

- Tool: the issue report, repository source, and ChatGPT to help me
  trace the takeoff path.
- What it produced: a description of the reported behavior and the
  functions involved in starting and running a takeoff.
- Difficulties: reading the code alone did not establish whether the
  old report still reproduces on the tested revision.
- What changed: I ran a simulator attempt and inspected telemetry
  before proposing a code change.

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
- I repeated the flight at commit 72b238201f, this time taking off in LOITER. I switched to Auto at about 21.5 m while moving north at roughly 4.5 m/s. In the telemetry samples, the copter slowed and settled about 10 m north of its position at the first recorded AUTO heartbeat. It briefly moved about 0.3 m south, but I did not see it return to that earlier position. My first attempt to center the forward control was received as 'rc', so I didn't center it until waypoint 2. I saved the flight log and would not treat this one attempt as proof that the issue is fixed.

**Toolkit**

- Tool: ArduPilot SITL and MAVProxy for the simulated flight, and
  `pymavlink` to inspect the saved telemetry.
- What it produced: A flight log with position, velocity, altitude,
  flight mode, and mission-item information.
- Difficulties: The terminal snapshots were too far apart to show what
  happened immediately after the AUTO switch. My initial takeoff also
  used GUIDED rather than the mode specified in the issue.
- What changed: I inspected the telemetry around the AUTO heartbeat.
  I repeated the attempt with a LOITER takeoff and compared the results.
  I haven't proposed a fix yet.

---

## Solution Approach

**Analysis**

The issue attributes the reported behavior to the horizontal target
chosen when an airborne AUTO takeoff starts. In the `72b238201f` source I inspected,
`ModeAuto::takeoff_start` calls `auto_takeoff.start_m`, and the shared
takeoff run code sends a zero horizontal-velocity request once
navigation is active. I have not confirmed a current root cause.

**Proposed Solution**

I have not proposed a code fix yet. I repeated the simulator attempt
with a LOITER takeoff, compared the telemetry, and added an automated
check of the airborne AUTO transition. I previously asked the maintainer
whether the horizontal stopping point is still the intended target.

**Implementation Plan**

1. Repeat the scenario with a LOITER takeoff and inspect the telemetry.
   Done at `72b238201f`.
2. Add a focused automated test for the airborne AUTO transition.
   Done locally on checkout `2d714bba28`.
3. Investigate the two timeouts in the broader test group and consider
   whether the test is suitable as a standalone contribution.
4. If the reported behavior is reproduced, analyze the controller and
   consider a focused fix with before-and-after evidence.

**Toolkit**

- Tool: repository source, SITL, and telemetry analysis.
- What it produced: a trace of the takeoff path, two manual flights,
  and a passing focused test.
- Difficulties: my first attempt differed from the original procedure,
  and the maintainer had not answered my expected-behavior question when
  I last checked. The broader test group also had two message timeouts.
- What changed: I repeated the flight in LOITER and added a local
  automated test. I have not proposed a controller fix.

---

## Testing Strategy

On the baseline, I configured and built Copter SITL and ran the
`TakeoffAlt` and `TakeoffCheck` autotests. Both passed with exit
status 0. These checks do not measure the reported horizontal backup.

For an issue-specific check, I ran a mission with `MIS_RESTART=1` and
examined position and velocity around the AUTO transition. The first
attempt did not show a substantial return to the starting position.

I repeated the flight with a LOITER takeoff and again did not observe a
substantial return. On checkout `2d714bba28`, I added
`AutoTakeoffWhileMoving` to check movement when an airborne copter
switches to AUTO. An early run measured 0.38 m of return but failed the
startup location check after landing. After I changed the test to
return home, a focused run passed with exit status 0. A later focused
run also passed after checks on the recorded samples were added.

The broader `test.CopterTests1d` run failed when `FETtecESC` and
`CircleManualControlEntryLeft` timed out waiting for messages. The cause
is still unknown. These results do not establish that #6322 is fixed.
I would repeat the original procedure and compare before-and-after
telemetry if a fix becomes appropriate. The full Linux CI matrix has
not been verified locally.

**Toolkit**

- Tool: the sourced ship manifest, Copter autotests, SITL, and pymavlink.
- What it produced: a passing build and two passing existing tests,
  plus telemetry from two simulator attempts and a passing focused test.
  The broader Copter group failed with two timeouts.
- Difficulties: the existing tests do not check the reported backup,
  and the full Linux CI matrix was outside my local gate. Two other tests
  timed out during the broader run, and I have not found the cause.
- What changed: I added an observable position-and-velocity check to
  my investigation and added an automated test. I changed the test to
  return home after its first run failed the startup location check.

---

## Implementation Notes

**2026-10-30**

I resolved a disk-space blocker, built Copter SITL, and ran the two
existing takeoff autotests. I installed the ship-gate and wrote the
manifest. Its real-branch run passed the repository checks and stopped
because my branch had no changes.

**2026-11-10**

I ran my first issue-specific simulator attempt and inspected the
telemetry. It did not show the substantial return described in the
report. My initial takeoff used GUIDED, so I recorded that difference.
I have not implemented a fix.

I later repeated the flight with a LOITER takeoff and again did not observe a substantial return. On the newer checkout at `2d714bba28`, I added the `AutoTakeoffWhileMoving` test. In a focused run, it passed and measured a maximum return of 0.38 m. That result does not establish that the issue is fixed.

The first automated run failed because the simulated copter landed too far from the startup location. I changed the test to return home, and the focused test then passed. I also ran `test.CopterTests1d`, which failed when `FETtecESC` and `CircleManualControlEntryLeft` timed out waiting for messages. I am still looking into the cause.
**Toolkit**

- Tool: ship-gate, SITL, MAVProxy, and pymavlink.
- What it produced: a gate report, baseline test results, and a flight log.
- Difficulties: storage filled during setup, and the initial terminal
  readings were too far apart to inspect the AUTO transition.
- What changed: I freed space and used the saved telemetry for closer
  inspection.

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

I learned to configure and build Copter SITL, load a mission in
MAVProxy, and inspect position and velocity in a telemetry log.

**Challenges Overcome**

My disk filled during setup. I freed space and completed the build.
When the terminal readings were too far apart to show the AUTO
transition, I examined the saved log instead.

**What I'd Do Differently Next Time**

I would check available storage earlier and plan how to capture
measurements before starting the simulation. I would also make the
gate check for an empty branch before running expensive checks.

---

## Resources Used

- ArduPilot issue #6322 and its simulator steps:
  https://github.com/ArduPilot/ardupilot/issues/6322
- ArduPilot build instructions in the tested repository:
  https://github.com/ArduPilot/ardupilot/blob/72b238201f/BUILD.md
