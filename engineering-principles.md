# 45 Engineering Principles — How to Learn, How to Build

I'm an infrastructure engineer in training, building a Windows domain lab from nothing and writing down what I learn. This file is the part that isn't about Windows.

**What this is:** the standard I hold myself to. Not a plan, not a syllabus, not a checklist. These apply to a lab, a certification, a codebase, or a job you take in five years.

**How to use it:** when a plan and this file disagree, this file is probably right and the plan is probably tired. When you're stuck, the answer is usually a principle you skipped.

**The test for anything in this file:** if it names a tool, a vendor, a certification, or a date, it doesn't belong here. Principles outlive all of those.

**Contents**

- [0. The one thing](#0-the-one-thing)
- [I. How you learn](#i-how-you-learn) — P01–P12
- [II. How you build](#ii-how-you-build) — P13–P26
- [III. How you fail](#iii-how-you-fail) — P27–P33
- [IV. How you decide](#iv-how-you-decide) — P34–P40
- [V. How you prove it](#v-how-you-prove-it) — P41–P45
- [The short version](#the-short-version)

---

## 0. The one thing

```text
The artifact is the excuse. The engineer is the deliverable.
```

Every lab, exam, ticket, and project is a machine for producing a person who can do the work. Finish everything and understand nothing and you have built nothing.

This is the principle the other forty-four serve. When in doubt, ask which choice produces the better engineer, not the faster result.

---

## I. How you learn

**P01 — Understanding is the deliverable; completion is the receipt.**
"Done" and "understood" are different states. You can have the first without the second and not notice for months — until someone asks you a question and the gap opens under you. Track the second.

**P02 — Evidence beats confidence. Always. Yours included.**
What you believe about the system is a hypothesis. Output, logs, and state are facts. The gap between them is where every expensive mistake lives. When you catch yourself saying "it should be" — stop and go look.

**P03 — Source of truth has an order.**

```text
1. Live evidence — output, logs, actual state
2. Current official documentation
3. Your own written notes and decisions
4. Memory — yours or anyone's
```

Memory tells you where to look. It does not tell you what is true. Anything you "remember" about a system is a rumour until step 1 confirms it.

**P04 — Know normal before you diagnose abnormal.**
Capture the baseline while nothing is wrong. "It feels slow" is a feeling. A number next to an older number is an observation. You cannot recognise a deviation from a state you never measured.

**P05 — Learn chains, not steps.**
A sequence of steps is a recipe and it rots. A dependency chain — *this needs that, which needs the other* — is a tool you keep. Anyone can follow the recipe. The person who knows the chain can diagnose it when a link breaks, which is the only time anyone calls you.

**P06 — Go one layer down.**
Running the command is a technician. Knowing what the command sets in motion is an engineer. When you ask "why," the honest answer moves *down* a layer — into the mechanism — not sideways into a different abstraction. The layer below is where the failures actually happen.

**P07 — By hand first, then automate.**
You cannot automate what you don't understand. You'll only automate the misunderstanding, and now it runs at scale and looks authoritative. Do it manually until it's boring. Boring is the signal that you've earned the shortcut.

**P08 — Not everything deserves memory. Decide which.**
Treating everything as equally important means nothing sticks. Triage every fact:

- **Memorise** — you'll use it weekly; it's diagnostic; looking it up in front of someone would be embarrassing.
- **Note** — you need to know it exists and when to reach for it. Look up the syntax forever, without guilt.
- **Copy** — ceremony. Long paths, identifiers, one-time setup. Paste it and forget it.

Deciding the tier is the work. Most people skip it and then wonder why nothing lands.

**P09 — Recall from nothing, not recognition.**
Reading your notes and nodding is recognition, and recognition is a liar — it feels exactly like knowledge and isn't. Close the notes and reproduce it from a blank page. That's the only honest test.

**P10 — Sleep is part of the method.**
Memory consolidates during sleep, not during the session. The same hours spread across more days are worth more than the same hours crammed. Passing a test at 9pm on material you learned at 7pm proves nothing about November. Volume is not the constraint. Spacing is.

**P11 — Confusion is a signal, not a failure.**
The moment something stops making sense is the moment you've found the edge of what you actually know. That's valuable. Push there. The instinct to move on and "come back to it" is the instinct to leave a hole in the foundation.

**P12 — If you can't explain it, you don't have it.**
Not "explain it with the notes open." Explain it to someone who wasn't there, out loud, in your own words, including *why it's built that way and what breaks if it isn't.* The moment you find yourself reciting instead of explaining, you've found what you don't know.

---

## II. How you build

**P13 — Model the dependencies before you act.**
What talks to what, over what path, as whom, with what rights, relying on what else? Draw it if you have to. Every action you take lands somewhere in that graph, and most bad surprises are edges you didn't know existed.

**P14 — Blast radius first.**
Before any change: *if this is wrong, what breaks, and how far does it travel?* A cosmetic setting and an identity setting are not the same act, even though both are two clicks. Say the radius out loud before you commit — the sentence itself catches the mistake surprisingly often.

**P15 — Rollback exists before the change, or the change doesn't happen.**
A snapshot, an export, a backup, or a written way back — created *before*, not improvised after. "I'll just redo it" is not a rollback plan; it's optimism with a deadline.

**P16 — An untested backup is a belief.**
Restore it once, deliberately, while a mistake is still free. Everyone intends to test recovery later. Later is during the outage, in front of people, at 2am. That's a rehearsal you want to have already had.

**P17 — Small changes, proven one at a time.**
Change one thing. Prove the effect. Continue. Two changes at once and a broken system gives you no information — you've destroyed the only variable that could have told you anything. This feels slow and is dramatically faster.

**P18 — Reversibility is a feature. Weight it.**
Between two options, the reversible one is worth more than its benefits suggest, and the irreversible one costs more than its risks suggest. You will be wrong sometimes. Buy yourself the right to be wrong cheaply.

**P19 — Safe to run twice, or it's broken.**
Anything that runs must survive running again — check before you create, tolerate what already exists, fail loudly on the unexpected. A thing that only works on a clean system only works once. That isn't a tool; it's a stunt.

**P20 — See it before you fix it.**
The inspection comes before the mutation. You cannot manage what you cannot see, and you cannot fix what you haven't measured. Build the thing that tells you the state before you build the thing that changes it — because you'll reach for it when everything is already on fire, and that's the worst possible moment to be adding new variables.

**P21 — Repetitive manual work is a bug, not diligence.**
Doing it by hand the fifth time isn't discipline; it's an unlogged defect. Name it. Then choose deliberately: automate it, or accept it with a stated reason. Both are fine. Not noticing is not.

**P22 — Standardise before you automate.**
Automation makes a good process faster and a broken process faster *and harder to see*. If the manual version is inconsistent, you're about to industrialise the inconsistency.

**P23 — Boring is a feature.**
Well-understood, widely-used, heavily-documented. Novelty is paid for in incidents, at the worst time, by you. Choose the dull option and spend your attention on the problem you're actually being paid to solve.

**P24 — Least privilege, and separate identities by risk.**
The rights needed, where needed, for as long as needed. Admin identities, daily identities, and service identities carry different blast radii and should never be the same object. Collapsing them is always convenient and always the thing you have to explain afterwards.

**P25 — Visibility is a control.**
If you can't see it — a device, a change, a failure, a cost — you aren't managing it, you're hoping about it. Absence of alarm is not evidence of health.

**P26 — Cost and expiry are architecture.**
Trials end. Licences lapse. Bills arrive. Anything with a clock is a design constraint, not an administrative detail. Write the date down the day it starts, not the week it bites.

---

## III. How you fail

Failure is the only part of this that can't be simulated by reading. Treat it as curriculum, not interruption.

**P27 — Troubleshoot in layers, bottom-up, every time.**
Start at the bottom of the stack and walk up. Always the same order, even when you're sure. Most wasted hours come from starting in the middle because the symptom *sounded* like it came from the top. Symptoms lie about their origin. The layer order doesn't.

**P28 — Symptoms lie. The user's diagnosis is data, not a conclusion.**
"The network is down" means something is wrong and the reporter has a theory. Take the observation seriously and the theory lightly.

**P29 — Write down being wrong.**
Every real failure gets a record, and the most valuable line in it is *what you thought was wrong before you knew.* Nobody writes that down, which is exactly why nobody learns from it. Right answers teach you very little; the rejected hypothesis is the whole lesson.

**P30 — A postmortem without a rejected hypothesis is fiction.**
If you were right first time on every guess, you already knew the answer — you were documenting, not diagnosing. Real diagnosis has wrong turns in it. A record with none is a changelog wearing a lab coat.

**P31 — Break it on purpose, while it's cheap.**
Deliberately induce the failure before reality does. You get the recognition pattern without the outage, the audience, or the cost. This is the highest-value hour available to you and it is always the first one people skip.

**P32 — Diagnose honestly, even when you planted it.**
When you broke it yourself, the temptation is to "fix" it from memory. That teaches nothing. Work the symptom from the bottom of the stack as though you'd walked in cold. The point isn't the fix — it's the path to the fix.

**P33 — One incident properly understood beats ten skimmed.**
Depth compounds. Breadth without depth evaporates.

---

## IV. How you decide

**P34 — One thing at a time.**
One active piece of work. Everything else is closed. Parallel tracks feel productive and finish nothing — the cost isn't the time, it's that no single thread ever gets deep enough to matter.

**P35 — Backlog is not rejection. Write it down and move on.**
Every good idea that isn't *this* thing gets one line somewhere permanent, and then gets out of your head. This is how you keep good ideas without letting them eat the work. The list is a service to your future self, not a graveyard.

**P36 — A tangent needs all three, not two.**

1. It unblocks the current work.
2. It's relevant to where you're actually going.
3. It can be done without delaying the main path.

Two out of three is a no. The default answer is *not now.*

**P37 — Don't fix a stall by redesigning the plan.**
Starting over *feels* like progress. It's the most expensive habit in this business, and it's seductive precisely because it produces the sensation of work without the risk of failure. A stall is a blocker with a name: write down what's stuck, what you tried, what the evidence says, and the smallest next action. Then take it.

**P38 — Perfectionism is procrastination in a good suit.**
Polishing the plan, restructuring the notes, researching the tooling — these are all ways of not doing the thing while feeling responsible. Notice the tell: it's the work that has no failure mode.

**P39 — A minimum session still counts.**
Twenty-five minutes, one action, one piece of evidence, one line written down. Momentum survives that. Momentum does not survive waiting for the perfect three-hour block, because it never arrives.

**P40 — Decide on paper before you build.**
Anything structural gets written down *before* it exists: what you chose, why, what you rejected, and how you'll know you were right. Written before, this forces the trade-off into the open. Written after, it's a press release. The rejected option is the most important line — it's the only evidence you actually made a choice.

---

## V. How you prove it

**P41 — Prove it, don't claim it.**
Every sentence you say about your own work maps to something someone could look at. Adjectives are free and everyone has them. Evidence is the whole difference.

**P42 — History is evidence.**
A record of small, deliberate, well-reasoned changes is worth more than the finished artifact. The artifact shows you got there. The history shows how you think — which is the thing anyone is actually trying to find out.

**P43 — The reason matters more than the change.**
"Fixed it" is worthless. *What was broken, why, what you changed, and how you know it worked* is a story someone can trust. Write the reason down at the moment you know it, because you will not remember it later and you'll never write it as well.

**P44 — Claim precisely, and only what exists.**
Say what you built, what you proved, and what you didn't. Overclaiming isn't just dishonest — it's fragile, and it collapses under exactly one competent follow-up question. Precision is the credible thing. "I hit this constraint, made this compromise deliberately, and here's why" is a stronger sentence than any success story.

**P45 — Sanitise before you share.**
Secrets, identifiers, personal data, anything private. Once it's published, assume it's permanent. Check before, not after — there is no after.

---

## The short version

```text
Understand before you finish.
Look before you believe.
Model it, then touch it.
Know the way back before you go.
Small changes, proven.
Break it while it's cheap.
Write down being wrong.
One thing at a time.
Prove it, don't claim it.
```

If you forget the rest:

> **The artifact is the excuse. The engineer is the deliverable.**
