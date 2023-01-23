# Week 3: Group Exercises on Naive Bayes and Sentiment

## Part 1: Quick Naive Bayes review

First, form a group of 3 students to work together!

We want to build a naive bayes sentiment classifier using add-1 smoothing, as described in the lecture (not binary naive bayes, regular naive bayes). 
Here is our training corpus:

**Training Set**

    - the movie has no plot
    - honestly pretty boring
    + pretty interesting movie 
    

**Test Set**

    pretty enjoyable plot
    
Answer the questions below given the sets above.

  1. Compute the prior for the two classes + and -, and the likelihoods for each word given the class (leave in the form of fractions).

  2. Then compute whether the sentence in the test set is of class positive or negative (you may need a computer for this final computation).

  3. Would using binary multinomial Naive Bayes change anything?

  4. Why do you add |V| to the denominator of add-1 smoothing, instead of just counting the words in one class?


## Part 2: Ethics and Conceptual Problems

  5. For the following problem, please choose a group facilitator/representative who will also take notes on your discussion. 
     When we come back to the lecture room, I will call for volunteer groups to report back to the whole class on your thoughts or results, and so I will call some of the representatives to the stage.
     
     Data ethics is an important component of any supervised machine learning task, since the data has to come from somewhere. 
     For example, all research involving human subjects in the United States must follow the 3 "Belmont Principles", which are:

     * justice (the equitable distribution of benefits and burdens of research),

     * beneficence (the obligation to not only "do no harm" but to actively maximize benefits and minimize harms to subjects)

     * respect for persons (people should be able to make autonomous decisions), 
     
     (the full Belmont report is [here](https://www.hhs.gov/ohrp/sites/default/files/the-belmont-report-508c_FINAL.pdf))

     Do the following:
     
     * Figure out (from the web; Googling may help) the age of the person who wrote this sentence in a movie review:
       
        > There are many, many characters to enjoy in this fanciful tale; my personal favorite has always been Inigo Montoya.

     * Discuss these questions in your group, with the goal of some groups reporting back to the whole class:
        
        *  Is it ok that we can figure this out?
        *  Is it ok that we are using this person's data to train our sentiment analyzers?
        *  Should researchers not use publically posted data like this?
        *  Is it ok that we are using IMDB data for a homework?
        *  Should IMDB.com own this review?
        *  How does this all relate to the Belmont Principles? 
      
        Your opinions will likely differ from each other, and of course we expect you will discuss this respectfully!! 
        It may be helpful to bring up cases you've seen in the news relating to privacy or fairness and machine learning.

      Once you are done, we will discuss as a class!
      
   6. Choose a different group facilitator/representative. 
      Then first do this short ML conceptual problem: It is sometimes the case that more complex features (like trigrams or bigrams) perform better than simple features (like unigrams) on the **training** set, but perform worse than simple features on the **test** set. 
      This is a particular case of the phenomenon called `overfitting` in machine learning. 
      Discuss why this might be. 
      Can you create a tiny training set with 2 3-word documents and a test set with one document for which this overfitting situation holds?
   
   7. Choose a third group facilitator/representative. 
      Now let's continue thinking about data sources. 
      Our data sources are the basis of all of our optimization problems. 
      But who are we optimizing for? 
      Should we be concerned that we are building systems in response to the people who create the most text on the web, namely men from wealthy, English-speaking countries? 
      According to the [researcher Ricardo Baeza-Yates in the journal CACM](https://cacm.acm.org/magazines/2018/6/228035-bias-on-the-web/fulltext), "the percentage of all publicly reported Wikipedia female editors is just 11%". 
      Furthermore, "it is estimated that over 50% of the most popular websites are in English, while the percentage of native English speakers in the world is approximately only 5%".

      What are the potential effects of biases in internet data sources? 
      Does this bias negatively impact some populations more than others? 
      If so, whom and in which ways? Do think we should attempt to remediate these biases, and if so do you have any ideas for steps that can be taken? 
      Or do you think that remediation should not be a focus for data researchers? 
      Again, your opinions will likely differ from each other, and of course we expect you will discuss this respectfully!!

      Once you are done, we will discuss as a class!
      
## Part 3: Extra Credit 


   8. Now go to the Sentiment demo at https://demo.allennlp.org/sentiment-analysis/glove-sentiment-analysis. 
      Come up with 5 sentences that the classifier gets wrong. 
      Can you figure out what is causing the errors?
      Once you are done, we will discuss any particularly interesting sentiment examples as a class!
      
      Submit your 5 sentences here for extra credit: https://forms.gle/57okKzZzWeijP4RL7
      
