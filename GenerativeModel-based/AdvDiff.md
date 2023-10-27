# [AdvDiff: Generating Unrestricted Adversarial Examples using Diffusion Models](https://arxiv.org/abs/2307.12499)

## 摘要
过去基于GAN的对抗样本生成方法理论上可解释性差，且生成结果不够真实，与自然数据有较大差异。文章提出了一种基于扩散模型产生无限制对抗样本的方法，其中包含了两种新的对抗引导方法。所提出的方法在MNIST和ImageNet上的表现超过了基于GAN的方法。

## 方法-Adversarial Diffusion Sampling
### Rethinking Unrestricted Adversarial Examples
在AC-GAN的文章中提出了Unrestricted Adversarial Examples的概念，也就是不向原始图片中添加像素扰动，而是直接从头生成一个新的样本，并且能够骗过目标分类器，即可以看成分类任务中的假阴性(False negetive)样本。过去的方法是在GAN的latent空间中添加扰动。理想情况下，产生的对抗样本与真实数据应该很难区分。但实际上，由于GAN训练所使用的数据分布 $z$ 和对抗样本所从属的分布 $z\_{adv}$ 存在差异，使用一般的GAN来生成对抗样本会显著降低其生成质量。而生成质量的好坏在Attack Success Rate(ASR)上是体现不出来的。

相比之下，diffusion model具有


## 实验
