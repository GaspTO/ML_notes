# Setup
This experiment was done by calculating the shape bias benchmark and shape dimensionality benchmark, which returns both the shape, texture and residual dim percentage and the raw score, for each of the 4 alternative implementations. 

The feature maps are the ones of the **last layer** (after the last mlp and before the softmax). 16 pretrained models from torchvision were used:
![[Pasted image 20220922170851.png|450]]


**HOLD ON -> THESE FEATURE MAPS CORRESPOND TO CLASSES. THE RESULTS CAN BE MAPPED TO CLASS IMPORTANCE**

The question at hand is: what does it mean to have a high or low score for the shape bias? 

The results make me think we need more data to see if the relations are linear or exponential


# Overview
## Observation Summary:
1. Dim and scores predict very well shape bias (See Results 1 )
2. Score ratio of #0 is better at predicting shape bias, shape and texture match than score ratio of #4. However, score_shape and score_texture of #0 are not better at predicting shape_bias, shape and texture match. (See Results 1 and 2)
3. When, for #4, the score of attribute (shape or texture) correlates positively with that attribute and negative with the other. Is there anything interesting about having the sum of values be positive or negative? (see discussion)
4. Almost all score_shapes (of #2-4) are negative except the ones of the two efficient nets. All score_textures are positive.
5. The graph of score_texture of #0 and #2 is very close, which means the sum of absolutes and the sum is the same, and that is very very likely because, on each neuron, the texture correlation was positive.
6. score and dim of shape can (negatively) predict texture match, and vice-verse for texture and shape, respectively. (See Results 3)


## Interpretation Summary
1. The dim measure is accurate, the correlation seems to matter 
2. I don't have an explaination for this.
3. .
4. .
5. .
6. Roughly speaking, this means that when the neurons focus on shape, they seem to lose focus on texture. This might mean that, unlike previously suggested, that the shape bias is the model losing knowledge on texture and not just gaining on shape.  



## Results
1. ![[Pasted image 20220923012654.png|500]]
2. ![[Pasted image 20220923012845.png|500]]
3. ![[Pasted image 20220923011810.png]]


# Discussion

## Relationship between Shape bias and Shape Dim
 We will try to connect the following concepts: (**Shape-match**, **Texture-match**, **Shape-bias**) with (**Shape-dim**,**Texture-dim**, **Shape-dim ratio**).

#### Scratch:
* It seems that all the 5 dim estimations are good at predicting shape bias, which is going to be the main conclusion of the paper.

* The dim0 and dim1 seem to be slightly worse at predicting shape bias than every other. **But here's a very interesting discovery:** If I look at the ratio, of correlation sum (or scores, $s_F$, in the paper and my code's terminology), the scores of the dim0 and dim1 (which are the same) predict better the shape bias than the scores of the other dimensions. The prediction of the scores of dim0 is 0.82 and the dim2 is 0.66. The prediction of the dim0 shape-dim ratio is 0.815 (lower than the scores) and the dim2 is 0.877.
  
* The ratio of scores of both dim0 and dim4 correlate positively with shape bias. But the scores (not the ratio) correlate, respectively, negatively (-0.79) and positively (0.83). Regarding the dim4, where the scores are the raw sum, it implies that positive correlations indicate neurons that keep shape. This makes sense at the last layer, since, after this, there is a softmax and negative values after applying a texture, would swift the softmax towards lower probabilities. I wonder whether this is the case for previous layers. Maybe negative correlations aren't bad there. **QUESTION:** if this is correct, then blocking the most correlated neurons would destroy the concept of shape and the bias should go to texture.



##  Trade-off between score_shape and score_texture
**When the total score of shape increases, does that mean texture scores decrease?**  We're not talking about dimensionality, since there is a normalization factor, which would of course imply that, when one increases, the other decreases. 

It has been shown before, that a model with more shape bias, continues to be very good at recognizing shape. The shape bias is not, in this case, a decrease in texture understanding, but simply an increase in shape understanding. 

**If we assume "more" shape score equals more shape recognition:**
Here, we plot the shape score against the texture score, and see their correlation. We notice that there is a very good (negative) correlation, meaning that when the neurons understand better shape, they seem to do so at the expense of texture. In a sense, this is expected, since, if we assume there is a *maximum memory* capacity of the network, if it learns more of one concept, it has to let go of a bit of others. We should fit the equation to know *how much* of the concept of texture it loses every time it increases the concept of shape. 
**else:**
But the issue is that, to me, I don't understand why having bigger correlations of shape makes shape match positively correlated. Why not negative? Is it because this is the last layer right before the softmax? In my thinking, the absolute should correlate much more - even though it correlated very well. 
There is, however, an interesting observation. It seems that, for #4, curve fits linearly, but most of the score_shape is negative, only two being positive, and one of them doesn't fit the curve. When we look at #0, there seems to be an inverse exponential curve there. The closer to 0, the better. The assumption so far, is that the higher the score or the higher the absolute score, the better, but what if the more neutral score, the better? The data seems to support that claim too

![[Pasted image 20220923023725.png]]

We also put here the score_texture, It seems that almost each neuron was positive and that's why the sum of their absolute value and their sum seems to give very similar points. It is also not inconceivable that this is an inverse exponential curve.
![[Pasted image 20220923025353.png]]


