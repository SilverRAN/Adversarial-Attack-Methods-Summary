# FGSM: [Explaining and Harnessing Adversarial Examples](https://arxiv.org/abs/1412.6572)

总结：提出了对抗样本的线性解释，并设计了基于梯度反向传播，获取每个像素对loss贡献的大小，然后对每个像素添加不同大小的扰动以使得loss增大（与传统的训练目标相反）的经典对抗攻击方法。这被称为快速梯度符号法（Fast Gradient Sign Method，FGSM）。
