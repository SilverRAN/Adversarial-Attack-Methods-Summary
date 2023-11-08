# [Black-Box Targeted Adversarial Attack on Segment Anything](https://arxiv.org/abs/2310.10010)

## 主要内容
在black-box的情景下实现targeted adversarial attack，即产生一个对抗样本，使得SAM的预测结果与另一张参照的自然图像的mask类似。

## Boosting Cross-prompt Transferability
### Cross-prompt Attack-SAM
正如之前Attack-SAM的工作中所展现的那样，想要减小adversarial example对于prompt位置的依赖性，最直接的方法就是增加adversarial example生成过程中给定的prompt的数量，实验中训练时的prompt point取值范围为1～100。而为了让SAM的预测与给定的mask相接近，直接用其作为优化目标：

$$ \delta\^{*} = \mathop{\text{min}}\_{ \delta \in \mathbb{S} } \sum\_{k} \parallel SAM(prompt\^{(k)}, x\_{clean} + \delta) - Thres( SAM( prompt\^{(k)}, x\_{target} ) ) \parallel\^{2} $$

此处的图表没有看懂……为什么随着训练时prompt point的增加，训练损失下降而测试损失上升？

### Our Proposed Method
无论如何，通过增加训练时的prompt point的数量的方法来提升adversarial example的泛化性的做法是有限的。而考虑到SAM的网络结构，如果直接攻击其image encoder部分，使其产生与参照图像接近的feature maps，就可以无视prompt的位置，即针对任何prompt都具有泛化性：

$$ \delta\^{*} = \mathop{\text{min}}_{\delta \in \mathbb{S}} \mathcal{L}( SAM\_{embedding}(prompt, x\_{clean} + \delta), SAM\_{embedding}(prompt, x\_{target}) ) $$

此外，文章还探讨了black-box的情景，即在无法获取模型内部参数梯度的情况下的攻击效果。通过引入Relative Feature Strength的概念，发现随着迭代次数的增加，adversarial example的feature dominance是下降的，即泛化能力减弱。为此，文章在原有的MES loss项之后又加了一个regularization loss，但文中并没有给出具体的形式。本文的black-box情景被设置为可以使用SAM-B来得到adversarial examples，然后测试它们在SAM-L和SAM-H上的表现。
