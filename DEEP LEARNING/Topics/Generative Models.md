[Stanford Lecture about Generative Models (1:17:40)](https://www.youtube.com/watch?v=5WoItGTWV54&ab_channel=StanfordUniversitySchoolofEngineering)

# Overview
The task: Given training data, generates new samples from the same distribution. 
![[Pasted image 20220128010532.png]]

Two types:
- Explicit density estimation: explicitly define and solve $p_{\text{model}}(x)$.
- Implic density estimation: learn model that can sample from $p_{\text{model}}(x)$	 without explicitly defining it.


## Why Generative Models?
- Realistic samples for artwork, super-resolution, colorization, etc.
- Generative models of time-series data can be used for simulation and planning (reinforcement learning applications!)
- Training generative models can also enable inference of latent representations that can be useful as general features


## Taxonomy
![[Pasted image 20220128011006.png]]