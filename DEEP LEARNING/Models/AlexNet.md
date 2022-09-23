
paper: [***ImageNet Classification with Deep Convolutional Neural Networks**](https://proceedings.neurips.cc/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf)


It was the architecture that reignited the research on CNN in 2013. It improved from the LeNet (1995) by going from 5 to 8 layers.  It was state of the art in imagenet in 2013. It used a non-saturating unit to help with convergence and a dropout to fight against the over-fitting that was normal with this kind of depth.

![[alexnet.png]]


The sparse connections is because the model is spread across 2 gpus (you know, before good libraries could manage multiple gpus easily).

___
#Vision #Architecture #ConvolutionalNetwork 

