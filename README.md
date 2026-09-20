# Sandil Dinuwara - campus_life

# Unit 1

## What This Does

This is a Retrieval-Augmented Generation (RAG) system designed to answer student queries. I picked the `campus_life` corpus, which contains 88 documents including administrative guides and student advice threads. The system provides accurate, strictly grounded answers regarding housing lotteries, class registration, dining hall food, and other university procedures.
<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->

## Chunking Strategy

**Chunk size:** Dynamic (Paragraph-based)
**Overlap:** 0

Instead of cutting the documents at an arbitrary character limit (which cuts sentences in half), I updated `chunker.py` to split documents by paragraph boundaries (`\n\n`). Paragraphs naturally group complete thoughts together, providing the AI with the intact context it needs to generate accurate answers.

## Sample Chunks

======================================================================
Chunk 1  |  source: admin_add_drop_deadline.txt#0  |  produced by: chunker.py::split_documents
======================================================================
On the add/drop deadline

======================================================================
Chunk 2  |  source: course_cs_210_workload.txt#2  |  produced by: chunker.py::split_documents
======================================================================
It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.

======================================================================
Chunk 3  |  source: course_phys_130.txt#3  |  produced by: chunker.py::split_documents
======================================================================
The one piece of advice: the lab practical is worth 20% and almost nobody prepares for it.

======================================================================
Chunk 4  |  source: dining_verrill_street_grill.txt#1  |  produced by: chunker.py::split_documents
======================================================================
I'm a junior and I've done this twice now. Wait times: up to 30 minutes on Friday evenings, otherwise under 10. The thing worth going for is the burger, which is the only late-night hot food on campus. The thing to know is that one register, so the queue is asingle line no matter how busy.

======================================================================
Chunk 5  |  source: housing_morrow_house.txt#2  |  produced by: chunker.py::split_documents
======================================================================
The good: cheapest housing tier by about $900 a year, and the singles are real singles.

## Sample Answer

**Question:** is the housing lottery random?

**Answer:** The housing lottery is not entirely random in the way most people assume. While rising sophomores get a number drawn at random, juniors and seniors are ordered first by accumulated credit hours, with random tie-breaks used only for ties (*admin_housing_lottery.txt*).
(best distance 0.180, cutoff 0.6)

**My relevance cutoff:** 0.6

| Question | In corpus? | Best distance |
|---|---|---|
| is the housing lottery random? | Yes | 0.180 |
| How is class registration priority determined based? | Yes | 0.576 |
| What is the capital of Mongolia? | No | 0.799 |
| How do I change the oil in a diesel engine? | No | 0.850 |

## How I Used AI


**1.** I used AI as a sounding board to brainstorm and refine my 5 acceptance criteria in `criteria.md`, specifically ensuring that my rules were observable and measurable rather than subjective opinions.

**2.** I collaborated with an AI assistant to write the Python logic for my paragraph-based chunking strategy (`\n\n` split) in `chunker.py`, ensuring it properly handled whitespace and empty strings.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 |  |  |  |
| 5 |  |  |  |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
