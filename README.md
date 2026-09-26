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

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunks contain the answer | 4 of 5 | 2/5 | 2/5 | 2/5 | MISSED |
| 2. Every answer names a source | 5 of 5 | 3/5 | 2/5 | 2/5 | MISSED |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Chunks align perfectly with natural paragraph boundaries | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Answer contains expected keywords | 5 of 5 | 2/5 | 1/5 | 1/5 | MISSED |

*Output produced by `run_eval.py::main`, saved in `results/run_2026-09-23_2041.md`:*

### is the housing lottery random? — run 1

- Best distance: 0.1801 (passed the gate)
- Sources retrieved: admin_housing_lottery.txt, admin_parking_permits.txt, advising_registration.txt, housing_morrow_house.txt

```
The housing lottery is not entirely random; while rising sophomores get a number drawn at random, juniors and seniors are ordered by accumulated credit hours first, with random selection used only as a tie-breaker. 

Source: admin_housing_lottery.txt
```

### is the housing lottery random? — run 2

- Best distance: 0.1801 (passed the gate)
- Sources retrieved: admin_housing_lottery.txt, admin_parking_permits.txt, advising_registration.txt, housing_morrow_house.txt

```
The housing lottery is not entirely random; rising sophomores get a number drawn at random, but juniors and seniors are ordered by accumulated credit hours first with random tie-breaking. 

Source: admin_housing_lottery.txt
```

### is the housing lottery random? — run 3

- Best distance: 0.1801 (passed the gate)
- Sources retrieved: admin_housing_lottery.txt, admin_parking_permits.txt, advising_registration.txt, housing_morrow_house.txt

```
The housing lottery is not entirely random; rising sophomores get a number drawn at random, but juniors and seniors are ordered by accumulated credit hours first with random tie-breaking. 

Source: `admin_housing_lottery.txt`
```

### How is class registration priority determined based? — run 1

- Best distance: 0.5757 (passed the gate)
- Sources retrieved: admin_add_drop_deadline.txt, admin_grade_appeals.txt, advising_registration.txt, course_stat_150.txt, dining_verrill_street_grill_followup.txt

```
Registration priority is determined by credit hours, with the times staggered similarly to the housing lottery. 

Source: advising_registration.txt
```

### How is class registration priority determined based? — run 2

- Best distance: 0.5757 (passed the gate)
- Sources retrieved: admin_add_drop_deadline.txt, admin_grade_appeals.txt, advising_registration.txt, course_stat_150.txt, dining_verrill_street_grill_followup.txt

```
Registration priority is staggered by credit hours, which is the same as the housing lottery (advising_registration.txt).
```

### How is class registration priority determined based? — run 3

- Best distance: 0.5757 (passed the gate)
- Sources retrieved: admin_add_drop_deadline.txt, admin_grade_appeals.txt, advising_registration.txt, course_stat_150.txt, dining_verrill_street_grill_followup.txt

```
Registration priority is staggered by credit hours, just like the housing lottery (advising_registration.txt).
```

### What do students say about the quality of food in the dining hall? — run 1

- Best distance: 0.4669 (passed the gate)
- Sources retrieved: admin_dining_dollars.txt, dining_pellew_dining_hall.txt, dining_pellew_dining_hall_followup.txt, housing_fenwick_court.txt, housing_tamsin_court.txt

```
I do not have enough information to answer your question, as the provided documents do not mention the quality of food in the dining hall.
```

### What do students say about the quality of food in the dining hall? — run 2

- Best distance: 0.4669 (passed the gate)
- Sources retrieved: admin_dining_dollars.txt, dining_pellew_dining_hall.txt, dining_pellew_dining_hall_followup.txt, housing_fenwick_court.txt, housing_tamsin_court.txt

```
I do not have enough information to answer this question.
```

### What do students say about the quality of food in the dining hall? — run 3

- Best distance: 0.4669 (passed the gate)
- Sources retrieved: admin_dining_dollars.txt, dining_pellew_dining_hall.txt, dining_pellew_dining_hall_followup.txt, housing_fenwick_court.txt, housing_tamsin_court.txt

```
I do not have enough information to answer this question.
```

### How do students find books for research? — run 1

- Best distance: 0.4768 (passed the gate)
- Sources retrieved: admin_library_holds.txt, admin_study_abroad.txt, money_textbooks.txt, study_library_hours.txt

```
I do not have enough information to answer how students find books for research. 

Source: money_textbooks.txt, admin_library_holds.txt, study_library_hours.txt, admin_study_abroad.txt
```

### How do students find books for research? — run 2

- Best distance: 0.4768 (passed the gate)
- Sources retrieved: admin_library_holds.txt, admin_study_abroad.txt, money_textbooks.txt, study_library_hours.txt

```
I don't have enough information to answer how students find books for research.
```

### How do students find books for research? — run 3

- Best distance: 0.4768 (passed the gate)
- Sources retrieved: admin_library_holds.txt, admin_study_abroad.txt, money_textbooks.txt, study_library_hours.txt

```
I don't have enough information to answer how students find books for research.
```

### What scholarships and awards are available for students? — run 1

- Best distance: 0.4451 (passed the gate)
- Sources retrieved: admin_campus_jobs_and_financial_aid.txt, admin_grade_appeals.txt, admin_graduation_requirements.txt, admin_study_abroad.txt, money_textbooks.txt

```
I do not have enough information to answer what scholarships and awards are available for students, as the provided documents do not mention them.
```

### What scholarships and awards are available for students? — run 2

- Best distance: 0.4451 (passed the gate)
- Sources retrieved: admin_campus_jobs_and_financial_aid.txt, admin_grade_appeals.txt, admin_graduation_requirements.txt, admin_study_abroad.txt, money_textbooks.txt

```
I do not have enough information to answer what scholarships and awards are available for students, as the provided documents do not mention them.
```

### What scholarships and awards are available for students? — run 3  

- Best distance: 0.4451 (passed the gate)
- Sources retrieved: admin_campus_jobs_and_financial_aid.txt, admin_grade_appeals.txt, admin_graduation_requirements.txt, admin_study_abroad.txt, money_textbooks.txt

```
I do not have enough information to answer what scholarships and awards are available for students.
```

## Verdicts

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunks contain the answer | MISSED | The target was 4 of 5, but the system only successfully retrieved the answer for 2 out of 5 questions (Q1 and Q2) on all runs. |
| 2 | Every answer names a source | MISSED | The target was 5 of 5, but the system only included a source in 3/5 on the first run, and 2/5 on the second and third runs. |
| 3 | Gate stops out-of-corpus questions | MET | The target was 4 of 5, and the gate successfully refused 5 out of 5 out-of-scope questions on all runs. |
| 4 | Chunks align perfectly with natural boundaries | MET | The target was 4 of 5. Because I implemented paragraph-based chunking (\n\n) in Unit 1, 5 out of 5 sampled chunks remained fully intact. |
| 5 | Answer contains expected keywords | MISSED (Revised) | The initial test scored 2/5, 1/5, 1/5. However, Question 2 failed due to a flawed measurement (looking for "academic" instead of "credit hours"). I revised this in `questions.py` and `criteria.md`. |

## Diagnoses

* **Criterion 1 (Retrieved chunks contain the answer):** 
  * **Stage:** Retrieval
  * **Mechanism:** For the questions about dining hall food, finding books, and scholarships, the system retrieved chunks that passed the relevance gate, but those chunks did not actually contain the answers. Pure semantic search struggled to pull the exact paragraphs containing those details, pulling marginally related chunks instead.

* **Criterion 2 (Every answer names a source):**
  * **Stage:** Generation
  * **Mechanism:** When the retrieved chunks do not contain the answer, the model correctly outputs a refusal. However, the generation prompt does not strictly instruct the model to cite the retrieved source documents *even when* it is refusing to answer. Therefore, it just drops the source citation completely.

* **Criterion 5 (Answer contains expected keywords):**
  * **Stage:** Measurement / Retrieval
  * **Mechanism:** One failure (Q2) was a measurement error at the Evaluation stage (the expected keyword was wrong in the test itself). The other failures were downstream effects of the Retrieval stage failing — because the correct chunks weren't retrieved for Q3, Q4, and Q5, the model couldn't generate the expected keywords in the answer.

## The Improvement

**What I changed:**
I updated the `GROUNDING_INSTRUCTION` in `generate.py` to explicitly command the model: *"YOU MUST ALWAYS LIST THE SOURCE DOCUMENTS at the end of your response, EVEN IF you are stating that you do not have enough information."*

**Why I picked it:**
My diagnosis for Criterion 2 pointed directly to the Generation stage, where the LLM was dropping citations whenever it correctly refused to answer; updating the prompt directly addresses this exact mechanism.

### Run Log — After

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 2/5 | 2/5 | 2/5 | MISSED |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Chunks align perfectly with natural boundaries | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Answer contains expected keywords | 5 of 5 | 2/5 | 2/5 | 2/5 | MISSED |

*Output produced by `run_eval.py::main`, saved in `results/run_2026-09-26_1325_after.md`:*

### What do students say about the quality of food in the dining hall? — run 1

- Best distance: 0.4669 (passed the gate)
- Sources retrieved: admin_dining_dollars.txt, dining_pellew_dining_hall.txt, dining_pellew_dining_hall_followup.txt, housing_fenwick_court.txt, housing_tamsin_court.txt

```
I do not have enough information to answer what students say about the quality of food in the dining hall.

Source Documents:
- housing_tamsin_court.txt
- dining_pellew_dining_hall.txt
- dining_pellew_dining_hall_followup.txt
- admin_dining_dollars.txt
- housing_fenwick_court.txt
```

### How do students find books for research? — run 1

- Best distance: 0.4768 (passed the gate)
- Sources retrieved: admin_library_holds.txt, admin_study_abroad.txt, money_textbooks.txt, study_library_hours.txt

```
I do not have enough information to answer how students find books for research.

Source Documents:
- money_textbooks.txt
- admin_library_holds.txt
- study_library_hours.txt
- admin_study_abroad.txt
```

### What scholarships and awards are available for students? — run 1

- Best distance: 0.4451 (passed the gate)
- Sources retrieved: admin_campus_jobs_and_financial_aid.txt, admin_grade_appeals.txt, admin_graduation_requirements.txt, admin_study_abroad.txt, money_textbooks.txt

```
I do not have enough information to answer what scholarships and awards are available for students.

Source Documents:
- admin_campus_jobs_and_financial_aid.txt
- admin_graduation_requirements.txt
- admin_study_abroad.txt
- admin_grade_appeals.txt
- money_textbooks.txt
```


**Did it help?**
Yes, it worked exactly as intended. Before the change, Criterion 2 failed (scoring 3/5, 2/5, 2/5) because the LLM omitted source file names when it didn't have enough information to answer. After the prompt update, it successfully cited the source documents across all 5 questions on all 3 runs, changing the verdict to MET.

## What's Still Broken

Criteria 1 and 5 are still failing (scoring 2/5). 

**What I'd do about it:** The root cause is at the Retrieval stage. Semantic search alone is failing to retrieve paragraphs containing exact matches for terms like "scholarships" or "research". To fix this, I would implement a Hybrid Search (combining Vector Search with BM25 keyword search) to ensure exact terms are pulled successfully.

**Why I stopped:** The project instructions strictly require making and measuring only *one* change to avoid confounding variables. I chose to fix the Generation prompt (Criterion 2) first because it was a direct, isolated fix, and I stopped there to ensure I could accurately measure its impact.

## What I'd Do Differently

Knowing what I know now, I would write Criterion 5 ("Answer contains expected keywords") differently. 

Currently, this criterion evaluates the generation stage, but it is entirely dependent on the retrieval stage. When retrieval fails, the model correctly refuses to answer to prevent hallucinations. However, doing its job correctly automatically causes Criterion 5 to fail. In the future, I would write a generation criterion that evaluates formatting independently of retrieval (e.g., "All responses are exactly 3 sentences or fewer, including refusals") so the stages are properly isolated.


## How I Used AI
In Unit 2, I used an AI assistant to help analyze the terminal run logs and diagnose the specific mechanisms behind my failing criteria. The AI helped me spot the pattern that the generation stage was successfully refusing out-of-context questions but dropping the source citations in the process. This guided me to my targeted prompt engineering fix in `generate.py`.