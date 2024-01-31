# Week 5: Solutions to Group Exercises on Information Retrieval

## Part 1: IR with tf-idf weighting

First, form a group of 3 students to work together! Introduce yourselves to one another.

Imagine you are using a simple IR system. It has the following term-document count matrix:

   | Term    | Doc1 | Doc2 |
   |:--------|:----:|:----:|
   | apple   | 3    | 1    | 
   | phone   | 0    | 2    | 
   | fruit   | 2    | 0    | 

You want to manually verify whether Doc1 or Doc2 will be ranked higher for the one-word query “apple”, given these counts for the (only) 2 documents in the corpus. You will do this by computing the tf-idf cosine between the query and Doc1, and the cosine between the query and Doc2, and choose the highest value.

You will use the following equation:

   ![tf-idf simplified equation](tf_idf_simple_equation.png)


* Note that this is a **simplified version** of the regular tf-idf cosine score. The regular tf-idf cosine score is explained in the [Jurafsky textbook](https://web.stanford.edu/~jurafsky/slpdraft/14.pdf) (Chapter 14, pages 4-6). (The simplified version we are having you compute is sometimes called the ltc.lnn weighting variation, using the SMART notation defined in the [Manning textbook](https://site.ebrary.com/lib/stanford/docDetail.action?docID=10240274), although you don’t have to remember that). This version omits all the grayed out components from the regular score.

Here is an [example](https://docs.google.com/spreadsheets/d/1GI3yJCODven4HAY--tGCpOVPGvcamSjYWKDRbjfzFhQ/edit?usp=sharing) of how to compute the *regular* tf-idf score for the example outlined in the textbook on page 5 of Chapter 14. This example uses Excel formulas to implement the math between columns (e.g. tf-idf is the product of the tf and idf columns).

Once this example makes sense yo you, fill out this spreadsheet [TODO], which has the term and document counts for the IR system we’ve outlined above. You will need to fill in the Excel formulas to implement the math between columns. Use the above formula. (Hint: your formulas should be very similar to those in the textbook example, but some computations can be omitted based on the parts of the equation above that are grayed out.)

1. Which document is returned for your one-word query, “apple”, and what is the cosine?
   ```
   Solution below:
   ```

   Let's start with our query "apple".
   
   |  term   | tf-raw |     tf-wt      |
   |---------|--------|----------------|
   | apple   |   1    | 1 + log(1) = 1 |
   
   We don't need to do anything else for the query vector (e.g. normalize) because we are simply using lnn weights.
   
   Now let's do our calculations for Doc 1.
   
   |  term  | tf-raw |       tf-wt        | df |       idf        |        tf-idf         |                  normalized                   |
   |--------|--------|--------------------|----|------------------|-----------------------|-----------------------------------------------|
   | apple  |   3    | 1 + log(3) = 1.477 | 1  | log(2/1) = 0.301 | 1.477 x 0.301 = 0.445 | 0.445 / sqrt(0.445^2 + 0^2 + 0.392^2) = 0.750 |
   | phone  |   0    | 0                  | 1  | log(2/1) = 0.301 | 0 x 0.301 = 0         | 0 / sqrt(0.445^2 + 0^2 + 0.392^2) = 0         |
   | fruit  |   2    | 1 + log(2) = 1.301 | 1  | log(2/1) = 0.301 | 1.301 x 0.301 = 0.392 | 0.392 / sqrt(0.445^2 + 0^2 + 0.392^2) = 0.661 |
   
   Now calculations for Doc 2.
   
   |  term  | tf-raw |       tf-wt        | df |       idf        |          tf-idf       |           normalized                  |
   |--------|--------|--------------------|----|------------------|-----------------------|---------------------------------------|
   | apple  |   0    | 0                  | 1  | log(2/1) = 0.301 | 0 x 0.301 = 0         | 0 / sqrt(0^2 + 0.392^2 + 0^2) = 0     |
   | phone  |   2    | 1 + log(2) = 1.301 | 1  | log(2/1) = 0.301 | 1.301 x 0.301 = 0.392 | 0.392 / sqrt(0^2 + 0.392^2 + 0^2) = 1 |
   | fruit  |   0    | 0                  | 1  | log(2/1) = 0.301 | 0 x 0.301 = 0         | 0 / sqrt(0.282^2 + 0^2) = 0           |
   
   Now for the cosine similarity of each document, we take the dot product of the query vector and the normalized 
   vector for that document. We get the following:
   
   `Doc1: (1 x 0.750)  = 0.750`
   
   
   `Doc2: (1 x 0) = 0`
   
   The highest score belongs to Doc1, so this is the returned document with a cosine of 0.750.

Now, imagine the IR system has been tracking and logging your previous queries. The last query you searched was “new phone”. In a simplified version of personalized search, the IR system adds “phone” to your one-word query under the hood, so that the final query used is “apple phone”.

2. Which document is returned for the two-word query, “apple phone”, and what is the cosine? (Hint: you should only have to change one cell in your spreadsheet to get the new answer.)
   ```
   Solution below:
   ```

   Our new query is "apple phone", so we only need to change one cell in our sheet: the count of phone in the query.
   
   |  term   | tf-raw |     tf-wt      |
   |---------|--------|----------------|
   | apple   |   1    | 1 + log(1) = 1 |
   | phone   |   1    | 1 + log(1) = 1 |
   
   Keeping the rest of the sheet the same, we again take the dot product of the query vector and the normalized 
   vector for each document. We get the following:
   
   `Doc1: (1 x 0.750) + (1 x 0)  = 0.750`
   
   `Doc2: (1 x 0) + (1 x 1) = 1`
   
   Now, the highest score belongs to Doc2, so this is the returned document with a cosine of 1. This cosine value should make sense
   to you, given the document!

Search engines commonly track our queries in order to personalize search results and maximize relevance. However, they frequently do this without us knowing. For the following three questions, write your answers down and be prepared to share.

3. What are the potential benefits and risks of personalized searches? 

```
Some potential benefits:
- Improved search quality: more easily find the information you're looking for

Some risks:
- Personalized/targeted ads, which may sometimes mischaracterize you and show you things you don't want to be  shown
- Overpersonalization: users seeing the same content over and over again, with no opportunities to expand their horizons.
```

4. Are there certain kinds of data that are OK for search engines to use? If so, what are they, and why is it OK for search engines to use them? What is the limit to the kinds of data that search engines should be allowed to use to personalize our searches?
```
TODO
```

5. Some search engines allow users to make private searches by not recording user queries or tracking users at all (e.g., DuckDuckGo). What are the potential benefits, limitations, and consequences of these private search engines? 
```
TODO
```

We will now go back to the whole class and discuss group answers for Part 1 in a plenary session.


## Part 2: Precision and Recall


For this next part, have someone take notes for your group so that you are prepared to share. 

Imagine you have a more sophisticated IR system than the one in Part 1, with far more query terms and documents. You are tasked with evaluating its performance and how it might be used for different searches. 

For a particular query, your system returns 8 relevant documents and 10 non-relevant documents. There are a total of 20 relevant documents in the collection.

6. What is the precision of the system on this search, and what is its recall? 

```
Precision: 8/18 (or 4/9)
Recall: 8/20 (or 2/5)
```

You know that people will be using your IR system primarily to search for treatments to medical ailments they face. You are trying to decide whether you should improve the recall of your system. Suppose we define a true positive document as: a document which does not just describe the relevant ailment, but also, which contains accurate treatment information for this ailment. 

7. Given the above context, answer the following and be prepared to explain:
      
<ol type="a">
   <li>Why might you prioritize high precision in this task?
      <code>I might prioritize precision to make sure the documents I return are likely to be relevant to the user's need, because I think the purpose of my system is for users to get accurate and relevant treatment information. I run the risk of missing a few relevant documents.</code>
   </li>
   <li>Why might you prioritize high recall in this task?
      <code>I might prioritize recall so I don't miss any documents that might be relevant, because I think the purpose of my system is for users to be able to see as much relevant treatment information as possible. I run the risk of returning a few irrelevant documents.</code>
   </li>
</ol>

Precision and recall are relevant metrics not just in IR tasks, but also, in broader NLP tasks. These metrics can have significant consequences not just on the performance of your system, but also on the social impact of your system. For example, let’s consider the role of precision and recall when evaluating a hate speech classifier.

[TODO rest of 8]

8. Discuss the tradeoff between precision and recall in the hate speech classifier, which you would prioritize, and why.
```
Arguments for either are valid:
Precision - Avoid censoring people's benign speech
Recall - Ensure that the majority of existing hate speech is correctly flagged
```
   
Now that you’ve played around with definitions of precision and recall, it’s time to come up with your own scenarios.

9. Write a scenario related to an IR or broader NLP task where you would prioritize *precision* over recall. Make sure you define what a “true positive” is, as in the examples above.
```
Any answer with justification is valid.
Examples are: criminal justice applications, like recidivism risk assessments
(scenarios where false positives could be costly).
```

10. Write a scenario related to an IR or broader NLP task where you would prioritize *recall* over precision. Make sure you define what a “true positive” is, as in the examples above.
```
Any answer with justification is valid.
Examples are: medical diagnoses, loan approval systems, content moderation
(scenarios where false negatives could be costly).
```

We will now go back to the whole class and discuss group answers for Part 2 in a plenary session.

## Part 3: The Right to be Forgotten

[TODO add]

```
Answer TODO
```

We will now go back to the whole class and discuss group answers for Part 3 in a plenary session.