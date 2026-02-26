# Week 9: Collaborative Filtering & Ethical Use of LLMs in the Classroom

<sub><sup>*written by isabel sieh & ishan khare, cs124 staff team, winter 2025*</sup></sub>

Agenda:
 - Collaborative Filtering
 - LLM Ethics

## Part 1: Collaborative Filtering (~30 min)

Let's work through an example of item-item collaborative filtering similar to what is used in PA7:

It will help to make a copy and follow along in [this spreadsheet](https://docs.google.com/spreadsheets/d/1Sag1-jVuPQoIHhAu1dqJICY0UXyYIB-Qmhz2dvGo96I/edit?usp=sharing).

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

In PA7, we have different synthetic users (in `synthetic_users.py`) where a user profile has a **list of movies they like**. 
In PA7, we later represent this a 0/1 vector (e.g. in `user_ratings_dict`). This is done for you (no code written):

- **liked** movie → **1**
- not listed / unrated → **0**

In this lab, we have a new user wthat has built a profile of movies they like, similar to `synthetic_users.py` in PA7.
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

## Part 2: The use of LLMs in the classroom (~50 min)
For this next section, form groups of 3-4 people as always. Your goal for today is to collaborate on developing a clear, concise policy on using generative AI tools (e.g. ChatGPT, Copilot) for CS 124 for Dan to use in next year’s course! You’ll first  examine some policies from other classes at Stanford. Then, by the end of today, you and your group will develop your own policy! Feel free to work on one laptop or start a Google Doc for collaboration. Dan will then choose from your policies to create next year’s class policy. 

### Review Existing Policies (~15 mins)
Writing course policies on the use of generative AI in the classroom is hard; we need to balance the potential benefits of using these tools for both students and faculty against the potential educational harms that may arise from their misuse. Below, we’ve compiled some example course policies that balance these tradeoffs well. First, review those policies. As you review these policies, discuss the following questions among your group members. Spend no more than 15 minutes on this exercise:
- What might be the benefits of a strict ban of generative AI tools in the classroom? What might be the drawbacks?
- What might be the benefits of a policy that allows AI with disclosure? What risks does it introduce?

<b>[CS 106B](https://web.stanford.edu/class/cs106b/): strict prohibition of AI <br/></b>

The syllabus from CS106B states: <i>University guidance on the use of generative AI in classroom settings treats use of generative AI analogously to receiving assistance from another human. As a result, using ChatGPT or other generative AI tools on any graded work is a violation of the Honor Code, regardless of whether that use is disclosed.</i><br/>
<br/>

<b>[CS 224N](https://web.stanford.edu/class/cs224n/): allows AI assistance with the expectation that students independently produce and fully understand their solutions, treating AI as a “collaborator” with appropriate discussions <br/></b>

The syllabus from CS224N states: <i>Students are required to independently submit their solutions for CS224N homework assignments. Collaboration with generative AI tools such as Co-Pilot and ChatGPT is allowed, treating them as collaborators in the problem-solving process. However, the direct solicitation of answers or copying solutions, whether from peers or external sources, is strictly prohibited.</i><br/>
<br/>

<b>[LINGUIST 130A/230A](https://web.stanford.edu/class/linguist130a/syllabus.html): views AI-generated content as "another person’s original work," meaning it must be properly cited if used. Excessive reliance on AI-generated text is unlikely to be evaluated positively.<br/></b>

The syllabus from LINGUIST 130A/230A states: <i>We interpret "another person's original work" to include content that was produced by an AI writing assistant like ChatGPT. This follows either by treating the AI assistant as a person for the purposes of this policy (controversial) or acknowledging that the AI assistant was trained directly on people's original work. Thus, while you are not forbidden from using these tools, you should consider the above policy carefully and quote where appropriate. Assignments that are in large part quoted from an AI assistant are very unlikely to be evaluated positively. In addition, if a student's work is substantially identical to another student's work, that will be grounds for an investigation of plagiarism regardless of whether the prose was produced by an AI assistant.</i><br/>
<br/>

After reading these course policies, spend some time with your group discussing the following questions regarding how LLMs might be used—or misused—in a variety of classroom and real-world scenarios:<br/>

#### Homework Assignments
- When might it be acceptable to use LLMs for coding or written homework? What harms might arise from using them (e.g. quality of learning, academic misconduct)?<br/>
- What kind of disclosure should be required if students use LLMs for homework?<br/>

#### Grading & Feedback
- For TAs or instructors, when do you think it would be okay to use an LLM to help grade assignments or provide feedback? What risks might arise from incorporating LLMs into the workflow of grading and giving feedback (e.g. privacy risks, errors in grading, quality of feedback)?<br/>
- Should instructors disclose that AI was used in evaluating their work?<br/>

### Building Your Own Policy (~20 mins)
After discussing the policies above with your group, spend the next ~20 minutes crafting your own Generative AI policy for CS 124. We expect <b><ins>2-4 short paragraphs</ins></b> (in total) to specify clear cases in which 1) students can use LLMs in their coursework, and 2) the teaching team can use LLMs in grading and providing feedback. Here’s how we recommend structuring your work:<br/>
#### Trade Offs
What might be lost if you ban the use of generative AI altogether? What would be gained? Where should the line be drawn between acceptable help and academic misconduct?

#### Identify Key Areas to Address
1. <b>Scope:</b> Which assignments or activities does your policy cover? (e.g., homework, exams, coding projects, written reflections)
2. <b>Allowed vs. Prohibited Uses:</b> 
   - For students: Define what is allowed, such as brainstorming, debugging, etc.
   - For the teaching team: Specify acceptable practices, including providing feedback on papers, autograding, or other instructional support.
3. <b>Disclosure:</b> If you allow AI usage, must students disclose it? If so, how?
4. <b>Consequences:</b> How does your policy tie into Stanford’s Honor Code? What happens if a student violates your AI rules?

#### Draft a Concise Policy
Work together with your group to write a <b><ins>2-4 short paragraph policy</ins></b> that clearly states how students in CS 124 can or cannot use generative AI tools. 
Keep the language clear and direct, and feel free to use the examples we provided above as inspiration or jumping off points. 

#### Consider the Broader Impact
Include a brief (1–2 sentence) explanation of why you made the choices you did. For example, if you’re allowing limited AI usage, how do you ensure it fosters learning rather than replacing it? Think about potential inequalities (e.g., some students may have more familiarity or better access to AI tools). Consider how your policy might affect collaboration and group work.

#### Submission Instructions
Save your file as a PDF, and submit at [this Google form](https://docs.google.com/forms/d/e/1FAIpQLSfdDQ4r5H9NE5S7U7-r3Juj5EHlsQrlA62WSi-7Kur93_sN2Q/viewform). This is also how we'll track your attendance for today's lab.

### Conclusion & Share Out (~15 mins)
We will now go back to the whole class and share out the policies you've created!
