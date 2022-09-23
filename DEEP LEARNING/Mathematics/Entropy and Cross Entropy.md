# Overview
Entropy and Cross Entropy is a way of measuring information.

For instance, if you have to toss a coin in the air and it has 50/50 chances in each side, you have no idea of what's about to happen, your information is minimal or, in other words, your entropy is maximal. But, if one side has 80% chance and the other 20%, then you have some clue about what side is going to win. Your entropy is not as big.


# Entropy
**You can think of entropy as the average amount of questions you have to ask to be certain of something.**

For instance, let's say you're pulling a ball out of a bag of balls. There are 4 types of balls: Green, Blue, Red, Yellow.

If all are equally likely to come out, then the minimal number of questions you need to ask is 2.  First, is it green or blue? If yes, is it green? If not, is it red? 

But what if they have different probabilities? Let's say Green: 0.5, Blue:0.25, Red: 0.125, Yellow: 0.125.  

You can think of this as an unbalenced tree:
![[Pasted image 20220205175638.png|300]]

If it is green, the number of questions needed to be asked were 1. If Blue, then 2. If Red or Yellow, 3.  If we calculate the average value of questions needed to be asked: $0.5 \cdot 1 + 0.25 \cdot 2 + 0.125 \cdot 3 + 0.125 \cdot 3 = 1.75$

A way of knowing the number of questions is to know the depth of the tree. An event of probability 0.25 is at depth 3 or $log_2(1/0.25)$.   The total number of branches at that depth is $1/0.25$.

In other words, the average number of questions to some even with probability, $p$ is $log_2(1/p)$.

You can not always put everything in such a clean tree. For instance if, in the two coins example, one of the faces of the coin has $75\%$ chance and the other $25\%$, then the amount of questions associated with the first is $log_2(1/0.75) = 0.41$ and, obviously, you can not just ask $41\%$ of a question, but the interpretation is still the same.  

The entropy of this scenario (the average amount of questions) is therefore $0.75 \cdot log_2(1/0.75) + 0.25 \cdot log_2(1/0.25) = 0.81$

**ENTROPY CAN BE ALSO SEEN AS THE NUMBER OF BITS NEEDED IN NUMBER OF BITS NECESSARY TO ENCODE THE INFORMATION**. Each bit corresponds to an hypothetical question and answer with yes or no (1 or 0).


# Cross Entropy
Now, let's say you do not know the real probability of the events when you encoded you decided the number of questions to ask.

(To continue)