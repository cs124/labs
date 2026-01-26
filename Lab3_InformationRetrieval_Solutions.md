# Week 4: Solutions to Group Exercises on Information Retrieval

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
0.750
```
You can find the calculations in [this spreadsheet](https://docs.google.com/spreadsheets/d/1bGQbz7Ojwa4_h6Nga310u3zl916KjE2-3IsBPoK9ksI/edit?usp=sharing) or written below:

   For our query "apple":
   
   |  term  | tf-raw |       tf-wt        | df |       idf        |        tf-idf         |                  normalized                   |
   |--------|--------|--------------------|----|------------------|-----------------------|-----------------------------------------------|
   | apple   |   1    | 1 + log(1) = 1 | 1  | log(2/1) = 0.301 | 1 x 0.301 = 0.301 | 0.301 / sqrt(0.301^2 + 0^2 + 0^2) = 1 |
   
   For Doc1:
   
   |  term  | tf-raw |       tf-wt        | df |       idf        |        tf-idf         |                  normalized                   |
   |--------|--------|--------------------|----|------------------|-----------------------|-----------------------------------------------|
   | apple  |   3    | 1 + log(3) = 1.477 | 1  | log(2/1) = 0.301 | 1.477 x 0.301 = 0.445 | 0.445 / sqrt(0.445^2 + 0^2 + 0.392^2) = 0.750 |
   | phone  |   0    | 0                  | 1  | log(2/1) = 0.301 | 0 x 0.301 = 0         | 0 / sqrt(0.445^2 + 0^2 + 0.392^2) = 0         |
   | fruit  |   2    | 1 + log(2) = 1.301 | 1  | log(2/1) = 0.301 | 1.301 x 0.301 = 0.392 | 0.392 / sqrt(0.445^2 + 0^2 + 0.392^2) = 0.661 |

   For the cosine similarity, we take the dot product of the normalized vector for the query and the normalized vector for Doc1, and get `(1 x 0.750)  = 0.750`.

We’ll pause here to check our calculations so far as a class.

**Question 2.** Finish filling in the spreadsheet. Once you are done, determine which document is returned for your one-word query, “apple”, and what the cosine similarity is.

 ```
Doc1; 0.750
```
You can find the calculations in [this spreadsheet](https://docs.google.com/spreadsheets/d/1bGQbz7Ojwa4_h6Nga310u3zl916KjE2-3IsBPoK9ksI/edit?usp=sharing) or written below:

 For Doc2:
   
   |  term  | tf-raw |       tf-wt        | df |       idf        |          tf-idf       |           normalized                  |
   |--------|--------|--------------------|----|------------------|-----------------------|---------------------------------------|
   | apple  |   0    | 0                  | 1  | log(2/1) = 0.301 | 0 x 0.301 = 0         | 0 / sqrt(0^2 + 0.392^2 + 0^2) = 0     |
   | phone  |   2    | 1 + log(2) = 1.301 | 1  | log(2/1) = 0.301 | 1.301 x 0.301 = 0.392 | 0.392 / sqrt(0^2 + 0.392^2 + 0^2) = 1 |
   | fruit  |   0    | 0                  | 1  | log(2/1) = 0.301 | 0 x 0.301 = 0         | 0 / sqrt(0.282^2 + 0^2) = 0           |
   
   For the cosine similarity, we take the dot product of the normalized vector for the query and the normalized vector for Doc2, and get `(1 x 0) = 0`. As we'll recall from **Question 1**, the cosine similarity for Doc1 was 0.750, which is higher, so Doc1 is the returned document.

We will now reconvene as a class to discuss our calculations.

### B. tf-idf vs. Dense Retrieval

Now imagine you're using a dense retrieval system. Suppose the query is still “apple”, and the system is ranking the following two documents: “apple fruit” and “pineapple fruit."

**Question 3.** Explain why the dense retrieval system might assign a non-zero similarity to “pineapple fruit” for the query “apple,”  while our simple IR system would assign it little or no score.

```
Dense retrieval represents words and documents as vectors in a space where semantic similarity (similarity in
meaning) is reflected by closeness. Because “pineapple” and “apple” are both fruits and have related meanings,
their vectors are near each other, so the cosine similarity between the query vector (“apple”) and the document
vector (“pineapple fruit”) is greater than zero. In contrast, tf-idf only counts exact word matches, so
“pineapple” does not match “apple” and contributes nothing to the score.
```

**Question 4.** What does this example illustrate about what dense retrieval captures that tf-idf does not?

```
This example illustrates that dense retrieval captures semantic similarity beyond exact word matches. It
can recognize that words with similar meanings (like “apple” and “pineapple”) are related, while tf-idf only
captures literal word overlap. Dense retrieval can therefore retrieve relevant documents even if they do not
contain the exact query terms.
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
Precision: 8/18 (or 4/9)
Recall: 8/20 (or 2/5)
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
Arguments for either are valid:
Precision - Avoid censoring people's benign speech
Recall - Ensure that the majority of existing hate speech is correctly flagged
```

Now that you’ve played around with definitions of precision and recall, it’s time to come up with your own scenarios.

**Question 7.** Write a scenario related to an IR or broader NLP task where you would need to prioritize *either* precision over recall or recall over precision. Clearly define what constitutes a “true positive” in your scenario, as in the examples above. Explain why the chosen metric is more important. Some domains you might consider writing scenarios about include: criminal justice applications (like recidivism risk assessments), medical diagnoses, or loan approval systems. 

```
Any answer with justification is valid.
Example scenarios for precision over recall are: criminal justice applications, like recidivism risk assessments (scenarios where false positives could be costly).
Example scenarios for recall over precision are: medical diagnoses, loan approval systems, content moderation (scenarios where false negatives could be costly).
```

We will now go back to the whole class and discuss group answers for Part 2 in a plenary session.

