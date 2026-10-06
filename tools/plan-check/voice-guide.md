# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm a student contributor learning to work in larger open source codebases. When I comment on an issue, I'm documenting what I actually tested and observed rather than presenting myself as an expert on the project.

Readers can expect me to be specific about what I tried, provide evidence for my conclusions, and be clear when I am still investigating or unsure.


<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: Say what I will actually do

When claiming an issue, I describe the investigation or reproduction I am taking on. I do not promise a fix, outcome, or deadline before I understand the problem.

- Wrong: "I'll get this fixed and submit a PR by tomorrow."
- Right: "I'd like to investigate this issue. I'll set up the project, try to reproduce the reported behavior, and report back with what I find."

### Rule: Separate observation from explanation

I say what my test actually showed before suggesting why it happened. I do not present a suspected cause as something I proved.

- Wrong: "I reproduced this and confirmed the parser is causing the bug."
- Right: "I reproduced the reported behavior and saw the same error during parsing. I haven't confirmed the underlying cause yet."

### Rule: Include specifics instead of filler

I name the relevant action, behavior, or result instead of posting a generic message that could belong to any issue.

- Wrong: "I tested this and can confirm the issue."
- Right: "I reproduced the issue by following the reported steps, and the command returned the same error described in the issue."

### Rule: Be explicit when reproduction fails

If I cannot reproduce something, I say that directly and report what happened instead. I do not turn an unsuccessful reproduction into a confident conclusion about the issue.

- Wrong: "The issue seems to be fixed now."
- Right: "I wasn't able to reproduce the reported behavior in this environment. After following the steps above, I observed the expected output instead."

### Rule: Do not overstate certainty

I keep my conclusion within what my evidence demonstrates and leave unresolved questions unresolved.

- Wrong: "This definitely proves the bug only happens on this version."
- Right: "I observed the behavior on this version; I haven't tested enough other versions to determine whether it is version-specific."

## Things I never post

- A promise that I will fix an issue before I have investigated it.
- A deadline I have not committed to and know I can meet.
- A claim that I found the root cause when I only reproduced the symptom.
- "Same here" or "can confirm" without my own reproduction details.
- A confident conclusion that goes beyond the evidence I collected.
- Boilerplate that could have been posted unchanged on a different issue.

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->
