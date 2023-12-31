---
tags:
  - AI/GAI/CV/Diffusion
aliases:
  - Diffusion Models
  - Diffusion
---
The Generative Diffusion Model (GDM) is a cutting-edge class of generative models based on probability, which demonstrates SotA results in the field of [[Computer vision]].

It works by progressively corrupting data with multiple-level noise perturbations and then learning to reverse this process for sample generation.

## Diffusion Model

#### Model Formulations
Diffusion Models are mainly formulated into 3 categories:
[[DDPM]]
[[SGM]] 
[[NCSN]]
Score SDE generalizes previous two formulations into continuous settings, where noise perturbations and denoising processes are solutions to stochastic differential equations.

#### Training Enhancement
[[TDPM]] and [[ES-DDPM]] improve sampling speed by truncating the diffusion process with early stop. To generate sample from reverse process initialized by a non-Gaussian distribution, another pre-trained generative model such as [[VAE]] or [[GAN]] is introduced to approximate such distribution.

#### Efficient Training-free Sampling
Instead of additional training, training-free sampling directly reduce the number of discretized time steps while minimizing discretization errors. Under same training objective, [[DDIM]] generalizes [[DDPM]] to a class of non-Markovian diffusion process and introduces jump-step acceleration. [[Analytic-DPM]] provides more efficient inference by estimating the analytic form of optimal model reverse variance and KL-divergence w.r.t its score function. There are also works which directly figure out optimal sampling trajectories via [[Dynamic programming]].

#### Noise Distribution
The distribution of noise is an essential part of diffusion models and most of them are Gaussian. Meanwhile, fitting such distribution with more degrees of freedom could benefit performance.

#### Mixed Modeling
Mixed-modeling is aimed at combining diffusion model with another category of generative model to take al their advantages, which could provide stronger expressiveness or higher sampling speed. 
- [[DiffuseVAE]]: merges a standard [[VAE]] into the [[DDPM]] pipeline by conditioning diffusion sampling process with blurry image reconstructions generated by VAE.
- [[LSGM]]: trains [[SGM]]s in the latent space of VAE, which generalizes SGMs into non-continuous data and enables smoother SGMs learning in a small space.
- Denoising diffusion GANs introduces conditional GANs into DDPM pipeline to parameterize denoising process with a more expressive multimodal distribution, which provides large denoising steps.
- [[DiffFlow]]: integrates flow function into trajectories of SDE-based diffusion model, which makes forward steps also trainable.

## Diffusion Models

```dataview
TABLE date, parameters, author
FROM #Model  AND #Diffusion  
SORT date DESC
```


## Diffusion; Embedding more information

>[!note]
>[[U-Net]] can take in more information in the form of embeddings

- __Time embedding__: related to the timestep and noise level
	- Information about what kind of noise level we need

- __Context embedding__: related to controlling the generation, e.g. text description or factor (more later).

![[Screenshot 2023-09-09 at 11.08.50 PM.png]]

```python
# Embed context and timestep
cemb1 = self.contextembed1(c).view(-1, 2 * n_feat, 1, 1)
temb1 = self.timeembed1(t).view(-1, 2 * n_feat, 1, 1)

up2 = self.up1(cemb1 * up1 + temb1, down2)
```


## Training Diffusion model


Goal: Predict noise, and really it learns the distribution of what is noise on the image, but also what is not noise.

![[Screenshot 2023-09-09 at 11.17.57 PM.png]]

![[Pasted image 20230916010759.png]]


![[Pasted image 20230916010828.png]]

![[Pasted image 20230916010850.png]]

## Denoising in Diffusion Model

![[Pasted image 20230916012125.png]]


![[Pasted image 20230916012136.png]]



## Controlling: Conditional Diffusion Models
We can use a method called _conditioning_ to steer our models outputs toward specific types or classes of images.

![[Screenshot 2023-09-09 at 11.55.43 PM.png]]



## Image corruption
![[Pasted image 20230910193538.png]]
[_Cold Diffusion_](https://arxiv.org/abs/2208.09392)
Adding noise remains the most popular approach for several reasons:
- We can easily control the amount of noise added, giving a smooth transition from 'perfect' to 'completely corrupted'.
- We can have many valid starting points for inference, unlike some methods which may only have a limited number of possible initial states.


## Alternative Architectures

- [[Transformer]] architecture
	- [“Scalable Diffusion Models with Transformers”](https://arxiv.org/abs/2212.09748)
		- Performs very well but with high compute and memory requirements

- [[UViT]] architecture
	- [Simple Diffusion paper](https://arxiv.org/abs/2301.11093)
		- Replacing the middle layers of the U-Net with a large stack of transformer blocks.

- Recurrent Interface Networks
	- [RIN paper](https://arxiv.org/abs/2212.11972) (Jabri et al)


## Improving Efficiency: Latent Diffusion

![[Pasted image 20230910204828.png]]
_Architecture introduced in the [Latent Diffusion Models paper](https://arxiv.org/abs/2112.10752)_
 
- [[Self-attention]], where the amount of operations grows quadratically with the number of inputs.

Latent diffusion tries to mitigate this issue by using a separate model called a [[VAE]]


#### [[Stable diffusion]]: Components in Depth
A text-conditioned latent diffusion model

## See more
- [[Diffusion model in PyTorch]]
- [[Diffusion models; Sampling]]
- [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239)


