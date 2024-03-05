# Week 9: PA7 Overview, Group Exercises on Recommendation Systems, Github Setup, Group Norms

Agenda:
 - PA7 Overview (15 minutes)
 - Recommendation Systems Question (15 minutes)
 - Getting started with Github (30 minutes)
 - Group Norms (20 minutes)

## Part 1: PA7 Overview

For PA7 we will be building a custom movie recommendation chatbot!  We will implement our chatbot with 3 different designs: slot-filling dialogue agent (GUS), LLM prompting chatbot, and a hybrid LLM augmented bot.

PA7 is worth 16\% of your total grade, and conveniently split into exactly 100 points!  You're going to earn 5 of them today!

Here's the point breakdown:
 - [PA7 Group Work Form](https://docs.google.com/document/d/12G0x5YhdnO6qaR2RHvKaTGUG6k2pYj3-PRLZSE0BsPY/edit?usp=sharing) (5 Points)
 - [PA7 Coding](https://docs.google.com/spreadsheets/d/1MXqnPk60nwNWoNQQcLK2rTWIolZsi8u8pmQx31ewh8Y/edit#gid=2094045160) (85 Points)
 - [PA7 User Testing](https://docs.google.com/document/d/1liX4MC5qfBceQB0oCPICgxnRmoH0RKfdq5jmwsp7wRw/edit#heading=h.k5axkkjlu35) (5 Points)
 - [PA7 Reflection](https://docs.google.com/document/d/1gDKuWzuI3f6Nmue60UqoESsrWIzRK62GmdKRh1_y_DM/edit#heading=h.90ic4jpslfqf) (5 points)

 We will cover the Group Work Form at the end of today's Lab!

You can find the rubric for PA7 coding [here](https://docs.google.com/spreadsheets/d/1MXqnPk60nwNWoNQQcLK2rTWIolZsi8u8pmQx31ewh8Y/edit#gid=2094045160).

All submissions will be made through gradescope.  **Note that no late days can be used for PA7!!!**

We will now go over a live demo of the different components of the project!

## Part 2: Collaborative Filtering

Let's work through an example of item-item collaborative filtering similar to what will be used in PA7:

It will help to make a copy and follow along in [this spreadsheet](https://docs.google.com/spreadsheets/d/1RalPHyrnGHc3dGnDVzAZNZG4uNiZxrcMiO--Xo8awsw/edit#gid=0).

Let's say we have this matrix of movie reviews from various users:

|    | U1 | U2 | U3 | U4 | U5 | U6 | U7 | U8 | U9 | U10 | U11 | U12 |
|----|---:|---:|---:|---:|---:|---:|---:|---:|---:|----:|----:|----:|
| M1 |  1 |    |  3 |    |    |  5 |    |    |  5 |     |   4 |     |
| M2 |    |    |  5 |  4 |    |    |  4 |    |    |   2 |   1 |   3 |
| M3 |  2 |  4 |    |  1 |  2 |    |  3 |    |  4 |   3 |   5 |     |
| M4 |    |  2 |  4 |    |  5 |    |    |  4 |    |     |   2 |     |
| M5 |    |  4 |  3 |  4 |  2 |    |    |    |    |     |   2 |   5 |
| M6 |  1 |    |  3 |    |  3 |    |    |  2 |    |     |   4 |     |

We have a new user with the following preferences:

U = [M1: ??, M2: ??, M3: 5, M4: 2, M5: 3, M6: ??]

Out of the movies they have not seen (M1, M2, and M6) we want to recommend the film that they are likely to rate the highest.

### Step 1.  Binarize the Ratings

Start by binarizing the ratings!  We will use the following cutoffs (same as PA7):

0 $\lt$ rating $\leq$ 2.5: **-1**

Unrated: **0**

2.5 $\lt$ rating $\leq$ 5: **1**

We partially fill out this table for you.  Please fill out the missing binarization for M1 and M6, as well as the New User.

Binarized Matrix:
|    | U1 | U2 | U3 | U4 | U5 | U6 | U7 | U8 | U9 | U10 | U11 | U12 |
|----|---:|---:|---:|---:|---:|---:|---:|---:|---:|----:|----:|----:|
| M1 |  ? |  ? |  ? |  ? |  ? |  ? |  ? |  ? |  ? |   ? |   ? |   ? |
| M2 |  0 |  0 |  1 |  1 |  0 |  0 |  1 |  0 |  0 |  -1 |  -1 |   1 |
| M3 | -1 |  1 |  0 | -1 | -1 |  0 |  1 |  0 |  1 |   1 |   1 |   0 |
| M4 |  0 | -1 |  1 |  0 |  1 |  0 |  0 |  1 |  0 |   0 |  -1 |   0 |
| M5 |  0 |  1 |  1 |  1 | -1 |  0 |  0 |  0 |  0 |   0 |  -1 |   1 |
| M6 |  ? |  ? |  ? |  ? |  ? |  ? |  ? |  ? |  ? |   ? |   ? |   ? |


New User:
| movie | binarized rating |
|-------|------------------|
|    M1 |                ? |
|    M2 |                ? |
|    M3 |                ? |
|    M4 |                ? |
|    M5 |                ? |
|    M6 |                ? |


### Step 2: Compute Similarity Scores

For this part (as in PA7) we will use raw cosine similarity, NOT mean-centered overlapping item cosine similarity.  Note that in PA7 we do not use mean-centering at all.

Recall the formula for the cosine similarity of two vectors:

$$\texttt{sim}(v_1,v_2) = \frac{v_1 \cdot v_2}{||v_1||||v_2||} = \frac{v_1 \cdot v_2}{\sqrt{\sum\nolimits_{i=1}^{n} v_{1,i}^{2}} \cdot \sqrt{\sum\nolimits_{i=1}^{n} v_{2,i}^{2}}}$$

Use the binarized vectors when computing the cosine similarity.  We provide a few of the calculations for you, fill in the similarities for $\texttt{sim}(M1, M2)$, $\texttt{sim}(M1, M5)$, and $\texttt{sim}(M5, M6)$.

Note this is a symmetric matrix, that is $\texttt{sim}(M1,M2) = \texttt{sim}(M2,M1)$.

|    | M1 | M2 |            M3 |           M4 |           M5 |          M6 |
|----|---:|---:|--------------:|-------------:|-------------:|------------:|
| M1 |  1 |  0 |            ?? |            0 |           ?? |         0.6 |
| M2 |    |  1 | -0.29 | 0.37 | 0.67 |           0 |
| M3 |    |    |             1 | -0.47 |            0 | 0.16 |
| M4 |    |    |               |            1 |            0 |           0 |
| M5 |    |    |               |              |            1 |          ?? |
| M6 |    |    |               |              |              |           1 |

$\texttt{sim}(M1, M3)$ = ??

$\texttt{sim}(M1, M5)$ = ??

$\texttt{sim}(M5, M6)$ = ??

### Step 3: Compute New User's Ratings

Based on the New User's provided ratings for movies 3, 4, and 5, predict how they would rate movies 1, 2, and 6.

We will take the weighted average over the binarized ratings of all movies rated by the new user.  We will weigh on similarity.

For example the predicted rating for movie 2 is:

$\texttt{Rating}(M2) = \texttt{sim}(M2,M3) \cdot \texttt{binarized rating M3} + $
$\texttt{sim}(M2,M4) \cdot \texttt{binarized rating M4} + $
$\texttt{sim}(M2,M5) \cdot \texttt{binarized rating M5}$

$\texttt{Rating}(M2) = (-0.29)(1) + (0.37)(-1) + (0.67)(1) = 0.01$

Now you calculate for M1 and M6.

### Step 4: Recommend a Movie

Now that we have the expected ratings of the user for the movies they have not seen we need to actually recommend a movie.  Recommend the movie with the highest predicted rating!

## Part 3: Getting started with Github

If you don't have a Github account yet, make one [here](https://github.com/)!  **Everyone in your group will need a GitHub account.**  We have found that chrome and edge work better for this than safari due to GitHub's unique captcha test.  After account creation any browser should work fine!

Also be sure to install [Github desktop](https://desktop.github.com) which will be our tool of choice for connecting to GitHub and sending/recieving updates to our codebase.

### Step 1: Creating a Private Repository

Only one person in your group needs to do this.  Navigate to the [new repository creation page](https://github.com/new) on GitHub.

![Create New Repo](img/GitHubRepoCreation.png)

You can call the repository whatever you'd like but be sure to mark it as a `Private` repository since we'll be storing your project code here.

Leave the rest of the settings to their defaults and click `Create repository`.

### Step 2: Inviting Collaborators

Click the `Invite collaborators` button to invite your group partners to the repository.

![Invite collaborators by selecting the invite collaborators button](img/InviteCollaborators.png)

Under `Manage access` click `Add people`.  Search for your partners and add them to the repository.  You can add them with `Admin` access.  They will need to check their email for an invitation.

### Step 3: Setting up GitHub Desktop

**All Group Members should follow this step!**

Open up Github Desktop which you can download [here](https://desktop.github.com).

Sign in to GitHub Desktop.  There may be a button to do so when you first open it, but if not you can still sign in through the menu.  If you are on mac, click `GitHub Desktop` and then `Settings`.

![Settings Mac](img/GithubDesktopSettingsMac.png)

Then click `Sign Into GitHub.com`.

If you are on Windows, click `File` then `Options`

![Options Windows](img/GithubDesktopSettingsWindows.png)

Then click `Sign In` under `GitHub.com`, not GitHub Enterprise.

Sign in with your browser and enable the permissions requested for GitHub Desktop.

### Step 4: Setting up your Local Repository

If this is your first time using Github Desktop you may see a `Clone a repository from the internet` button

![Clone a repository from the internet](img/GitHubDesktopClone.png)

Otherwise you'll find the `Clone` button under `Current Repository`, `Add`, `Clone Repository`

There you'll find the option to clone `Your Repositories` from GitHub.com.  You'll also find repositories that you've been added as a collaborator to.

Choose to `Clone`

## Part 4: Setting Group Norms

We are giving you 5 points for just taking some time now to communicate expectations for this project with your group!  Please follow this [discussion guide](https://docs.google.com/document/d/12G0x5YhdnO6qaR2RHvKaTGUG6k2pYj3-PRLZSE0BsPY/edit), answer the questions, and **submit to gradescope by 11:59PM tonight**!  Everyone needs to submit individually in your group.
