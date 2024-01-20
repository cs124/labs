# Week 3: Group Exercises on Naive Bayes and Sentiment

## Part 1: Naive Bayes review

First, form a group of 3 students to work together!

We want to build a naive bayes sentiment classifier using add-1 smoothing, as described in the lecture (not binary naive bayes, regular naive bayes). Our training and testing data consist of comments on a social media platform. Here is our training and testing corpus:

**Training Set**

    - a cat ruined my sweater
    - hate that cat
    + my friends rock

**Test Set**

    adore my cat

Answer the questions below given the sets above.

1. Compute the prior for the two classes + and -, and the likelihood for each word that appears in the test sentence given either class.
   ```
   |V| = 9, n- = 8, n+ = 3
   P(-) = 2/3, P(+) = 1/3
   P(adore | - ) = unk
   P(adore | + ) = unk
   P(my | - ) = (1 + 1) / (n- + |V|) = (1 + 1) / (8 + 9) = 2/17
   P(my | + ) = (1 + 1) / (n+ + |V|) = (1 + 1) / (3 + 9) = 2/12
   P(cat | - ) = (2 + 1) / (n- + |V|) = (2 + 1) / (8 + 9) = 3/17
   P(cat | + ) = (0 + 1) / (n+ + |V|) = (0 + 1) / (3 + 9) = 1/12
   ```

2. Then compute whether the sentence in the test set is of class positive or negative (you may need a computer for this final computation).
   ```
   C = {+, -}
   P(c | "adore my cat") ∝ P(c) * P ("adore my cat" | c)
   = P(c) * P(adore | c) * P(my | c) * P(cat | c) ~= P(c) * P(my | c) * P(cat | c), 'adore' is unknown
   P( - | "adore my cat") ∝ (2/3) * (2/17) * (3/17) = 0.01384
   P( + | "adore my cat") ∝ (1/3) * (2/12) * (1/12) = 0.00463
   P( - | "adore my cat") is greater, so the test set sentence is classified as class negative.
   ```

3. Do you think that the predicted class is the correct sentiment of this sentence? Explain, in your own words, what about the Naïve Bayes classifier led it to the correct or incorrect answer. What assumptions made by Naïve Bayes led to this result? Identify a specific assumption and be prepared to justify.
   ```
   We visually understand the sentence as having positive sentiment, but the limitations of the Naive Bayes classifier lead to it being classified as negative. Specifically, since "cat" shows up twice in the negative set and never in the positive set, test sentences containing "cat" are more likely to be classified as negative. One assumption that leads to this behavior is Bag of Words-- Naive Bayes ignores sentence semantics and simply looks at word counts, so it might miss more nuanced aspects of language.
   ```

4. Why do you add |V| to the denominator of add-1 smoothing, instead of just counting the words in one class?
   ```
   In add-1 smoothing we assume we have seen each word once regardless of whether they appear in the original class or not and thus add |V| to the denominator. 
   Note that words that do not appear in the training set are unk and we just pretend they aren't there, and they are not included in the vocab. 
   ```

5. Suppose "cat" was a demographic attribute like "woman" or "engineer" or "Asian”. What harms from the lecture could this result in, and why?
   ```
   Representational (harms caused by a system that demeans a social group).
   ```

We will now go back to the whole class and discuss group answers for Part 1 in a plenary session.

## Part 2: Naive Bayes Toxicity Classification

For the following problem, please choose a group facilitator/representative who will also take notes on your discussion.

6. Imagine you’re working for Reddit. Toxic comments are a real problem, so you decide to build a sentiment classifier using Naive Bayes and an automoderator that deletes negative comments. Your classifier trains on these examples:

**Training Set**

    - the dangerous bears
    - bears destroyed the yard
    + beautiful redwood trees

   The week after training, the Big Game (between Stanford and Cal) happens. Users on /r/bayarea are excited. Alice, a Cal student, wants to support her team and comments `“Go bears!”`. Bob, a Stanford student, wants to support his team and comments `“Go trees!”`. Answer the following questions:

   <ol type="a">
      <li>Is Alice’s comment deleted by your automoderator?
         <code>Yes -- the presence of "bears" means it is classified as negative.</code>
      </li>
      <li>Is Bob’s comment deleted by your automoderator?
         <code>No -- the presence of "trees" means it is classified as positive.</code>
      </li>
      <li>Which two groups of people is your automoderator treating differently, even though membership in these groups doesn’t have a clear connection to comment toxicity?
         <code>Cal students (people who use "bears") and Stanford students (people who use "trees") are being treated differently.</code>
      </li>
      <li>What kind of harm from the lecture is experienced by the group whose comments are getting deleted?
         <code>Representational harms, same as Part 1 (harms caused by a system that demeans a social group).</code>
      </li>
      <li>Having realized the harm from part (d) that your automoderator is causing, you change it so that negative comments are not deleted. Instead, they are just highlighted in red and marked “negative”. Now, what kind of harm from lecture is the aforementioned group experiencing?
         <code>Censorship harms (incorrectly flagging non-toxic sentences that simply
         mention identities).</code>
      </li>
      <li>You are now trying to improve the automoderator to reduce this type of harm from occurring in the future. As a group, describe 3 distinct problems in the automoderator that led to this harm.
         <code>Potential answers:
         1. Using a sentiment analysis tool to analyze toxicity when sentiment and toxicity are not the same thing
         2. Toxicity is contextual. However, there was no consideration of context in the example (e.g., "Bears" might be likely to mean something else in the r/bayarea subreddit vs. the r/camping subreddit)
         3. Naive bayes is relatively simple, it doesn't consider language semantics.
         </code>
      </li>
      <li>As a group, describe a plan for how you might fix one of the problems you described in part f. If you don’t have any ideas, explain why this seems hard. For whatever question you answer, your group should write at least 2-4 sentences.
         <code>Answers may vary. One answer is that it is hard to differentiate between the contexts of different subreddits on a specific word, when using one classifier for all contexts, meaning mis-classification is almost guaranteed in some context.</code>
      </li>
   </ol>


   We will now go back to the whole class and discuss group answers for Part 2 in a plenary session.

## Part 3: Performance Disparities

For the last problem, please continue taking notes on your discussion.

7. Your manager at Reddit comes to you and asks you to completely scrap the Naïve Bayes sentiment classifier in the automoderation system. Instead, you are to use an entirely new, state of the art, *toxicity* classifier that is trained on the entire internet. Your manager believes this will solve all the harms introduced by the Naïve Bayes classifier. But you know otherwise. To convince your manager that things aren’t so simple, find a subreddit where harm might be more likely to occur because of a performance disparity in the general classifier and explain why in 2-4 sentences. 

```
Answers may vary. Any subreddit with a less-common language, vernacular, or context will work here.
```

We will now go back to the whole class and discuss group answers for Part 3 in a final plenary session.