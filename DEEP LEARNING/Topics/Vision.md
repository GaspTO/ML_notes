### Dropout
Said in the inception paper: *Going deeper with convolutions*: "For larger datasets such as Imagenet, the recent trend has been to increase the number of layers and layer size , while using dropout to address the problem of overfitting"

### Batch Normalization
Since it's introduction in Inception-v2 that it became a go to, mainly accelarating learning. A theory is that it also acts as a regularizer.

### Small filters
The tendency (especially since VGG) has been to use small filters. 

### Residual Connections
In inception-Resnet-v2 paper the findings were that residual connections  did not necessarily converged to a better optimum, but they fastened the training. In the original resnet paper, these connections helped with the final results.

### Training Error and Test Error
If the training error also diminuishes, it seems to indicate learning stronger features. If it is only the testing error, it can be just regularization (from the ResNext paper).