
# Overview
This is an experiment about training on imagenet. 
I took advantage of the code that I did for [[Reducing Texture bias by fine-tuning|VISUM]], just to try to get some intuiton behind training models, since I haven't done much of it so far.

# Reports
## 3rd Week of July 2022
I have been using a Resnet-18 and using simple data augmentations (besides the standard normalization and the 224-resizing). I've been using only 20% of imagenet to train.

* I tried the 
 *torchvision.models.ResNet50_Weights.DEFAULT.transforms()* and uses Adam 1-e04 and it overfitted (training acc on 90-something % and val accuracy on 20-something %). I think it was Adam's fault, but it might have been the way I was validating, because I was only resizing and normalizing for the validation.


* Attempt two. Following transforms: 
![[Pasted image 20220802173837.png|400]]
SGD with lr=1e-4, momentum=0.9
I thought I was using a resnet, specifically resnet-18, but the code is using Alexnet, so now I don't if I used resnet-18, resnet-50 or alexnet.

### Results:

![[Pasted image 20220802173433.png]]
![[Pasted image 20220802173536.png]]
![[Pasted image 20220802173553.png]]![[Pasted image 20220802173606.png]]
![[Pasted image 20220802173500.png]]
![[Pasted image 20220802173509.png]]
![[Pasted image 20220802173519.png]]
![[Pasted image 20220802173635.png]]

## 7-9 August 2022
**Success, we achieved 70% on resnet18, higher than the torchvision pretrianed which is 69%. It took 2 days as 12 hours, 86 epochs.** 
The ReduceLRonPlateau was very good. If you see in the training, there are spikes at 400k, almost 800k and 1.2M, those are when the learning rate decreases. Batch was 64.
![[Pasted image 20220809183823.png|400]]
Resnet18, Transfroms:
![[Pasted image 20220809183628.png|300]]![[Pasted image 20220809183406.png|200]]
![[Pasted image 20220809183414.png|200]]
![[Pasted image 20220809183452.png|200]]
![[Pasted image 20220809183507.png|200]]
![[Pasted image 20220809183519.png|200]]

## 9 -15 August
Two experiments
**Blue** and **Red**, both are similar to the previous except they have different data augmentations for training and a batch size of 256 instead of 64.

for **Blue**, the data augmentation is;


# Conclusions
Stepwise lr is very good.
It's not that hard to train a network, they seem to be pretty consistent in results.