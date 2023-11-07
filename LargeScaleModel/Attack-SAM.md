# Attack-SAM: Towards Attacking Segment Anything Model with Adversarial Examples

## 摘要
文章研究了以SAM为代表的视觉基础模型面对对抗样本时的稳健性，通过基础的FGSM和PGD方法，实现了语义掩码移除和生成指定的掩码。局限性是方法没什么创新，只是利用已有的FGSM和PGD方法在SAM上做了测试，换了个Loss函数，实验也不够充分；整体上都是white-box attack，只有cross-task部分勉强可以看成是black-box attack；此外没有测试physical world attack。

## 方法
### Task definition
在本研究中，由于SAM生成的masks是没有语义标签的，因此没有办法计算其生成结果与对应类别之间的loss。一个相对简易的方法是直接使其移除所有的masks，即识别不到任何内容。一个输入图片 $x$ ，当其像素 $x\_{ij}$ 位置的预测值 $y\_{ij} > 0$ 时即被识别为mask，否则被识别为background。研究的目标是让SAM在所有像素位置的预测值均为 $y\_{ij} < 0$ 。

### Loss design
为了实现上述目标，期望设计一个loss以实现如下目标：1）可以把SAM的预测值 $y$ 降为负数；2）对原本较大的正预测值 $y\_{ij}$ 施加更大的惩罚，而对较小的正值或非正值施加较小的惩罚；3）为降低随机性，将预测值降低到一个特定的负值，而不是仅仅比0小一点。综合上述要求，可以使用带有特定负阈值的MSE loss：

$$ \delta\^{*} = \mathop{\text{min}}\_{\delta \in \mathbb{S}} \parallel SAM(prompt, x\_{clean} + \delta) - Neg\_{th} \parallel \^{2} $$

但是，上述Loss会使得已经小于 $Neg\_{th}$ 的预测值在迭代更新时逐渐接近 $Neg\_{th}$ ，而实际上这个行为是没有意义的。因此，文章提出直接裁切掉原本已经小于 $Neg\_{th}$ 的那些预测值，从而使得这些地方的梯度为0。这被称为ClipMSE loss:

$$ \delta\^{*} = \mathop{\text{min}}\_{\delta \in \mathbb{S}} \parallel Clip(SAM(prompt, x\_{clean} + \delta), min=Neg\_{th}) - Neg\_{th} \parallel \^{2} $$

### Attack details
主要使用了FGSM和PGD两种基本的攻击方法，扰动强度被设置为8/255，步长分别为8/255和2/255。PGD的迭代次数为10次，阈值 $Neg\_{th}$ 设置为-10。

## Experiment
实验部分主要给出的是使用point prompt的情况下SAM受对抗样本影响的结果。实验的定量结果其实并不充分，没有在特定的数据集上进行严格的定量评估，而感觉仅仅是拿几张图随便测试了一下。实验结果显示使用ClipMSE作为loss时PGD-10的攻击效果会明显优于MSE loss，而FGSM受影响不大。

## Transfer-based attacks
这里文章探索了两种迁移场景：1）cross-prompt transfer，即在 $(prompt\_{i}, x\_{adv})$ 的前提下攻击成功的话，将输入换成 $(prompt\_{j}, x\_{adv})$ ；2）cross-task transfer，即将针对分类器的对抗样本输入SAM，看是否会影响它的分割效果。

Cross-Prompt transfer情境下，发现在更换point prompt的位置之后攻击效果大大下降，认为攻击只对 $prompt\_{source}$ 的周边作用比较明显。尝试增加 $prompt\_{source}$ 的数量 $K$ 之后，发现对于全图的攻击效果都显著提升。

Cross-Task transfer情境下，发现使用针对分类模型的对抗样本也能一定程度上干扰SAM的分割结果，尽管干扰效果非常有限。此处使用的分类模型是ViT。

## Beyond mask removel
上述实验仅仅探讨了mask移除的目标。这里探讨了是否可能生成指定的mask，包括mask扩大、mask操纵（偏移、翻转等）以及生成指定mask。

针对mask扩大，通过与mask移除类似的思路，只是将原本loss中的阈值 $Neg\_{th}$ 从负数换成正数：

$$ \delta\^{*} = \mathop{\text{min}}\_{\delta \in \mathbb{S}} \parallel Clip(SAM(prompt, x\_{clean} + \delta), max=Pos\_{th}) - Pos\_{th} \parallel \^{2} $$

文章发现，仅仅通过变换阈值就可以实现从mask到background以及从background到mask的转换。

其中比较有意思的是实现了mask的移动、翻转和指定形状mask的生成，然而遗憾的是，正文包括附录中都没有具体介绍实现的方法和loss设计。猜测其实现方法应该就是把指定的mask当作PGD的优化target。
