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
      <code>I might prioritize precision to make sure the documents I return are likely to be relevant to the user's need, running the risk of missing a few relevant documents.</code>
   </li>
   <li>Why might you prioritize high recall in this task?
      <code>I might prioritize recall so I don't miss any documents that might be relevant, running the risk of returning a few irrelevant documents.</code>
   </li>
</ol>

Precision and recall are relevant metrics not just in IR tasks, but also, in broader NLP tasks. These metrics can have significant consequences not just on the performance of your system, but also on the social impact of your system. For example, let’s consider the role of precision and recall when evaluating a hate speech classifier.

[TODO rest of 8]

8. Discuss the tradeoff between precision and recall in the hate speech classifier, which you would prioritize, and why.
```
Arguments for either are valid:
Precision - I don't want to censor people's benign speech, and prefer to make sure that what my classifier flags is actually toxic.
Recall - I don't want to miss any hate speech, and prefer to make sure that all hate speech is correctly flagged, even if I accidentally flag some benign comments as well.
```
   
Now that you’ve played around with definitions of precision and recall, it’s time to come up with your own scenarios.

9. Write a scenario related to an IR or broader NLP task where you would prioritize *precision* over recall. Make sure you define what a “true positive” is, as in the examples above.
```
Any answer with justification is valid. Examples are: criminal justice applications, like recidivism risk assessments (scenarios where false positives could be costly).
```

10. Write a scenario related to an IR or broader NLP task where you would prioritize *recall* over precision. Make sure you define what a “true positive” is, as in the examples above.
```
Any answer with justification is valid. Examples are: medical diagnoses, loan approval systems, content moderation (scenarios where false negatives could be costly).
```

We will now go back to the whole class and discuss group answers for Part 2 in a plenary session.

## Part 3: The Right to be Forgotten

[TODO add]

```
Answer TODO
```

We will now go back to the whole class and discuss group answers for Part 3 in a plenary session.

--------------

1. An IR system returns eight relevant documents and ten non-relevant documents. There are a total of twenty relevant documents in the collection. 
What is the precision of the system on this search, and what is its recall?

   ```
   Precision: 8/18 (or 4/9)
   Recall: 8/20 (or 2/5)
   ```

2. Compute cosines to find out whether Doc1, Doc2, or Doc3 will be ranked higher for the two-word query "Linus pumpkin", 
given these counts for the (only) 3 documents in the corpus:

   | Term    | Doc1 | Doc2 | Doc3 |
   |:--------|:----:|:----:|:----:|
   | Linus   | 10   | 0    | 1    |
   | Snoopy  | 1    | 4    | 0    |
   | pumpkin | 4    | 100  | 10   |

   Do this by computing the tf-idf cosine between the query and Doc1, the cosine between the query and Doc2, and the cosine between the query and Doc3, 
and choose the highest value. **You should use the ltc.lnn weighting variation (remember that's ddd.qqq), the same weighting you will use for PA4,** using the following table:

   ![Weighting variations table](cosinechart.jpeg)

   **Note that logs are in base 10.**
   
   **Hints:**
   - If you are confused about what to do, please read through this [useful handout](CS124_IR_Handout.pdf)!
   - You should only need to calculate IDF once per term
   - To help you compute tf-idf cosines, you should make a table for the query and each document similar to slide 56 of [this deck](https://spark-public.s3.amazonaws.com/cs124/slides/ir-2.pdf), but note that you are using the ltc.lnn variation, not the variation shown on the slide.

   ```
   Let's start with our query "Linus pumpkin".
   
   |  term   | tf-raw |     tf-wt      |
   |---------|--------|----------------|
   | Linus   |   1    | 1 + log(1) = 1 |
   | pumpkin |   1    | 1 + log(1) = 1 |
   
   We don't need to do anything else for the query vector because we are simply using lnn weights.
   
   Now let's do our calculations for Doc 1.
   
   |  term   | tf-raw |       tf-wt        | df |       idf        |        wt         |                  normalized                   |
   |---------|--------|--------------------|----|------------------|-------------------|-----------------------------------------------|
   | Linus   |   10   | 1 + log(10) = 2    | 2  | log(3/2) = 0.176 | 2 x 0.176 = 0.352 | 0.352 / sqrt(0.352^2 + 0.176^2 + 0^2) = 0.894 |
   | Snoopy  |   1    | 1 + log(1) = 1     | 2  | log(3/2) = 0.176 | 1 x 0.176 = 0.176 | 0.176 / sqrt(0.352^2 + 0.176^2 + 0^2) = 0.447 |
   | pumpkin |   4    | 1 + log(4) = 1.602 | 3  | log(3/3) = 0     | 1.602 x 0 = 0     | 0 / sqrt(0.352^2 + 0.176^2 + 0^2) = 0         |
   
   Now calculations for Doc 2.
   
   |  term   |  tf-raw  |       tf-wt        | df |       idf        |          wt           |           normalized            |
   |---------|----------|--------------------|----|------------------|-----------------------|---------------------------------|
   | Linus   |   0      | 0                  | 2  | log(3/2) = 0.176 | 0 x 0.176 = 0         | 0                               |
   | Snoopy  |   4      | 1 + log(4) = 1.602 | 2  | log(3/2) = 0.176 | 1.602 x 0.176 = 0.282 | 0.282 / sqrt(0.282^2 + 0^2) = 1 |
   | pumpkin |   100    | 1 + log(100) = 3   | 3  | log(3/3) = 0     | 3 x 0 = 0             | 0 / sqrt(0.282^2 + 0^2) = 0     |
   
   Now calculations for Doc 3.
   
   |  term   | tf-raw |     tf-wt       | df |       idf        |        wt         |           normalized            |
   |---------|--------|-----------------|----|------------------|-------------------|---------------------------------|
   | Linus   |   1    | 1 + log(1) = 1  | 2  | log(3/2) = 0.176 | 1 x 0.176 = 0.176 | 0.176 / sqrt(0.176^2 + 0^2) = 1 |
   | Snoopy  |   0    | 0               | 2  | log(3/2) = 0.176 | 0 x 0.176 = 0     | 0                               |
   | pumpkin |   10   | 1 + log(10) = 2 | 3  | log(3/3) = 0     | 2 x 0 = 0         | 0 / sqrt(0.176^2 + 0^2) = 0     |
   
   Now for the cosine similarity of each document, we take the dot product of the query vector and the normalized 
   vector for that document. We can disregard the Snoopy element in each of the documents vectors becasue the 
   query vector does not contain the word Snoopy. As such, we get the following:
   
   Doc1: (1 x 0.894) + (1 x 0) = 0.894
   Doc2: (1 x 0) + (1 x 0) = 0
   Doc3: (1 x 1) + (1 x 0) = 1
   
   Now we simply choose the highest score and that belongs to Doc3. Doc3 will be ranked highest for the query 
   "Linus pumpkin", because between Doc1, Doc2, and Doc3, we see that Doc3 has a greater cosine similarity
   ```

3. **Bias** in IR. 

Discuss the following questions: 

   a. Google "professor style" and select "Images". Google "teacher style" and select "Images." Note the gender bias. Is it OK for a system to be biased if it amplifies a bias in the world? What if it faithfully represents the world?
   
   b. Go back to All search results. Using any search engine, type in the queries "why coffee is good for you" and "why coffee is bad for you". Explore other variations on this query, like: "is coffee good for you" and "is coffee bad for you". Does a system have a responsibility to give us unbiased information when we ourselves are biased? What are the potential impacts of this kind of bias in search results? 
   
   c. Clearly, bias in IR is an unsolved problem! If you were the CEO of a search engine company and wanted to reduce bias, how would you modify the algorithm? Some things you can think about: are there any other factors the algorithm should consider besides the similarity of the query to the retrieved document? How should it detect and handle opinionated queries or queries with potential for gender bias? Feel free to be creative in your responses!

   After you have brainstormed your own ideas, you can check out these additional resources on what search engines are doing/have attempted doing to reduce bias:
   - https://www.theverge.com/2022/5/11/23064883/google-ai-skin-tone-measure-monk-scale-inclusive-search-results 
   - https://blogs.bing.com/search-quality-insights/february-2018/toward-a-more-intelligent-search-bing-multi-perspective-answers
   
   You can check out these additional resources if you are looking for more readings about the impact of advertising, media, and search results 
on the perception of different identity groups.
   - https://www.tandfonline.com/doi/abs/10.1080/00913367.1990.10673179  
   - https://journals.sagepub.com/doi/10.1177/002193479902900303
   - https://psycnet-apa-org.stanford.idm.oclc.org/fulltext/2020-42793-001.html
   - https://journals.sagepub.com/doi/abs/10.1177/1090198120957949

4. **Privacy** in IR: Personalization is an important topic in information retrieval; after all, we'd like our search results to be relevant to us and our interests.
 However, as with many other tasks involving people's personal data, this has ethical implications. Do the following in your group:
 
   a. Google "marguerite". What is the first search result? Would you expect another person - say, someone in New York - to get the same search result? 
Discuss any incidents in which your group members have had search engines return such examples of personalization based on location, search and browsing history, or social media?
  
   b. Discuss: What are potential benefits and risks of getting personalized searches? Is it okay that search engines are using our data to personalize our searches? Or is there a limit to what kind of data should be okay for search engines to use? Does any of your group use anonymous search engines to avoid this?
      
   c. Discuss: In 2009, the French government signed the "Charter of good practices on the right to be forgotten on social networks and search engines." 
      Do you think people should have the right to remove information about themselves from the web (the right to be forgotten)? 
Do you think Google should be required to remove information about an individual upon request?

Optional reading: If you are interested, you can read the Charter here: ["Charter of good practices on the right to be forgotten on social networks and search engines"](https://fr.wikisource.org/wiki/Charte_du_droit_%C3%A0_l%E2%80%99oubli_dans_les_sites_collaboratifs_et_les_moteurs_de_recherche).
