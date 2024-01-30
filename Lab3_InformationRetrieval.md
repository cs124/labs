# Week 5: Group Exercises on Information Retrieval

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

Once this example makes sense yo you, fill out this spreadsheet [TODO], which has the term and document counts for the IR system we’ve outlined above. You will need to fill in the Excel formulas to implement the math between columns. Use the ltc.lnn variation of cosine score. (Hint: your formulas should be very similar to those in the textbook example, but some computations can be omitted based on the parts of the equation above that are grayed out.)

1. Which document is returned for your one-word query, “apple”, and what is the cosine?

Now, imagine the IR system has been tracking and logging your previous queries. The last query you searched was “new phone”. In a simplified version of personalized search, the IR system adds “phone” to your one-word query under the hood, so that the final query used is “apple phone”.

2. Which document is returned for the two-word query, “apple phone”, and what is the cosine? (Hint: you should only have to change one cell in your spreadsheet to get the new answer.)

Search engines commonly track our queries in order to personalize search results and maximize relevance. However, they frequently do this without us knowing. For the following three questions, write your answers down and be prepared to discuss. Be prepared to share your answers in class.  

3. What are the potential benefits and risks of personalized searches? 

4. Are there certain kinds of data that are OK for search engines to use? If so, what are they, and why is it ok for search engines to use them? What is the limit to the kinds of data that search engines should be allowed to use to personalize our searches?

5. Some search engines allow users to make private searches by not recording user queries or tracking users at all (e.g., DuckDuckGo). What are the potential benefits, limitations, and consequences of these private search engines? 

We will now go back to the whole class and discuss group answers for Part 1 in a plenary session.

## Part 2: Precision and Recall

For this next part, have someone take notes for your group so that you are prepared to discuss. 

Now imagine you have a more sophisticated IR system than the one in Part 1, with far more query terms and documents. You are tasked with evaluating its performance and how it might be used for different searches. 

For a particular query, your system returns 8 relevant documents and 10 non-relevant documents. There are a total of 20 relevant documents in the collection.

6. What is the precision of the system on this search, and what is its recall?

You know that people will be using your IR system primarily to search for medical information. Specifically, people will use your IR system to search for treatments to ailments they face. You are trying to decide whether you should improve the recall of your system. Suppose we define a true positive document as: a document which does not just describe the ailment, but also, which contains accurate treatment information. 

7. Given the above system, answer the following:
   <ol type="a">
      <li>Why might you prioritize high precision in this task? Be prepared to explain. </li>
      <li>Why might you prioritize high recall in this task? Be prepared to explain. </li>
   </ol>

Precision and recall are relevant metrics not just in IR tasks, but also, in broader NLP tasks. These metrics can have significant consequences not just on the performance of your system, but also on the social impact of your system. For example, let’s consider the role of precision and recall when evaluating a hate speech classifier.

[TODO rest of 8]

8. Discuss the tradeoff between precision and recall in the hate speech classifier, which you would prioritize, and why. Be prepared to explain your answer. 

Now that you’ve played around with definitions of precision and recall, it’s time to come up with your own scenarios.

9. Write a scenario related to an IR or broader NLP task where you would prioritize precision over recall. Make sure you define what a “true positive” document is, as in the examples above. Be prepared to explain your response.  

10. Write a scenario related to an IR or broader NLP task where you would prioritize recall over precision. Make sure you define what a “true positive” document is, as in the examples above. Be prepared to explain your response. 

We will now go back to the whole class and discuss group answers for Part 2 in a plenary session.

## Part 1: The Right to be Forgotten

[TODO add]