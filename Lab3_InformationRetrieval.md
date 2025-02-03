# Week 5: Group Exercises on Information Retrieval

<sub><sup>*created by cs124 staff team, winter 2025*</sup></sub>

## Part 1: IR with tf-idf weighting

First, form a group of 3 students to work together! Introduce yourselves to one another.

Imagine you are using a simple IR system. It has the following term-document count matrix:

   | Term    | Doc1 | Doc2 |
   |:--------|:----:|:----:|
   | apple   | 3    | 0    | 
   | phone   | 0    | 2    | 
   | fruit   | 2    | 0    | 

You want to manually verify whether Doc1 or Doc2 will be ranked higher for the one-word query “apple”, given these counts for the (only) 2 documents in the corpus. You will do this by computing the tf-idf cosine between the query and Doc1, and the cosine between the query and Doc2, and choose the highest value.

You will use the following equation:

   ![tf-idf simplified equation](tf_idf_simple_equation.png)


This tf-idf cosine score equation is explained in the [Jurafsky textbook](https://web.stanford.edu/~jurafsky/slp3/14.pdf) (Chapter 14, pages 4-6). Here is an [example](https://docs.google.com/spreadsheets/d/12GBxGoEST_m9GpLGbwmQTHDMaoxPAr7mRaU1UBh9EQo/edit?usp=sharing) of how to compute the tf-idf score for the example outlined in the textbook on page 5 of Chapter 14. This example uses Excel formulas to implement the math between columns (e.g. tf-idf is the product of the tf and idf columns).

Once this example makes sense to you, make a **copy** of [this spreadsheet](https://docs.google.com/spreadsheets/d/1cVnvF6pELowuuMkAvZLHMHaIJyd46yqjbSQ9PTi-mX8/edit?usp=sharing), which has the term and document counts for the IR system we’ve outlined above. You will need to fill in the Excel formulas to implement the math between columns. Use the above formula.

1. Which document is returned for your one-word query, “apple”, and what is the cosine?

Now, imagine the IR system has been tracking and logging your previous queries. The last query you searched was “new phone”. In a simplified version of personalized search, the IR system adds “phone” to your one-word query under the hood, so that the final query used is “apple phone”.

2. Which document is returned for the two-word query, “apple phone”, and what is the cosine?

We will now go back to the whole class and discuss group answers for Part 1 in a plenary session.

## Part 2: Precision and Recall

For this next part, have someone take notes for your group so that you are prepared to share. 

Here's a quick refresher on the definition of precision and recall, from the [Jurafsky textbook](https://web.stanford.edu/~jurafsky/slp3/4.pdf) (Chapter 4, page 12):

Precision measures the percentage of the items that the system detected (i.e., the system labeled as positive) that are in fact positive. It asks the question, "_Out of everything the model identified as positive, how many were actually positive?_ Precision is defined as:

   ![precision-defn](precision_equation.png)

Recall measures the percentage of items actually present in the input that were correctly identified by the system. It asks the question, _"Out of all the actual positives, how many did the model catch?"_ If there are false negatives (positives that the model missed), recall will decrease. Recall is defined as:

   ![recall-defn](recall_equation.png)

Now, imagine you have a more sophisticated IR system than the one in Part 1, with far more query terms and documents. You are tasked with evaluating its performance and how it might be used for different searches. 

For a particular query, your system returns 8 relevant documents and 10 non-relevant documents. There are a total of 20 relevant documents in the collection.

5. What is the precision of the system on this search, and what is its recall? 

Precision and recall are relevant metrics not just in IR tasks, but also, in broader NLP tasks. These metrics can have significant consequences not just on the performance of your system, but also on the social impact of your system. For example, let’s consider the role of precision and recall when evaluating a hate speech classifier.

Suppose you implement a hate speech classifier which classifies a Reddit comment as either “toxic” or “benign”. You define true positives, true negatives, false positives, and false negatives as follows (this is sometimes called a confusion matrix):

   | Term                 | Actual positive | Actual negative |
   |:---------------------|:---------------:|:---------------:|
   | Predicted positive   | True Positive: a comment classified as toxic that is actually toxic    | False Positive: a comment classified as toxic that is actually benign    | 
   | Predicted negative   | False Negative: a comment classified as benign that is actually toxic    | True Negative: a comment classified as benign that is actually benign    | 

You are trying to decide whether to prioritize precision vs. recall for your system.

If you prioritize precision, your classifier will minimize *false positives*, meaning it will try not to mis-identify benign speech as toxic. This means the comments you classify as toxic are likely to be actually toxic – but you might miss some toxic comments.

If you prioritize recall, your classifier will minimize *false negatives*, meaning it will try not to mis-identify toxic speech as benign. This means your classifier will correctly classify most of the existing toxic comments as toxic, but might be over-eager, and classify benign comments as toxic as well.

7. Discuss the tradeoff between precision and recall in the hate speech classifier, which you would prioritize, and why.

Now that you’ve played around with definitions of precision and recall, it’s time to come up with your own scenarios.

8. Write a scenario related to an IR or broader NLP task where you would need to prioritize *either* precision over recall or recall over precision. Clearly define what constitutes a “true positive” in your scenario, as in the examples above. EXplain why the chosen metric is more important. Some domains you might consider writing scenarios about include: criminal justice applications (like recidivism risk assessments), medical diagnoses, or loan approval systems. 

We will now go back to the whole class and discuss group answers for Part 2 in a plenary session.