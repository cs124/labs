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
   ```
   |V| = 8, n- = 7, n+ = 4
   P(-) = 2/3, P(+) = 1/3
   P(the | - ) = (1 + 2) / (n- + |V|) = (1 + 2) / (7 + 8) = 3/15 = 1/5
   P(the | + ) = (1 + 1) / (n+ + |V|) = (1 + 1) / (4 + 8) = 2/12 = 1/6
   P(friendly | - ) = unseen in training*
   P(friendly | + ) = unseen in training*
   P(bears | - ) = (1 + 2) / (n- + |V|) = (1 + 2) / (7 + 8) = 3/15 = 1/5
   P(bears | + ) = (1 + 0) / (n+ + |V|) = (1 + 0) / (4 + 8) = 1/12
   P(rest | - ) = unseen in training*
   P(rest | + ) = unseen in training*
   * Words in test set that were completely unseen in training are simply removed from the test set. So, we do not compute these probabilities.
   ```

2. Then compute whether the sentence in the test set is of class positive or negative (you may need a computer for this final computation).
   ```
   C = {+, -}
   P(c | "the friendly bears rest") ∝ P(c) * P ("the friendly bears rest" | c)
   = P(c) * P(the | c) * P(friendly | c) * P(bears | c) * P(rest | c) ~= P(c) * P(the | c) * P(bears | c), 'friendly' and 'rest' are unseen in training, so we ignore it in computing probabilities
   P( - | "the friendly bears rest") ∝ (2/3) * (1/5) * (1/5) = 0.02667
   P( + | "the friendly bears rest") ∝ (1/3) * (1/6) * (1/12) = 0.00463
   P( - | "the friendly bears rest") is greater, so the test set sentence is classified as class negative.
   ```

3. Do you think that the predicted class is the correct sentiment of this sentence? Explain, in your own words, what about the Naïve Bayes classifier led it to the correct or incorrect answer. What assumptions made by Naïve Bayes led to this result? Identify a specific assumption and be prepared to justify.
   ```
   We visually understand the sentence as having positive sentiment, but the limitations of the Naive Bayes classifier lead to it being classified as negative. Specifically, since "bears" shows up twice in the negative set and never in the positive set, test sentences containing "bears" are more likely to be classified as negative. One assumption that leads to this behavior is Bag of Words-- Naive Bayes ignores sentence semantics and simply looks at word counts, so it might miss more nuanced aspects of language.
   ```

4. Why do you add |V| to the denominator of add-1 smoothing, instead of just counting the words in one class?
   ```
   In add-1 smoothing we assume we have seen each word once regardless of whether they appear in the original class or not and thus add |V| to the denominator. 
   Note that words that do not appear in the training set are unk and we just pretend they aren't there, and they are not included in the vocab. 
   ```

We will now go back to the whole class and discuss group answers for Part 1 in a plenary session.

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
           <code>Alice's gets deleted -- the presence of "bears" means it is classified as negative. Bob's does not get deleted as the presence of "trees" means it is classified as positive. Censorship harms (incorrectly deleting non-toxic sentences that simply mention identities).</code>
      <li>Having realized the harm that your automoderator is causing, you change it so that negative comments are not deleted. Instead, they are just highlighted in red and marked “negative”. Now, what kind of harm from lecture is the aforementioned group experiencing?</li>
         <code>Representational harms, same as Part 1 (harms caused by a system that demeans a social group).</code>
      <li>You're now trying to improve the automoderator to reduce this type of harm from occurring in the future. As a group, describe 3 distinct problems in the automoderator that led to this harm.</li>
         <code>Potential answers:
         1. Using a sentiment analysis tool to analyze toxicity when sentiment and toxicity are not the same thing
         2. Toxicity is contextual. However, there was no consideration of context in the example (e.g., "Bears" might be likely to mean something else in the r/bayarea subreddit vs. the r/camping subreddit)
         3. Naive bayes is relatively simple, it doesn't consider language semantics.
         </code>
      <li>As a group, describe a plan for how you might fix one of the problems you described in the prior part. If you don’t have any ideas, explain why this seems hard. For whatever question you answer, be prepared to share.</li>
         <code>Answers may vary. One answer is that it is hard to differentiate between the contexts of different subreddits on a specific word, when using one classifier for all contexts, meaning mis-classification is almost guaranteed in some context.</code>
      </li>
   </ol>

   We will now go back to the whole class and discuss group answers for Part 2 in a plenary session.