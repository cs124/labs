# Week 9: Collaborative Filtering & Ethical Use of LLMs in the Classroom

<sub><sup>*written by veronica rivera & jeong shin, cs124 staff team, winter 2025*</sup></sub>

Agenda:
 - Collaborative Filtering
 - LLM Ethics

## Part 1: Collaborative Filtering

Let's work through an example of item-item collaborative filtering similar to what is used in PA7:

It will help to make a copy and follow along in [this spreadsheet](https://docs.google.com/spreadsheets/d/1RalPHyrnGHc3dGnDVzAZNZG4uNiZxrcMiO--Xo8awsw/edit#gid=0).

Let's say we have this matrix of movie reviews from various users:

|    | U1 | U2 | U3 | U4 |
|----|---:|---:|---:|---:|
| M1 |  1 |  5 |  3 |    |
| M2 |    |    |  5 |  4 |
| M3 |  2 |  4 |    |  1 |
| M4 |    |  2 |  4 |    |
| M5 |    |  4 |  3 |  4 |
| M6 |  1 |    |  3 |    |

We have a new user with the following preferences:

U = [M1: ??, M2: ??, M3: 5, M4: 2, M5: 3, M6: ??]

Out of the movies they have not seen (M1, M2, and M6) we want to recommend the film that they are likely to rate the highest.

### Step 1.  Binarize the Ratings

Start by binarizing the ratings!  We will use the following cutoffs (same as PA7):

0 $\lt$ rating $\leq$ 2.5: **-1**

Unrated: **0**

2.5 $\lt$ rating $\leq$ 5: **1**

We fill out this table for you in the interest of time!  Please ensure you understand how we got this matrix!

Binarized Matrix:
|    | U1 | U2 | U3 | U4 |
|----|---:|---:|---:|---:|
| M1 | -1 |  1 |  1 |  0 |
| M2 |  0 |  0 |  1 |  1 |
| M3 | -1 |  1 |  0 | -1 |
| M4 |  0 | -1 |  1 |  0 |
| M5 |  0 |  1 |  1 |  1 |
| M6 | -1 |  0 |  1 |  0 |


New User:
| movie | binarized rating |
|-------|------------------|
|    M1 |                0 |
|    M2 |                0 |
|    M3 |                1 |
|    M4 |               -1 |
|    M5 |                1 |
|    M6 |                0 |


### Step 2: Compute Similarity Scores

**Important Note:** In lecture and Quiz 8, you will use mean-centered overlapping cosine similarity. But here (and in PA7), we will use raw cosine similarity, NOT mean-centered overlapping item cosine similarity.

Recall the formula for the cosine similarity of two vectors:

$$\texttt{sim}(v_1,v_2) = \frac{v_1 \cdot v_2}{||v_1||||v_2||} = \frac{v_1 \cdot v_2}{\sqrt{\sum\nolimits_{i=1}^{n} v_{1,i}^{2}} \cdot \sqrt{\sum\nolimits_{i=1}^{n} v_{2,i}^{2}}}$$

Use the binarized vectors when computing the cosine similarity.  We provide a few of the calculations for you, fill in the similarities for $\texttt{sim}(M1, M3)$, $\texttt{sim}(M1, M5)$, and $\texttt{sim}(M5, M6)$.

Note this is a symmetric matrix, that is $\texttt{sim}(M1,M2) = \texttt{sim}(M2,M1)$.

Compute the cosine similarity over all the ***MOVIE*** (item) vectors.  These are the rows of the above matrix.

|    | M1 | M2 |            M3 |           M4 |           M5 |          M6 |
|----|---:|---:|--------------:|-------------:|-------------:|------------:|
| M1 |  1 | 0.41 |          **??** |            0 |         **??** |        0.82 |
| M2 |    |  1 | -0.41         | 0.5          | 0.82         |         0.5 |
| M3 |    |    |             1 |        -0.41 |            0 |        0.41 |
| M4 |    |    |               |            1 |            0 |         0.5 |
| M5 |    |    |               |              |            1 |         **??**  |
| M6 |    |    |               |              |              |          1  |

$\texttt{sim}(M1, M3)$ = ??

$\texttt{sim}(M1, M5)$ = ??

$\texttt{sim}(M5, M6)$ = ??

### Step 3: Compute New User's Ratings

**Important Note:** In lecture and Quiz 8, you will normalize the rating by dividing the sum of the similarity scores. But here (and in PA7), we will NOT do normalization.

Based on the New User's provided ratings for movies 3, 4, and 5, predict how they would rate movies 1, 2, and 6.

We will take the weighted average over the binarized ratings of all movies rated by the new user.  We will weigh on similarity.

For example the predicted rating for movie 2 is:

$\texttt{Rating}(M2) = \texttt{sim}(M2,M3) \cdot \texttt{binarized rating M3} + $
$\texttt{sim}(M2,M4) \cdot \texttt{binarized rating M4} + $
$\texttt{sim}(M2,M5) \cdot \texttt{binarized rating M5}$

$\texttt{Rating}(M2) = (-0.41)(1) + (0.5)(-1) + (0.82)(1) = -0.09$

Now you calculate for M1 and M6.

### Step 4: Recommend a Movie

Now that we have the expected ratings of the user for the movies they have not seen we need to actually recommend a movie.  Recommend the movie with the highest predicted rating!

## Part 2: The use of LLMs in the classroom
For this next section, form groups of 3-4 people as always. Your goal for today is  to collaborate on developing a clear, concise policy on using generative AI tools (e.g. ChatGPT, Copilot) for CS 124 for Dan to use in next year’s course! You’ll first  examine some policies from other classes at Stanford, and then by the end of today you and your group will develop your own policy! Dan will then choose from your policies to create next year’s class policy. 

### Review Existing Policies (15 mins)
Writing course policies on the use of generative AI in the classroom is hard; we need to balance the potential benefits of using these tools for both students and faculty against the potential educational harms that may arise from their misuse. Below, we’ve compiled some example course policies that balance these tradeoffs well. First, review those policies. As you review these policies, discuss the following questions among your group members. Spend no more than 15 minutes on this exercise:
What might be the benefits of a strict ban of generative AI tools in the classroom? What might be the drawbacks?
What might be the benefits of a policy that allows AI with disclosure? What risks does it introduce?

[CS 106B](https://web.stanford.edu/class/cs106b/): strict prohibition of AI
The syllabus from CS106B states:
University guidance on the use of generative AI in classroom settings treats use of generative AI analogously to receiving assistance from another human. As a result, using ChatGPT or other generative AI tools on any graded work is a violation of the Honor Code, regardless of whether that use is disclosed.

[CS 224N](https://web.stanford.edu/class/cs224n/): allows AI assistance with the expectation that students independently produce and fully understand their solutions, treating AI as a “collaborator” with appropriate discussions
The syllabus from CS224N states:
Students are required to independently submit their solutions for CS224N homework assignments. Collaboration with generative AI tools such as Co-Pilot and ChatGPT is allowed, treating them as collaborators in the problem-solving process. However, the direct solicitation of answers or copying solutions, whether from peers or external sources, is strictly prohibited.

[LINGUIST 130A/230A](https://web.stanford.edu/class/linguist130a/syllabus.html): views AI-generated content as "another person’s original work," meaning it must be properly cited if used. Excessive reliance on AI-generated text is unlikely to be evaluated positively.
The syllabus from LINGUIST 130A/230A states:
We interpret "another person's original work" to include content that was produced by an AI writing assistant like ChatGPT. This follows either by treating the AI assistant as a person for the purposes of this policy (controversial) or acknowledging that the AI assistant was trained directly on people's original work. Thus, while you are not forbidden from using these tools, you should consider the above policy carefully and quote where appropriate. Assignments that are in large part quoted from an AI assistant are very unlikely to be evaluated positively. In addition, if a student's work is substantially identical to another student's work, that will be grounds for an investigation of plagiarism regardless of whether the prose was produced by an AI assistant.

After reading these course policies, spend some time with your group discussing the following questions regarding how LLMs might be used—or misused—in a variety of classroom and real-world scenarios:

Homework Assignments
When might it be acceptable to use LLMs for coding or written homework? What harms might arise from using them (e.g. quality of learning, academic misconduct)?
What kind of disclosure should be required if students use LLMs for homework?

Grading & Feedback
For TAs or instructors, when do you think it would be okay to use an LLM to help grade assignments or provide feedback?
What risks might arise from incorporating LLMs into the workflow of grading and giving feedback (e.g. privacy risks, errors in grading, quality of feedback)? 
Should instructors disclose that AI was used in evaluating their work?

Building Your Own Policy (20 mins)
After discussing the policies above with your group, spend the next ~20 minutes crafting your own Generative AI policy for CS 124. Here’s how we recommend structuring your work:
Trade Offs
What might be lost if you ban the use of generative AI altogether? What would be gained?
Where should the line be drawn between acceptable help and academic misconduct?
Identify Key Areas
Identify the main areas to address:
Scope: Which assignments or activities does your policy cover? (e.g., homework, exams, coding projects, written reflections)
Allowed vs. Prohibited Uses: Specify exactly what is permissible– brainstorming, debugging, small snippets of generated code, etc.–and what clearly isn’t.
Disclosure: If you allow AI usage, must students disclose it? If so, how?
Consequences: How does your policy tie into Stanford’s Honor Code? What happens if a student violates your AI rules?
Draft a Concise Policy
Work together with your group to write a 1-2 paragraph policy that clearly states how students in CS 124 can or cannot use generative AI tools. 
Keep the language clear and direct, and feel free to use the examples we provided above as inspiration or jumping off points. 
Consider the Broader Impact
Include a brief (1–2 sentence) explanation of why you made the choices you did. For example, if you’re allowing limited AI usage, how do you ensure it fosters learning rather than replacing it?
Think about potential inequalities (e.g., some students may have more familiarity or better access to AI tools).
Consider how your policy might affect collaboration and group work.
Submission Instructions
Save your file as a PDF, and submit at [Google Form TBD].

15 mins for sharing distinctive attributes of your policy/something that stands out.
