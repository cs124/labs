# Week 5: Group Exercises on Information Retrieval

<sub><sup>*written by julia biswas & isha sinha, cs124 staff team, winter '25/'26*</sup></sub>

## Part 1: tf-idf Weighting and Dense Retrieval

First, form a group of 3 students to work together! Introduce yourselves to one another.

### A. tf-idf Ranking

Imagine you are using a simple IR system. It has the following term-document count matrix:

   | Term    | Doc1 | Doc2 |
   |:--------|:----:|:----:|
   | apple   | 3    | 0    | 
   | phone   | 0    | 2    | 
   | fruit   | 2    | 0    | 

You want to manually verify whether Doc1 or Doc2 will be ranked higher for the one-word query “apple”, given these counts for the (only) 2 documents in the corpus. You will do this by computing the tf-idf cosine between the query and Doc1, and the cosine between the query and Doc2, and choose the highest value.

You will use the following equation:

   ![tf-idf simplified equation](tf_idf_simple_equation.png)


This tf-idf cosine score equation is explained in the [Jurafsky textbook](https://web.stanford.edu/~jurafsky/slp3/11.pdf) (Chapter 11, pages 4-8). 

[This spreadsheet](https://docs.google.com/spreadsheets/d/1GmcHc-PP0rQuHV_3CVhyHFzE0tBA8DBWr3gE2j7Ke5g/edit?usp=sharing) contains an example of how to compute the tf-idf score for the example outlined in the textbook on page 8 of Chapter 11. Take a look at it to understand how it uses Excel formulas to implement the math between columns (e.g. tf-idf is the product of the tf and idf columns). 

Once this example makes sense to you, make a **copy** of [this spreadsheet](https://docs.google.com/spreadsheets/d/1cVnvF6pELowuuMkAvZLHMHaIJyd46yqjbSQ9PTi-mX8/edit?usp=sharing), which has the term and document counts for the IR system we’ve outlined above.

**Question 1.** Using the formula given above, fill in the Excel formulas to implement the math between columns for the query ("apple") and Doc1. Once you are done, determine what the cosine similarity is between the query and Doc1.

```
# Your answer here
```

We’ll pause here to check our calculations so far as a class.

**Question 2.** Finish filling in the spreadsheet. Once you are done, determine which document is returned for your one-word query, “apple”, and what the cosine similarity is.

```
# Your answer here
```

We will now reconvene as a class to discuss our calculations.

### B. tf-idf vs. Dense Retrieval

Now imagine you're using a dense retrieval system. Suppose the query is still “apple”, and the system is ranking the following two documents: “apple fruit” and “pineapple fruit."

**Question 3.** Explain why the dense retrieval system might assign a non-zero similarity to “pineapple fruit” for the query “apple,”  while our simple IR system would assign it little or no score.

```
# Your answer here
```

**Question 4.** What does this example illustrate about what dense retrieval captures that tf-idf does not?

```
# Your answer here
```

We will now go back to the whole class and discuss group answers for Part 1B in a plenary session.

## Part 2: Precision and Recall

For this next part, have someone take notes for your group so that you are prepared to share. 

Here's a quick refresher on the definition of precision and recall, from the [Jurafsky textbook](https://web.stanford.edu/~jurafsky/slp3/4.pdf) (Chapter 4, pages 22-23):

Precision measures the percentage of the items that the system detected (i.e., the system labeled as positive) that are in fact positive. It asks the question, _"Out of everything the model identified as positive, how many were actually positive?"_ Precision is defined as:

   ![precision-defn](precision_equation.png)

Recall measures the percentage of items actually present in the input that were correctly identified by the system. It asks the question, _"Out of all the actual positives, how many did the model catch?"_ If there are false negatives (positives that the model missed), recall will decrease. Recall is defined as:

   ![recall-defn](recall_equation.png)

Now, imagine you have a more sophisticated IR system than the one in Part 1, with far more query terms and documents. You are tasked with evaluating its performance and how it might be used for different searches. 

For a particular query, your system returns 8 relevant documents and 10 non-relevant documents. There are a total of 20 relevant documents in the collection.

**Question 5.** What is the precision of the system on this search, and what is its recall?

```
# Your answer here
```

Now, let’s consider precision and recall in a more consequential case. After all, these metrics are relevant not just in IR tasks, but also in broader NLP tasks, where they can have significant consequences not only on system performance, but on the system's social impact as well. For example, let’s examine the role of precision and recall when evaluating a hate speech classifier.

Suppose you implement a hate speech classifier which classifies a Reddit comment as either “toxic” or “benign”. You define true positives, true negatives, false positives, and false negatives as follows (this is sometimes called a confusion matrix):

   | Term                 | Actual positive | Actual negative |
   |:---------------------|:---------------:|:---------------:|
   | Predicted positive   | True Positive: a comment classified as toxic that is actually toxic    | False Positive: a comment classified as toxic that is actually benign    | 
   | Predicted negative   | False Negative: a comment classified as benign that is actually toxic    | True Negative: a comment classified as benign that is actually benign    | 

You are trying to decide whether to prioritize precision vs. recall for your system.

If you prioritize precision, your classifier will minimize *false positives*, meaning it will try not to mis-identify benign speech as toxic. This means the comments you classify as toxic are likely to be actually toxic – but you might miss some toxic comments.

If you prioritize recall, your classifier will minimize *false negatives*, meaning it will try not to mis-identify toxic speech as benign. This means your classifier will correctly classify most of the existing toxic comments as toxic, but might be over-eager, and classify benign comments as toxic as well.

**Question 6.** Discuss the tradeoff between precision and recall in the hate speech classifier, which you would prioritize, and why.

```
# Your answer here
```

Now that you’ve played around with definitions of precision and recall, it’s time to come up with your own scenarios.

**Question 7.** Write a scenario related to an IR or broader NLP task where you would need to prioritize *either* precision over recall or recall over precision. Clearly define what constitutes a “true positive” in your scenario, as in the examples above. Explain why the chosen metric is more important. Some domains you might consider writing scenarios about include: criminal justice applications (like recidivism risk assessments), medical diagnoses, or loan approval systems. 

```
# Your answer here
```

We will now go back to the whole class and discuss group answers for Part 2 in a plenary session.
