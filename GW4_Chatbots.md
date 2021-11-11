# Week 8: Group Exercises on Chatbots/Dialogue Agents, and Recommendation Engines

Your goal today is to think deeply about both chatbot implementation and the ethical issues around both chatbots and computer-mediated dialogue in general. 
After each of the following questions, we will report back some of your results to the whole room, so make note of your group's interesting results. 

## Part 1: Dialogue Agent Performance

Let's evaluate the features of the dialogue agents on your devices (`Siri`, `Google Now`, `Alexa`, `Cortana`, etc.) or chatbots online like `XiaoIce`, perhaps keeping in mind the rubric for `PA6`. 
Use voice to write text or emails, add a calendar appointment, get recommendations for a business, or use text to talk to a chatbot, perhaps trying out systems in other languages like `XiaoIce`! 

* Write down some great errors or issues that you'd like to post in [polleverywhere](https://pollev.com/danjurafsky451)! 
  Can you characterize their causes? 
  For example did they fail because of speech recognition (the wrong words were recognized) or natural language understanding (the words were right, but the system still didn't understand), or is the problem in the recommendation engine? 
* What does the system do if it is unsure about what you said? What seems to be the confirmation strategy? (e.g., explicit vs. implicit)? 
  Does it vary?
* Does the system seem to be responding only to your exact last sentence or is the system modeling the discourse history in some way (previous turns, remembering things you said, etc)
* Does the system ever surprise you in a positive way? 
* Discuss in your groups some abilities you'd like to add to the chatbot. 

## Part 2: Ethical Issues: Gender and Privacy

As I discussed in the chatbot lecture and the textbook chapter, ethical issues are especially relevant in chatbots, given that they interface with humans. 
Discuss these two issues: 

* Chatbots are often given female voices, names, pronouns, or avatars. 
  Moreover, users are more likely to apply gender stereotypes to the chatbot when the it conforms to gender norms or stereotypes [McDonnell & Baxter, 2019](https://academic.oup.com/iwc/article/31/2/116/5448907?casa_token=JOyJzMZWjYUAAAAA:A3gCGZjlXH-I5szqdQ-TGb9SCCwLU0TTsoOu9S4LKymDE9HN3zGS5UTepWMdJ1-OG8wbeRa6n2v9OQ)
  What are some potential consequences of chatbots perpetuating existing gender norms?
  What are some ways that you could alter current chatbots to minimize the gender stereotypes that are built into them? 
* Chatbots also present privacy concerns. 
  Human-like chatbots can lead people to disclose more [Ischen et al., 2020](https://link-springer-com.stanford.idm.oclc.org/chapter/10.1007%2F978-3-030-39540-7_3), and humans are less likely to be worried about their personal information when the chatbot is perceived to be more human-like.
  Given this trust in the chatbot, companies can collect valuable data about their consumers, often without them knowing this is occurring.
  Are you more likely to share personal information with a chatbot that has human-like characteristics? 
  Do you think computer scientists have a responsibility to specify that a chatbot is a machine and attempt to minimize the amount of data users share with the system? 
  Do you have any other privacy concerns surrounding chatbots? 
  
## Part 3: Ethical Issues in Computer Mediated Human Dialogue

Computers interacting with humans is a subcase of the general issue of computers and conversation, which also includes computers mediating humans interacting with each other. 
Let's talk about some of the ethical issues in that space, focusing on the recently released Facebook Papers, leaked internal documents about Facebook's role in dealing with problematic human-human interaction. 
Two ethical issues have been raised in this area: 
* Should companies who publish social media posts be responsible for detecting and removing calls for genocide, hate speech, and so on?
  Facebook Papers [suggest](https://cs124.stanford.edu/restricted/WSJhatespeech.pdf) that Facebook chose not to do so in many countries, for example ignoring issues of hate speech in countries like Myanmar, India, and Egypt, choosing not to build hate-speech classifiers for many languages (like Assamese, the lingua franca of Assam, the eastern-most state in India next to Myanmar, or Arabic). 
  What do you think the responsibility should be of such social media publishers of people's dialogues online? 
* What should be the balance between removing hate-speech and allowing free speech?
  Facebook documents suggest (the same WSJ article as above) that the company emphasized leaving up the vast majority of most hate speech in every country to avoid accidentally removing any non-hate-speech, i.e. emphasized avoiding "false positives" in hate speech detection over avoiding "false negatives".
  Polls suggest that in the US, people in general are more worried about posts being deleted incorrectly (false positives), while in many places abroad, people prioritize hate speech being taken down and are more worried about false negatives. 
  What is your perspective?
  
