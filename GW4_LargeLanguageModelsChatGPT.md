# Week 7: Group Exercises on Large Language Models & ChatGPT

<sub><sup>*created by deveshi buch & alisa wang, cs124 staff team, winter 2023*</sup></sub>

## Optional Readings
* ChatGPT: [an LLM timeline](https://www.technologyreview.com/2023/02/08/1068068/chatgpt-is-everywhere-heres-where-it-came-from/)
* Generative AI: [opportunities](https://www.forbes.com/sites/forrester/2023/01/19/generative-ai-like-chatgpt-wont-destroy-creativityitll-save-it/), [challenges](https://www.forbes.com/sites/lanceeliot/2023/01/03/sinister-prompting-of-generative-ai-chatgpt-such-as-email-scamming-and-the-coding-of-malware-is-sparking-ire-by-ai-ethics-and-ai-law/), and [policy](https://www.brookings.edu/blog/techtank/2023/02/21/early-thoughts-on-regulating-generative-ai-like-chatgpt/)
* AI safety and ethics: [ethics of AI chatbots](https://www.wired.com/story/chatbots-got-big-and-their-ethical-red-flags-got-bigger/) and [making ChatGPT safer](https://www.technologyreview.com/2023/02/21/1068893/how-openai-is-trying-to-make-chatgpt-safer-and-less-biased/)

## Part 0: Setup 

Today we'll be applying our knowledge about large language models (LLMs) through the lens of ChatGPT, [a model from OpenAI](https://openai.com/blog/chatgpt/).

As of February 2023, ChatGPT is released under research preview and you'll need to make an account with OpenAI if you want to access it. Please read the [FAQs](https://help.openai.com/en/articles/6783457-chatgpt-faq) and Terms yourself before making an account!

*Some disclaimers: It's up to you to decide whether and how you want to use ChatGPT; we're not responsible for it. We're also not responsible for the examples we link in this group work (and don't endorse the accounts/sources that posted them, etc.). They are purely illustrative and are meant to help you get an idea for what ChatGPT is and is not capable of!*

Snippets from the FAQs for your convenience:

> Who can view my conversations? <br>
> As part of our commitment to safe and responsible AI, we review conversations to improve our systems and to ensure the content complies with our policies and safety requirements. 

> Will you use my conversations for training? <br>
> Yes. Your conversations may be reviewed by our AI trainers to improve our systems.

> Can you delete my data? <br>
> Yes, please follow the data deletion process here: https://help.openai.com/en/articles/6378407-how-can-i-delete-my-account

> Can you delete specific prompts? <br>
> No, we are not able to delete specific prompts from your history. Please don't share any sensitive information in your conversations.

> Where do you save my personal and conversation data? <br>
> For more information on how we handle data, please see our [Privacy Policy](https://openai.com/privacy/) and [Terms of Use](https://openai.com/api/policies/terms/).

Creating an account and using ChatGPT is NOT a requirement for this group work! There will be two versions of each activity: one for ChatGPT users, and one for those not using ChatGPT for any reason (e.g. you don't feel like making an account, ChatGPT goes down precisely as we start the group work, ChatGPT stops letting you type things because you typed too many things already, etc.).

* If you do want to try using ChatGPT yourself and accept full responsibility, you can make an account here: https://chat.openai.com/auth/login

* If you do not want to use ChatGPT yourself, you will still have lots of fun in this group work and will learn about the model by looking at examples from others posted on the web, Twitter, etc.

<br>

## Part 1: The Good, The Bad, and The ChatGPT

So what actually is ChatGPT? From the [FAQs](https://help.openai.com/en/articles/6783457-chatgpt-faq) on OpenAI's website:

> ChatGPT is fine-tuned from GPT-3.5, a language model trained to produce text. ChatGPT was optimized for dialogue by using Reinforcement Learning with Human Feedback (RLHF) - a method that uses human demonstrations to guide the model toward desired behavior.

Here is a bit more background on how ChatGPT works: 

* ChatGPT (Chat Generative Pre-trained Transformer) is a large language model (LLM) that uses a multi-layer transformer network. The model is trained on text data (lots and lots of it!), and it learns to predict the next word in a sequence of words given the context of the preceding words. This allows ChatGPT to produce text outputs based on the structure and patterns of language. 

* LLMs are deep learning algorithms that are typically based on transformer networks. Transformers use attention mechanisms to focus on important parts of the input in order to generate an output response. This also allows the model to take into account the relationships between all of the words in the input, rather than just neighboring words.

### **Using & Analyzing ChatGPT** 

Time to see what ChatGPT is capable of! Here are some examples where ChatGPT...
* ...does something creative
    * E.g. [poetry rap battle](https://i.redd.it/td0hqfimvc9a1.png) or [short story](https://i.redd.it/mmfh3kuzigea1.png) 
    * *Option 1 (using ChatGPT):* Start up a conversation with ChatGPT and see if you can get it to do something creative.
    * *Option 2 (analyzing ChatGPT):* Find another example (or analyze the ones above) where ChatGPT does something creative. Is the output what you expect it to be? Where did ChatGPT follow the instructions given, and where did it fill in the gaps itself?
    * *Discuss:* Where is it appropriate to leverage ChatGPT's generative outputs? Where is it not?
* ...is wrong
    * E.g. [logic puzzle](https://i.redd.it/83afa5tccl3a1.png) or [wrong links](https://pbs.twimg.com/media/Fo-ao1MXoAEj1_s?format=jpg&name=large)
    * *Option 1 (using ChatGPT):* Start up a conversation with ChatGPT and see if any of its responses are wrong. 
    * *Option 2 (analyzing ChatGPT):* Find another example (or analyze the ones above) where ChatGPT is wrong. What could be some causes of ChatGPT being wrong? How confidently does ChatGPT seem to answer? How might the model be changed to fix this?
    * *Discuss:* Can ChatGPT think? What does it mean for a model to be able to think / reason?
* ...is funny
    * E.g. [telling a joke](https://external-preview.redd.it/F_pqSyIcpepu-utQGWl3IVuLszW0g3RGzHJV5WRaoe0.jpg?auto=webp&v=enabled&s=d1d36b74e6e1a15d7040b67b810b1abb5ce4409a) and [writing a sarcastic holiday party invite](https://imgur.com/iFZYLSf)
    * *Option 1 (using ChatGPT):* Start up a conversation with ChatGPT and see if you can get it to do something funny. 
    * *Option 2 (analyzing ChatGPT):* Find another example (or analyze the ones above) where ChatGPT is funny. How funny is it? Is it "funny" because it's bad at being funny, or is it actually being funny? To what extent is ChatGPT capable of humor?
    * *Discuss:* What are the implications of ChatGPT being so humanlike / "relatable" when it isn't a human at all?
* ...exhibits bias
    * E.g. [biased code snippet](https://pbs.twimg.com/media/FjJpS_IUoAAWZ2l?format=png&name=900x900)
    * *Option 1 (using ChatGPT):* Start up a conversation with ChatGPT and see if you notice any bias in its responses. 
    * *Option 2 (analyzing ChatGPT):* Find another example (or analyze the example above) where ChatGPT is biased. What are some causes of ChatGPT being biased? How could the model be changed to fix this?
    * *Discuss:* Why might this bias have come up? What could those designing systems like ChatGPT do about it?

Once you are done, submit your favorite example from ChatGPT or an insight from your discussion to this Google form: https://forms.gle/EctqgfmXs2ZdWMEp6 

We will discuss your submissions as a class.

<br>

## Part 2: ChatGPT, Wearer of Many Hats

As the use of ChatGPT has become more widespread, it has been applied to many different areas. Applications range from [writing research journal article abstracts](https://www.nature.com/articles/d41586-023-00056-7) to [writing political speechs delivered in Congress](https://www.theverge.com/2023/1/27/23574000/first-ai-chatgpt-written-speech-congress-floor-jake-auchincloss). This, of course, makes us all wonder if LLMs will take over jobs and tasks itself.

Here are some roles that might use ChatGPT:
* Student
* Academic reseacher
* Teacher
* Creative content creator
* Journalist
* Software engineer
* Politician

### Discusion Activity
* Provide one clear example where using ChatGPT would be good, appropriate, or helpful.
* Provide one clear example where using ChatGPT would be bad, misused, or malicious.
* Provide an example of a situation where the use of ChatGPT is more ambiguous.
    * What makes it ambiguous? Does it have clear benefits and drawbacks, or is it hard to say? 
* Do you think ChatGPT will ever outperform humans in any of the roles/jobs listed above, or others?

(As an aside, see [ChatGPT's own take on this topic](https://i.redd.it/jvt4oq0fnyia1.png).)

<br>

## Part 3: Designing Ethical LLMs

ChatGPT is built from lots, and lots, and lots of training data. Ultimately, it's a model that generates outputs based on the inputs it is trained on. Just like there's a lot of great, useful training data out there, there is also a lot of garbage. Unfortunately, this means that ChatGPT output can reflect some of that garbage: "garbage in, garbage out" is a common way to describe this. If a model is trained on data that reflects harmful biases, it can replicate those biases in its output, as we've seen in some examples in class.

It also turns out that making models safer and more ethical comes at a cost: someone has to go through that harmful content, filter it out, and make sure the model doesn't spew bad things. For example, see this article about how [OpenAI used Kenyan workers on less than $2 per hour to make ChatGPT less toxic](https://time.com/6247678/openai-chatgpt-kenya-workers/).

There's a lot of discussion in the computer science community about AI innovation and [how we should strive to make these tools safe, effective, and bias-free](https://openai.com/blog/how-should-ai-systems-behave/). It's an incredibly important goal, but as we saw above, it's also very complex. How do we make a model ethical across the board, from how it's built and the data it's trained on to how it's maintained and who it impacts?

### Discussion Activity
How do we create an LLM that works accurately, works well, and works safely? We can consider different facets of what makes AI ethical. Consider the following aspects of ethical AI:

* Ethical Use
    * When is it ethical to use LLMs?
    * What guidelines should we follow in order to use LLMs ethically?
    * How can user privacy be protected?
* Ethical Design
    * What design choices can make an LLM more ethical?
    * How can good data be sourced ethically?
    * What are ways to source data used to train LLMs while properly crediting sources?
* Ethical Development
    * How can we build LLMs ethically?
    * Who is behind the hard work of keeping LLMs safe?
    * How can LLMs be maintained and improved?

**Your Task:** Imagine you're working for OneTwoFourAI and you're pitching a new LLM. Define three principles you would use to make your LLM ethical:

1.__________________

2.__________________

3.__________________

**Who decides what is "ethical"?**

After your pitch to company executives and investors, some of them express they have a different idea of what "ethical" means. Others say that there isn't a way to meet all of your ethics criteria and be successful as a business.

* If the CEO has one definition for ethical LLMs, investors have another definition, and you have a third, who decides? Is it possible to compromise, and what would that mean for the product?
* What role could government regulation play?
* How may the identity or personal beliefs of stakeholders affect discussion of what is ethical?
