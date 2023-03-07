# Week 9: Group Exercises on Chatbots/Dialogue Agents, and Recommendation Engines Solutions

## Part 1: Dialogue Agent Performance
For discussion!

## Part 2: Collaborative Filtering

Let's work through an example of item-item collaborative filtering similar to what will be used in PA7:

You are working for OneTwoFourAI and you have an idea for a wonderful new app - a food recommendation system! 

* You debate with a colleague on whether to use item-item collaborative filtering or user-user collaborative filtering. Describe both of these methods. Which one would you use and why?


>Item-item collaborative filtering aims to find similar items based on ratings for other items. User-user collaborative filtering aims to find similar users based on ratings for other users. In practice, item-item collaborative filtering often works better than user-user collaborative filtering because items are often similar, and users have multiple tastes. (People are more complex than objects.)



You decide to move forward with a simplified version of item-item collaborative filtering. You have access to the following dataset: 


|        | User 1 | User 2 | User 3 | User 4 |
|--------|--------|--------|--------|--------|
| Food 1 | 3      | 5      | 4      |        |
| Food 2 | 4      |        | 1      |        |
| Food 3 |        | 3      |        | 2      |
| Food 4 | 2      |        | 4      | 1      |

Each cell in the table contains either a rating from 1 to 5 or no rating. 


1. Binarize the values in the dataset. Convert the values to +1 (like), 0 (no rating), and -1 (dislike). Ratings 3 to 5 correspond to liking and ratings 1 to 2 correspond to dislike. 


|        | User 1 | User 2 | User 3 | User 4 |
|--------|--------|--------|--------|--------|
| Food 1 | +1     | +1     | +1     |        |
| Food 2 | +1     |        | -1     |        |
| Food 3 |        | +1     |        | -1     |
| Food 4 | -1     |        | +1     | -1     |


2. You collect two new ratings from User X. User X gives Food 2 a rating of 2 and Food 4 a rating of 5. Update the table to reflect the new ratings from User X. 

|        | User 1 | User 2 | User 3 | User 4 | User X |
|--------|--------|--------|--------|--------|--------|
| Food 1 | +1     | +1     | +1     |        |        |
| Food 2 | +1     |        | -1     |        | -1     |
| Food 3 |        | +1     |        | -1     |        |
| Food 4 | -1     |        | +1     | -1     | +1     |

3. Use the formula $r_{xi} = \sum_{j\in(f1, f2)} s_{ij}r_{xj}$ to find the best food to recommend. Your colleague found that $r_{x1} = 0$ and that $r_{x2} = -1.8660$. You will calculate $r_{x3}$ and $r_{x4}$. What food would you recommend to User X?

Food 1:

$s_{1,2} = \frac{(1)(1) + (1)(-1)}{\sqrt{(1^2) + (1^2) + (1^2)} * \sqrt{(1^2) + (-1^2) + (-1^2)}} = 0$

$s_{1,4} = \frac{(1)(-1) + (1)(1)}{\sqrt{(1^2) + (1^2) + (1^2)} * \sqrt{(-1^2) + (1^2) + (-1^2) + (1^2)}} = 0$

$r_{x1} = \sum_{j\in(f2, f4)} s_{ij}r_{xj} = 0$ 

For Food 3:

$s_{3,2} = 0$

$s_{3,4} = \frac{(-1)(-1)}{\sqrt{(1)^2 + (-1)^2} * \sqrt{(-1^2) + (1^2) + (-1^2) + (1^2)}} = \frac{1}{\sqrt{2} + \sqrt{4}} ≈ 0.2929$

$r_{x3} = \sum_{j\in(f2, f4)} s_{ij}r_{xj} ≈ (0)(-1) + 1(0.2929) = 0.2929$ 


$r_{x3}$ is higher than $r_{x1}$. So, we will recommend **Food 3** to User X. 
