# [Diffusion-Based Adversarial Sample Generation for Improved Stealthiness and Controllability](https://arxiv.org/abs/2305.16494)

## 方法-Diffusion-based PGD
### 1.SDEdit方法
Stochastic Differential Editing方法的基本思想是：给定一个不属于真实图像分布的样本(如一张简笔画 $Stroke$ )，我们希望将它转换成一个真实图像 $Image$ 。通过前向加噪过程扩大两个分布的支撑集，直到两者产生交集。然后从交集中的一点出发，用在真实图像上预训练好的扩散模型逆向去噪，回到真实分布。
![image](https://github.com/SilverRAN/Adversarial-Attack-Methods-Summary/assets/89587304/075c8b65-b84e-410b-9d8c-069c624fa219)

### 2.传统PGD方法
Projected Gradient Descent通过梯度反向传播迭代优化初始输入。给定一个干净图像 $x$ 和其对应的真实标签 $y$ ，期望生成一个对抗样本 $x\_{adv}$ 来欺骗目标分类器 $f\_{\theta}$ 。对抗梯度的损失表示为 $g=\nabla\_{x} l ( f\_{\theta}(x), y )$ 。PGD中步长为 $\eta$ 且迭代次数为 $n$ 的第 $t$ 步更新为：

$$ x\^{t+1} = \mathcal{P}\_{B\_{\infty} (x,\epsilon) } \[ x\^{t} + \eta \ \text{sign} \nabla\_{x\^{t}} l (f\_{\theta}(x\^{t}),y) \] $$

但文章认为这种方法会使样本远离决策边界，从而偏离真实样本分布 $p(x\_{0})$ 。因此，文章提出，对于PGD的第 $t$ 步迭代中，先使用扩散模型的SDEdit方法求解一个符合真实图像分布的纯化样本 $x\_{0}\^{t}$ ，然后使用该纯化样本 $x\_{0}\^{t}$ 和原始标签 $y$ 来计算对抗梯度损失：

$$ x\_{0}\^{t} = \text{SDEdit} (x\^{t},K) \quad \text{and} \quad x\^{t+1} = \mathcal{P}\_{B\_{\infty} (x,\epsilon) } \[ x\^{t} + \eta \ \text{sign} \nabla\_{x\^{t}} l (f\_{\theta}(x\^{t}\_{0}),y) \] $$

最终，经过 $n$ 次迭代之后，Diff-PGD会生成两个彼此略有不同的输出 $(x\_{0}\^{n}, x\^{n})$ ，其中 $x\^{n}$ 是仅通过PGD方法得到的对抗样本，与真实样本分布有差异； 而 $x\_{0}\^{n}$ 是通过SDEdit迭代纯化后的样本，它从属于对抗样本和真实样本两个分布的支撑集交集，因而是现实的和对抗的。
