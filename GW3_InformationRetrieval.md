# Week 4: Group Exercises on Information Retrieval

1. An IR system returns eight relevant documents and ten non-relevant documents. There are a total of twenty relevant documents in the collection. 
What is the precision of the system on this search, and what is its recall?

2. Do modern web search engines use stemming? If so, are all suffixes removed or just some of them? How do search engines deal with Boolean terms like OR or AND? 
Do some experimenting with some search engines.

3. Compute cosines to find out whether Doc1, Doc2, or Doc3 will be ranked higher for the two-word query "Linus pumpkin", 
given these counts for the (only) 3 documents in the corpus:

   | Term    | Doc1 | Doc2 | Doc3 |
   |:--------|:----:|:----:|:----:|
   | Linus   | 10   | 0    | 1    |
   | Snoopy  | 1    | 4    | 0    |
   | pumpkin | 4    | 100  | 10   |

   Do this by computing the tf-idf cosine between the query and Doc1, the cosine between the query and Doc2, and the cosine between the query and Doc3, 
and choose the highest value. **You should use the ltc.lnn weighting variation (remember that's ddd.qqq), the same weighting you will use for PA3,** using the following table:

   ![Weighting variations table](cosinechart.jpeg)

   **Hint:**
   - Logs are in base 10!
   - It might help to look at this [useful handout](http://web.stanford.edu/class/cs124/lec/CS124_IR_Handout.pdf)

4. **Privacy** in IR: Personalization is an important topic in information retrieval; after all, we'd like our search results to be relevant to us and our interests.
 However, as with many other tasks involving people's personal data, this has ethical implications. Do the following in your group:
   1. Google "marguerite". What is the first search result? Would you expect another person - say, someone in New York - to get the same search result? 
Discuss any incidents in which your group members have had search engines return such examples of personalization based on location, search and browsing history, or social media?
   
   2. Discuss these questions:
      1. Is it okay that search engines are using this data to personalize our searches? Or is there a limit to what kind of data should be okay for search engines to use? 
Does any of your group use anonymous search engines to avoid this?
      2. Are there any risks with getting personalized searches? Or do the benefits outweigh the risks? What about using people's queries about HIV or opioids 
for public health research? How should decide how to weigh benefits against risks?
      3. In 2009, the French government signed the ["Charter of good practices on the right to be forgotten on social networks and search engines"](https://fr.wikisource.org/wiki/Charte_du_droit_%C3%A0_l%E2%80%99oubli_dans_les_sites_collaboratifs_et_les_moteurs_de_recherche). 
      Do you think people should have the right to remove information about themselves from the web (the right to be forgotten)? 
Do you think Google should be required to remove information about an individual upon request?

4. **Bias** in IR. Google "professor style" and select "Images". Google "teacher style" and select "Images"
   1. What do you notice about the results? Do you notice any biases in the image results?
   2. What do you think are the impacts of potential misrepresentation in results? Discuss!!  

      You can check out these additional resources if you are looking for more readings about the impact of advertising, media, and search results 
on the perception of different identity groups. If you would like, you can use these articles to guide your discussion.
      - https://www.tandfonline.com/doi/abs/10.1080/00913367.1990.10673179
      - https://journals.sagepub.com/doi/10.1177/002193479902900303
      - https://psycnet-apa-org.stanford.idm.oclc.org/fulltext/2020-42793-001.html
      - https://journals.sagepub.com/doi/abs/10.1177/1090198120957949
