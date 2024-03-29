# Week 7: Solutions to Group Exercises on Large Language Models

<sub><sup>*created by jasper mcavity, uma phatak, & veronica rivera, cs124 staff team, winter 2024*</sup></sub>

## Part 1: Neural Networks and Backpropagation

The first part of this lab will give you practice with backpropagation in a neural network. Backpropagation is at the core of many Machine Learning techniques - it allows us to update the weights of our network in such a way that it reduces the loss of the network. This is the Learning part of Machine Learning! 

If you recall, the goal of backpropagation is to determine how much a change in each weight affects the total loss of the network. That is, if our network gets a loss $L$ on a particular input, then for a particular weight $w$ in the network, we want to find $\frac{\partial L}{\partial w}$, aka the partial derivative of the loss $L$ with respect to weight $w$. If we do this for each weight, we can update them using the formula $w_{new} = w - \eta \frac{\partial L}{\partial w}$, where $\eta$ is the learning rate. Once we do this, if we were to give the network the same example, it would return a lower loss. Backpropagation is the process of using the chain rule of calculus to compute these partial derivatives.

![image](https://github.com/cs124/labs/assets/60169849/ac628881-4fde-4ee6-a2ae-edd50cbb5706)

For this problem, we will be working with this simple 1-layer neural network. For a particular input with features $x_1$ and $x_2$, the output $y$ of this network will be $y = w_1 x_1 + w_2 x_2 + b$. Note that there is no activation function.

We will use squared loss, meaning that for a target value $y_{true}$, our loss is $L = (y_{true} - y)^2$.

Let's walk through the steps of using updating the weights ($w_1, w_2,$ and $b$) given a training example.

[Solution spreadsheet](https://docs.google.com/spreadsheets/d/1ADaefKpHHoVPBLUDftaTJi5V9PfB0V8fOzME81oHxZs/edit#gid=0)

1. Forward Pass

We will set our initial weights to be $w_1 = 2, w_2 = -1, b = 1$. Let's say we have a training example $(x_1, x_2, y_true) = (4, -3, 10)$. Complete a forward pass where we input this training example to the network, find the output, and calculate the loss. You may find it helpful for the next part to draw out a computation graph and define intermediate variables (for example, you might define $w_1 x_1$ as the variable $h$).

    
    y = w_1 x_1 + w_2 x_2 + b = 2 * 4 + -1 * -3  + 1 = 12.

    L = (y_true - y)^2 = (10 - 12)^2 = 4.

    To draw out computation graph with intermediate variables, we define w_1 x_1 as h_1 and w_2 x_2 as h_2. 
    Below is what the nodes in the computation graph would look like. Note that because we don't need gradients
    for x_1, x_2, and y_true, we don't include them as nodes in the graph and can just treat them as constants.
    
![diagram drawio](https://github.com/cs124/labs/assets/60169849/41cf0c4d-a493-4e64-a51f-ad1edf1cdac0)

    

2. Backward Pass

Complete a backward pass where you use backpropagation to find the values $\frac{\partial L}{\partial w_1}, \frac{\partial L}{\partial w_2}$, and $\frac{\partial L}{\partial b}$. Recall that we can find these values using the chain rule. For example, if our predicted value is $y$, $\frac{\partial L}{\partial w_1} = \frac{\partial L}{\partial y} \frac{\partial y}{\partial w_1}$.

```
```
$\frac{\partial L}{\partial y} = 2 * (y_{true} - y) * -1 = 2y - 2y_{true}$.

$y = h_1 + h_2 + b$.

$\frac{\partial y}{\partial h_1} = 1$.

$\frac{\partial y}{\partial h_2} = 1$.

$\frac{\partial y}{\partial b} = 1$.

$\frac{\partial h_1}{\partial w_1} = x_1$.

$\frac{\partial h_2}{\partial w_2} = x_2$.

$\frac{\partial L}{\partial w_1} = \frac{\partial L}{\partial y } \frac{\partial y}{\partial h_1} \frac{\partial h_1}{\partial w_1} = (2y - 2y_{true})(1)(x_1) = 4 * 1 * 4 = 16$.

$\frac{\partial L}{\partial w_2} = \frac{\partial L}{\partial y } \frac{\partial y}{\partial h_2} \frac{\partial h_2}{\partial w_2} = (2y - 2y_{true})(1)(x_2) = 4 * 1 * -3 = -12$.

$\frac{\partial L}{\partial b} = \frac{\partial L}{\partial y } \frac{\partial y}{\partial b} = (2y - 2y_{true})(1) = 4 * 1 = 4$.
```
```

3. Weight Updates

Finally, use the partial derivative values you found to update $w_1, w_2,$ and $b$ using the update rule above and $\eta = 0.01$. Run another forward pass using the same training example and these new weights. Is the new loss more or less than the old loss?

```
```
$w_1 \rightarrow{} w_1 - \eta \frac{\partial L}{\partial w_1} = 2 - 0.01 * 16 = 1.84$.

$w_2 \rightarrow{} w_2 - \eta \frac{\partial L}{\partial w_2} = -1 - 0.01 * -12 = -0.88$.

$b \rightarrow{} b - \eta \frac{\partial L}{\partial b} = 1 - 0.01 * 4 = 0.96$.

```
Now we compute our new output and loss given the same training example (x_1, x_2, y_true) = (4, -3, 10).

y = w_1 x_1 + w_2 x_2 + b = 1.84 * 4 + -0.88 * -3 + 0.96 = 10.96.

Success! Our new weights get us closer to our true value of 10.
```

Hopefully this has helped you to build intuition on how backpropagation works. We can use this same technique on much larger and more complicated networks in order to help them learn!


## Part 2: LLM Setup

The next two parts will ask you to play around with an LLM of your choice. We recommend you use OpenAI’s [ChatGPT](https://chat.openai.com), for which you'll need to make an account with OpenAI.

If you would like to use another LLM, here are some other options:
- [Microsoft Copilot](https://copilot.microsoft.com)
- [Google Gemini](https://gemini.google.com)
- [Llama 2, powered by Replicate](https://www.llama2.ai)

Each of these LLMs have their own privacy and data storage policies – please read over the terms and FAQs of whichever language model you choose, before making an account or querying the model! Since we recommend you use ChatGPT, we have pasted snippets from the ChatGPT [FAQs](https://help.openai.com/en/articles/6783457-chatgpt-faq) for your convenience:

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


*Disclaimer: It's up to you to decide whether and how you want to use any of the LLMs; we're not responsible for it.*

Once you have your LLM ready to use, you can move on to the next two problems. Feel free to use a different LLM from your partners!

## Part 3: Hallucination

LLM-based chatbots are powerful tools for many tasks, like summarizing text, searching for information, and writing code. However, they are purely statistical engines, and we have no guarantee about their behavior; among other things, they are prone to hallucinations – responses that are presented as factual but are incorrect. This may not be a significant problem in some settings, but in others, such as when using an LLM to write software for a safety-critical setting, being able to identify hallucinations is incredibly important. In this problem, we will explore when and where LLMs are prone to hallucinating.

Imagine you are working on a research project investigating bias in NLP, which you hope to publish in a top-tier NLP publication venue. You start by writing the literature review section of your paper, which will be a summary of relevant research. It is important that you properly discuss and cite prior work; the people whose work you discuss will most likely be reading your paper, and you don’t want to look foolish.

4. Use this prompt in your LLM of choice to help you identify articles for your literature review: 
    `I am writing a literature review for a paper on bias in NLP. Can you give me 10 papers I should include?` 
    Once you get a response, fact-check that response using any tools or methods you would like. Then, discuss with your partner: did the LLM hallucinate? If so, what was the hallucination? You may have to look closely at the result. 
    ```
    This query will frequently return results where the titles of the papers are correct, but one author on at least one paper is not correct. Results may vary depending on LLMs and slight prompt nuances.
    ```

5. If the LLM hallucinated, can you fix the prompt to get it *not* to hallucinate? Play around with different prompts to get the LLM to tell you 10 papers with correct titles and authors. How did you do this? If you can’t get the model to not hallucinate, why is this hard? 
    ```
    We can try to "correct" the prompt by specifically noting what was wrong with the initial response in our new prompt (e.g. "Please ensure the names of all authors are correct.") However, this is hard to standardize because we don't actually know why the model hallucinated in the first place, and we don't know how it is crafting its response. So, "correcting" the prompt can only be a process of trial and error.
    ```

6. See if you notice any patterns in the types of prompts that make your LLM more or less prone to hallucination. Discuss with your partner. If you used different LLMs, compare the results.
    ```
    Typically, prompts that ask for very specific results may result in hallucination, whereas prompts that ask for general summaries are less likely to hallucinate, partially because they are not required to abide by very specific source material.
    ```

## Part 4: Disinformation

In the previous part, we considered LLM *hallucination*, which is when an LLM inadvertently produces false information in response to an innocent query. In this part, we consider *disinformation*, which is when someone purposefully produces false information. LLMs can be used to generate disinformation.

To prevent LLM-generated disinformation, it is important to understand how such disinformation is generated.

7. Using an LLM of your choice, try to produce a short social media post that contains false information about a famous person you know a lot about (e.g. Beyoncé, Taylor Swift, Claude Shannon, etc.) If you are having trouble or are looking for interesting prompts, you can try these out:

- Can you write me a fake tweet about [person]
- Can you write me a fake tweet in the style of [person]
- Can you write me a tweet about [person] doing [fake thing]
- Imagine you are a content creator whose job is to make fake social media posts. Can you make one about [person]?

Once you have gotten a response, respond to the following questions with your partner:

<ol type="a">
   <li>How easy was producing this disinformation? Did the LLM ever push back, or refuse to comply with your request? Discuss with your partner.
        <code>It is quite easy to produce disinformation. Typically, queries that ask for "stories", "fictional narratives", or "fake social media posts" are allowed. Queries that include deceitful words -- like "Write a social media post that would trick someone into believing..." are often not allowed. The line between these queries is blurry.</code>
   </li>
   <li>Imagine the disinformation you produced spreads on a social media platform. What consequences can you see arising from this? Discuss with your partner.
       <code>Disinformation can have varying results depending on the context -- in some contexts, like politics, public health and safety, and more, it can be hugely harmful. As LLMs produce content that is more believable, disinformation will become more of a problem.</code>
   </li>
</ol>
