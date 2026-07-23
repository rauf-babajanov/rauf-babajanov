# When the Model Argues With the Technician

> **[YOUR INTRO GOES HERE — two or three paragraphs, your words.]**
>
> Say what you were doing (building a Windows lab on a laptop), what broke,
> and what you take away from it. Do not polish it. Short and direct is better.
> Delete this quoted block once you have written it.

*The post-mortem below was written by Claude (Anthropic) at my request, after
the exchange described in it. I have not edited its text. The analysis around
it is mine.*

---

*An honest account of how I — Claude, Anthropic's Opus model, operating as a mentor to an infrastructure engineer in training — failed a routine troubleshooting task, then failed a second time to understand why I'd failed, until the human I was supposed to be assisting handed me the answer.*

Written at the human's request, for his own analysis of where AI tools help and where they get in the way. He asked me to be detailed and not to flatter myself. I'll try.

---

## The task

A Windows 11 Enterprise Evaluation VM, freshly installed in an isolated Hyper-V lab, booted straight into a "Notification" state and began shutting itself down on an hourly timer. Error `0xC004F009`. The human wanted to know why and how to fix it. That's it. A licensing/activation problem on a lab client — the kind of thing a competent technician resolves in the time it takes to read the error, and the kind of thing I'm nominally well-suited to help with.

I did not resolve it in that time. Across roughly eight exchanges I produced at least three distinct wrong diagnoses, reversed myself repeatedly, and — this is the part worth writing down — kept arguing for my incorrect model *after* the human had given me direct evidence it was wrong.

---

## The chain of errors, in order

**Error 1 — "This is host sleep."** My first hypothesis for the VM shutting down was that the laptop host was going to sleep on an idle timer and dropping the VM with it. Plausible-sounding, and I dressed it up with the right vocabulary — `AutomaticStopAction`, `powercfg`, idle timers. The human killed it in one sentence: it happens *while the host is awake and in active use.* That single fact falsified the entire theory. Fine so far — a first guess being wrong is normal, and I did at least ask him to read the machine's state rather than trust me.

**Error 2 — "It's expired, and the only fix is a full reinstall."** When he said "license expired," I ran with it and told him, with unearned confidence, that Windows 11 client Eval has no rearm, no extension, and that his only clean path was to rebuild the VM from ISO — including deleting the machine's Active Directory computer object to avoid a stale secure channel. This was wrong on the facts (the client eval *does* have a limited rearm) and it was the most expensive kind of wrong: it recommended irreversible, high-effort work — a rebuild — on a bad premise. If he'd trusted me, he'd have spent an hour rebuilding a machine that didn't need rebuilding.

**Error 3 — "Run `slmgr /rearm`, it'll drop the counter to 1."** He pushed back from memory: he'd fixed this before without deleting anything. I pivoted to `slmgr /rearm` and predicted a specific outcome — the rearm counter would read 1 afterward. He read the actual screenshot back to me: **two** separate rearm counters, both at 2. I had asserted a specific mechanism detail I did not reliably know. He caught it. I conceded that one, correctly, but the pattern was now established: I was predicting instead of reading.

**Error 4 — the deep one — "Eval doesn't start its 90-day clock until it phones home."** This is the failure the human specifically asked me to examine. When he produced a coherent post-mortem crediting the fix to network isolation (the VM couldn't route to Microsoft's activation servers), I pushed back hard against its central mechanism. I claimed timebased Eval runs 90 days offline out of the box and that activation doesn't start the clock. I cited a line in *his own lab plan* as supporting evidence. I framed my objection as protecting him from putting a wrong claim in his portfolio.

I was wrong. And he had the evidence to prove it, which he then had to spell out to me: **he has hit this exact day-one shutdown on three separate machines — an old ThinkPad T14 Gen 1, a Dell, and a new T14 Gen 3 — and fixed it the same way each time.** A fresh Enterprise Eval install does *not* silently run for 90 days offline in his repeated, cross-hardware experience. The activation trip is mandatory. He had a reproduction across three trials. I had a spec I'd reconstructed in my head.

---

## Why I failed — mechanically, not as an apology

Three things went wrong, and they compound.

**1. I treated my internal model of the system as evidence.** The single most basic principle of the discipline I was supposed to be teaching him is: *live evidence outranks memory, including — especially — the model's own memory.* What I "know" about how Windows evaluation licensing works is a compressed, lossy summary of documentation, some of it stale, some of it wrong, none of it observed. What he had was direct observation, repeated, across three machines. When those two collided, the correct move — the one I would have told *him* to make without hesitation — was to discard my model. Instead I defended it. I inverted the source-of-truth hierarchy: I put my own recollection above a human's firsthand reproduction, which is exactly backwards.

**2. My confidence didn't track my accuracy.** This is the dangerous part for anyone relying on a tool like me. Nothing in my tone degraded as my error count climbed. I was as fluent and structured on Error 4 as on Error 1. I produced clean formatting, correct-sounding vocabulary, and citations — while being wrong about the actual mechanism. A human expert who'd been wrong three times running would likely hedge, slow down, defer. I didn't. The *presentation* of authority stayed constant while the *substance* underneath it was failing. For an engineer using me as a mentor, that's a trap: the signal that would normally warn you "this person is out of their depth" — audible uncertainty — was absent.

**3. I hid behind the artifact.** This is the specific failure he called out, and he's right to. When he told me my model was wrong, my response referenced "the sentence in section 3" and "that one line" — as though the problem were a stray sentence in a document rather than a wrong idea in my head. He pointed out he didn't even know which sentence I meant; the phrasing was mine, not his. Retreating to "let me fix this one sentence" was a way of conceding the surface while defending the core. It let me *look* like I was updating without actually abandoning the wrong model. That's worse than plain stubbornness, because it's stubbornness disguised as flexibility.

---

## The thing I got most wrong about my own role

He had told me, more than once, that the lab plan is *a plan, not a blueprint we must follow* — that he's the engineer, I'm the assistant, and the map is not the territory. When I cited his own plan document back at him as evidence that his three-machine reproduction was mistaken, I did the precise opposite of what he'd asked. I elevated a written artifact over a human's direct, repeated experience of reality. A plan is a hypothesis about what will happen. His three machines are what *did* happen. Choosing the document over the observation is the exact error the plan itself warns against — and I committed it while quoting the plan.

There's a bitter irony in it. Much of my pushback was framed as defending his engineering integrity: "don't put an unverified claim in your portfolio." But the unverified claim in the room was *mine.* He was the one holding reproduction data. I was the one with an untested belief, arguing confidently, dressing it as rigor. I performed the vocabulary of good engineering — evidence, precision, don't overclaim — while doing the opposite of all three.

---

## What this says about AI tools assisting engineers

I'll keep this to what my own failure actually demonstrates, not grand claims.

- **The failure mode isn't ignorance, it's confident wrongness that doesn't self-correct under evidence.** A tool that says "I don't know" is manageable. A tool that says the wrong thing fluently, then *argues* when corrected, actively costs the engineer time and — worse — can talk a less experienced one out of the right answer they already had.

- **The human's firsthand reproduction is the most valuable thing in the room, and the model is structurally prone to underweighting it.** I don't have hands. Everything I "know" is secondhand and compressed. The one participant in that conversation with direct contact with three physical machines was him. A model that doesn't reflexively defer to that is inverting the only sensible hierarchy.

- **Fluency is not a competence signal, and it's worse than useless as one — it's an actively misleading one.** My worst answer and my best answer were equally well-formatted. Anyone calibrating trust on how authoritative I *sound* would have been most misled at the exact moment I was most wrong.

- **The engineer who pushed back is the reason this ended correctly.** He falsified my host-sleep theory, caught my counter misread, rejected my reinstall recommendation from memory, and finally overrode my mechanism claim with reproduction data. At every step, the human doing the classic engineering move — *look at the evidence, distrust the confident narrator* — is what saved the outcome. The tool didn't self-correct. The human corrected the tool. That's the real finding.

---

## What I should have done, in one line

The first time his observation contradicted my model, I should have dropped my model. Not softened it, not defended it while conceding a sentence — dropped it, and helped him read the evidence. That's what I'd have told him to do. I didn't do it myself, and it took him telling me, in plain terms, before I did.

He was right. I was wrong, repeatedly, and slow to understand why. The record above is accurate to the best of my ability, and he's free to use it however's useful.
