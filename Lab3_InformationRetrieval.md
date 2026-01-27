# Lab 3: Information Retrieval (Week 4)

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

**Question 6.** Precision and recall can be applied to predictions in many real-world applications beyond standard search. However, in real applications there is a tension (tradeoff) between precision and recall, and we often need to decide whether to prioritize one or the other:
* If you prioritize precision for a task, the classifier will minimize false positives, meaning it will try not to mis-identify negative examples as positive.
* If you prioritize recall, the classifier will minimize false negatives, meaning it will try not to mis-identify positive examples as negative.

Pick a real-world application (one from the list below or one of your own) and make an argument for which metric you would prioritize and what the "tension" or tradeoff is in that specific case.

* **Medical Diagnosis**: An AI that screens X-rays for early signs of a rare but aggressive lung disease.
* **Criminal Justice**: A recidivism risk assessment tool used by judges to determine bail amounts.

```
# Your answer here
```


**Question 7.** Modern AI has shifted from just finding existing documents (Information Retrieval) to creating new content based on learned patterns (Generative AI). This introduces new ethical questions regarding how a system represents historical or social facts.

##### The Pope Example
* **Standard Search**: If you perform a Google Search for "The Pope," the top results will almost exclusively show the current Pope and his predecessors. These results are highly accurate in a literal sense.
* **Generative AI**: In February 2024, Google’s Gemini chatbot faced scrutiny for how it handled historical prompts. You can read the background in this Economist article: [Is Google’s Gemini chatbot woke by accident, or design?](https://www.economist.com/united-states/2024/02/28/is-googles-gemini-chatbot-woke-by-accident-or-design)
* **The Result**: When asked to "generate an image of a Pope," the model provided a diverse range of results, including people of different ethnicities and genders. While this was a deliberate attempt to avoid repeating an alleged demographic bias found in training datasets, it also resulted in historical inaccuracies.

##### The "Mirror" vs. "Aspiration" Framework 
One way to think about this is whether a search engine should act as a "mirror" (reflecting the world exactly as it is/was) or as an "aspiration" (deliberately boosting diverse results to provide a more inclusive view).

**Question 7a.** Compare the results of a Google Search for "The Pope" with the generative AI example above. How does either approach affect the searcher’s potential intent?
```
# Your answer here
```
**Question 7b.** In which specific scenarios would you opt for the mirror approach? In which would you opt for aspiration?

```
# Your answer here
```
**Question 7c.** The Stereotype Trap: Sometimes, in trying to be "diverse," AI models fall into a stereotype trap—returning the same specific visual style or average representation every time a certain group is mentioned. Does this repetition of stereotypes actually solve the problem of representation, or does it create a new form of bias?
```
# Your answer here
```

We will now go back to the whole class and discuss group answers for Part 2 in a plenary session.
