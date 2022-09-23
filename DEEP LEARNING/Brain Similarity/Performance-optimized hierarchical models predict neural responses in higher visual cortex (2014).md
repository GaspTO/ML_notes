Paper: [Performance-optimized hierarchical models predict neural responses in higher visual cortex](https://www.pnas.org/doi/pdf/10.1073/pnas.1403112111)

# Overview

The summary of the paper: **A model with perfect neural predictivity in IT will necessarily exhibit high performance, because IT itself does. Here we demonstrate that the converse is also true within a biologically appropriate model class**. They explore a wide range of biologically plausible hierarchical neural network models and then assess them against measured IT and V4 neural response data. They show that there is a strong correlation between a model’s performance on a challenging high-variation object recognition task and its ability to predict individual IT neural unit responses.


## Evaluation
They collected some neurons response to certain images in the IT region of the brain. Then they make a linear regression using, as variables, the neurons of the network model and try to predict the firing of one particular real life neuron

![[Pasted image 20220330165016.png|700]]

In the previous figure, there seems to be a correlation between performance on the test set and correlation with the IT neurons firing. Which might be useful to use the firings as a validation method to see if it seems the model is doing well.



![[Pasted image 20220330171623.png|400]]