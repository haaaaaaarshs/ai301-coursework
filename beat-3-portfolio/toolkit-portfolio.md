# Toolkit Portfolio

**Student:** Harsh Sabu

## Featured Tools

I chose these four tools because each helped me at a different stage of my open-source contribution. Issue Scout was recommended by Margaret, my course instructor, and helped me narrow my search for suitable issues while getting acquainted with the available tools. I used Voice Guard to double-check my communication and make sure my comments followed the course guidelines. I wanted to sound professional while keeping my writing authentic. Ship Gate helped me run ArduPilot-specific checks, while PR Monitor helped me organize my pull request follow-ups and understand the steps involved in maintaining a contribution after submission.

**Issue Scout**

**What it is, and the judgment hole I wrote:** I upgraded my Unit 1 issue-select skill to Issue Scout and wrote searches for Python API bugs, dataset issues, and C++ simulation bugs based on my interests in backend development, machine learning, and robotics.

**How it evolved, dated:** On October 18, 2026, I refined my availability checks after finding that unassigned issues could already have open pull requests. I also added a filter for issues marked fixed on the development branch and clarified that a project using AI does not automatically establish its policy on AI-assisted contributions.

**Evidence:** My first scan accepted one issue and rejected four. The results showed that my original rubric did not adequately account for issues already being addressed or fixed.

**Voice Guard**

**What it is, and the judgment hole I wrote:** I used Voice Guard to check GitHub drafts against my voice guide and the course's communication expectations. I wanted to communicate clearly and professionally with maintainers while keeping my own voice.

**How it evolved, dated:** By October 30, 2026, I had installed Voice Guard and used it to check a progress-update draft. It passed the draft, but another review caught a typo that it missed. I corrected the typo before posting. Later, I updated my voice guide to allow two follow-up messages rather than relying entirely on the monitor's default approach.

**Evidence:** My Unit 6 toolkit record states: "It passed my progress-update draft, but another review caught a typo that the guard missed, so I corrected the typo before posting."

**Ship Gate**

**What it is, and the judgment hole I wrote:** I used Ship Gate for project-specific checks on ArduPilot. I wrote a gate manifest requiring SITL configuration, a Copter build, and the TakeoffAlt and TakeoffCheck automated tests.

**How it evolved, dated:** On October 30, 2026, I installed Ship Gate around my existing PR precheck and adapted it to the ArduPilot repository. I tested whether the guards would stop an inappropriate command, and both Voice Guard and Ship Gate fired. The actual Ship Gate run passed its three checks but stopped because the branch contained no changes.

**Evidence:**

```text
Three checks passed:
- Configure SITL
- Build Copter
- Run TakeoffAlt and TakeoffCheck

Final stop:
"There is nothing to ship yet."
```

The empty-diff stop was not a passing submission verdict. My recorded improvement was to check for an empty branch before spending time on builds and tests.

**PR Monitor**

**What it is, and the judgment hole I wrote:** I used PR Monitor to track my pull request and help me understand the steps needed after opening one. It helped me approach maintainer communication and follow-ups more systematically. I found it the most useful tool because it supported the professional approach I wanted to maintain.

**How it evolved, dated:** On November 10, 2026, I installed PR Monitor and ran its free state check. It reported `NO PR YET`, so there was no PR activity for it to inspect. I examined five recently merged ArduPilot pull requests and wrote a playbook with follow-up checkpoints around days 10 and 24, along with a possible closing decision after approximately a month without a response.

**Evidence:**

```text
Initial monitor state: NO PR YET
Follow-up checkpoints: approximately day 10 and day 24
Playbook: all four sections filled
```

The monitor's built-in follow-up format allowed one polite reminder, while I wanted an option for a second carefully reviewed follow-up. I updated my voice guide to reflect that preference.

## Inventory

- **issue-select:** My original issue-selection skill. I later developed it into Issue Scout with additional availability and policy checks.
- **repro-check:** My Unit 2 tool for evaluating reproduction evidence. I refined its evidence guide after discovering that some legitimate cannot-reproduce reports were being rejected.
- **plan-check:** My Unit 3 tool for evaluating implementation plans. I adjusted its rubric to accept supported diagnoses that acknowledged uncertainty without claiming to prove an exact root cause.
- **pr-precheck:** My Unit 4 tool for checking PR readiness. I later used it alongside Ship Gate to make submission checks more specific to ArduPilot.

## The Arc

**Beat 1**

Date: October 18, 2026

Friction: My issue search showed that unassigned issues could still have open PRs or already be fixed. My initial rubric did not adequately handle these cases.

Change: I updated Issue Scout's availability checks, added the fixed-on-development filter, and clarified AI-policy verification.

Result: The updated approach gave me clearer criteria for evaluating candidates before choosing ArduPilot #6322.

**Beat 2**

Date: October 30, 2026

Friction: I needed a project-specific process for checking my ArduPilot work before submitting it.

Change: I installed Ship Gate, adapted its scope, and wrote a manifest for SITL configuration, building Copter, and running focused tests.

Result: All three checks passed, but the tool correctly stopped submission because the branch had an empty diff.

**Beat 3**

Date: November 10, 2026

Friction: I needed a clearer process for managing a pull request after opening it, including when to follow up with maintainers.

Change: I installed PR Monitor, examined recently merged ArduPilot PRs, and wrote a follow-up playbook.

Result: The initial state check returned `NO PR YET`, and the completed playbook gave me a structured approach for handling future PR activity.

## Limitations and What's Next

PR Monitor was the most useful tool for me because it helped me stay organized and approach the pull request process professionally. Ship Gate was more frustrating because its setup felt complicated, and its checks could be time-consuming even when there were no changes to submit.

The biggest lesson I learned was that these tools are most useful when they support my own judgment rather than replace it. I still need to review their results, verify the evidence, and make decisions based on the specific project's requirements.

If I continued developing the toolkit, I would improve Ship Gate's efficiency by checking for an empty diff before running expensive tests. I would also continue improving the writing and follow-up guidance in Voice Guard and PR Monitor.

**Tool limits**

issue-scout: An accepted result does not guarantee that an issue is still available or suitable. I still need to review the issue and its discussion.

voice-guard: It can miss typos and other writing problems, so I still need to proofread my comments.

ship-gate: It can spend time running builds and tests before discovering that there are no changes to submit.

pr-monitor: It can help organize follow-ups, but it cannot decide how a maintainer will respond or guarantee that a PR will be merged.
