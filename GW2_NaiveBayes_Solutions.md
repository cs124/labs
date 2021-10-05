# Week 3: Group Exercises on Naive Bayes and Sentiment

## Part 1: Quick Naive Bayes review

First, form a group of 3 students to work together!

We want to build a naive bayes sentiment classifier using add-1 smoothing, as described in the lecture (not binary naive bayes, regular naive bayes). 
Here is our training corpus:

**Training Set**

  ```
  - the movie has no plot
  - honestly pretty boring    
  + pretty interesting movie 
  ```

**Test Set**

  ```
  pretty enjoyable plot
  ```
    
Answer the questions below given the sets above.

  1. Compute the prior for the two classes + and -, and the likelihoods for each word given the class (leave in the form of fractions).

     ```
     |V| = 9, n- = 8, n+ = 3
     P(-) = 2/3, P(+) = 1/3
     P(any_vocab_word_in_-_sentence | - ) = (1 + 1) / (8 + 9) = 2/17, e.g. P('boring' | - )
     P(any_vocab_word_not_in_-_sentence | - ) = (0 + 1) / (8 + 9) = 1/17, e.g. P('interesting' | - )
     P(any_vocab_word_in_+_sentence | + ) = (1 + 1) / (3 + 9) = 2/12, e.g. P('movie' | + )
     P(any_vocab_word_not_in_+_sentence | + ) = (0 + 1) / (3 + 9) = 1/12, e.g. P('boring' | + )
     ```

  2. Then compute whether the sentence in the test set is of class positive or negative (you may need a computer for this final computation).

     ```
     C = {+, -}
     P(c | "pretty enjoyable plot") ∝ P(c) * P ("pretty enjoyable plot" | c)
     = P(c) * P(pretty | c) * P(enjoyable | c) * P(plot | c) ~= P(c) * P(pretty | c) * P(plot | c), 'enjoyable' is unknown
     P( - | "pretty enjoyable plot") ∝ (2/3) * (2/17) * (2/17) = 0.009227
     P( + | "pretty enjoyable plot") ∝ (1/3) * (2/12) * (1/12) = 0.004629
     P( - | "pretty enjoyable plot") is greater, so the test set sentence is classified as class negative. 
     ```

  3. Would using binary multinomial Naive Bayes change anything?

     ```
     No, using binary NB would not change anything - there are no duplicate vocabulary items within the same class. 
     ```

  4. Why do you add |V| to the denominator of add-1 smoothing, instead of just counting the words in one class?

     ```
     In add-1 smoothing we assume we have seen each word once regardless of whether they appear in the original class or not and thus add |V| to the denominator. 
     Note that words that do not appear in the training set are unk and we just pretend they aren't there, and they are not included in the vocab. 
     ```

## Part 2: Conceptual Problems

   5. Ethics Question 1: For discussion! 
      
   6. It is sometimes the case that more complex features (like trigrams or bigrams) perform better than simple features (like unigrams) on the **training** set, but perform worse than simple features on the **test** set. 
      This is a particular case of the phenomenon called `overfitting` in machine learning. 
      Discuss why this might be. 
      Can you create a tiny training set with 2 3-word documents and a test set with one document for which this overfitting situation holds?

      ```
      In overfitting, your model is too complicated for the small amount of data you have, and the model will fit to random patterns in the data.
      So a complex feature might just occur accidentally in the training set, but will give it a very high probability.
      Such a rare `accidental' feature might never occur in the test set, or if it does might simply randomly occur with the other class. 
      ```

   7. Go to the Sentiment demo at http://nlp.stanford.edu:8080/sentiment/rntnDemo.html. 
      Come up with 5 sentences that the classifier gets wrong. 
      Can you figure out what is causing the errors?
      
      ```
      One example that the classifier gets wrong: "I don't not like you." The double negation is interpreted incorrectly.
      ```
   
   8. Ethics Question 2: For discussion!
