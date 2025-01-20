# Lab 2: Naive Bayes, Sentiment, and Harms of Classification (Week 3)

<sub><sup>*written by uma phatak, veronica rivera & jeong shin, cs124 staff team, winter '24/'25*</sup></sub>

## Part 1: Naive Bayes review

First, form a group of 3 students to work together!

We want to build a naive bayes sentiment classifier using add-1 smoothing, as described in the lecture (not binary naive bayes, regular naive bayes). Our training and testing data consist of comments on a social media platform. Here is our training and testing corpus:

**Training Set**

    - the dangerous bears
    - bears destroyed the yard
    + the beautiful redwood trees

**Test Set**

    the friendly bears rest

Answer the questions below given the sets above.

1. Compute the prior for the two classes + and -, and the likelihood for each word that appears in the test sentence given either class.

2. Then compute whether the sentence in the test set is of class positive or negative (you may need a computer for this final computation).

3. Do you think that the predicted class is the correct sentiment of this sentence? Explain, in your own words, what about the Naïve Bayes classifier led it to the correct or incorrect answer. What assumptions made by Naïve Bayes led to this result? Identify a specific assumption and be prepared to justify.

4. Why do you add |V| to the denominator of add-1 smoothing, instead of just counting the words in one class?

We will now go back to the whole class and discuss group answers for questions 3 and 4 form part 1 in a plenary session.

## Part 2: Naive Bayes Toxicity Classification

For the following problem, please choose a group facilitator/representative who will also take notes on your discussion.

5. Imagine you’re working for Reddit. Toxic comments are a real problem, so you decide to build a sentiment classifier using Naive Bayes and an automoderator that deletes negative comments. Your classifier trains on the same training set from Part 1:

**Training Set**

    - the dangerous bears
    - bears destroyed the yard
    + the beautiful redwood trees

   The week after training, the Big Game (between Stanford and Cal) happens. Users on /r/bayarea are excited. Alice, a Cal student, wants to support her team and comments `“Go bears!”`. Bob, a Stanford student, wants to support his team and comments `“Go trees!”`. Answer the following questions. Please keep your responses to parts (a) through (c) concise, while we encourage more in-depth discussions for parts (d) and (e).
   <ol type="a">
      <li>Whose comment, Alice's or Bob's, is deleted by your automoderator, and what kind of harm (from the lecture) are they experiencing?</li>
      <li>Having realized the harm that your automoderator is causing, you change it so that negative comments are not deleted. Instead, they are just highlighted in red and marked “negative”. Now, what kind of harm from lecture is the aforementioned group experiencing?</li>
      <li>You're now trying to improve the automoderator to reduce this type of harm from occurring in the future. As a group, describe 3 distinct problems in the automoderator that led to this harm.</li>
      <li>As a group, describe a plan for how you might fix one of the problems you described in the prior part. If you don’t have any ideas, explain why this seems hard. For whatever question you answer, be prepared to share.</li>
   </ol>


   We will now go back to the whole class and discuss group answers for Part 2 in a plenary session.