# Week 9: Group Exercises on Chatbots/Dialogue Agents, and Recommendation Engines

## Part 1: Dialogue Agent Performance

Let's evaluate the features of the dialogue agents on your devices ([Siri](https://support.apple.com/guide/iphone/use-siri-iph83aad8922/ios), [Google Now](https://assistant.google.com/), [Alexa](https://alexa.amazon.com/), [Cortana](https://www.microsoft.com/en-us/cortana), etc.) or online chatbots, perhaps keeping in mind the rubric for `PA7`. 
Use voice to write text or emails, add a calendar appointment, get recommendations for a business, or use text to talk to a chatbot! If you know another language try out systems in other languages like `XiaoIce`! 

* Write down some great errors or issues that you'd like to post in [polleverywhere](https://pollev.com/danjurafsky451)! 
  Can you characterize their causes? 
  For example did they fail because of speech recognition (the wrong words were recognized) or natural language understanding (the words were right, but the system still didn't understand), or is the problem in the recommendation engine? 
* What does the system do if it is unsure about what you said? What seems to be the confirmation strategy? (e.g., explicit vs. implicit)? 
  Does it vary?
* Does the system seem to be responding only to your exact last sentence or is the system modeling the discourse history in some way (previous turns, remembering things you said, etc)

## Part 2: Collaborative Filtering

Let's work through an example of item-item collaborative filtering similar to what will be used in PA7:

You are working for OneTwoFourAI and you have an idea for a wonderful new app - a food recommendation system! 

* You debate with a colleague on whether to use item-item collaborative filtering or user-user collaborative filtering. Describe both of these methods. Which one would you use and why?

You decide to move forward with a simplified version of item-item collaborative filtering. You have access to the following dataset: 


|        | User 1 | User 2 | User 3 | User 4 |
|--------|--------|--------|--------|--------|
| Food 1 | 3      | 5      | 4      |        |
| Food 2 | 4      |        | 1      |        |
| Food 3 |        | 3      |        | 2      |
| Food 4 | 2      |        | 4      | 1      |

Each cell in the table contains either a rating from 1 to 5 or no rating. 


1. Binarize the values in the dataset. Convert the values to +1 (like), 0 (no rating), and -1 (dislike). Ratings 3 to 5 correspond to liking and ratings 1 to 2 correspond to dislike. 

2. You collect two new ratings from User X. User X gives Food 2 a rating of 2 and Food 4 a rating of 5. Update the table to reflect the new ratings from User X. 

3. Should you recommend food 1 or food 3 to User X?
