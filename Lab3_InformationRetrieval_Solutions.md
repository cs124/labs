# Week 5: Solutions to Group Exercises on Information Retrieval

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
