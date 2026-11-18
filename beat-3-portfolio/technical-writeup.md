## Overview

My open-source contribution focused on a reported issue in ArduPilot, a project that develops software for vehicles including multicopters. In [issue #6322](https://github.com/ArduPilot/ardupilot/issues/6322), the reporter described a copter moving backward after switching to an AUTO mission while already flying forward. The mission began with a takeoff command, even though the vehicle was already airborne.

The issue suggested that the takeoff command could use the copter’s position at the moment AUTO began as its horizontal target. Because the copter was already moving, it could travel forward before slowing down and then return toward that earlier position.

My goal was to investigate the reported behavior in the simulator and add an automated test for that scenario. My [pull request](https://github.com/ArduPilot/ardupilot/pull/35129) adds the test; it does not change the flight-controller code or claim to fix the reported issue.

## What happened

I began by setting up ArduPilot’s software-in-the-loop simulator, or SITL, on my Mac. This allowed me to run a simulated copter and observe its behavior without flying a physical vehicle. One practical difficulty was making enough disk space available for the build and tests.

I tried to follow the reported scenario: configure `MIS_RESTART` to `1`, create a mission beginning with a takeoff to 50 meters, take off in a manual flight mode, move forward, and switch to AUTO. I did not reproduce the substantial backward return described in the issue during my initial attempts.

I then added `AutoTakeoffWhileMoving` to `Tools/autotest/arducopter.py`. The test creates a mission with a takeoff command followed by a waypoint. It takes off in LOITER, builds horizontal speed, and switches to AUTO. While the takeoff mission item is active, the test collects position messages and measures movement along the original direction of travel. It checks the largest return from the furthest point reached against a provisional 1.5-meter limit.

The test also checks that it collected enough position data and that there were no excessively large gaps between samples. Those checks matter because a result would be less useful if the test missed much of the copter’s movement.

After I opened the PR, maintainer Peter Barker [suggested trying a higher speed](https://github.com/ArduPilot/ardupilot/pull/35129#pullrequestreview-5910170832) to see whether the reported behavior could be reproduced. He also suggested simplifying parts of the test, including its cleanup and unnecessary delays. I revised the test to wait for a higher speed, removed some unnecessary code, and changed the cleanup to disarm and reboot the simulated vehicle.

In my latest local run of the revised test, the log showed:

```text
Initial horizontal speed: 10.17 m/s
AUTO samples span 18700 ms; max gap 50 ms
Maximum return during takeoff: 0.57 m
```

The maximum return is the largest measured decrease along the initial direction of travel from the furthest point reached during the observation. The result was below the test’s provisional limit.

The selected `TakeoffAlt`, `TakeoffCheck`, `AutoTakeoffWhileMoving`, and `AutoYawDO_MOUNT_CONTROL` tests passed in that run, and Flake8 passed for the changed file. An earlier, broader local test run had timeouts in `FETtecESC` and `CircleManualControlEntryLeft`; their cause was unknown.

I used Claude and ChatGPT/Codex to help with revisions and interpreting some results. I also ran the revised test and checks locally and reviewed the output.

## Limitations and what's next

Although the test passed, I have not reproduced the larger return described in the original issue. A passing test under these conditions does **not** prove the issue is fixed, and I have not established that this test would fail on a version exhibiting the reported behavior. The 1.5-meter limit is provisional and may need maintainer feedback.

The PR remains open and under review. Another maintainer asked about the purpose of the test, so I [clarified the already-airborne scenario and shared the latest result](https://github.com/ArduPilot/ardupilot/pull/35129#issuecomment-6932139778).

My next steps are to continue discussing whether the test is useful, review the remaining feedback, and investigate which conditions or versions might reproduce the original report. This contribution taught me that a passing test is evidence about the conditions tested, and that sharing what I could **not** reproduce is just as important as reporting what passed.

