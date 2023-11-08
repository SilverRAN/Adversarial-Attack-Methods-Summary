# [Segment Anything Meets Universal Adversarial Perturbation](https://arxiv.org/abs/2310.12431)

## 摘要
不同于传统的image-centric的攻击方法，本文提出了一种perturbation-centric的通用攻击方法（Universal Adversarial Perturbation, UAP）。

## 方法
传统的基于梯度攻击的方法是针对每张特定的图片生成一个扰动，而image-agnostic方法意图找到一个对绝大部分输入图片都生效的扰动。有些方法通过在PGD每次迭代过程中更改初始图片的方法来防止生成的扰动对某一张特定图片过拟合，以提高生成的扰动的泛化能力。但是，这种方法生成的扰动的攻击效果很不好，文章推测是因为在每次迭代中更换图片导致其优化目标发生了更改，不利于优化。本文舍弃了之前着重于 $x\_{clean}$ 和 $x\_{adv}$ 各自生成的mask的角度，而将UAP视为一个独立的输入，并提出用自监督对比学习的方法来优化UAP。

但是，文章中对方法的描述非常不清晰。从Figure.1(b)中我们可以看到，待优化的perturbation被视为anchor sample，其augmented后的结果为positive sample，自然图片经image encoder处理后的feature maps作为negative sample，优化目标是最小化anchor sample和positive sample的距离而最大化anchor sample与negative sample之间的距离。然而，图中没有解释此处的model究竟是什么（是SAM？），以及perturbation是以什么样的形式输入model？如果是噪声图，噪声图为何要直接输入SAM？又如何能保证它加在自然图像上能起到攻击效果？如果是叠加输入，那为什么要最小化一个对抗样本和一个augmented对抗样本的predict之间的距离？
