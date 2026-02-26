# Week 9: Collaborative Filtering & Ethical Use of LLMs in the Classroom

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

**Solutions:** Filled-in similarity matrix and worked calculations:

|    | M1 | M2 |            M3 |           M4 |           M5 |          M6 |
|----|---:|---:|--------------:|-------------:|-------------:|------------:|
| M1 |  1 | 0.40 |          0.81 |         0.83 |         0.77 |        0.53 |
| M2 |    |  1 | 0.14          | 0.70         | 0.76         |        0.74 |
| M3 |    |    |             1 | 0.39         | 0.68         |        0.14 |
| M4 |    |    |               |            1 | 0.70         |        0.85 |
| M5 |    |    |               |              |            1 |         0.44  |
| M6 |    |    |               |              |              |          1  |

> $\texttt{sim}(M1, M3)$: M1 = [1,5,3,0], M3 = [2,4,0,1]. Dot = 2+20+0+0 = 22. $|M1|=\sqrt{35}$, $|M3|=\sqrt{21}$. $\texttt{sim} = 22/\sqrt{735} \approx 0.81$.

> $\texttt{sim}(M1, M5)$: Dot = 0+20+9+0 = 29. $|M5|=\sqrt{41}$. $\texttt{sim} = 29/\sqrt{1435} \approx 0.77$.

> $\texttt{sim}(M5, M6)$: M6 = [1,0,3,0]. Dot = 0+0+9+0 = 9. $|M6|=\sqrt{10}$. $\texttt{sim} = 9/\sqrt{410} \approx 0.44$.

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

**Solutions:**

> $\texttt{score}(M1) = \texttt{sim}(M1,M3) + \texttt{sim}(M1,M5) = 0.81 + 0.77 = 1.58$

> $\texttt{score}(M6) = \texttt{sim}(M6,M3) + \texttt{sim}(M6,M5) = 0.14 + 0.44 = 0.58$

### Step 4: Recommend a Movie

Now that we have a **score** for each candidate movie (M1, M2, M6), recommend the movie with the **highest score**.

**Solution:**

> M1 has the highest score (1.58), so we recommend M1.

## Part 2: The use of LLMs in the classroom (~50 min)

Enjoy the discussion!
