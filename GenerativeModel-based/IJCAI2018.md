# AdvGAN: [Generating adversarial examples with adversarial networks](https://arxiv.org/abs/1801.02610)

GAN / Black-box / One-shot attack
### 摘要
本文提出AdvGAN，旨在通过生成网络来学习和模拟原始数据的分布，以高效率地产生高质量的对抗样本。文章在半白盒(Semi-white box)和黑盒(Black box)场景下测试了模型的攻击性能。
主要贡献：
1. 不同于以往的基于优化的方法，本文训练了一个条件对抗网络来生成对抗样本；
2. 提出了一种基于蒸馏模型的黑盒攻击方法；
3. 在使用SOTA defense方法的场景下，AdvGAN实现了最高的攻击成功率；
4. 在MNIST challenge(2017)拿到了了Top精度。

### 方法
本质上还是一个简单的GAN网络，原始输入通过一个生成器网络 $\mathcal{G}$ 得到扰动 $\mathcal{G}(x)$ , 与原图叠加得到对抗样本 $\mathcal{G}(x) + x$ , 然后分别输入一个判别器 $\mathcal{D}$ 和要攻击的目标模型 $f$ , 得到与原始输入相似性的损失 $\mathcal{L}\_{GAN}$ 以及与真实标签差异的损失 $\mathcal{L}\_{adv}$ 。此外，为了将产生的扰动限制在一个较小范围内，再添加一个损失 $\mathcal{L}\_{hingn}$ 。优化的目标loss为三者的加权和。

### 实验
在MNIST和CIFAR-10上面进行了semi-whitebox和black-box攻击测试，并在ImageNet上实验了semi-whitebox attack。
