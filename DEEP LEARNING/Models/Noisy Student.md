Paper: [***Self-training with Noisy Student improves ImageNet classification***](https://arxiv.org/pdf/1911.04252.pdf)

# Overview
The idea is to train a teacher, then use the [[distillation]] mechanism to train a better student and then use that student as a teacher to a new student network, progressively incrementing the quality of the newer students. The student is usually a more complex network (unlike the distillation paper).

The new student also learns based on the outputs of the unlabeled data of the teacher.

The student learns using noise (noisy student) , which is the real merit of this paper.  The formal algorithm is:
![[Pasted image 20220913005029.png|400]]

The student noise can be divided into input and model noise:
- For input noise, we use data augmentation with [[RandAugment]]
- For model noise, we use [[Dropout (to do)|Dropout]]  and [[Stochastic Depth]].

# Experiments
## Adversarial Robustness
It improves
![[Pasted image 20220527043427.png|300]]

# Ablation Experiments
## The Importance of Noise in Self-training
Since we use soft pseudo labels generated from the teacher model, when the student is trained to be exactly the same as the teacher model, the cross entropy loss on unlabeled data would be zero and the training signal would vanish. Hence, a question that naturally arises is why the student can outperform the teacher with soft pseudo labels. they hypothesize that noising the student is needed so that it does not merely learn the teacher’s knowledge.

Varying noise in the student. In addition, we compare using a noised teacher and an unnoised teacher to study if it is necessary to disable noise when generating pseudo labels:
![[Noisy student noise study.png|300]]


noise such as stochastic depth, dropout and data augmentation plays an important role in enabling the student model to perform better than the teacher.


## A Study of Iterative Training
It helps.
![[Noisy student iterations.pn]]


## Additional Ablation Study Summarization
Several findings summarized here:
1.  Using a large teacher model with better performance leads to better results.
2.  A large amount of unlabeled data is necessary for better performance.
3.  Soft pseudo labels work better than hard pseudo labels for out-of-domain data in certain cases.
4.  A large student model is important to enable the student to learn a more powerful model.
5.  Data balancing is useful for small models. 
6.  Joint training on labeled data and unlabeled data outperforms the pipeline that first pretrains with unlabeled data and then finetunes on labeled data.
7.  Using a large ratio between unlabeled batch size and labeled batch size enables models to train longer on unlabeled data to achieve a higher accuracy.
8.  Training the student from scratch is sometimes better than initializing the student with the teacher and the student initialized with the teacher still requires a large number of training epochs to perform well.
	
	
# Notes
In typical self-training with the teacher-student framework, noise injection to the student is not used by default, or the role of noise is not fully understood or justified. The main difference between this work and prior works is that this one identifies the importance of noise, and aggressively inject noise to make the student better.
	

___
#General #Vision  #ConvolutionalNetwork  #EfficientNetwork  #Distillation #TransferLearning


