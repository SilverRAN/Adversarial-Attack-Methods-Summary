# [FGSM: Explaining and Harnessing Adversarial Examples](https://arxiv.org/abs/1412.6572)

总结：对抗攻击的开山之作，首次提出了对抗样本(Adversarial Examples)的概念，并设计了基于梯度反向传播，获取每个像素对loss贡献的大小，然后对每个像素添加不同大小的扰动以使得loss增大（与传统的训练目标相反）。这被称为快速梯度符号法（Fast Gradient Sign Method，FGSM）。
