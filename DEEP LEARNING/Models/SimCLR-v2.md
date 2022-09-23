Paper: [***Big Self-Supervised Models are Strong Semi-Supervised Learners***](https://arxiv.org/pdf/2006.10029.pdf)

**(Incomplete)**

# Overview
It's three things:
1.  To fully leverage the power of general pretraining, we explore larger ResNet models. Unlike SimCLR and other previous work, whose largest model is ResNet-50 (4×), we train models that are deeper but less wide. The largest model we train is a 152-layer ResNet with 3× wider channels and selective kernels (SK), a channel-wise attention mechanism that improves the parameter efficiency of the network. By scaling up the model from ResNet-50 to ResNet-152 (3×+SK), we obtain a 29% relative improvement in top-1 accuracy when fine-tuned on 1% of labeled examples.

2. We also increase the capacity of the non-linear network g(·) (a.k.a. projection head), by making it deeper.2 Furthermore, instead of throwing away g(·) entirely after pretraining as in SimCLR, we fine-tune from a middle layer (detailed later). This small change yields a significant improvement for both linear evaluation and fine-tuning with only a few labeled examples. Compared to SimCLR with 2-layer projection head, by using a 3-layer projection head and fine-tuning from the 1st layer of projection head, it results in as much as 14% relative improvement in top-1 accuracy when fine-tuned on 1% of labeled examples.

3. We also incorporate the memory mechanism from MoCo, which designates a memory network (with a moving average of weights for stabilization) whose output will be buffered as negative examples. Since our training is based on large mini-batch which already supplies many contrasting negative examples, this change yields an improvement of ∼1% for linear evaluation as well as when fine-tuning on 1% of labeled examples (see Appendix D).


# Fine Tuning
Fine-tuning is a common way to adapt the task-agnostically pretrained network for a specific task. In SimCLR, the MLP projection head g(·) is discarded entirely after pretraining, while only the ResNet encoder f(·) is used during the fine-tuning. Instead of throwing it all away, we propose to incorporate part of the MLP projection head into the base encoder during the fine-tuning. In other words, we fine-tune the model from a middle layer of the projection head, instead of the input layer of the projection head as in SimCLR. Note that fine-tuning from the first layer of the MLP head is the same as adding an fully-connected layer to the base network and removing an fully-connected layer from the head, and the impact of this extra layer is contingent on the amount of labeled examples during fine-tuning (as shown in our experiments).


# Self-training / knowledge distillation via unlabeled examples
![[Pasted image 20220520143923.png]]

___
#SelfSupervisedLearning #ContrastiveLearning #Vision 