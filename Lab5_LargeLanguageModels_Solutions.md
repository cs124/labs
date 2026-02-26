# Week 9: Collaborative Filtering & Ethical Use of LLMs

## Part 1: Collaborative Filtering

Let's work through an example of item-item collaborative filtering similar to what is used in PA7:

It will help to make a copy and follow along in [this spreadsheet](https://docs.google.com/spreadsheets/d/1Sag1-jVuPQoIHhAu1dqJICY0UXyYIB-Qmhz2dvGo96I/edit?usp=sharing).

**Check out the solutions spreadsheet [here](https://docs.google.com/spreadsheets/d/1ne8lkV_2DZo4KmcpD3RXeuLxiTH3ECS7JpW9OI8JBsE/edit#gid=0).**

We have a **ratings matrix** from various users. Ratings are raw (e.g., 1–5). Missing entries are stored as 0 in the matrix (for computing similarity).

### Ratings Matrix (raw values, used for similarity)

|    | U1 | U2 | U3 | U4 |
|----|---:|---:|---:|---:|
| M1 |  1 |  5 |  3 |  0 |
| M2 |  0 |  0 |  5 |  4 |
| M3 |  2 |  4 |  0 |  1 |
| M4 |  0 |  2 |  4 |  0 |
| M5 |  0 |  4 |  3 |  4 |
| M6 |  1 |  0 |  3 |  0 |

Do **not** binarize this matrix. In PA7 you compute movie–movie similarity from these raw rating vectors (rows).

### Step 1: Build the synthetic user "likes" vector (0/1)

In PA7, synthetic users (in `synthetic_users.py`) have a list of movies they like, represented as a 0/1 vector in `user_ratings_dict`. This synthetic user likes M3 and M5 only (no -1).

**New User (0/1):**

| movie | liked? |
|------:|:------:|
|    M1 |    0   |
|    M2 |    0   |
|    M3 |    1   |
|    M4 |    0   |
|    M5 |    1   |
|    M6 |    0   |

### Step 2: Compute Similarity Scores

We use **raw** cosine similarity on the rating rows (no mean-centering). $\texttt{sim}(M_i, M_j) = \frac{M_i \cdot M_j}{\|M_i\|\|M_j\|}$ with $M_i$ the row of raw ratings.

|    | M1 | M2 |   M3 |   M4 |   M5 |   M6 |
|----|---:|---:|-----:|-----:|-----:|-----:|
| M1 |  1 | 0.40 | 0.81 | 0.83 | 0.77 | 0.53 |
| M2 |    |  1 | 0.14 | 0.70 | 0.76 | 0.74 |
| M3 |    |    |    1 | 0.39 | 0.68 | 0.14 |
| M4 |    |    |      |    1 | 0.70 | 0.85 |
| M5 |    |    |      |      |    1 | 0.44 |
| M6 |    |    |      |      |      |    1 |

> $\texttt{sim}(M1, M3)$: M1 = [1,5,3,0], M3 = [2,4,0,1]. Dot = 2+20+0+0 = 22. $|M1|=\sqrt{35}$, $|M3|=\sqrt{21}$. $\texttt{sim} = 22/\sqrt{735} \approx 0.81$.

> $\texttt{sim}(M1, M5)$: Dot = 0+20+9+0 = 29. $|M5|=\sqrt{41}$. $\texttt{sim} = 29/\sqrt{1435} \approx 0.77$.

> $\texttt{sim}(M5, M6)$: M6 = [1,0,3,0]. Dot = 0+0+9+0 = 9. $|M6|=\sqrt{10}$. $\texttt{sim} = 9/\sqrt{410} \approx 0.44$.

### Step 3: Score each candidate movie (for ranking)

We do **not** normalize. For each movie not on the user's profile, we compute a **score** (sum of similarities to movies on their profile). These scores are for **ranking** only—higher means more similar to their likes; they are not predicted 1–5 ratings.

$\texttt{score}(j) = \sum_{i \in L} \texttt{sim}(j, i)$ where $L$ = movies on their profile (the 1s). User likes M3 and M5, so:

$\texttt{score}(M2) = \texttt{sim}(M2,M3) + \texttt{sim}(M2,M5) = 0.14 + 0.76 = 0.90$

> $\texttt{score}(M1) = \texttt{sim}(M1,M3) + \texttt{sim}(M1,M5) = 0.81 + 0.77 = 1.58$

> $\texttt{score}(M6) = \texttt{sim}(M6,M3) + \texttt{sim}(M6,M5) = 0.14 + 0.44 = 0.58$

### Step 4: Recommend a Movie

Now that we have a **score** for each candidate (M1, M2, M6), we recommend the movie with the highest score.

> M1 has the highest score (1.58), so we recommend M1.

## Part 2: Ethical Use of LLMs in the Classroom

Enjoy the discussion!
