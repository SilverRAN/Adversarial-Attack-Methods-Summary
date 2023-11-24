# [Semantic Adversarial Attacks via Diffusion Models](https://arxiv.org/abs/2309.07398)

## [Github repository](https://github.com/steven202/semantic_adv_via_dm)

## 总结
**Semantic-attack / White-and-black-box / Diffusion**

目前基于语义攻击的方法有两大方向：1）转换色彩和纹理特征；2）在潜在空间里进行操作以实现语义编辑。作者认为过去的方法存在以下问题：1）基于GAN的方法不够真实；2）生成过程耗时很长，无法实现大规模攻击。因此，基于diffusion model，本文提出了ST(Semantic Transformation)方法和LM(Latent Masking)方法。

## 方法
ST方法是基于DiffusionCLIP，给定一张原图 $\text{x}\_{0}$ 和一个属性prompt $t$ , 通过DiffusionCLIP生成编辑后的图像，然后计算分类器与真实标签之间的loss以反向传播并更新latent space中的 $\text{x}\_{T}$ . 这种方法可以说跟之前基于attribute-conditioned GAN的方法非常相似，仅仅是把GAN换成了DM。

至于LM方法则是基于图像合成的思想，通过Grad-CAM或saliency maps方法找到原图中对于目标分类器贡献较大的部分，然后将其替换（或部分替换）为另一张类似图片的对应部分，以实现误导分类器的效果。**缺点是仍然需要待攻击模型的梯度信息，并且对待攻击图像和目标图像的相似度要求较高**。
