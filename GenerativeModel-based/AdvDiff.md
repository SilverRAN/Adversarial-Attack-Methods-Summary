# [AdvDiff: Generating Unrestricted Adversarial Examples using Diffusion Models](https://arxiv.org/abs/2307.12499)

## 摘要
文章认为过去基于GAN的对抗样本生成方法理论上可解释性差，且生成结果不够真实，与自然数据有较大差异。同时，现有的基于Attack Success Rate(ASR)的评价方法无法体现生成质量的好坏。扩散模型的生成质量比较高且可解释性更强，因此文章提出了一种基于扩散模型产生无限制对抗样本的方法，通过向扩散过程的每一步采样中逐渐添加不可察觉的微小扰动来保证生成结果的质量。所提出的方法在MNIST和ImageNet上的表现超过了基于GAN的方法。

## 方法-Adversarial Diffusion Sampling
### Adversarial Guidance
在Classifier Guidance Diffusion Model中，通过向Diffusion Model添加一个分类器的梯度引导，实现了根据类别生成样本。与Classifier-guided Guidance diffusion model类似，文章将对抗梯度注入到扩散过程中，从而使模型在基于给定标签 $y$ 的同时向攻击目标 $y\_{adv}$ 靠近。文章希望使用一个conditional diffusion model来生成符合ground truth $y$ 的样本 $x\_{0}$ ，同时该样本可以骗过目标分类器使得 $p\_{f}(x\_{0}) \not= y$ 。其关键在于将反向扩散过程中每一步生成样本关于分类器的梯度替换为攻击目标的对抗梯度，并注入到每一步的扩散过程中。文章发现给定一个攻击标签 $y\_a$ 有助于这一过程：

$$ x\_{t-1} = \mu(x\_{t}, y) + \sigma\_{t} \varepsilon, $$

$$ x\_{t-1}\^{*} = x\_{t-1} + \sigma\_{t}\^{2} s \nabla\_{x\_{t-1}} \text{log}\_{p\_{f}} (y\_{a} | x\_{t-1}) $$

通过上述过程，原本属于类别 $y$ （例如狗）的初始样本 $x$ （狗的图片）和假类别 $y\_a$ （猫）被一起输入，而diffusion model在分类器的错误引导下将狗的视觉特征与分类器的结果“猫”建立起了联系，从而可以生成很多会被分类器识别为“猫”的狗图片。

## 实验
在MNIST和ImageNet数据集上进行了测试。
