# Week 9: Collaborative Filtering & Ethical Use of LLMs in the Classroom

<sub><sup>*written by isabel sieh & esi eneinyang & ishan khare, cs124 staff team, winter 2025*</sup></sub>

## Agenda
 - Collaborative Filtering
 - LLM Ethics

## Part 1: Collaborative Filtering (~30 min)

Let's work through an example of item-item collaborative filtering similar to what is used in PA7:

It will help to make a copy and follow along in [this spreadsheet](https://docs.google.com/spreadsheets/d/1Sag1-jVuPQoIHhAu1dqJICY0UXyYIB-Qmhz2dvGo96I/edit?usp=sharing).

**Check out the solutions spreadsheet [here](https://docs.google.com/spreadsheets/d/1B9eXhi9rBuhHHMBU_DY0OV7jmmb6qonjNcI3U66toCQ/edit?usp=sharing).**

We have a **ratings matrix** from various users. Ratings are raw (e.g., 1–5). Missing entries are stored as 0 in the matrix.

### Ratings Matrix (raw values, used for similarity)

|    | U1 | U2 | U3 | U4 |
|----|---:|---:|---:|---:|
| M1 |  1 |  5 |  3 |    |
| M2 |    |    |  5 |  4 |
| M3 |  2 |  4 |    |  1 |
| M4 |    |  2 |  4 |    |
| M5 |    |  4 |  3 |  4 |
| M6 |  1 |    |  3 |    |

### Step 1: Build the synthetic user "likes" vector (0/1)

In PA7, we have different synthetic users (in [`synthetic_users.py`](https://github.com/cs124/pa7-agent/blob/main/synthetic_users.py)) where a user profile has a **list of movies they like**. 
In PA7, we later represent this a 0/1 vector (e.g. in `user_ratings_dict`). This is done for you (no code written):

- **liked** movie → **1**
- not listed / unrated → **0**

In this lab, we have a new user that has built a profile of movies they like, similar to [`synthetic_users.py`](https://github.com/cs124/pa7-agent/blob/main/synthetic_users.py) in PA7.
| movie | liked? |
|------:|:------:|
|    M1 |    0   |
|    M2 |    0   |
|    M3 |    1   |
|    M4 |    0   |
|    M5 |    1   |
|    M6 |    0   |

This synthetic user below likes M3 and M5 (and hasn't liked the rest).

### Step 2: Compute Similarity Scores

**Important Note:** In lecture and Quiz 8, you use mean-centered overlapping cosine similarity. Here (and in PA7) we use **raw** cosine similarity on the rating rows—no mean-centering.

Recall the formula for the cosine similarity of two vectors:

$$\texttt{sim}(M_i, M_j) = \frac{M_i \cdot M_j}{\|M_i\|\|M_j\|}$$

where $M_i$ is the **row vector of raw ratings** across dataset users (e.g. $M_1 = [1, 5, 3, 0]$, $M_5 = [0, 4, 3, 4]$).

Compute the cosine similarity over all **movie** (item) row vectors. We provide a few of the calculations for you, fill in the similarities for $\texttt{sim}(M1, M3)$, $\texttt{sim}(M1, M5)$, and $\texttt{sim}(M5, M6)$.
Note this is a symmetric matrix, that is $\texttt{sim}(M1,M2) = \texttt{sim}(M2,M1)$.


|    | M1 | M2 |            M3 |           M4 |           M5 |          M6 |
|----|---:|---:|--------------:|-------------:|-------------:|------------:|
| M1 |  1 | 0.40 |          **??** |         0.83 |         **??** |        0.53 |
| M2 |    |  1 | 0.14          | 0.70         | 0.76         |        0.74 |
| M3 |    |    |             1 | 0.39         | 0.68         |        0.14 |
| M4 |    |    |               |            1 | 0.70         |        0.85 |
| M5 |    |    |               |              |            1 |         **??**  |
| M6 |    |    |               |              |              |          1  |

$\texttt{sim}(M1, M3)$ = ??  

$\texttt{sim}(M1, M5)$ = ??  

$\texttt{sim}(M5, M6)$ = ??

### Step 3: Score each candidate movie (for ranking)

**Important:** In lecture and Quiz 8, you may normalize by the sum of similarities. Here (and in PA7) we do **not** normalize.

For each movie the user has **not** put on their profile, we compute a **score**—the sum of its similarities to every movie on their profile. These scores are used only to **rank** candidates (higher = more similar to their likes); they are not predicted ratings on a 1–5 scale.

For each movie $j$ with $\texttt{liked}[j] = 0$, compute:

$$\texttt{score}(j) = \sum_{i \in L} \texttt{sim}(j, i)$$

where $L$ is the set of movies the synthetic user put on their profile (the ones with 1). So we're just **summing similarities to liked movies**.

Because the user vector is 0/1, this is the same as:

$$\texttt{score}(j) = \sum_i \texttt{sim}(j, i) \cdot \texttt{liked}[i]$$

For our example (user likes M3 and M5):

- $\texttt{score}(M1) = \texttt{sim}(M1, M3) + \texttt{sim}(M1, M5)$
- $\texttt{score}(M2) = \texttt{sim}(M2, M3) + \texttt{sim}(M2, M5)$
- $\texttt{score}(M6) = \texttt{sim}(M6, M3) + \texttt{sim}(M6, M5)$

**Example:** $\texttt{score}(M2) = \texttt{sim}(M2,M3) + \texttt{sim}(M2,M5) = 0.14 + 0.76 = 0.90$

Now you calculate $\texttt{score}(M1)$ and $\texttt{score}(M6)$.

### Step 4: Recommend a Movie

Now that we have a **score** for each candidate movie (M1, M2, M6), recommend the movie with the **highest score**.

## Part 2: The Use of LLMs in the Classroom (~50 min)

### The AI Policy Problem (~5 min)

Before we dive in, let's take a quick pulse of the room.

**Think about the classes you're taking this quarter:**

* How many have an explicit policy on generative AI use?  
* How many of those policies contradict each other?  
* Have you ever felt unsure whether using an AI tool for a specific task was "allowed"?

Stanford's [Generative AI Policy Guidance](https://communitystandards.stanford.edu/generative-ai-policy-guidance) provides a university-wide framework, but it leaves the specifics to each instructor. That means every class you take can have a completely different set of rules — and as a student, you're expected to track and follow all of them.

Today, you're going to grapple with *why* that's so hard. By the end of this lab, you and your group will have drafted an AI policy for a real Stanford course — and had another group try to break it.

---

### Gray Area Scenarios (~10 min)

Form groups of 3-4 people. Select a scribe and start a Google Doc to write your eventual attendance sheet submission responses. For each scenario below, discuss with your group for 2-3 minutes and decide: **Acceptable**, **Unacceptable**, or **It Depends**. There are no right answers, and the point is to surface where you and your groupmates disagree, and why.

**Scenario 1 — PWR (Program in Writing and Rhetoric)**

A student in PWR is working on an argumentative essay. They paste their draft into ChatGPT and ask it to "reorganize my argument to be more persuasive." They rewrite all the prose themselves, but the structure of the essay is now based on the AI's suggestion.

**Scenario 2 — CS 124**

A student working on a CS 124 programming assignment gets a cryptic error message. They paste the error into ChatGPT, which suggests a fix involving a regex pattern the student hasn't seen before. The student uses the fix and it works, but they aren't sure they could reproduce it on their own.

**Scenario 3 — PWR**

A non-native English speaker uses Claude to proofread and polish the grammar on their PWR essay before submitting. The ideas and arguments are entirely their own, but many sentences have been reworded by the AI.

**Scenario 4 — CS 124**

A student uses Cursor while working on a CS 124 PA. They write a comment describing what the function should do, and Cursor autocompletes the entire function. The student reviews it, confirms it looks right, and moves on.

**Scenario 5 — Teaching Team**

A TA uses an LLM to draft feedback comments on student assignments. The TA reviews and edits the AI-generated comments before posting them, but roughly 70% of the language in the final feedback was generated by the AI.

After discussing, take note of where your group disagreed. Those disagreements are exactly the gaps that a written policy needs to address. **You will submit a summary of these disagreements in the attendance form (Q1).**

---

### Split Room: Draft Your Policy (~20 min)

Now your group will draft a generative AI policy for a real Stanford course. Here's the twist: **not every course should have the same policy.**

* **Side A of the room:** Draft a Gen AI policy for **CS 124** (an NLP course — coding-heavy, conceptual understanding matters, but AI tools are increasingly standard in the industry your students are entering).  
* **Side B of the room:** Draft a Gen AI policy for **PWR** (Stanford's required writing course — the written output *is* the learning objective; developing your own voice, argument, and critical thinking is the entire point).

Work together on your shared Google Doc. Your policy should be **2-4 short paragraphs** and must address the following:

**1\. Allowed vs. Prohibited Uses**

* For students: What specific uses of Gen AI tools are permitted? What is off-limits? Be concrete — "don't use AI to cheat" is not a policy.  
* For the teaching team: Can TAs or instructors use AI in grading, feedback, or lesson planning? Under what conditions?

**2\. Disclosure Requirements**

* If AI use is allowed, must students disclose it? How — a footnote, a separate statement, an honor code checkbox?

**3\. Consequences**

* How does your policy connect to Stanford's Honor Code? What happens if a student violates your AI rules? Be specific.

**4\. Rationale (1-2 sentences)**

* Why did your group make the choices it did? What is the core learning objective of this course, and how does your policy protect it?

As you draft, think back to the scenarios from the previous exercise. **Your policy should be able to cleanly handle each scenario relevant to your course.** If it can't, that's a sign something is underspecified. **You will submit your policy in the attendance form (Q2).**

---

### Stress Test Swap (~10 min)

Time to break each other's policies.

Each **CS 124 group** will swap their draft with a **PWR group** (and vice versa). Your job as the reviewing group:

1. **Find the loopholes.** Identify a realistic scenario where a student could technically comply with the policy while clearly undermining the learning objective.  
2. **Find the edge case.** What about a student with a disability who uses AI as an accessibility tool? What about a student who uses AI to understand the assignment prompt but not to complete it? What about a student with more money that can afford fancier language models?  
3. **Find the ambiguity.** Is there any phrase in the policy that two reasonable people could interpret differently?

Write **2-3 specific critiques** and pass the policy back. Original groups: review the feedback and note what you would revise. **You will submit a summary of these critiques in the attendance form (Q3).**

---

### Share Out & Wrap-Up (~5 min)

Let's come back together as a class. A few questions for the room:

* **Which course was harder to write a policy for — and why?**  
* **Did any group find that their scenario verdicts from earlier contradicted the policy they ended up writing?**  
* **What's the one thing you think every AI policy should include, regardless of the course?**

The key takeaway: there is no universal AI policy that works for every class, because the *learning objective* of a course should drive the policy. A writing class where the output is the learning and a CS class where the output demonstrates the learning require fundamentally different rules — even if they're both happening on the same campus, in the same quarter, for the same students.