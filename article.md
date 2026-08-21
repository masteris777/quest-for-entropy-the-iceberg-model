# The Iceberg Model

*Quest for Entropy #13: what if the quantum weirdness is just what a big distributed system looks like from the inside?*

![hero](assets/hero.jpg)

## The question

In the last episode I let one line slip: I have been building *a compute model of the universe with a blockchain in its heart*. That was a tease. This is the reveal.

Here is the question that has been driving this whole series. Quantum mechanics works — better than anything we have ever built. But every explanation of it sounds like a magic show. Particles that act on each other across the galaxy, instantly. A cat that is alive and dead at once. A wave function of the entire universe, so large no memory could hold it. Reality that isn't decided until somebody looks. We calculate with all of this every day, and we still describe it with words like *spooky*.

I'm a software architect, and I kept noticing something. Every one of those sentences assumes the same hidden thing: a master view. A screen somewhere showing "the real state of the universe, right now." Take that screen away — really away, not just hidden — and ask the question again. What does a huge distributed system look like to a small observer who lives *inside* it, is made of its records, and has no place to stand outside?

That question turned into a model. The model now has a name, a first version, a paper, and a public test harness. This episode introduces all four.

## The toy

The model is called **the Iceberg Model**, and the name is the architecture. An iceberg has a small visible tip and a huge hidden bulk. So does this universe.

**The tip — the visible layer.** Everything we call physics happens here: space, time, motion, gravity. The idea underneath it is one sentence: *the universe runs on a fixed budget of compute.* Nothing in the model has geometry installed. There is no rule that says "mass curves space" and no rule that says "moving clocks run slow." There is only a grid of simple nodes doing bookkeeping, and the bookkeeping has a price. A region crowded with records computes slower. And from that one fact, the familiar furniture emerges on its own: clocks in busy regions tick slower, waves bend toward the slow regions — that bending *is* falling — and as the total record grows and everything smoothly slows together, an insider sees distances stretch. Gravity is congestion. Expansion is the storage bill. This is the model's central idea, and the one I'd defend the longest.

**The bulk — the hidden layer.** Below the waterline sits the state: who is correlated with whom, which possibilities are still open, what has been decided. The model keeps that state the way a distributed ledger would — append-only records, no central database, and one rule: *a fact is a transaction co-signed by two parties.* Entangled particles are parties to one shared contract. Measurement is settling the contract. Copying an unknown state is a double-spend, and the ledger forbids it by construction.

Let me fence this immediately, because it matters: **the universe is obviously not a blockchain.** I am not claiming there are miners in the vacuum. The claim is narrower and, I think, more interesting: blockchain mechanics turn out to *approximate the hidden layer of quantum behaviour remarkably well* — well enough that puzzle after puzzle stops being a puzzle when you translate it into ledger language. The ledger is an approximation instrument. It earns its place by what it demystifies.

The two layers are joined by a single coupling, and it is the whole model in five words: **every write costs budget.** Recording things down in the hidden layer is what loads the visible layer. One iceberg, not two.

My favourite way to hold the whole thing: picture a company with thousands of offices and *no head office*. No central database, no master dashboard. Just one ledger book per office, and facts that only exist when two offices co-sign them. Now try to ask "what is the company's state right now?" — and watch the question fall apart. Payments are always in flight. Different auditors drawing different "as of" lines get different snapshots, and all of them agree on every posted transaction. Most of the famous quantum paradoxes, I'll argue below, are questions addressed to the head office. There is no head office.

## Solaris

The Iceberg Model is an architecture, not a finished machine — the way "Transformer" is an architecture and a particular trained model is an instance of it. So the first concrete instance gets its own name: **Solaris**.

The name is from Stanisław Lem's novel — the planet-wide thinking ocean. I picked it deliberately. In this model the compute grid *is* an ocean: matter is not little balls sitting on it, matter is *waves in it* — and one of my favourite measured results in the harness is that only waves fall; a point particle would sail straight through gravity untouched. An ocean that computes, whose waves are the things, seemed like the right patron.

Solaris 1.0.0 is the reference build: the architecture, one full implementation of every mechanism, and one canonical configuration (the fabric is a closed 3-sphere — a space with no edge; more on what that does below). Versions matter here because the model is *not complete* and I say so in the paper's first section. The architecture is the bet. The implementations and parameters are expected to change, and every change has to re-sit the same exams.

## The run

That's the other half of the announcement: the exams. From the start, this programme has had one discipline — every claim must be a numbered test row, with its pass mark written down *before* the code runs, and never softened after. The full battery now stands at **150 rows, and 147 pass**.

The other three fail. On purpose, and they stay red.

I'll come back to those in the Confession, because they are my favourite part. But the headline is: the whole battery — every number in the paper, every number in this article — reproduces on your machine with one command. The model, the paper, and the tests live together in one public repository. The paper is the repository's manifest; when the model changes, the paper changes as a visible commit.

## What the mysteries become

Here is the heart of the paper, briefly. For each famous weirdness, the model offers a plain, boring, checkable mechanism that produces the same behaviour in the toy — no spookiness, no unicorns. I am *not* claiming this is how nature does it. The claim is smaller and testable: a mechanism of this kind is *sufficient* — the weirdness does not force magic on us, because at least one mundane machine produces it. Each line is backed by measured test rows in the harness.

**The speed of light.** Not a speed limit imposed on things — the pace of record propagation itself. A record cannot cite a record that doesn't exist yet. "Faster than light" would mean citing the future.

**Time dilation.** A moving thread spends its causal steps on moving, and writes fewer records about itself between the same milestones. Its clock ticks slower. No rule says so; it's just bookkeeping arithmetic.

**"Now".** A distributed system cannot be photographed at an instant. "The state of the universe right now" is the auditor's ill-posed question — you can only draw an "as of" line, and different legitimate lines disagree about the in-flight part while agreeing on everything settled.

**Spooky action at a distance.** Nothing acts at a distance. Two entangled particles are parties to *one contract* — a single shared record. The correlation lives in the record, not in a signal between two places. Measuring is settling the contract, and in the toy, settlement measurably leaves no trace an insider could use: no signal, no order-dependence.

**Measurement and collapse.** Nothing special about observers. A measurement is an ordinary co-signed settlement between two systems; "collapse" is the ledger pruning branches that didn't settle. Irreversible from inside — your own records can't be un-written — and plain bookkeeping from outside.

**Schrödinger's cat.** The cat is never alive-and-dead, because a cat is not a bystander — it's a huge settling system, co-signing thousands of records a second. The outcome settles at the first co-signature inside the box. What the human outside holds is an unmerged view. In programmer's terms, the boxed cat is a `Future<Cat>` — an unresolved promise — and opening the box is the `await`. Asking "but is it *really* alive right now?" is asking for the value of an unresolved future: a type error, not a mystery.

**Wigner's friend.** Two offices with different books — which *cannot contradict*, because the only way to compare books is to meet and co-sign, and comparing *is* reconciling. "When did the collapse really happen?" translates to "which as-of line are you reading?"

**The Born rule's square.** Why squared amplitudes? In the model: because every transaction has exactly two sides. Two signers, each weighing the branch, multiply into the square. One signer would give plain weights; three would give cubes; measured in the toy, only two gives quantum statistics.

**The universal wave function.** Its fearsome size is a representation artifact. The flat vector is the fully-replicated view of a sharded database; the ledger stores one contract per entangled cluster, and in the settling regime the storage grows *linearly* with particle count. Re-tensor the contracts and the full wave function comes back exactly. Just big. No black magic.

**Many-worlds.** Simply the ledger that never settles. Switch settlement off and the contracts merge into one giant table — literally the Everett picture. Not a rival theory; this architecture's no-settlement corner.

**Gravity.** Nothing curves. Dense regions compute slower, and waves refract toward slow regions. Falling is refraction — and only waves fall.

**The expanding universe, and its edge.** The record grows, holding it costs budget, everything smoothly slows — and from inside, everything slowing together is indistinguishable from everything moving apart. The fabric is closed and has no edge; the "edge of the visible universe" is emergent — the radius beyond which the accumulated slowing outruns the causality limit, and every observer finds one centered on *themselves*. Dark energy enters only as a candidate, stated carefully: the stretch accelerates exactly when record growth is super-linear — a queued test row, not a fitted answer.

**Black holes.** A region packed so dense its compute rate approaches zero — approaches, never reaches. The "singularity" is continuum language dividing by zero; the machine never divides by zero, it just runs ever slower. No infinities anywhere in the toy.

**No-cloning, and the arrow.** Copying an unknown state is a double-spend, forbidden by the very mechanism blockchains were invented for. And time has an arrow because the ledger is append-only: the past is fixed because records don't un-write; the future is open because it isn't written yet.

One observation I can't shake: we — the AI systems and I — built none of these behaviours in on purpose. We built a budget and a ledger, then sat the machine down for exams.

## The Confession

Every episode of this series confesses. This one has the most to confess, because a model this broad has real debts.

**The amplitudes are imported.** Pending branches in the ledger carry complex amplitudes and interfere. The model *uses* that; it does not *explain* it. This is the deepest residue — the one place the quantum enters by hand. (Related fence: the `Future<Cat>` analogy demystifies the cat paradox, not quantum computing. Ordinary promises resolve to plain values; quantum branches interfere, which is where the computational power lives. A machine of futures is not a quantum computer.)

**Mercury is wrong by 17%.** The current composition law for gravitational slowing overshoots Mercury's perihelion precession by 17%. A measured implementation change — rates that *compound* instead of add — brings it under 1% in the toy, but it isn't adopted into Solaris yet, so the honest number today is 17%.

**Three tests fail, and we predicted the class of failure.** The model's derived black hole is built by inflow — and its thermodynamics come out backwards compared to a real one (bigger holes come out hotter, not colder). We pre-registered those marks, ran the tests, and kept the three red rows in the record. This is the known hard boundary of analog-gravity models — you get the kinematics of horizons long before you get their dynamics — and now the toy has measured itself on the honest side of it.

**The words themselves are AI-written.** Like every text in this project, this article and the paper were written by AI from my guidance and under my editing — the direction, the concepts and the calls are mine; the sentences are the machine's. I put this in the Confession and not only in the fine print, because a reader deciding how much to trust this deserves to know exactly how it was made. The same goes for the numbers: AI-assisted, under continuing verification — which is why everything reproduces with one command.

**And the big one:** 147 passed rows in a toy prove *mechanism sufficiency*, not truth. The model has never touched a real dataset. We are not claiming to have built the universe on a laptop.

## What this does NOT claim

> This is a toy model with a test harness — a demonstration that mundane mechanisms *can* produce the famous quantum and relativistic behaviours, inside a toy, at stated tolerances. It is not a claim about how nature actually works, not a replacement for any working theory, and not a fitted cosmology. The interference of pending branches is imported, not explained. The futures analogy carries no quantum-computing claim. All numbers were produced with AI assistance and are under continuing verification — which is exactly why everything reproduces with one command.

## The neighbors

The universe-as-computation idea is old and well-populated, and this model makes no priority claim on it: Zuse's *Calculating Space*, Fredkin's digital philosophy, Lloyd's universe-as-quantum-computer, Wolfram's hypergraph programme, Whitworth's virtual-reality framing. The sprinkled causal fabric is causal set theory (Bombelli, Lee, Meyer, Sorkin). The flowing-fabric gravity sector lives in the analog-gravity tradition (Unruh; Barceló, Liberati, Visser) — including the kinematics-versus-dynamics boundary the three red rows just measured. Gravity-from-information has Jacobson and Verlinde. 't Hooft and Bohm are the elders of the deterministic-underneath family, and Barandes's indivisible-stochastic-processes formulation is the closest living neighbour — his framework deliberately leaves open what the underlying process is, and the ledger is offered as one concrete candidate. What I think is ours: the fixed-compute budget as the *source* of spacetime phenomenology, the ledger dictionary for the hidden layer, and the exam discipline with pre-registered failures.

## Run it yourself

The model, the paper, and the full test battery: [github.com/questforentropy/iceberg-model](https://github.com/questforentropy/iceberg-model) — one command: `python labs/run_exams.py all --record solaris-1.0.0 --expect-red GR-34,GR-35,GR-36`. Expected: 150 rows — 147 pass, exactly 3 red. The paper is the repository's manifest: [paper/the-iceberg-model.md](https://github.com/questforentropy/iceberg-model/blob/main/paper/the-iceberg-model.md). Archived: DOI [10.5281/zenodo.22046196](https://doi.org/10.5281/zenodo.22046196).

## How this was made

I'm a software architect. I built an adversarial research harness around AI agents and ran a physics toy-model programme through it; this piece reports the part that survived. The direction, the concepts, the questions and the accept/reject calls are mine; AI systems (Anthropic's Claude Fable, Opus and Sonnet, plus DeepSeek) executed the experiments from frozen, pre-declared specifications and wrote the text — this article and the paper included — from my guidance and under my editing. Every number is code-generated and reproducible from the repository above. A public honesty ledger records every commissioning error the process caught.

## Next time

This piece opens a series: one short article per mechanism, each walking through the actual tests behind it — emergent spacetime, spooky action at a distance, the expanding universe, gravity as congestion, black holes with no singularity. We start small and quick: *the poor cat experiment* — Schrödinger's cat and Wigner's friend, demystified with a programming concept every developer already uses daily.

---

*Quest for Entropy is written by Marijus Masteika. Entropy was always the dark horse for me — connected to information, and maybe hiding answers to everything. That's the quest.*
